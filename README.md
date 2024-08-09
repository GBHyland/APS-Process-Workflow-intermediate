# APS-Process-Workflow-Intermediate

<h2>Lab 1. Create an Intake Task</h2>
1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
  a.	 Sign in with your provided username and password. You will be directed to the Activiti App home page.
2.	Select the App Designer tile to navigate to the Business Process Models page.
3.	To create a new process, press the Create Process button in the top right of the page.
4.	In the Create a new business process model popup, enter the following information:
  a.	Model name: [Your user #] New Customer Onboarding (ex: U1 New Customer Onboarding)
  b.	Description: Add a new customer.
  c.	Editor Type: Leave as BPMN editor
  d.	Stencil: Default BPMN
  e.	Press the Create new model button.
5.	With nothing selected on the stage, select the Variables attribute in the bottom configuration panel.
6.	Select the “+” icon below the chart to add a new variable. Give it the following information:
  a.	Name: newCustomerId
  b.	Variable Type: string
  c.	Save the variable.
7.	Create a new script task connected to the start event. In the bottom configuration panel, configure  the following attributes:
  a.	Name: Set Customer ID
  b.	Script format: groovy
  c.	Script: enter this code in the popup script window:
```
execution.setVariable('newCustomerId', execution.getProcessinstanceId());
```
8.	Create a new User Task connected to the script task.   
9.	Give the user task a name by double-clicking on the task to open a text field. Name this task Gather Customer Data.
10.	With the user task selected, notice that the Referenced form value in the bottom configuration window is No reference selected. To create an intake form for this task, click on the No reference selected value.
11.	In the Form reference popup window, select the New Form button.
12.	In the Create a new form window, enter the following values:
  a.	Form name: Add New Customer Data
  b.	Description: Gathers information about the new customer.
  c.	Stencil: Default form
  d.	Select the Create form button. 
13.	 Follow these steps to create the form you’ll need to intake a new hire employee.
  a.	From the left object menu, drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer ID and click the Close button.
  b.	Drag a Display Value object and dop it into the Header object.
  c.	Click on the pencil icon on the Display Value object to open the edit prompt.
  d.	Select the blue Variable button. In the Dropdown below the button, select the newCustomerId variable. The Label should change to match the variable name. Close the prompt.
  e.	Drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Customer Information and click the Close button.
  f.	Drag a Text object onto the canvas and drop it into the header object. Click the pencil to edit the text object and configure the following information: 
    i.	Label: First Name: 
    ii.	Select the Override ID check box.
    iii.	Enter newCustomerFirstName into the ID field.
    iv.	Select the Required check box. 
    v.	Click on the Close button.
  g.	Perform the previous step again to create a Text object with the following information: 
    i.	Label: Last Name:
    ii.	ID: newCustomerLastName 
    iii.	Required: checked
  h.	Create another Text object with the following information:
    i.	Label: Address:
    ii.	ID: newCustomerAddress 
    iii.	Required: checked
  i.	Create another Text object with the following information:
    i.	Label: City:
    ii.	ID: newCustomerCity
    iii.	Required: checked
  j.	Create a Dropdown object with the following information:
    i.	Label: State:
    ii.	ID: newCustomerState
    iii.	Required: checked
    iv.	Select the Options tab. Configure the following information in the options tab:
      1.	Select the Rest Service button.
      2.	Rest URL: https://run.mocky.io/v3/0329674b-51fe-450f-90cf-be8f53cb68a6
      3.	Path to array in JSON response: states
      4.	ID property: abbreviation
      5.	Label property: name
      6.	Press the Test button. A popup window should appear showing a JSON string of states. Close the window.
      7.	Close the dropdown prompt.
  k.	Create another Text object with the following information:
    i.	Label: Zip Code:
    ii.	ID: newCustomerZipCode
    iii.	Required: checked
  l.	Create another Text object with the following information:
    i.	Label: Email Address:
    ii.	ID: newCustomerEmail
    iii.	Required: checked
  m.	Create another Text object with the following information:
    i.	Label: Phone Number:
    ii.	ID: newCustomerPhoneNumber
    iii.	Required: checked
  n.	To save the form and return to your process model, click on the save button in the top left of the page.   
  o.	On the Save form popup window, click the Save and close editor button to return to your process model.
14.	Create a connected end event by selecting the Gather Customer Data user task and clicking on the end event icon in the small popup menu.   
15.	Save the process model by clicking on the Save icon in the top left of the page.   
16.	In the Save model popup window, press the Save and close editor button.


