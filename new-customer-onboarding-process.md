## New Customer Onboarding Process

|  **Overall Scenario – 9 Second Insurance: New Customer Process** |
| ----------- |
| As a systems architect working for 9 Second Insurance, you are tasked with creating a process that will allow agents to onboard new customers and add them to 9SI's customer database: |
| Data intake mechanism to capture a customer's information. |
| Save the new customer's information to 9SI's external customer database. |
| Document generation and saving to content management system. |
| A Know-Your-Customer check to determine if an Out-of-State Waiver is applicable or not. |
| A custom email method that sends documents to the customer. |

### Lab 1. Create an Intake Task
1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
    - Sign in with your provided username and password. You will be directed to the Activiti App home page.
2.	Select the App Designer tile to navigate to the Business Process Models page
3.	To create a new process, press the Create Process button in the top right of the page.
4.	In the Create a new business process model popup, enter the following information:
    -	Model name: ```[Your user #] New Customer Onboarding``` (ex: U1 New Customer Onboarding)
    -	Description: ```Add a new customer.```
    -	Editor Type: Leave as BPMN editor
    -	Stencil: Default BPMN
    -	Press the Create new model button.
5.	With nothing selected on the stage, select the Variables attribute in the bottom configuration panel.
6.	Select the “+” icon below the chart to add a new variabl- Give it the following information:
    -	Name: ```newCustomerId```
    -	Variable Type: string
    -	Save the variable
7.	Create a new script task connected to the start event. In the bottom configuration panel, configure  the following attributes:
    -	Name: ```Set Customer ID```
    -	Script format: ```groovy```
    -	Script: enter this code in the popup script window:
```
execution.setVariable('newCustomerId', execution.getProcessInstanceId());
```
8.	Create a new User Task connected to the script task.   
9.	Give the user task a name by double-clicking on the task to open a text field Name this task: ```Gather Customer Data```.
10.	With the user task selected, notice that the Referenced form value in the bottom configuration window is No reference selecte- To create an intake form for this task, click on the No reference selected value
11.	In the Form reference popup window, select the New Form button.
12.	In the Create a new form window, enter the following values:
    - Form name: ```Add New Customer Data```
    - Description: ```Gathers information about the new customer.```
    - Stencil: Default form
    - Select the Create form button. 
13.	Follow these steps to create the form you’ll need to intake a new customer:
    - From the left object menu, drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer ID and click the Close button.
    - Drag a Display Value object and dop it into the Header object.
    - Click on the pencil icon on the Display Value object to open the edit prompt.
    - Select the blue Variable button. In the Dropdown below the button, select the **newCustomerId** variable The Label should change to match the variable nam- Close the prompt.
    - Drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer Information and click the Close button.
    - Drag a Text object onto the canvas and drop it into the header object. Click the pencil to edit the text object and configure the following information: 
    - Label: ```First Name: ```
        - Select the Override ID check box.
        - Enter ```newCustomerFirstName``` into the ID fiele
        - Select the Required check box. 
        - Click on the Close button.
    - Perform the previous step again to create a Text object with the following information: 
        - Label: ```Last Name:```
        - ID: ```newCustomerLastName ```
        - Required: checked
    - Create another Text object with the following information:
        - Label: ```Address:```
        - ID: ```newCustomerAddress ```
        - Required: checked
    - Create another Text object with the following information:
        - Label: ```City:```
        - ID: ```newCustomerCity```
        - Required: checked
    - Create a Dropdown object with the following information:
        - Label: ```State:```
        - ID: ```newCustomerState```
        - Required: checked
        - Select the Options ta- Configure the following information in the options tab:
            1.	Select the Rest Service button.
            2.	Rest URL: ```https://run.mocky.io/v3/0329674b-51fe-450f-90cf-be8f53cb68a6```
            3.	Path to array in JSON response: ```states```
            4.	ID property: ```abbreviation```
            5.	Label property: ```name```
            6.	Press the Test button. A popup window should appear showing a JSON string of states. Close the window.
            7.	Close the dropdown prompt.
    - Create another Text object with the following information:
        - Label: ```Zip Code:```
        - ID: ```newCustomerZipCode```
        - Required: checked
    -	Create another Text object with the following information:
        - Label: ```Email Address:```
        - ID: ```newCustomerEmail```
        - Required: checked
    -	Create another Text object with the following information:
        - Label: ```Phone Number:```
        - ID: ```newCustomerPhoneNumber```
        - Required: checked
    -	To save the form and return to your process model, click on the save button in the top left of the pag-   
    -	On the Save form popup window, click the Save and close editor button to return to your process model.
14.	Create a connected end event by selecting the Gather Customer Data user task and clicking on the end event icon in the small popup menu.   
15.	Save the process model by clicking on the Save icon in the top left of the pag-   
16.	In the Save model popup window, press the Save and close editor button.


|  **Next Steps: Process Application** |
| ----------- |
| In APS, a process cannot deploy by itself; it needs to be included in a Process Application. The Process Application is the engine that deploys and drives processes.  |
| A process application can deploy mutliple processes. |
| A Process App is a container for handling a group of published processes and deploying them to a Process Engine. |
| [Creating a Process Application](https://docs.alfresco.com/process-services/latest/using/process/app-designer/#create-your-first-app) |

### Lab 2: Create a Process Application
1.	From the Activiti home page, select the App Designer tile to navigate to the Business Process Models page
2.	Select the Apps hyperlink in the top blue banner.
3.	Select the Create App button. 
4.	Give your application the App definition name: [Your User #] 9SecondInsurance (ex: U1 9SecondInsurance). 
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
    -	If not, deploy your new application by selecting the blank tile, depicted with a plus sign “+” (“Add a new app” appears when you hover your mouse over it. Select this tile, then select your application in the Add app to landing page popup window. Press the Deploy button on that window. Your application is now deployee

|  **Next Steps: Creating and Saving a Customer Document** |
| ----------- |
| Since we are now capturing customer information we need to be able to create a real document for our customer and save it to our content management system (ACS). |
| In APS, generating and saving a document are two seperate steps. |
| [Generating a Document](https://docs.alfresco.com/process-automation/latest/model/connectors/generate/) |

### Lab 3: Generating and Saving a Document
1.	Access the App Designer tile from the homepage of the Activiti App (Process Services).
2.	Enter your New Customer Onboarding process in edit mode by selecting the edit icon when hovering your mouse over its til-    
3.	Remove the End event and the line connecting to it by selecting each one and clicking the trash can icon that appears. 
4.	Add a Generate Document task to the process:
    -	From the left task menu, select Activities drop down to reveal the activities tasks. Click and drag the Generate document task dropping it onto the stage.
    -	Drag it into place and connect it to the Approve Sequence flow line
    -	To configure the Generate Document task select it to show its attribute values in the bottom configuration panel.
    -	Name your Generate Document task: ```Create NC Doc```
    -	Select the Document Variable attribute and enter the value ```newCustDoc``` into the field.
    -	Select the Template attribute to open the Change value for “Template” popup window.
        -	Select the Custom Template tab.
        -	Select the Choose File button to open a file browsing window. From the file package you received, navigate to the following file path and choose the file named: ```Aps_documents/form_doc_templates/9si_newCustomer.docx```
        -	The name of the template file should now appear as the value of the Template attribute.
    -	Select the File name attribute to open the Change value for “File name” popup box.
        -	Enter the following file name: ```customer_${newCustomerId}```
        -	Press the save button to close the popup window.
5.	Add a Publish to Alfresco task to the process:
    -	From the left task menu, select Alfresco drop down to reveal Alfresco related tasks. Click and drag the Publish to Alfresco task dropping it onto the stage. Drag it into place on the right side of the Create Claim Doc task.
    -	Connect a flow line from the Create Claim Doc task to the new Publish to Alfresco task. 
6.	Name your _Publish to Alfresco task_ by double clicking on task and opening the name field.
    -	Name the task ```Save Doc to CMS```.
7.	To configure the Publish to Alfresco task select it to show its attribute values in the bottom configuration panel.
8.	Select the Alfresco Content attribute to open the Change Value popup window. Choose Publish all content uploaded in process from the dropdown menu and press the Save button.
9.	Select the Alfresco Destination attribute to open the Change Value popup window.
    -	Next to Destination, click the _Select folder_ button to open the browse Alfresco popup window. The site you created previously should populate here. Choose your site, then navigate to and select the following folder path: _9SecondInsurance/ documentLibrary/Customer Information_
-	Press the Save button on the Change Value popup window.
10.	Finally, add an end event to your process. Select the Publish to Alfresco task, then select the end event icon from the small popup menu.
11.	Save your process and close the editor.
12.	Navigate to your application and republish.
13.	Test your updated process.

|  **Next Steps: Saving Information to 9 Second Insurance's Customer Database** |
| ----------- |
| We are capturing customer information, creating and saving a customer document, and now we need to save our customer's data to the 9SI customer database. |
| First, we'll need to create a Data Model that will allow us to specifiy the correct format of data that exists in the Database Table we want to map our process data to. |
| We'll then need to create a Service Task that implements the Data Model and allows us to map our process variables to the data model structure. |
| [BPMN Tasks](https://docs.alfresco.com/process-automation/latest/model/processes/bpmn/#tasks) |

### Lab 4: Create a Data Model
1. Access the data Model page within the App Designer.
2. Select the Create Data Model button.
3. Enter ```U[user #]9siCustomerData``` in the Data Model Name field and click the Create button.
4. In the Database Tab:
    -    Select ```aps-oracle-db``` from the Data Source dropdown.
    -    **DO NOT Click the Import button.**
    -    Click the Add Entity button. Enter the following information for the Entity:
            -	Entity Name: ```newCustomers```
            -	Entity Description: ```Details of all customers```
            -	Table Name: ```CUSTOMERS```
    -	**DO NOT Click the Import Attributes button.**
    -	Use the **Add Attribute** button to create new attributes. Use the chart below to add attributes and fill in the _attribute name_, _column name_, and select the correct _attribute type_. **Note:** the id variable should have the Primary key check box checked All others do not.

| Attribute name | Column name  | Attribute type |
| ----------     | ---------    | -------------- |
| id             | ID           | Number         |
| firstname      | FIRSTNAME    | String         |
| lastname       | LASTNAME     | String         |
| address        | ADDRESSLINE1 | String         |
| city           | CITY	        | String         |
| state          | STATE        | String         |
| zipCode        | ZIPCODE      | String         |

5. Select the Alfresco Tab Select alfresco1 in the Repository Source dropdown.
6. Save and Close the Model.

### Lab 5: Create a Store Entity Task / Save Values to Database
1. Access the App Designer tile from the homepage of the Activiti App (Process Services).
2. Enter your New Claims process in edit mode by selecting the edit icon when hovering your mouse over its tile.    
3. Delete the sequence flow line going from the Add Customer Data task to the Create Cust Doc task.
4. Add a Store Entity Task to the process and connect it in place of the deleted flow line. Connect flow lines to and from the Add Customer Data and Create Cust Doc tasks. Your Store Entity task should be connected like this:	 
5. Select the new Store Entity Task and set the following configuration in the bottom panel:
    -	Name: Save Cust data to DB
    -	Select Attribute Mapping to open the mapping popup window. Perform the following actions:
        -	Mapped Data model: ```9siCustomerDatabase```
        -	Mapped entity: ```newCustomers```
        -	New Variable: ```newCustomerdata```
        -	Configure the following attributes in the mapping table by selecting each Attribute Name and choosing the variable / form field it is associated with:
            1.	Id: newCustomerId (Variable)
            2.	Firstname: First Name (Form field)
            3.	Lastname: Last Name (Form field)
            4.	Address: Address (Form field)
            5.	City: City (Form field)
            6.	State: State (Form field)
            7.	zipCode: Zip Code (Form field)
        -	Press Save on the mapping window.
6. Save and close your process.

### Lab 6: Create a Database Lookup Process
1.	Create a new Process called Lookup DB Values.
2.	In the bottom configuration panel, launch the Variables attribute window and add the following variable:
    -	Variable name: ```recordList```
    -	Variable type: ```string```
    -	Save the variable.
3.	Create a Script Task and use the configuration panel to assign the following values:
    -	Name: ```Get DB Values```
    -	Script Format: ```groovy```
    -	Script: (paste the following code into the scripting window)
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
}

def url = 'jdbc:oracle:thin:@//aps-custom-oracle-db.cp58lgpzkwpy.us-east-1.rds.amazonaws.com/ORCL'
def user = 'admin'
def password = 'administrator'
def driver = 'oracle.jdbc.driver.OracleDriver'
def sql = Sql.newInstance(url, user, password, driver)

rowNum = 0;
def recordList = [];

sql.eachRow('SELECT ID, FIRSTNAME, LASTNAME, ADDRESSLINE1, CITY, STATE, ZIPCODE FROM CUSTOMERS') { row ->
   
  def r = new Record( recId: row.id, firstname:row.firstname, lastname:row.lastname, address:row.addressLine1, city:row.city, state:row.state, zip:row.zipcode)
    recordList.add(r);
  
}

  println new JsonBuilder( recordList ).toPrettyString()
  execution.setVariable("recordList", new JsonBuilder( recordList ).toPrettyString())
```
4.	Add a User Task to the process and connect it from the Script Task. Name it: Display DB Values.
5.	Select the referenced form attribute and open the form prompt.
6.	Select New form.
7.	In the Form Editor page follow these steps to create the form:
    -	Drag a Dynamic Table onto the form stage. Select the pencil icon to go into edit mode.
    -	Enter into Label field: ```CUSTOMERS```
    -	Select the Override Id checkbox.
    -	Enter into the ID field: ```recordList```
    -	Select the Table Columns tab
    -	Press the "+" icon button to create new property mappings with the following values:
        -	Column 1:
            1.	Property ID: ```recId```
            2.	Property Name: ```ID```
            3.	Property Type: ```string```
        -	Column 2:
            1.	Property ID: ```firstname```
            2.	Property Name: ```First name```
            3.	Property Type: ```string```
        -	Column 3:
            1.	Property ID: ```lastname```
            2.	Property Name: ```Last Name```
            3.	Property Type: ```string```
        -	Column 4:
            1.	Property ID: ```address```
            2.	Property Name: ```Address```
            3.	Property Type: ```string```
        -	Column 5:
            1.	Property ID: ```state```
            2.	Property Name: ```State```
            3.	Property Type: ```string```
        -	Column 6:
            1.	Property ID: ```city```
            2.	Property Name: ```City```
            3.	Property Type: ```string```
        -	Column 7:
            1.	Property ID: ```zip```
            2.	Property Name: ```Zip```
            3.	Property Type: ```string```
    -	Close the edit prompt.
    -	Save and close the form.
8.	Add an End Event to the end of the process.
9.	Save and close the process.
10.	Add the processes to your app that you created in this and the previous lab
11.	Use the digital workspace to test both processes. The New Customer Onboarding process should now save the customer data into the database. Use the Display DB Values process to view added entries.

### Lab 7: Create a Service Task with Java Delegate
1.	Enter your New Customer Onboarding process in edit mode.
2.	Select the Variables attribute in the configuration panel to add a new variable.
3.	Add a variable titled ```stateVerify``` as a ```string```
4.	From the left panel, under Activities, drag a Service Task into the stage. 
5.	Select the Name attribute and give it the following name: KYC Delegate
6.	Connect this task in the workflow between the Save New Customer Data task and the Create Cust Doc task. 
7.	Select the Class attribute and paste in the following text: ```com.activiti.extension.bean.KYCJavaDelegate```
8.	Save and close the editor. 
9.	Navigate to the Process Application and publish your App so you may test the new additions.
10.	You may now test your process in the Digital Workspace. 
    -	*Note: To ensure the KYC Java Delegate task is working you can view the saved document and check the “out of state” box to ensure the true or false variable is being set by the Delegate.

### Lab 8: Add an Exclusive Gateway and Splitting Paths
1.	Open your New Customer process in edit mode.
2.	Delete the end event.
3.	From the left panel, under Gateways, add an Exclusive Gateway to your process. Connect it from the KYC Delegate task.
4.	Select the gateway and add a **User task** using the small menu that appears.
5.	Name the user task: ```Manager Follow-Up```
6.	With the user task selected, click on the Referenced form No reference selected value.
7.	In the Form reference popup window, select the New Form button.
8.	In the Create a new form window, enter the following values:
    - Form name: ```Manager Follow-Up```
    - Description: ```Allows manager to follow up with out-of-state customers.```
    - Stencil: ```Default form```
    - Select the Create form button. 
9.	Follow these steps in the Form Editor to create the form:
    - Add a Header to the page and go into edit mode. Configure with the following information:
        - Label: ```Customer Information```
    - Add a Display Text field to the header. Configure with the following information:
      text:
        ```
        Out of State Customer Information:
                        
        ${newCustomerLastName}, ${newCustomerFirstName} - ${newCustomerId}
        ${newCustomerPhoneNumber}
        ${newCustomerEmail}
        
        Please follow up with customer regarding out-of-state insurance waiver.
        ```
    - Add a Header to the page below the first header and go into edit mode. Configure with the following information:
        - Label: ```Manager’s Notes:```
    - Add a Multi-line text object to the new header. Configure with the following information:
        - Label: ```Please indicate summary of conversation with customer or enter “No Contact”.```
        - Override ID: checked
        - ID: ```customerNotes```
        - Required: checked
  	- Save and close the form editor.
11.	Select the Manager Follow-Up task and select the Assignment attribute in the configuration panel. This will open an assignment popup window. Set the following configuration in the popup window:
    - Type: ```Identity Store```
    - Assignment: ```Assigned to group manager```
    - Source: ```Search```
    - Search for and select the Claims-Team group. The Group attribute should now show the Claims-Team value.
    - Press the Save button.
12.	Connect a sequence flow line from the gateway task to the Create NC Doc task. Remove any associated sequence flow lines that connect the Create NC Doc task to any previous tasks.
    - With this flow line selected, check the **Default Flow** check-box in the bottom configuration panel.
13.	Create a sequence flow line from the Manager Follow-Up task to the Create NC Doc task. With this flow selected, select the **Flow condition** attribute and configure the following condition in the pop-up menu:
    - Condition Type: ```Simple```
    - Depends on: ```Variable```
    - Variable Drop-down: ```stateVerify```
    - Operator: ```equal```
    - Value: ```false```
14.	Save your process, redeploy your application, and test the process.


### Lab 9: Create a Stencil for Custom process Tasks
1.	From the AppDesigner page, select Stencils from the top, blue banner.
2.	Select the Create Stencil button.
    -	Name the stencil [user #] - Custom Email.
    -	Select BPMN from the dropdown.
3.	Select the Stencil Editor button to go into edit mode.
4.	Add a new group by selecting the + Add new group button. 
    - Name the group ```My Custom Components```
5.	Add a new item by selecting the + Add new item button. Enter the following information:
    - Task Type: ```Service Task```
    - Name: ```Send Email with Attachments```
    - Description: ```Send an email with attachments``` (or enter your own)
    - Group: use the dropdown selector to select the group you created above (My Custom Components)
    - Icon: Use the change icon prompt to browse your local machine. Find the folder you were provided and select the mail_icon.png found in the Icons folder.
    - Delegate Expression: ```${emailServiceWithAttachments}```
    - Asynchronous: checked
    - Select the Edit hyperlink next to Properties. 
        - In the popup window select the + Add a new property link. Fill in the following information.
            1.	Name: ```Email Template Name```
            2.	ID: ```email_template_name```
            3.	Type: String
            4.	Select the Include property as field extension checkbox.
            5.	Name of field extension: ```emailTemplate```
            6.	Select Save Property button.
        - Add another property by selecting the + Add a new property link.
            1.	Name: ```Send To```
            2.	ID: ```send_to```
            3.	Type: String
            4.	Select the Include property as field extension checkbox.
            5.	Name of field extension: ```toList```
            6.	Select Save Property button.
        - Add another property by selecting the + Add a new property link.
            1.	Name: ```Email Subject```
            2.	ID: ```email_subject```
            3.	Type: String
            4.	Select the Include property as field extension checkbox.
            5.	Name of field extension: ```subject```
            6.	Select Save Property button.
        - Add another property by selecting the + Add a new property link.
            1.	Name: ```Attachments```
            2.	ID: ```attachments```
            3.	Type: String
            4.	Select the Include property as field extension checkbox.
            5.	Name of field extension: ```contentField```
            6.	Select Save Property button.
        - Add another property by selecting the + Add a new property link.
            1.	Name: ```Include Attachments```
            2.	ID: ```inc_attachments```
            3.	Type: Boolean
            4.	Select the Include property as field extension checkbox.
            5.	Name of field extension: ```includeAttachments```
            6.	Select Save Property button.
        - Add another property by selecting the + Add a new property link.
            1.	Name: ```Send CC```
            2.	ID: ```send_cc```
            3.	Type: String
            4.	Select the Include property as field extension checkbox.
            5.	Name of field extension: ```ccList```
            6.	Select Save Property button.
        - Add another property by selecting the + Add a new property link.
            1.	Name: ```From```
            2.	ID: ```from```
            3.	Type: String
            4.	Select the Include property as field extension checkbox.
            5.	Name of field extension: ```from```
            6.	Select Save Property button.
        - Close the Edit stencil properties popup window.
6.	Save the Stencil using the save icon in the top, left of the page.

### Lab 10: (FACILITATOR ONLY) Create an Email Template
1.	Navigate to the Identity Management section.
2.	Select Tenants from the top blue banner.
3.	Select Email Templates from the left menu.
4.	Select Custom Email Templates.
5.	Select the +Create new email template button.
6.	Configure the email template with the following information:
    -	Name: ```9si-oos-email```
    -	Subject: ```Welcome to 9 Second insurance```
    -	Email content:
```
Hello, ${newCustomerFirstName}!

We value your business more than ever and look forward to providing you with exceptional service.

You have indicated that your home address is located within the state of ${newCustomerState}. Since 9 Second Insurance is an Ohio-based company, and due to federal regulations, we need to capture just a little bit more information from you. Attached is a Out-Of-State Policy form that we need completed in order to offer you the best insurance policy. Please complete and return at your leisure.

Best Regards,

9 Second Insurance, Onboarding Team.
```
-	Save the email template.


### Lab 11. Build a Custom Email Sub-process 
1.	Navigate to the App Designer.
2.	Create a new process with the following configuration:
    -	Name: ```[User #] Send Custom Email```. Ex: ```U1 Send Custom Email```.
    -	Description: ```Email sub-process.```
    -	Stencil: Select the Custom Email stencil
3.	In the bottom configuration panel, select the Variables attribute. Enter the following variables:
    -	ccEmailAddress: string
    -	fromEmailAddress: string
    -	newCustomerEmail: string
4.	Create a Script task. Configure with the following information:
    -	Name: ```Set Default Vars```
    -	Script format: ```groovy```
    -	Script:
```
execution.setVariable("fromEmailAddress", "claims-team@example.com");
execution.setVariable("ccEmailAddress", "blumbergh@example.com");
```
5.	Create a User task connected to the script task. Name it: Configure Email.
6.	Select the Referenced Form attribute from the configuration panel to open the referenced for popup. Select  New Form and create a new form with the following config:
    -	Name:  ```Email Config Form```
    -	Description: ```Used to configure custom email```
7.	Add a Header to the form. 
    -	Label it: ```Customer Information:```
    7a. Add a Display Text object and set the Display Text to:
```
Customer Information:

${newCustomerLastName}, ${newCustomerFirstName} - ${newCustomerId}
${newCustomerAddress}
${newCustomerCity}, ${newCustomerState}. ${newCustomerZipCode}
```
8.	Add a new Header to the form below the first header. Label it: Email Configuration:
9.	Add a Text object to the Email header with the following configuration:
    -	Label: 	```Email Subject:```
    -	ID: ```emailsubject```
    -	Required: checked
10.	Add a Text object to the Email header with the following configuration:
    -	Label: 	```To:```
    -	Override ID: checked
    -	ID: ```newCustomerEmail```
    -	Required: checked
11.	Add a Text object to the Email header with the following configuration:
    -	Label: 	```From:```
    -	Override ID: checked
    -	ID: ```fromEmailAddress```
    -	Required: checked
12.	Add a Text object to the Email header with the following configuration:
    -	Label: 	```CC:```
    -	Override ID: checked
    -	ID: ```ccEmailAddress```
    -	Required: checked
13.	Add a Check Box object to the Email header with the following configuration:
    -	Label: 	```Include Attachments:```
    -	Override ID: unchecked
    -	ID: ```includeattachments```
    -	Required: checked
14.	Add a Drop-Down object to the Email header with the following configuration:
    -	Label: 	```Select Email Template```
    -	Override ID: unchecked
    -	ID: ```selectemailtemplate```
    -	Required: checked
    -	Select the options tab and add the following option(s):
        -	Label: ```Out-of-State Email```
        -	ID: ```9si-oos-email```
15.	Add a Attach File object to the Email header with the following configuration:
    -	Label: 	```Attachment```
    -	Override ID: checked
    -	ID: ```content```
    -	Required: checked
16.	Save and close the form editor.
17.	Add the custom Send Email w Attachments task that was created during the Stencil la- Give it the following configuration:
    -	Name: ```Send Email```
    -	Email Template Name: ```${selectemailtemplate}```
    -	Sent To: ```${newCustomerEmail}```
    -	Email Subject: ```${emailsubject}```
    -	Attachments: ```content```
    -	Include Attachments: checked
    -	Send CC: ```${ccEmailAddress}```
    -	From: ```${fromEmailAddress}```
18.	Add an End Event connected to the Send Mail task.
19.	Save and close the process editor.


### Lab 12: Add a Sub-process (Custom Email process)
1.	Open the Customer Onboarding Process in edit mode.
2.	Add a Collapsed Subprocess to the process after the Manager Follow-up task.
    -	Connect the collapsed subprocess from the manager foll-up and to the Create NH Doc task.
3.	Give the Collapsed Subprocess event the following configuration:
    -	Name: ```Send OOS Email```
    -	Referenced Subprocess: choose the ```Send Custom Email process``` created in previous la-
4. Save and test the process.


---



