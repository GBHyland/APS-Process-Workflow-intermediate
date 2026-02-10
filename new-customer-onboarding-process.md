## New Customer Onboarding Process

|  **Overall Scenario – 9 Second Insurance: New Customer Process** |
| ----------- |
| As a systems architect working for 9 Second Insurance, you are tasked with creating a process that will allow agents to onboard new customers and add them to 9SI's customer database. The process should include: |
| Data intake mechanism to capture a customer's information. |
| Save the new customer's information to 9SI's external customer database. |
| Create a new user for our customer in the APS user database. |
| Document generation and saving to content management system. |
| A Know-Your-Customer check to determine if the customer is Out-of-State in order to route documents accordingly. |


-----------

## Configure the ACS Environment.  
Before creation of the Process, a folder and Content Model (Aspects) structure needs to be created to support Alfresco Actions the process will leverage. The next few labs will focus on creating the folders and Aspects.

### ACS Lab: Create Insurance Content Model
**FACILITATOR ONLY LAB**.   
In this lab, the instructor will create the Data Model that all user Aspects will be added to. Facilitator should demo this step.
1. Access the **Admin Tools**.  
2. Select **Model Manager**.  
3. Select the **Create Model** button to open the Content Model wizard. Configure with the following info:
   - **Namespace:** ```Insurance```
   - **Prefix:** ```ins```
   - **Name:** ```Insurance```
4. This lab is complete.


### ACS Lab: Add Aspects
In this lab, each user will create their own Aspect using the Data Model created in the previous lab.   
1. Access the **Admin Tools**.  
2. Select **Model Manager**. 
3. Select the **Insurance** Data Model. 
4. Select the **Create Aspect** button. 
5. Name the Aspect: ```cus[user#]```. _Example if you are user 10: cus10_
6. Press the **Create** button.
**NOTE:** At the time of this guide, you are not automatically navigated to the Aspect page. If this occurrs, refresh the page and select the Aspect you created.
7. On the Aspect page, Select **Create Property**. 
**NOTE:** In the property wizard you only need to provide a **Name**. You'll input the name, then press the **Create and Start Another** button at the bottom of the wizard. Repeat this process and add the following properties:  

| **Property** |
| ------------ |
| FirstName |
| LastName |
| StreetAddress |
| City |
| State |
| ZipCode |
| Email |
| Phone |
| PolicyNumber |

8. This lab is complete.


### ACS Lab: Create Folder Structure
**FACILITATOR ONLY LAB**.   
In this lab, the facilitator will create the folder structure necessary to store files generated from the process. 
1. Create a site titled: ```9 Second Insurance```.
2. Make the site **Public**.
3. Access the DocumentLibrary.
4. Create a Folder titled: ```Customer Policies```. Navigate into that folder. 
5. In the _Customer Policies_ folder, create **TWO** new folders, titled: ```Ohio Based Customers``` and ```Non-Ohio Based Customers```.  
6. This lab is complete.

-----------

## Configure Your APS Environment. 
**FACILITATOR ONLY LAB**.     
**Scenario:** Before you can begin the labs here you must configure a few elements within your APS environment in order for the functionality to work. These configuration steps must be performed by a user with **administration** privledges. _Configure the following:_
1. **The Share Connector:** _Allows APS to connect and communicate with your ACS environment._
   - In APS, navigate to the Tenants section.
   - Select **Alfresco Repositories**.
   - Select the **Plus Sign** below the repository table.
   - Enter the following configuration:
     1. **Name:** _The formal name of the ACS repository:_ ```alfresco-1```
     2. **Alfresco tenant:** _Tenant name to use in Alfresco:_ ```alfresco-1```
     3. **Repository Base URL:** _Base URL for Alfresco repository install:_ ```http://content:8080/alfresco```
     4. **Share base url:** _Base URL for Alfresco Share install:_ ```http://content:8080/share```
     5. **Sites:** _Location to the Sites directory within ACS:_ ```Sites```
     6. **Alfresco Version:** _Select 6.1.1 (or higher)_
     7. _Select default authentication_
     8. Save.
   - Select the **Personal** tab at the top navigation.
   - Select the option under **Alfresco Repositories**.
   - Sign in with your user account credentials. It should now show _alfresco-1_ as your configured ACS repo.
   - Your share connector is configured for APS to work with your ACS service. Good job!
2. **Data Source**: _Allows APS to connect to an external relational database._
   - From the Tenants page, select **Data Sources**.
   - Select the **Plus Sign** below the data sources table.
   - Enter the following configuration:
     1. **Name:** _The formal name of the Database:_ ```nine-si-db```
     2. **Jdbc url::** _The web url for the database location:_ ```jdbc:oracle:thin:@//aps-custom-oracle-db.cp58lgpzkwpy.us-east-1.rds.amazonaws.com:1521/ORCL```
     3. **Driver class:** _The class name within the JDBC Driver JAr package installed in your APS environment:_ ```oracle.jdbc.driver.OracleDriver```
     4. **Username:** _The username used to authenticate into your database:_ ```your-username```
     5. **Password:** _The password used to authenticate into your database:_ ```your-password```
     6. Save.
   - Your Data Source is configured for APS to connect to your external relational database. Great work!

------------
## Process Services Labs:   
The remainder of the labs should be performed by the class attendees in the Activiti-App (APS).  

### Lab 1. Create and Configure a New Process.
This lab will walk you through creating a new process and configuring the variables needed to perform actions to support future labs. 
1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
    - Sign in with your provided username and password. You will be directed to the Activiti App home page.
2.	Select the App Designer tile to navigate to the Business Process Models page
3.	To create a new process, press the Create Process button in the top right of the page.
4.	In the Create a new business process model popup, enter the following information:
    -	Model name: ```[Your user #] New Customer Onboarding``` (ex: U1 New Customer Onboarding)
    -	Description: ```Onboard a new customer.```
    -	Editor Type: Leave as BPMN editor
    -	Stencil: Default BPMN
    -	Press the Create new model button.
5.	With nothing selected on the stage, select the Variables attribute in the bottom configuration panel.
6.	Select the “+” icon below the chart to add new variables. Add the following variables:  

| Variable Name     | Variable Type     |
| ---               | ---               |
| firstName     | String            |
| lastName     | String            |
| customerId     | String            |
| policyNumber     | String            |
| address          | String            |
| city     | String            |
| state     | String            |
| zipCode     | String            |
| phoneNumber     | String            |
| email     | String            |


### Lab 2. Create an Intake Task. 
**Scenario:** In order to capture new customer data we need a data intake mechanism. In this lab you'll create a process with a Human Task with a Form attached that will allow you to capture customer data.
1.	Create a new **User (Human)** Task connected to the Start Event.
2.	Give the user task a name by double-clicking on the task to open a text field Name this task: ```Gather Customer Data```.
3.	With the user task selected, notice that the Referenced form value in the bottom configuration window is No reference selecte- To create an intake form for this task, click on the No reference selected value
4.	In the Form reference popup window, select the New Form button.
5.	In the Create a new form window, enter the following values:
    - Form name: ```Gather Customer Data```
    - Description: ```Gathers information about the new customer.```
    - Stencil: Default form
    - Select the Create form button. 
6.	Follow these steps to create the form you’ll need to intake a new customer:
    1. Drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it ```Customer Information:``` and click the Close button.
    2. Drag a Text object onto the canvas and drop it into the header object. Click the pencil to edit the text object and configure the following information: 
    3. Label: ```First Name: ```
        - Select the Override ID check box.
        - Enter ```firstName``` into the ID fiele
        - Select the Required check box. 
        - Click on the Close button.
    4. Perform the previous step again to create a Text object with the following information: 
        - Label: ```Last Name:```
        - ID: ```lastName ```
        - Required: checked
    5. Create another Text object with the following information:
        - Label: ```Address:```
        - ID: ```address ```
        - Required: checked
    6. Create another Text object with the following information:
        - Label: ```City:```
        - ID: ```city```
        - Required: checked
    7. Create a Dropdown object with the following information:
        - Label: ```State:```
        - ID: ```state```
        - Required: checked
        - Select the Options tab and Configure the following information:
            1.	Select the Rest Service button.
            2.	Rest URL: ```http://cic-webapps.alfdemo.com/rest-states/```
            3.	Path to array in JSON response: ```states```
            4.	ID property: ```abbreviation```
            5.	Label property: ```name```
            6.	Press the Test button. A popup window should appear showing a JSON string of states. Close the window.
            7.	Close the dropdown prompt.
    8. Create another Text object with the following information:
        - Label: ```Zip Code:```
        - ID: ```zipCode```
        - Required: checked
    9. Create another Text object with the following information:
        - Label: ```Email Address:```
        - ID: ```email```
        - Required: checked
    10. Create another Text object with the following information:
        - Label: ```Phone Number:```
        - ID: ```phoneNumber```
        - Required: checked
    11. Drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it ```Policy Information:``` and click the Close button.
    12. Create a Display Value object and add it to the _Policy Information_ Header. Configure with the following steps:
        - Select the **Variable** button.
        - Use the drop-down menu below the Variable button to select the variable ```customerID```.
        - Change the label to: ```Customer ID:```
        - Select Close.
    13. Create a Display Value object and add it to the _Policy Information_ Header. Configure with the following steps:
        - Select the **Variable** button.
        - Use the drop-down menu below the Variable button to select the variable ```policyNumber```.
        - Change the label to: ```Policy Number:```
        - Select Close.
    14. Click on the save button in the top left of the page to save and close the form and return to your process model.
    15. On the Save form popup window, click the Save and close editor button to return to your process model.
7.	Create a connected end event by selecting the Gather Customer Data user task and clicking on the end event icon in the small popup menu.   
8.	Save the process model by clicking on the Save icon in the top left of the pag-   
9.	In the Save model popup window, press the Save and close editor button.

---

|  **Next Steps: Process Application** |
| ----------- |
| In APS, a process cannot deploy by itself; it needs to be included in a Process Application. The Process Application is the engine that deploys and drives processes.  |
| A process application can deploy mutliple processes. |
| A Process App is a container for handling a group of published processes and deploying them to a Process Engine. |
| [Creating a Process Application](https://support.hyland.com/r/Alfresco/Alfresco-Process-Services/25.3/Alfresco-Process-Services/Using/Getting-started-with-Process-Services/Step-2-Create-and-publish-the-process-application) |

### Lab 3: Create a Process Application
1.	From the Activiti home page, select the App Designer tile to navigate to the Business Process Models page
2.	Select the Apps hyperlink in the top blue banner.
3.	Select the Create App button. 
4.	Give your application the App definition name: ```[Your User #] 9SecondInsurance``` (ex: U1 9SecondInsurance). 
    -	Optional: enter a description of your application describing it will do, ie: ```Deploys our claims processes.```
5.	Click the Create new ap definition button.
6.	OPTIONAL: On the App definition details page, you can change the color and icon of your application tile Use the Icon and Theme drop downs to customize your tile
7.	To add the process model you created, click on the Edit included models button below the tile 
8.	In the Models included in app definition popup window, select the process model you want to includ- NOTE: The selections toggle on and off when you click them, so be sure to click the one you want once and notice the blue + icon will appear to ensure it is selected 
9.	Click the Close button to close the popup window.
10.	Save the App definition by clicking on the save icon located in the top left of the page.
11.	In the Save app definition popup window, ensure you select the Publish? Checkbox, then click on the Save and close editor button.
12.	Navigate back to the home page by clicking the home button in the top, blue banner:  
13.	If your application already appears on the home page as a tile you are done.
    -	If not, deploy your new application by selecting the blank tile, depicted with a plus sign “+” (“Add a new app”) appears when you hover your mouse over it. Select this tile, then select your application in the Add app to landing page popup window. Press the Deploy button on that window. Your application is now deployee

---

|  **Next Steps: Add the Customer as New User to APS** |
| ----------- |
| Now that we're saving our new user to the customer 9 Second Insurance database, we also need to add the customer as a new user to APS so we can easily access their data within the system. |
| To do this, we'll leverage a REST-Call Task and a Script Task. |

### Lab 4. Add a User to APS from Within onboarding Process
1. Enter your New Customer Onboarding process in edit mode.
2. Add a **Script Task** to your process (under Activities).
   - Name the task ```Add User to APS```.
3. Connect this task in the workflow **AFTER** the _Gather Customer Data_ user task.
4. In the bottom configuration panel configure the following:
   - **Script format:** ```groovy```
   - **Script:**
     ```
        import com.activiti.service.idm.UserServiceImpl;
        import com.activiti.security.SecurityUtils;
        import com.activiti.domain.idm.UserStatus;
        import com.activiti.domain.idm.AccountType;
        import com.activiti.domain.idm.User;
        import com.activiti.repository.idm.UserRepository;
        
        
        User currentUser = SecurityUtils.getCurrentUserObject();
        
        
        String firstName = execution.getVariable("firstName");
        String lastName = execution.getVariable("lastName");
        String email = execution.getVariable("email");
        String password = "demo";
        String company = "na";
        String externalId = firstName + "" + lastName;
        externalId = externalId.toLowerCase();
        
        UserStatus initialStatus = UserStatus.ACTIVE;
        AccountType accountType = AccountType.ENTERPRISE;
        Long tenantId = currentUser.getTenantId();
        
        // createNewUser(java.lang.String, java.lang.String, java.lang.String, java.lang.String, java.lang.String, com.activiti.domain.idm.UserStatus, com.activiti.domain.idm.AccountType, java.lang.Long)
        
        print("Create new user with values: "+firstName+" / "+lastName+" / "+tenantId);
        
        User newUser = userService.createNewUser(
                email,
                firstName,
                lastName,
                password,
                company,
                initialStatus,
                accountType,
                tenantId
                //externalId
            );
        
        userRepository.save(newUser);
        
        Long nuid = newUser.getId();
        
        println("newUser.getId() >>> "+nuid);
        execution.setVariable('customerId', nuid);
        execution.setVariable('policyNumber', "9SI-"+nuid);
        
     ```
   - Save the script.
5. Save the process.


---

|  **Next Steps: Saving Information to 9 Second Insurance's Customer Database** |
| ----------- |
| **Instructor Presentation:** Data Models & Relational Databases** |
| We are capturing customer information, creating and saving a customer document, and now we need to save our customer's data to the 9SI customer database. |
| First, we'll need to create a Data Model that will allow us to specifiy the correct format of data that exists in the Database Table we want to map our process data to. |
| We'll then need to create a Service Task that implements the Data Model and allows us to map our process variables to the data model structure. |
| [BPMN Tasks](https://support.hyland.com/r/Alfresco/Alfresco-Process-Services/25.3/Alfresco-Process-Services/Using/Getting-started-with-Process-Services/Step-2-Create-and-publish-the-process-application) |



### Lab 5: Create a Data Model
For classes, please regard the instructor for a brief presentation on Data Sources, Data Models, and integrating a relational database into your process.
1. Access the data Model page within the App Designer.
2. Select the Create Data Model button.
3. Enter ```U[user #]9siCustomerData``` in the Data Model Name field and click the Create button.
4. In the Database Tab:
    -    Select ```aps-oracle-db``` from the Data Source dropdown.
    -    **DO NOT Click the Import button.**
    -    Click the Add Entity button. Enter the following information for the Entity:
            -	Entity Name: ```newCustomers```
            -	Entity Description: ```Details of all customers```
            -	Table Name: ```NINESI```
    -	**DO NOT Click the Import Attributes button.**
    -	Use the **Add Attribute** button to create new attributes. Use the chart below to add attributes and fill in the _attribute name_, _column name_, and select the correct _attribute type_. **Note:** The id variable should have the Primary key check box checked All others do not.

| Attribute name | Column name  | Attribute type |
| ----------     | ---------    | -------------- |
| id             | ID           | Number         |
| firstname      | FIRSTNAME    | String         |
| lastname       | LASTNAME     | String         |
| address        | STREETADDRESS | String         |
| city           | CITY	        | String         |
| state          | STATE        | String         |
| zip        | ZIPCODE      | String         |
| email        | EMAIL      | String         |
| phone        | PHONE      | String         |
| policynum        | POLICY_NUMBER      | String         |

5. Save and Close the Model.


### Lab 6: Create a Store Entity Task (Save Values to Database)
1. Enter your Customer Onboarding process in edit mode.  
2. Delete the end event.
3. Add a Store Entity Task to the process and connect it to the end of the process.
4. Select the new Store Entity Task and set the following configuration in the bottom panel:
    -	Name: ```Save Cust data to DB```
    -	Select Attribute Mapping to open the mapping popup window. Perform the following actions:
        -	Mapped Data model: _Select the Data Model you created that contains your user number_. Ex: ```U[user #]9siCustomerData```
        -	Mapped entity: ```newCustomers```
        -	New Variable: ```newCustomerdata```
        -	Configure the following attributes in the mapping table by selecting each Attribute Name and choosing the variable / form field it is associated with:  

| Attribute Name        | Mapped Value Type     | Variable.     |
| ---                   | ---                   | ---           |
| id    | Variable  | customerId    |
| firstname    | Variable  | firstName    |
| lastname    | Variable  | lastName    |
| address    | Variable  | address    |
| city    | Variable  | city    |
| state    | Variable  | state    |
| zip    | Variable  | zipCode    |
| email    | Variable  | email    |
| phone    | Variable  | phoneNumber    |
| policynum    | Variable  | policyNumber    |

   - Press Save on the mapping window.
5. Save the process (Do not save a close - without an end event you may get a validation error, save anyway).

---

|  **Next Steps: Verify Customer Data was Saved** |
| ----------- |
| We're now saving our customer data in two places, in the 9SI customer relational database and as a new user in APS. Before we move on, we need to add a review mechanism that retrieves and displays those records so we can ensure that the data was saved correctly. This is standard functionality in database operations. |

### Lab 7. Add Data Retrieval and Verification Method
1. With your Onboarding Process in edit mode, add a new variable to your process by selecting the **Variables** parameter in the bottom configuration panel. Add the following variable:
   - **Variable name:** ```recordList```
   - **Variable type:** ```string```
2. Add a **Intermediate Timer Catching Event** to your process (found under the Intermediate Catching Events header) connected from the _Add User to APS_ script task.
   - Name it: ```Delay of 5 Seconds```
   - In the configuration panel, set the **Time Duration** parameter to: ```PT5S```.
3. Add a **REST-Call Task** to your process (under Activities).
   - Name the task ```Get All Users```.
4. Connect this task in the workflow **AFTER** the _Delay of 5 Seconds_ timer event.
5. With the Task selected, select the **Endpoint** paramter in the bottom configuration panel to open the _Change value for endpoint_ popup window. Use the following configuration:
   - **HTTP Method:** GET
   - **Base enpoint:** _choose aps from the drop-down_ and save the configuration.
   - **To add URL parameters...:** ```/api/enterprise/users```
   - Save the configuration.
6. Select the **Response Mapping** paramter to open the _Change value for Response mapping_ pop-up window and add a new variable with the following config:
   - **Property name:** ```data```
   - **Variable type:** ```string```
   - **Variable name:** ```userData```
   - Save the configuration.
7. Add a **Script Task** connected to the _Get All Users_ Task. Configure the following parameters in the bottom configuration panel:
   - **NAme:** ```Get Database User```
   - **Script format:** ```groovy```
   - **Script:**
     ```
         import groovy.sql.Sql;
         import groovy.json.*
         import groovy.json.JsonBuilder
         
         class Record {
             String recId
             String firstname
             String lastname
             String address
             String city
             String state
             String zip
             String email
             String phone
             String policy
         }
         
         def url = 'jdbc:oracle:thin:@//aps-custom-oracle-db.cp58lgpzkwpy.us-east-1.rds.amazonaws.com/ORCL'
         def user = 'admin'
         def password = 'apsadmin'
         def driver = 'oracle.jdbc.driver.OracleDriver'
         def sql = Sql.newInstance(url, user, password, driver)
         
         rowNum = 0;
         def recordList = [];
         
         out.println('Query Customer: '+execution.getVariable("customerId"));
         
         sql.eachRow("SELECT * FROM NINESI WHERE ID = ${customerId}") { row ->
             
             def r = new Record( recId:row.id, firstname:row.firstname, lastname:row.lastname, address:row.streetaddress, city:row.city, state:row.state, zip:row.zipcode, email:row.email, phone:row.phone, policy:row.policy_number)
             recordList.add(r);
             
         }
         
         println new JsonBuilder( recordList ).toPrettyString()
         out.println('Customer Details: '+recordList);
         execution.setVariable("recordList", new JsonBuilder( recordList ).toPrettyString())
 
     ```
8. Create a new **User Task** connected to the _Get Database User_ script task.
   - Name the user task: ```Verify User Data```.
9. In the bottom config panel, select the **Form reference** parameter and choose Create Form on the form reference popup window. Name it: ```Verify User data```.
10. Follow these steps in the Form Editor to create a new form:
    - Drag a Dynamic Table onto the form stage. Select the pencil icon to go into edit mode.
    - Enter into Label field: ```Verify Database Data:```
    - Select the Override Id checkbox.
    - Enter into the ID field: ```recordList```
    - Select the Table Columns tab.
    - Press the "+" icon button to create new property mappings with the following values:
        - Column 1:
            1.	Property ID: ```recId```
            2.	Property Name: ```ID```
            3.	Property Type: ```string```
        - Column 2:
            1.	Property ID: ```firstname```
            2.	Property Name: ```First name```
            3.	Property Type: ```string```
        - Column 3:
            1.	Property ID: ```lastname```
            2.	Property Name: ```Last Name```
            3.	Property Type: ```string```
        - Column 4:
            1.	Property ID: ```address```
            2.	Property Name: ```Address```
            3.	Property Type: ```string```
        - Column 5:
            1.	Property ID: ```state```
            2.	Property Name: ```State```
            3.	Property Type: ```string```
        - Column 6:
            1.	Property ID: ```city```
            2.	Property Name: ```City```
            3.	Property Type: ```string```
        - Column 7:
            1.	Property ID: ```zip```
            2.	Property Name: ```Zip```
            3.	Property Type: ```string```
    - Close the edit prompt.
    - Add another **Dynamic Table** to the form.
    - Enter into Label field: ```Verify APS User Data:```
    - Select the Override Id checkbox.
    - Enter into the ID field: ```userData```
    - Select the Table Columns tab.
    - Press the "+" icon button to create new property mappings with the following values:
        - Column 1:
            1.	Property ID: ```id```
            2.	Property Name: ```ID```
            3.	Property Type: ```string```
        - Column 2:
            1.	Property ID: ```firstName```
            2.	Property Name: ```First Name```
            3.	Property Type: ```string```
        - Column 3:
            1.	Property ID: ```lastName```
            2.	Property Name: ```Last Name```
            3.	Property Type: ```string```
        - Column 4:
            1.	Property ID: ```company```
            2.	Property Name: ```Company```
            3.	Property Type: ```string```
        - Column 5:
            1.	Property ID: ```email```
            2.	Property Name: ```Email```
            3.	Property Type: ```string```
    - Close the edit prompt.
    - Save and close the form.
11. Add an end event to the _Verify User Data_ User task.
12. Save and close the process.
13. Deploy and test the process in ADW.

---

|  **Next Steps: Know-Your-Customer Logic for Out-of-State Documentation** |
| ----------- |
| 9 Second Insurance is a Ohio-based insurance company, and our leaders need to be able determine if a customer resides in-state or out-of-state. In order to meet state insurance regulations. We're already capturing a customer's state which allows us to implement the conditional logic we need to achieve this functionality. There are a few ways we could implement this, one of them a Script task, which we've already seen an example of. However, knowing that this KYC logic is likely to evolve in the near future to include additional conditions, we'll apply some forward thinking and use a Java Delegate in order to determine in or out of state. |
| [Java Delgates](https://support.hyland.com/r/Alfresco/Alfresco-Process-Services/24.3/Alfresco-Process-Services/Develop/Develop-extensions-for-Process-Services/Custom-Logic/Java-Delegates) |

### Lab 8: Create a Service Task with Java Delegate
1.	Enter your New Customer Onboarding process in edit mode.
2.	Select the Variables attribute in the configuration panel to add a new variable.
3.	Add a variable titled ```stateVerify``` as a ```string```
4.	From the left panel, under Activities, drag a Service Task into the stage. Connect it from the _Verify user Data_ user task.
5.	Select the Name attribute and give it the following name: ```KYC Delegate```
6.	Select the Class attribute and paste in the following text: ```com.activiti.extension.bean.KYCJavaDelegate```
7.  Add an **End Event** to the _KYC Delegate_ service task.
8.	Save your process. 


|  **Next Steps: Generating a Document** |
| ----------- |
| We've gathered and saved our customer data and created a user account within APS. 9 Second Insurance also wants a file generated with this data that can be stored in the customer repository. This next lab will walk you through how to generate a PDF file with those customer values. |
| **Note:** Generating and Saving a Document are TWO seperate steps in APS. First, we'll generate the document in the next lab and later we'll decide _where_ to save it. |

### Lab 9: Generate a Document. 
1.	Open your New Customer process in edit mode.
2. Add a **Generate document task** to the process and connect it to the _KYC Delegate_ task.
3. Name it: ```Generate PDF```.
4. In the configuration panel, select the **Document Variable** attribute and enter: ```custDoc```.
5. Select the **File Name** attribute and enter: ```customer-${lastName}-${policyNumber}```.
6. Select the **Template** attribute to open the template popup window:
   - Select the **Custom Template** tab and use the _Choose File_ button to navigate to the ```9si_newCustomer.docx``` file provided to you.
7. Ensure the **Output Format** attribute is set to ```PDF```.


|  **Next Steps: In-State and Out-of-State Impact on our Process** |
| ----------- |
| We're now using a Service Task to call a method that exists in our Java delegate to set a process variable to be true or false in relation to the customer being in or out of state. In the event our customer is Ohio-based, we need not do anything extra, however for all others 9SI would like a task to engage the team's manager and prompt them to reach out to the customer and send a Out-of-State waiver. |
| We'll add an exclusive gateway that will allow us to go down either path specified above. |
| Our Out-of-State path will include a User Task assigned to our Team's manager. |

### Lab 9: Add an Exclusive Gateway and Splitting Paths
1. Open your New Customer process in edit mode.
2. Delete the end event.
3. From the left panel, under Gateways, add an **Exclusive Gateway** to your process. Connect it from the _Generate Document_ task.
4. From the left panel, under Alfresco, add a **Publish to Alfresco** task to your process and connect it to the exclusive gateway.
5. Name it: ```Save to OH```.
6. Add another **Publish to Alfresco** task to your process and connect it to the exclusive gateway.
7. Name it: ```Save to Non-OH```.
8. Select the sequence flow line from the Gateway event to the _Save to OH_ task. In the bottom configuration panel, check the **Default Flow** checkbox. 
9. Select the sequence flow line from the Gateway event to the _Save to Non-OH_ task and use the **Flow condition** parameter to configure the following flow:
    - Condition Type: ```Simple```
    - Depends on: ```Variable```
    - Variable Drop-down: ```stateVerify```
    - Operator: ```equal```
    - Value: ```false```
10. Select the _Save to OH_ Publish to Alfresco task and give it the following configuration:
   - **Alfresco Destination** 
     - **Account:** ```Alfresco-1```
     - **Destination:** ```9 Second Insurance > DocumentLibrary > Customer Policies > Ohio Based Policies```
     - **Publish As:** _Process Initiator_
   - **Alfresco Content**
     - **Drop-Down:** _Publish content uploaded in field_
     - **Value Type:** _Variable_
     - **Variable:** _custDoc_
     - **Aspects:** _Select the plus sign button under the Aspect chart and add: ```ins:claim```_
   - Save the Config.
11. Select the _Save to Non-OH_ Publish to Alfresco task and give it the following configuration:
   - **Alfresco Destination** 
     - **Account:** ```Alfresco-1```
     - **Destination:** ```9 Second Insurance > DocumentLibrary > Customer Policies > Non-Ohio Based Policies```
     - **Publish As:** _Process Initiator_
   - **Alfresco Content**
     - **Drop-Down:** _Publish content uploaded in field_
     - **Value Type:** _Variable_
     - **Variable:** _custDoc_
     - **Aspects:** _Select the plus sign button under the Aspect chart and add: ```ins:claim```_
   - Save the Config.
12.	Save your process.

---



|  **Next Steps: Update Metadata in your Repository** |
| ----------- |
| We have a content file for our customers saved to our repository, where an **Aspect** is applied to any documents saved within our Customer Policies folder. We want the customer data values saved to that aspect, so they can be viewed without having to open the document to find essential data about the customer. |

### Lab 10: Update Metadata (Aspect) on a Saved Document
1. Enter your New Customer Onboarding process in edit mode.
2. From the left task menu under Activities, add an **Update Alfresco Properties** task to the process and attach a sequence flow line from **both** the _Save to OH_ and _Save to Non-OH_ tasks.
3. Name this task: ```Update Metadata```.
4. In the bottom configuration panel, select the **Alfresco Properties** attribute to open the property mapping wizard.
5. For **Value Type** select _Variable_.
6. Use the **File** drop-down to select the ```custDoc``` file we generated from the Generate Document task.
7. Use this table to map the values from the process to the Aspect values:

| File Property | Property Type  | Variable |
| ----------     | ---------    | -------------- |
| ins:[user#]:FirstName             | string           | firstName         |
| ins:[user#]:LastName      | string    | lastName         |
| ins:[user#]:StreetAddress       | string     | address         |
| ins:[user#]:City        | string | city         |
| ins:[user#]:State           | string	        | state         |
| ins:[user#]:ZipCode          | string        | zipCode         |
| ins:[user#]:Email        | string      | email         |
| ins:[user#]:Phone        | string      | phoneNumber         |
| ins:[user#]:PolicyNumber        | string      | policyNumber         |

8.	Save and close the property mapping wizard.
9. Add an end event to the process from the _Update Metadata_ task.
10. Save your process and close the editor.
11.	Navigate to your application and republish.
12.	Test your completed process in ADW.

---

