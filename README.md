# APS-Process-Workflow-Intermediate

##Lab 1. Create an Intake Task
1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
    - Sign in with your provided username and passwor- You will be directed to the Activiti App home pag-
2.	Select the App Designer tile to navigate to the Business Process Models pag-
3.	To create a new process, press the Create Process button in the top right of the pag-
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
    -	Save the variabl-
7.	Create a new script task connected to the start event. In the bottom configuration panel, configure  the following attributes:
    -	Name: Set Customer ID
    -	Script format: groovy
    -	Script: enter this code in the popup script window:
```
execution.setVariable('newCustomerId', execution.getProcessinstanceId());
```
8.	Create a new User Task connected to the script task.   
9.	Give the user task a name by double-clicking on the task to open a text fiel- Name this task Gather Customer Dat-
10.	With the user task selected, notice that the Referenced form value in the bottom configuration window is No reference selecte- To create an intake form for this task, click on the No reference selected valu-
11.	In the Form reference popup window, select the New Form button.
12.	In the Create a new form window, enter the following values:
    -	Form name: Add New Customer Data
    -	Description: Gathers information about the new customer.
    -	Stencil: Default form
    -	Select the Create form button. 
13.	 Follow these steps to create the form you’ll need to intake a new hire employe-
    -	From the left object menu, drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer ID and click the Close button.
    -	Drag a Display Value object and dop it into the Header object.
    -	Click on the pencil icon on the Display Value object to open the edit prompt.
    -	Select the blue Variable button. In the Dropdown below the button, select the newCustomerId variabl- The Label should change to match the variable nam- Close the prompt.
    -	Drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer Information and click the Close button.
    -	Drag a Text object onto the canvas and drop it into the header object. Click the pencil to edit the text object and configure the following information: 
      -	Label: First Name: 
      -	Select the Override ID check box.
      -	Enter newCustomerFirstName into the ID fiel-
      -	Select the Required check box. 
      -	Click on the Close button.
  g.	Perform the previous step again to create a Text object with the following information: 
      -	Label: Last Name:
      i-	ID: newCustomerLastName 
      ii-	Required: checked
  h.	Create another Text object with the following information:
      -	Label: Address:
      i-	ID: newCustomerAddress 
      ii-	Required: checked
  -	Create another Text object with the following information:
      -	Label: City:
      i-	ID: newCustomerCity
      ii-	Required: checked
  j.	Create a Dropdown object with the following information:
      -	Label: State:
      i-	ID: newCustomerState
      ii-	Required: checked
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
    i-	ID: newCustomerZipCode
    ii-	Required: checked
  l.	Create another Text object with the following information:
    -	Label: Email Address:
    i-	ID: newCustomerEmail
    ii-	Required: checked
  m.	Create another Text object with the following information:
    -	Label: Phone Number:
    i-	ID: newCustomerPhoneNumber
    ii-	Required: checked
  n.	To save the form and return to your process model, click on the save button in the top left of the pag-   
  o.	On the Save form popup window, click the Save and close editor button to return to your process model.
14.	Create a connected end event by selecting the Gather Customer Data user task and clicking on the end event icon in the small popup menu.   
15.	Save the process model by clicking on the Save icon in the top left of the pag-   
16.	In the Save model popup window, press the Save and close editor button.

##Lab 2: Create a Process Application
