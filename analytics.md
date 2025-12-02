# Lab: Analytics Application

## 1. Access the Analytics Application

1.  From the Process Services homepage, select the green **Analytics App* tile.

------------------------------------------------------------------------

## 2. Configure a Process Instance Report

a.  In the Analytics App, select the **Configure** button.\
b.  Select **Process Instances Overview** from the list of configurable reports.\
c.  Select the **Process Instances Overview** report.\
d.  Use the **Process Definition** drop-down to select your process.\
e.  Use the **Date range** field and select _Current Year_.\
f.  Use the **Process status** drop-down and select _All_.
g.  At the time of this writing, the date range will sometimes reset, if so select the date range again and select _Current Year_.
h.  The results should populate automatically.

------------------------------------------------------------------------

## 2. Configure a Task Overview Report

a.  In the Analytics App, select the **Configure** button.\
b.  Select **Task Overview** from the list of configurable reports.\
c.  Select the **Task Overview** report.\
d.  Use the **Process Definition** drop-down to select your process.\
e.  Use the **Date range** field and select _Current Year_.\
f.  Use the **Process status** drop-down and select _All_.
g.  At the time of this writing, the date range will sometimes reset, if so select the date range again and select _Current Year_.
h.  The results should populate automatically.

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
