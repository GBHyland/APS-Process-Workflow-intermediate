# Lab: API Explorer

## 1. Launch the API Explorer

1.  Launch the API Explorer by clicking the **Process Services API
    Explorer** link from the Alfresco demo platform page.
2.  Click the **Authorize** button and sign in with the credentials
    provided. Close the popup window once authorized.

------------------------------------------------------------------------

## 2. Retrieve a list of Process Definitions

a.  Scroll down and expand the **process-definitions** section.\
b.  Select the API titled: **GET
    /activiti-app/enterprise/process-definitions** (Retrieve a list of
    process definitions).\
c.  Select the **Try it out** button.\
d.  Click the blue **Execute** button.\
e.  Review the JSON reply in the **Response body** field. You should see
    an array of process definitions---each with name, description, id,
    etc.\
f.  Find a process that you own using the **name** value and copy its
    **id**.

------------------------------------------------------------------------

## 3. Get Forms for a Process & Look Up a Form

a.  Expand **GET
    /activiti-app/api/enterprise/process-definitions/{processDefinitionId}/forms**\
b.  Select **Try it out**\
c.  Paste the copied **processDefinitionId**\
d.  Press **Execute**\
e.  Review the returned array of forms\
f.  Copy the **modelId** from one of the forms\
g.  Expand the **form-models** section\
h.  Select **GET /activiti-app/api/enterprise/form-models/{formId} --
    Get a form model**\
i.  Paste the **modelId** and run the request\
j.  Review the JSON details for the form model---fields, outcomes,
    metadata, variables

------------------------------------------------------------------------

## 4. ACTIVE Tasks, Task Audit Log, Assigning a User

### Prepare

a.  Start a process in **activiti-app** or ADW and stop at a
    manual/human task (do *not* complete it).

------------------------------------------------------------------------

### Query Active Tasks

b.  Expand the **tasks -- Manage tasks** section\
c.  Expand **POST /activiti-app/api/enterprise/tasks/query -- List
    tasks**\
d.  Click **Try it out**\
e.  Replace the request body with:

``` json
{
  "state": "active"
}
```

f.  Press **Execute**\
g.  Copy the **id** of an active task

------------------------------------------------------------------------

### Get Audit Log

h.  Expand **GET /activiti-app/api/enterprise/tasks/{taskId}/audit --
    Get the audit log for a task**\
i.  Select **Try it out**\
j.  Paste the **taskId**\
k.  Press **Execute**\
l.  Review the returned task audit details\
m.  Note the **taskId** for later

------------------------------------------------------------------------

### Query Users

n.  Expand the **users** section\
o.  Select **GET /activiti-app/api/enterprise/users -- Query users**\
p.  Click **Try it out**\
q.  Enter `"demo"` in the **filter** field\
r.  Press **Execute**\
s.  Copy the returned user's **id**

------------------------------------------------------------------------

### Delegate a Task

t.  Expand the **task-actions** section\
u.  Expand **PUT
    /activiti-app/api/enterprise/tasks/{taskId}/action/delegate --
    Delegate a task**\
v.  Click **Try it out**\
w.  Paste the copied **taskId**\
x.  Enter the following into the request body, replacing `"XX"` with the
    user's id:

``` json
{
  "email": "demo@example.com",
  "userId": "XX"
}
```

y.  Press **Execute**\
z.  If no errors, return to **activiti-app** and verify the task is now
    assigned to **User 0**. (Facilitator log into the Demo account and show the delegation)
