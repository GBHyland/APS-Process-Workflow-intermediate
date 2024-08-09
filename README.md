# APS-Process-Workflow-Intermediate

## Lab 1. Create an Intake Task
1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
    - Sign in with your provided username and passwor- You will be directed to the Activiti App home page
2.	Select the App Designer tile to navigate to the Business Process Models page
3.	To create a new process, press the Create Process button in the top right of the page
4.	In the Create a new business process model popup, enter the following information:
    -	Model name: [Your user #] New Customer Onboarding (ex: U1 New Customer Onboarding)
    -	Description: Add a new customer.
    -	Editor Type: Leave as BPMN editor
    -	Stencil: Default BPMN
    -	Press the Create new model button.
5.	With nothing selected on the stage, select the Variables attribute in the bottom configuration panel.
6.	Select the “+” icon below the chart to add a new variabl- Give it the following information:
    -	Name: newCustomerId
    -	Variable Type: string
    -	Save the variable
7.	Create a new script task connected to the start event. In the bottom configuration panel, configure  the following attributes:
    -	Name: Set Customer ID
    -	Script format: groovy
    -	Script: enter this code in the popup script window:
```
execution.setVariable('newCustomerId', execution.getProcessinstanceId());
```
8.	Create a new User Task connected to the script task.   
9.	Give the user task a name by double-clicking on the task to open a text fiel- Name this task Gather Customer Date
10.	With the user task selected, notice that the Referenced form value in the bottom configuration window is No reference selecte- To create an intake form for this task, click on the No reference selected value
11.	In the Form reference popup window, select the New Form button.
12.	In the Create a new form window, enter the following values:
    -	Form name: Add New Customer Data
    -	Description: Gathers information about the new customer.
    -	Stencil: Default form
    -	Select the Create form button. 
13.	Follow these steps to create the form you’ll need to intake a new hire employee
    - From the left object menu, drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer ID and click the Close button.
    -	Drag a Display Value object and dop it into the Header object.
    -	Click on the pencil icon on the Display Value object to open the edit prompt.
    -	Select the blue Variable button. In the Dropdown below the button, select the newCustomerId variabl- The Label should change to match the variable nam- Close the prompt.
    -	Drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer Information and click the Close button.
    -	Drag a Text object onto the canvas and drop it into the header object. Click the pencil to edit the text object and configure the following information: 
    -	Label: First Name: 
        -	Select the Override ID check box.
        -	Enter newCustomerFirstName into the ID fiele
        -	Select the Required check box. 
        -	Click on the Close button.
  g.	Perform the previous step again to create a Text object with the following information: 
        -	Label: Last Name:
        - ID: newCustomerLastName 
        - Required: checked
  h.	Create another Text object with the following information:
        -	Label: Address:
        - ID: newCustomerAddress 
        - Required: checked
    -	Create another Text object with the following information:
        -	Label: City:
        -	ID: newCustomerCity
        -	Required: checked
  j.	Create a Dropdown object with the following information:
        -	Label: State:
        -	ID: newCustomerState
        -	Required: checked
        -	Select the Options ta- Configure the following information in the options tab:
            1.	Select the Rest Service button.
            2.	Rest URL: https://run.mocky.io/v3/0329674b-51fe-450f-90cf-be8f53cb68a6
            3.	Path to array in JSON response: states
            4.	ID property: abbreviation
            5.	Label property: name
            6.	Press the Test button. A popup window should appear showing a JSON string of states. Close the window.
            7.	Close the dropdown prompt.
  k.	Create another Text object with the following information:
        -	Label: Zip Code:
        -	ID: newCustomerZipCode
        -	Required: checked
  l.	Create another Text object with the following information:
        -	Label: Email Address:
        -	ID: newCustomerEmail
        -	Required: checked
  m.	Create another Text object with the following information:
        -	Label: Phone Number:
        -	ID: newCustomerPhoneNumber
        -	Required: checked
  n.	To save the form and return to your process model, click on the save button in the top left of the pag-   
  o.	On the Save form popup window, click the Save and close editor button to return to your process model.
14.	Create a connected end event by selecting the Gather Customer Data user task and clicking on the end event icon in the small popup menu.   
15.	Save the process model by clicking on the Save icon in the top left of the pag-   
16.	In the Save model popup window, press the Save and close editor button.

## Lab 2: Create a Process Application
1.	From the Activiti home page, select the App Designer tile to navigate to the Business Process Models page
2.	Select the Apps hyperlink in the top blue banner.
3.	Select the Create App button. 
4.	Give your application the App definition name: [Your User #] 9SecondInsurance (ex: U1 9SecondInsurance). 
    -	Optional: enter a description of your application describing it will do, --: Deploys our claims processes.
5.	Click the Create new ap definition button.
6.	OPTIONAL: On the App definition details page, you can change the color and icon of your application til- Use the Icon and Theme drop downs to customize your tile
7.	To add the process model you created, click on the Edit included models button below the til- 
8.	In the Models included in app definition popup window, select the process model you want to includ- NOTE: The selections toggle on and off when you click them, so be sure to click the one you want once and notice the blue + icon will appear to ensure it is selecte-   
9.	Click the Close button to close the popup window.
10.	Save the App definition by clicking on the save icon located in the top left of the pag-   
11.	In the Save app definition popup window, ensure you select the Publish? Checkbox, then click on the Save and close editor button.
12.	Navigate back to the home page by clicking the home button in the top, blue banner:  
13.	If your application already appears on the home page as a tile you are don- 
    -	If not, deploy your new application by selecting the blank tile, depicted with a plus sign “+” (“Add a new app” appears when you hover your mouse over it. Select this tile, then select your application in the Add app to landing page popup window. Press the Deploy button on that window. Your application is now deployee


## Lab 3: Generating and Saving a Document
1.	Access the App Designer tile from the homepage of the Activiti App (Process Services).
2.	Enter your New Customer Onboarding process in edit mode by selecting the edit icon when hovering your mouse over its til-    
3.	Remove the End event and the line connecting to it by selecting each one and clicking the trash can icon that appears. 
4.	Add a Generate Document task to the process:
    -	From the left task menu, select Activities drop down to reveal the activities tasks. Click and drag the Generate document task dropping it onto the stag- Drag it into place and connect it to the Approve Sequence flow line
    -	To configure the Generate Document task select it to show its attribute values in the bottom configuration panel.
    -	Name your Generate Document task: Create NH Do- 
    -	Select the Document Variable attribute and enter the value newCustDoc into the fiele
    -	Select the Template attribute to open the Change value for “Template” popup window.
        -	Select the Custom Template tae
        -	Select the Choose File button to open a file browsing window. From the file package you received with this document, navigate to the following file path and choose this file:
            1.	_Aps_documents/form_doc_templates/9si_newCustomer.docx_
        -	The name of the template file should now appear as the value of the Template attribute
    -	Select the File name attribute to open the Change value for “File name” popup box.
        -	Enter the following file name:
```
customer_${newCustomerId}
```
        
        -	Press the save button to close the popup window.
5.	Add a Publish to Alfresco task to the process:
    -	From the left task menu, select Alfresco drop down to reveal Alfresco related tasks. Click and drag the Publish to Alfresco task dropping it onto the stag- Drag it into place on the right side of the Create Claim Doc task.
    -	Connect a flow line from the Create Claim Doc task to the new Publish to Alfresco task. 
6.	Name your Generate Document task by double clicking on task and opening the name fiel- 
    -	Name the task Save Doc to CMS.
7.	To configure the Publish to Alfresco task select it to show its attribute values in the bottom configuration panel.
8.	Select the Alfresco Content attribute to open the Change Valu- popup window. Choose Publish all content uploaded in process from the dropdown menu and press the Save button.
9.	Select the Alfresco Destination attribute to open the Change Valu- popup window.
    -	Next to Destination, click the Select folder… button to open the browse Alfresco popup window. The site you created previously should populate her- Choose your site, then navigate to and select the following folder path: _9SecondInsurance/ documentLibrary/Customer Information_
-	Press the Save button on the Change Value popup window.
10.	Finally, add an end event to your process. Select the Publish to Alfresco task, then select the end event icon from the small popup menu.
11.	Save your process and close the editor.
12.	Navigate to your application and republish. 
13.	Test your updated process in the Digital Workspace

## Lab 4: Create a Data Model
1. Access the data Model page within the App Designer.
2. Select the Create Data Model button.
3. Enter 9siCustomerData in the Data Model Name field and click the Create button.
4. In the Database Tab:
    -    Select aps-oracle-db from the Data Source dropdown
    -    Click the Add Entity button. Enter the following information for the Entity:
            -	Entity Name: newCustomers
            -	Entity Description: Details of all customers
            -	Table Name: CUSTOMERS
    -	Click the Import Attributes button. Notice that the table attributes should import from the connected database. Ensure that the values mapped correctly:
    - **Note:** the id variable should have the Primary key check box checked. All others do not.

| Attribute name | Column name  | Attribute type |
| ----------     | ---------    | -------------- |
| id             | ID           | Number         |
| firstname      | FIRSTNAME    | String         |
| lastname       | LASTNAME     | String         |
| address        | ADDRESSLINE1 | String         |
| city           | CITY	        | String         |
| state          | STATE        | String         |
| zipCode        | ZIPCODE      | String         |

5	Select the Alfresco Ta- Select alfresco1 in the Repository Source dropdown.
6	Save and Close the Model.

