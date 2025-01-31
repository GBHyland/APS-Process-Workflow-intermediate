# Reimburesement Claims Process

### Scenario – 9 Second Insurance: New Customer Process
As a systems architect working for 9 Second Insurance, you are tasked with creating a process that will allow agents to create reimbursement claims. Your process will include:
- Data intake mechanism to capture claim information. 
- Data review mechanism to determine accuracy of information.
- Document generation and saving to content management system.
- A REST call to retrieve customer values from a custom database.
- A Decision Table to perform logic and edit values.


### Lab 1. Create an Intake Task
1.	From the Alfresco home page, launch the Activiti App (Process Services) by clicking on the Activiti App hyperlink. 
    1.	Sign in with your provided username and password. You will be directed to the Activiti App home page.
2.	Select the App Designer tile to navigate to the Business Process Models page.
3.	To create a new process, press the Create Process button in the top right of the page.
4.	In the Create a new business process model popup, enter the following information:
    1.	Model name: [Your user #] New Claim (ex: U1 New Claim)
    2.	Description: Create a new claim.
    3.	Editor Type: Leave as BPMN editor
    4.	Stencil: Default BPMN
    5.	Press the Create new model button.
5.	With nothing selected on the stage, select the Variables attribute in the bottom configuration panel.
6.	Select the “+” icon below the chart to add new variables. We’ll add a few variables that will be used in this process. Create the following variables with this configuration:
|  *VARIABLE NAME*    |  VARIABLE TYPE    |
|  ```cAddress```     |  string           |
|  ```cCity```        |  string           |
|  ```cFirstName```   |  string           |
|  ```cLastName```    |  string           |
|  ```cState```       |  string           |
|  ```cZip```         |  string           |
|  ```deductible```   |  integer          |
|  ```lu_lastname```  |  string           |
|  ```recordCount```  |  integer          |
|  ```recordList```   |  string           |

7.	Create a new User Task connected to the start event.   
8.	Give the user task a name by double-clicking on the task to open a text field. Name this task Customer Search.
9.	With the user task selected, notice that the Referenced form value in the bottom configuration window is No reference selected. To create an intake form for this task, click on the No reference selected value.
10.	In the Form reference popup window, select the New Form button.
11.	In the Create a new form window, enter the following values:
    1.	Form name: Customer Lookup
    2.	Description: Gather customer information to retrieve records.
    3.	Stencil: Default form
    4.	Select the Create form button. 
12.	 Follow these steps to create the form you’ll need to intake a new hire employee.
    1.	From the left object menu, drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Enter the Customer’s last name and click the Close button.
    2.	Drag a Text object and dop it into the Header object.
    3.	Click on the pencil icon on the Display Value object to open the edit prompt. Configure the text field with the following options:
        1.	Label: Last Name:
        2.	Override ID: check the box
        3.	ID: lookup_lastname
        4.	Required: check the box
        5.	Close the edit prompt.
    4.	Save and close the form editor by clicking on the save button.
    5.	On the Save form popup window, click the Save and close editor button to return to your process model
13.	Create a new script task connected to the Customer Search user task. In the bottom configuration panel, configure the following attributes:
    1.	Name: Get DB Values
    2.	Script format: groovy
    3.	Script:
enter this code in the popup script window:
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
14.	Create a new User Task and connect it to the script task. Name it Display DB Values.
15.	In the bottom configuration panel, select the Referenced Form attribute. 
16.	In the form popup window, select the New Form button.
17.	In the Create a new form window, enter the following values:
    1.	Form name: Display DB Values
    2.	Description: Displays values from customer DB lookup.
    3.	Stencil: Default form
18.	Select the Create form button.
19.	Follow these steps to create the form:
    1.	From the left object menu, drag a Header onto the canvas. To edit, click on the pencil icon that appears when you hover your mouse over the header object. In the Label field, name it Query and click the Close button.
    2.	Drag a Display Text object and dop it into the Header object. Select the pencil icon to edit.
        1.	In the Text to display field enter the following text, then close the prompt.
code:
    ```
      Found ${recordCount} records with the Last Name of "${lu_lastname}".
    
    ```
    3.	From the left panel drag a Dynamic Table onto the stage adding it below the Header object. Click on the pencil icon to edit.
    4.	Give it a Label of: CUSTOMERS
    5.	Select the Override ID check box.
    6.	Give it an ID of: recordList
    7.	Select the Table Columns tab at the top of the prompt.
    8.	Press the “+” icon button to create new property mappings with the following values (Note: for each property check all of the boxes Required, Editable, sortable, show in table):
        1.	Column 1:
            1.	Property ID: recId
            2.	Property Name: ID
            3.	Property Type: string
        2.	Column 2:
            1.	Property ID: firstname
            2.	Property Name: First name
            3.	Property Type: string
        3.	Column 3:
            1.	Property ID: lastname
            2.	Property Name: Last Name
            3.	Property Type: string
        4.	Column 4:
            1.	Property ID: address
            2.	Property Name: Address
            3.	Property Type: string
        5.	Column 5:
            1.	Property ID: state
            2.	Property Name: State
            3.	Property Type: string
        6.	Column 6:
            1.	Property ID: city
            2.	Property Name: City
            3.	Property Type: string
        7.	Column 7:
            1.	Property ID: zip
            2.	Property Name: Zip
            3.	Property Type: string











