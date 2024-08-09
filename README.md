# APS-Process-Workflow-Intermediate

1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
a.	Sign in with your provided username and password. You will be directed to the Activiti App home page.
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
