## New Claim Process

|  **Overall Scenario – 9 Second Insurance: New Claim Process** |
| ----------- |
| As a systems architect working for 9 Second Insurance, you are tasked with creating a process that will allow agents to create a new damage claim. The process should include: |
| Retrieve customer information from the customer database. |
| Data intake mechanism to capture claim information. |
| Document generation and saving to content management system. |
| Save properties to an Aspect attached to the claim document. |
| Apply a deductible dynamically. |
| Verify the claim information. |


### Lab 1. Create an Configure a New Process.
This lab will walk you through creating a new process and configuring the variables needed to perform actions to support future labs. 
1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
    - Sign in with your provided username and password. You will be directed to the Activiti App home page.
2.	Select the App Designer tile to navigate to the Business Process Models page
3.	To create a new process, press the Create Process button in the top right of the page.
4.	In the Create a new business process model popup, enter the following information:
    -	Model name: ```[Your user #] New Claim``` (ex: U1 New Customer Onboarding)
    -	Description: ```Create a customer claim.```
    -	Editor Type: Leave as BPMN editor
    -	Stencil: Default BPMN
    -	Press the Create new model button.
5.	With nothing selected on the stage, select the Variables attribute in the bottom configuration panel.
6.	Select the “+” icon below the chart to add new variables. Add the following variables:
| Variable Name     | Variable Type     |
| ---               | ---               |
| cFirstName     | String            |
| cLastName     | String            |
| cAddress     | String            |
| cState     | String            |
| cCity          | String            |
| cState     | String            |
| cZip     | String            |
| lu_lastname     | String            |
| recordCount     | Integer            |
| recordList     | String            |
| deductible     | Integer            |
| claimNotes     | String            |
| recordID     | String            |
| incidentDate     | String            |
| incidentType     | String            |
| damageSeverity     | String            |
| value     | String            |
| cStatus     | String            |


### Lab 2. Create an Intake Form on the Start Event. 
**Scenario:** In order to capture new customer data we need a data intake mechanism. In this lab you'll a Form attached to the Start Event of your process, allowing agents to quickly begin building a claim.
1. Select the **Start Event**.
2. In the bottom configuration panel, select the **Referenced Form** attribute.
3. In the Referenced Form popup window, select **New Form**.
4. Name the form: ```Customer Lookup```.
5. Description: ```Look up a customer```.
6. In the form builder, add a **Header** object. Configure with the following:
   - **Label:** ```Enter the Customer's last name:```
   - **Number of Columns:** ```1```
   - _Save and close the Header_
7. Add a **People** (not a Group of People) to the Header Object.
8. Configure the **Poeple  Object** with the following:
   - **Label:** ```Last Name:```
   - _Override the ID_
   - **ID:** ```lookup_lastname```
   - _Check the Required checkbox_
   - _Save and close the Poeple object_
9. Save and close the form.


### Lab 3. Retrieve Customer Information from Customer Database. 
**Scenario:** Using the **People Object** we can retrieve data from the selected user at runtime. We'll use that information to retrieve our full customer data from the 9 Second Insurance relational Customer database.
1. Add a **Script** task to your process connected to the Start Event with the following configuration:
   - **Name:** ```Get DB Values```
   - **Script Format:** ```groovy```
   - **Script:**
```
import groovy.sql.Sql;
import groovy.json.*;
import groovy.json.JsonBuilder;
import com.activiti.security.SecurityUtils;

class Record {
    String recId
    String firstname
    String lastname
    String address
    String city
    String state
    String zip
}

def ln = execution.getVariable("lookup_lastname");
execution.setVariable("lu_lastname", ln);

def url = 'jdbc:oracle:thin:@//aps-custom-oracle-db.cp58lgpzkwpy.us-east-1.rds.amazonaws.com/ORCL';
def user = 'admin';
def password = 'administrator';
def driver = 'oracle.jdbc.driver.OracleDriver';
def sql = Sql.newInstance(url, user, password, driver);

rowNum = 0;
def recordList = [];

sql.eachRow("SELECT ID, FIRSTNAME, LASTNAME, ADDRESSLINE1, CITY, STATE, ZIPCODE FROM CUSTOMERS WHERE LASTNAME = ${lu_lastname}") { row ->
   
    def r = new Record( recId: row.id, firstname:row.firstname, lastname:row.lastname, address:row.addressLine1, city:row.city, state:row.state, zip:row.zipcode)
    execution.setVariable("cFirstName", row.firstname);
    execution.setVariable("cLastName", row.lastname);
    execution.setVariable("cAddress", row.addressLine1);
    execution.setVariable("cCity", row.city);
    execution.setVariable("cState", row.state);
    execution.setVariable("cZip", row.zipcode);
    recordList.add(r);
  
}
execution.setVariable("recordCount", recordList.size);
  println new JsonBuilder( recordList ).toPrettyString();
  execution.setVariable("recordList", new JsonBuilder( recordList ).toPrettyString());

```
2. Add a **User Task** connected to the Script Task.
3. Name it: ```Display DB Values```.
4. Use the **Referenced Form** attribute for the User Task to create a new form for the user task.
5. Name the form: ```Display DB Values```.
6. In the form builder:
   1. Add a **Header** with the following configuration:
      - **Label:** ```Query Results:```
      - **Number of Columns:** ```1```
   2. Add a **Display Text** field to the Header configured with the following:
      - **Label:** ```Database Return:```
      - **Text to Display:** ```Found ${recordCount} records with the Last Name of "${lu_lastname}".```
   3. Add a **Dynamic Table** under the Header.
   4. Configure the General tab of the dynamic table with the follow:
      - **Label:** ```CUSTOMERS```
      - _Override the ID_
      - **ID:** ```recordList```
   5. Select the **Table Columns** tab in the dynamic table editor.
   6. Use the following chart to map the process variables to each Header of the dynamic table.
| **Property ID**       | **Property Name**     | **Property Type**     |
| ---       | ---       | ---       |
| redId     | ID        | String        |
| firstname     | First Name        | String        |
| lastname     | Last Name        | String        |
| address     | Address        | String        |
| city     | City        | String        |
| state     | State        | String        |
| zip     | Zip        | String        |

   7. Add a **Display Text** field under the dynamic table configured with the following:
      - **Label:** ```Prompt:```
      - **Text to Display:** 
```
Verify that this is the correct customer and proceed.
Press Back if customer does not appear.
```
   8. Save and Close the form.
7. Back in your process, add an **End Event** to the Human Task.
8. Save and close your process.
9. Navigate to the **Apps** section of the App Designer.
10. Add the **New Claim** process to your existing application and **Deploy**.
11. Test the functionality of your New Claim process.
   - At this point, you should be able to use the form on the start event to search for a user in the APS database. Select a user that you have added to the relational database from the _Onboarding Process_. The script task should attempt to pull user values from the Customer database and pass those values to the User Task, where those values should be displayed.



### Lab 4. Add Pathing via a Gateway to your process. 
**Scenario:** We're now able to select and retrieve customer values from the 9 Second Insurance database. We now need a method that will allow us to make a different user selection if we chose the wrong user.
1. Enter your New Claim process in edit mode.
2. Select the **Display DB Values** User task.
3. Select the **Referenced Form** attribute and **Open** the form.
4. In the form editor, select the **Outcomes** tab at the top of the form view.
5. Select the **Use custom outcomes for this form** radio button.
6. In the first **Possible Outcomes** field, enter ```Proceed```.
7. Select the **Add Outcome** button.
8. In the new **Possible Outcomes** field, enter ```Back```.
9. Save and close the form.
10. Remove the **End Event**.
11. Add an **Exclusive Gateway** to your process connected from the _Display DB Values_ User Task.
12. Add a **User Task** to the process connected from the Exclusive Gateway.
13. Name it: ```Customer Search```.
14. Select the **Referenced Form** attribute and use the same form created for the Start Event: ```Customer Lookup```.
15. Create a **Sequence Flow Line** from the **Customer Search** User Task back to the _Get DB Values_ Script Task.
16. Select the **Sequence Flow Line** stemming from the _Exclusive Gateway_ and to the _Customer Search_ User Task.
17. In the bottom configuration panel, select **Name** and name it: ```back```.
18. In the bottom configuration panel, select the **Flow Condition** attribute to open the flow editor popup.
19. Use the following information to configure the flow condition:
   - **Condition Type:** ```Simple```
   - **Depends On:** ```Form outcome```
   - **Select Form:** ```Display DB Values```
   - **Select Operator:** ```Equal```
   - **Select Outcome:** ```Back```
   - Save the flow condition.
20. Add an **End Event** stemming from the Exclusive Gateway. 
21. Select the **Sequence Flow Line** stemming from the _Exclusive Gateway_ and to the _End Event_.
22. In the bottom configuration panel, select **Name** and name it: ```proceed```.
23. In the bottom configuration panel, select the **Default Flow** checkbox.
24. Save and close the process.
25. Navigate to the **Apps** page and **Deploy** your application.
26. Test the new functionality.
   - In this version, we added form outcome functionality to the Display Values User Task which will allow us to either go back to selecting a user, or proceed to the next step in our process. Test the **Back** functionality, ensuring that you're able to route back to the new User Task and reselect a different user.



### Lab 5. Add the Create Claim Form to the Process.
**Scenario:** Now that we have a function to choose a customer we can continue on to capture details about the customer's claim using a User Task with a form.
1. Enter your New Claim process in edit mode.
2. Remove the **End Event** from your process.
3. Add a **User Task** to the process connected from the Exclusive Gateway's _Proceed_ path.
4. Name it: ```Create Claim```.
5. Select the **Referenced Form** attribute and create a new form.
6. Name it: ```Create Claim```.
7. In the form editor, add a **Header** to the form with the following configuration:
   - **Label:** ```Customer Information:```
   - **Number of Columns:** ```2```
8. Add a **Display Value** field to the Header and select the pencil icon to edit the field.
9. Select the blue **Variable** button and use the drop-down below the button to select the ```cFirstName``` variable.
10. Notice the **Label** changed to reflect the name fo the variable selected. You can now change the label to be more formal: ```First name:```.
11. Add more **Display Value** fields to the Header for each customer value coming from the customer database. Use the following chart to add each display value field.
| **Field Type**       | **Variable**     | **Label**     |
| ---       | ---       | ---       |
| Display Value     | cFirstName        | First Name:        |
| Display Value     | clastName        | Last Name:        |
| Display Value     | cAddress        | Street Address:        |
| Display Value     | cCity        | City:        |
| Display Value     | cState        | State:        |
| Display Value     | cZip        | Zip Code:        |

12. Add a **Header** to the form with the following configuration:
   - **Label:** ```Claim Information:```
   - **Number of Columns:** ```1```
13. Add a **Date Object** to the _Claim Information_ Header and select the pencil icon to edit it.
14. Label it: ```Incident Date:```.
15. Select the **Required** check box.
16. Select the **Advanced** tab.
17. In the **Date display format (D-M-YYYY)** field, add the following criteria: ```MM-DD-YYYY```. 
18. Add a **Drop Down Object** to the _Claim Information_ Header and select the pencil icon to edit it.
19. Label it: ```Incident Type:```.
20. Select the **Required** check box.
21. Select the **Options** tab.
22. Ensure the blue **Manual** button is selected. 
23. In the **+ Add a new option** field add: ```Damage```.
24. Press the **TAB** key on your keyboard (or click outside the text field). The option will be added. Edit the **ID:** field to: ```damage```.
25. Repeat the process to add new options and add the following options:
   - ```Lost```     |   ```lost```
   - ```Stolen```   |   ```stolen```
   - ```Other```    |   ```other```
26. Save and close the drop-down config window.
27. Add a **Drop Down Object** to the _Claim Information_ Header and select the pencil icon to edit it.
28. Label it: ```Damage Severity:```.
29. Select the **Required** check box.
30. Select the **Options** tab.
31. Ensure the blue **Manual** button is selected. 
34. Add the following options:
   - ```Minimal```  |   ```minimal```
   - ```Severe```   |   ```severe```
35. Save and close the drop-down config window.
36. Add an **Amount Object** to the _Claim Information_ Header and select the pencil icon to edit it.
37. Label it: ```Claim Amount:```. 
38. _Select the **Override ID** checkbox_
39. Provide an **ID:** of: ```value```. 
40. _Select the **Requires** checkbox_
41. Close the **Amount Object** config window.
42. Save and close the form.
43. Add an **End Event** to the _Create Claim_ User Task.
44. Save and close the process.
45. Deploy your application and test.



|  **Next Steps: Configure & Verify a Dynamic Deductible** |
| ----------- |
| We're searching for a customer and retrieving values from the customer database. We now need a method that will configure a scaling deductible based on the **amount** of the claim. After that, we'll need a method for the claim agent to verify the amount and continue with the claim. |

### Lab 6. Add a Decision Task to the Process.
A Decision Task uses a **Decision Table** which allows you to introduce conditional logic within your process using variables. We'll use a Decision task to manipulate a scaling deductible based on the amount of a customer's claim.
1. Enter your New Claim process in edit mode.
2. Remove the **End Event**.
3. Add a **Script Task** to your process and conect it from the _Create Claim_ User Task.
4. Name the script task: ```Set Deductible```. 
5. From the bottom configuraiton panel, set the **Script Format** to: ```groovy```. 
6. Select the **Script** attribute and paste the following code:
```
execution.setVariable("cStatus", "IN-PROGRESS");
execution.setVariable("deductible", 200);
```
7. Add a **Decision Task** to your process and conect it from the _Set Deductible_ Script task. Name it: ```Configure Deductible```.
8. From the bottom configuraiton panel, select the **Referenced Decision Table** to open the Decision Table Reference popup window.
9. Select **New Decision Table**.
10. In the Decision Table editor, select the **Blue Column Header** (Input Column). 
11. Configure the Input Column Header with the following configuration:
   - **Column Label:** ```Equipment Value```
   - **Variable Type:** ```Form Field```
   - **Form Field:** ```Claim Amount: value - amount```
   - _Save the Input Column Configuration_
12. Select the **Green Column Header** (Output Column). 
13. Configure the Output Column Header with the following configuration:
   - **Column Label:** ```Deductible```
   - **Column Type:** ```Existing```
   - **Variable Type:** ```Variable```
   - **Variable:** ```deductible - deductible - integer```
   - _Save the Input Column Configuration_
14. Select the pencil icon in the cell under the Input Column Header.
15. Configure the condition with the following configuration:
   - **Operator:** ```Less than```
   - **Variable Type:** ```Number```
   - **Number:** ```100```
   - _Select the **OK** button_
16. Select the pencil icon in the cell under the Output Column Header.
17. Configure the condition with the following configuration:
   - **Variable Type:** ```Variable```
   - **Variable:** ```deductible - deductible - integer```
   - **Calculation:**
      - **Method:** ```Subtract```
      - **Number:** ```200```
   - _Select the **OK** button_
18. Select the blue **Add Rule** button to add another rule.
19. Select the pencil icon in the cell under the Input Column Header for the new blank rule.
20. Configure the condition with the following configuration:
   - **Operator:** ```Less than```
   - **Variable Type:** ```Number```
   - **Number:** ```500```
   - _Select the **OK** button_
21. Select the pencil icon in the cell under the Output Column Header for the new blank rule.
22. Configure the condition with the following configuration:
   - **Variable Type:** ```Variable```
   - **Variable:** ```deductible - deductible - integer```
   - **Calculation:**
      - **Method:** ```Subtract```
      - **Number:** ```150```
   - _Select the **OK** button_
23. Select the blue **Add Rule** button to add another rule.
24. Select the pencil icon in the cell under the Input Column Header for the new blank rule.
25. Configure the condition with the following configuration:
   - **Operator:** ```Less than```
   - **Variable Type:** ```Number```
   - **Number:** ```1000```
   - _Select the **OK** button_
26. Select the pencil icon in the cell under the Output Column Header for the new blank rule.
27. Configure the condition with the following configuration:
   - **Variable Type:** ```Variable```
   - **Variable:** ```deductible - deductible - integer```
   - **Calculation:**
      - **Method:** ```Subtract```
      - **Number:** ```100```
   - _Select the **OK** button_
28. Select the blue **Add Rule** button to add another rule.
29. Select the pencil icon in the cell under the Input Column Header for the new blank rule.
30. Configure the condition with the following configuration:
   - **Operator:** ```Greater than or equals```
   - **Variable Type:** ```Number```
   - **Number:** ```1000```
   - _Select the **OK** button_
31. Select the pencil icon in the cell under the Output Column Header for the new blank rule.
32. Configure the condition with the following configuration:
   - **Variable Type:** ```Variable```
   - **Variable:** ```deductible - deductible - integer```
   - **Calculation:**
      - **Method:** ```Add```
      - **Number:** ```200```
   - _Select the **OK** button_
33. Save and close the Decision Table.
34. Save the process.



### Lab 7. Add a Review User Task.
1. Enter your New Claim process in edit mode (if not already).
2. Add a **User Task** to the process connected from the _Configure Deductible_ Decision Table.
3. Name it: ```Verify Claim```.
4. Select the **Referenced Form** attribute and _create a new form_.
5. Title the form: ```Verify Claim```.
6. In the form editor, add a **Header Object** to the form with the following configuration:
   - **Label:** ```Verify Claim Information```.
   - **Number of Columns:** ```1```.
7. Add a **Display Text** Field to the Header with the following configuration;
   - **Label:** ```Prompt:```.
   - **Text to Display:**
```
Mr. or Ms. ${cLastName}, based on the criteria of your ${incidenttype} claim with the total value of $${value} your deductible will be $${deductible}.

Would you like to continue with your claim?
```
8. Add a **Multiline Text** field to the Header below the _Display Text_ field with the following configuration:
   - **Label:** ```Notes:```
   - _Select the **Override ID** checkbox_
   - **ID:** ```claimNotes```
9. Select the **Form Outcomes** tab above the form layout.
10. Add **two** outcomes to the form:
   - ```Continue```
   - ```Hold```
11. Save and close the form editor.
12. Add an **End Event** to the process connected to the _Verify Claim_ User Task.
13. Save and close the process editor.
14. Publish your application and test the new functionality.



|  **Next Steps: File the Claim and Save Metadata Values.** |
| ----------- |
| Our process now contains a method to configure a scaling deductible based on equipment value and a verification that prompts the customer whether to move forward with the claim. Next, assuming the claim moves forward, we want to file the claim as a document in our repository and save key metadata values. |

### Lab 8. Add Pathing and Status Hold Automation.
In this lab, we'll add pathing coming from the **Verify Claim** User Task with a Scripting element that will set the status of the claim to **HOLD** if the customer decides _not_ to continue with the claim.
1. Enter your New Claim process in edit mode (if not already).
2. Remove the **End Event** from the process.
3. Add an **Exclusive Gateway** to the process connected to the _Verify Claim_ User Task.
4. Add a **Script Task** to the process connected to the _Exclusive Gateway_.
5. Name it: ```Set Status Hold```.
6. Set the **Script Format** to ```groovy```.
7. Select the **Script** attribute and paste in the following code:
```
execution.setVariable("cStatus", "HOLD");
```
8. Select the **Sequence Flow Line** coming from the _Exclusive Gateway_ and to the _Script Task_.
9. Program the **Flow Condition** with the following criteria:
   - **Condition Type:** ```Simple```
   - **Depends On:** ```Form Outcome```
   - **Select Form:** ```Verify Claim```
   - **Select Operator:** ```equals```
   - **Select Outcome:** ```Hold```
10. Save the configuration.
11. Save the process.


### Lab 9. Generate and Save a Document to the Repository.
In this lab, we'll perform two actions; generate a PDF document from a template with captured values from the claims process and save the document to the ACS repository.
1. Add a **Generate Docuement Task** to the process connected to the _Exclusive Gateway_ User Task.
2. Name it: ```Create Claim Document```.
3. Select the **Sequence Flow Line** coming from the _Exclusive Gateway_ and to the _Create Claim Document_ Task.
4. Check the **Default Flow** checkbox.
5. Select the _Create Claim Document_ Task. In the bottom configuration panel, select the **Document Variable** attribute and enter the following: ```claimDoc```.
6. Select the **File Name** attribute and enter the following: ```claim-${cLastName}-${recordID}```.
7. Select the **Template** attribute.
8. In the Template popup window, select **Custom Template**.
9. Use the **Choose File** button to navigate to and select the _docx_ file template titled: ```9si-claim-template.docx```.
10. Add a **Publish to Alfresco** Task to the process connected to the _Create Claim Document_ Generate Document Task.
11. Name it: ```Save to Repo```.
12. In the bottom configuration panel, select the **Alfresco Content** attribute.
13. In the _Alfresco Content_ popup window, use the dropdown field to select the option **Publish content uploaded in field**.
14. As the **Value Type** Select ```Variable```.
15. In the **Variable** dropdown, select ```claimDoc```.
16. Save the configuration.
17. In the bottom configuration panel, select the **Alfresco Destination** attribute.
18. In the _Alfresco Destination_ popup window, use the dropdown field to select the option ```Alfresco-1```. (Might be pre-selected)
19. Next to **Destination**, use the **Select Folder** button to navigate to this path in the ACS repository: ```9 Second Insurance > documentLibrary > Customer Claims```. Select the **Select Folder** button to save this location.
20. Save the configuration.
21. Save the process.



|  **Next Steps: Update Metadata in your Repository** |
| ----------- |
| We have a content file for our customer's claim saved to our repository, where an **Aspect** is applied to any documents saved within our Customer Claims folder. We want the claim data values saved to that aspect, so they can be viewed without having to open the document to find essential data about the customer. |

### Lab 10. Save Metadata values to the Document Aspect.
The process is now generating and saving a PDF document to the Content Repository in a folder that is configured to apply the **Claim** Aspect. In this lab, we'll apply some of the values captured about this claim and save them to that aspect.
1. Add a **Update Alfresco Properties** Task to your process and connect it to the _Publish t Alfresco_ Task.
2. Name it: ```Save Aspect Values```.
3. In the bottom configuration panel, select the **Alfresco Properties** attribute to open the properties mapping popup window.
4. As the **Value Type**, select **Variable**.
5. Using the **File** dropdown, select: ```claimDoc```.
6. Use the **Properties Mapping Table** to map each process variable to the Aspect property. Refer to this chart for the mapping:

| File Property | Property Type  | Variable |
| ----------     | ---------    | -------------- |
| claim:firstname             | string           | cFirstName         |
| claim:lastname      | string    | cLastName         |
| claim:address       | string     | cAddress         |
| claim:city        | string | cCity         |
| claim:state           | string	        | cState         |
| claim:zip          | string        | cZip         |
| claim:type        | string      | incidentType         |
| claim:severity        | string      | damageSeverity         |
| claim:status        | string      | cStatus         |
| claim:date        | string      | incidentDate         |

7. Save the configuration.
8. Add an **End Event** to the process connected to the _Save Aspect Values_ Task.
9. Save and exit the process editor.
10. Publich the process and test the final functionality.


--- 

