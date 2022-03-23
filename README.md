# TrackerFlow

The TrackerFlow application will manage and track requests for Human Resources, Legal, Facilities, and one other department (to be determined) in an automated way, using an easy-to-use user interface that is consistent with other corporate software

TrackerFlow:

Table Name: x_705716_trackerfl_trackerflow
Config: x_705716_trackerfl_trackerflow.CONFIG
List: x_705716_trackerfl_trackerflow.LIST
Users: Beth Anglin and Zane Sulikowski
Admin:  Fred Luddy and Alissa Mountjoy 
Client Scripts: 
There are two client scripts:
1. Name: TrackerFlow Request Type Options
Used: g_form.getValue(), g_form.clearOptions(), g_form.setValue(), g_form.isNewRecord()
Description: Change the "What needed" field based on  "Request type" field. Eg: If what needed is Legal, then change the Request type choices as Legal
2. Name: TrackerFlow Set Requested for
Used:  g_form.setValue(), g_form.isNewRecord()
Description: Set the Requested for to the currently logged in user for new records. Users can change the field value

UI Policies:



Business Rules:
1. Name: NeedIt When needed field date
Used: Before Insert, Created an object of GlideDateTime() class to get current date, Created an object of GlideDateTime(current.u_when_needed) to
get the time object as per populated in the field, gs.addErrorMessage(), before() to compare dates, current.setAbortAction(true) to abort the BR
Description: if "when needed" is before today's date, then abort the BR.  

Debugging BR i.e server side:
1. System Logs:
gs.info(), gs.warn(), gs.error(), gs.debug() 
See the log msgs in System Logs > System Log > Application Logs

2. Tracing:
System Diagnostics > Script Tracer
The Script Tracer begins tracing when a UI Action, such as the Save or Update button is clicked, or some other transaction occurs.

3. JS Debugger:
System Diagnostics > Script Debugger
The JavaScript Debugger is the primary strategy for debugging Business Rules and other synchronous server-side scripts.


Script Includes:
Types: On demand/classless, Extend an existing class, Define a new class
Write a script include and call them in a Business Rule
1. Name:  validateEmailAddress
Used: string.match() function to match 2 strings
Description: On demand Script Include to validate email address syntax using regular expressions.
Name of the Business Rule used to call the script include: Email Address Syntax Validate
Used: validateEmailAddress(current.u_requested_for_email) to call the script include, gs.addErrorMessage()


2. Script include for extending a class 
Although most ServiceNow classes are extensible, the most commonly extended classes are: GlideAjax, LDAPUtils & Catalog
Eg. Extending the Glide Ajax class using script include
Name of the script include:  GetEmailAddress
Used: GlideRecord("sys_user"), userRecord.get(this.getParameter('sysparm_userID'))
Description: The GlideAjax class is used by client-side scripts to send data to and receive data from the ServiceNow server. The client-side script passes parameters to the Script Include. The Script Include returns data as XML or a JSON object. The client-side script parses data from the response and can use the data in subsequent script logic.
Requested for is a referenced field i.e a record from the users table. In the form, we want to populate the email field (on client side) using info
from the users table (email field). This data transfer from server to client side is done by using the glide ajax class. 
The script include is client callable. We call the script include on the client side to get email of the user from the server side to client side.

Name of Client Script used to call the GetEmailAddress script include: TrackerFlow Populate Email Field
Used: Created an object of GlideAjax('GetEmailAddress'), getEmailAddr.addParam('sysparm_name','getEmail'),  getEmailAddr.getXML() to send 
request to the server

3. Utilities script include
Most applications store util functions in a utilities script include. Call them on server side. The SI is a class with different methods.
To use them, create an object of the SI class in the server script and call the required method.
Name of SI: TrackerFlowUtils


Other Server side scripts:
Script Action: respond to events
Scheduled Script Execution: Run a report and email it
UI Actions: Adding buttons
Notification Email Script
Scripted REST APIs: Request sent or received through web services
UI Page Processing Script: Script that is executed once UI page is submitted
Transform Map Script: Data import


Securing application against unauthorised users
Role: Frames rules for limits of access to the user
Group: Users that share a common purpose. Eg. for email notification, approvals etc.
Description: The open module of the application is accessbible to only admins. While the All module is accessible to all users.




Import data from a csv
Have 5 records in a .csv file. Import them into the trackerflow table.
Process: Load data in staging table, then create transform map, transform & check data integrity.
Map "Must have by source column" to "When needed" target column, Coalesce on all Field Map fields
Enforce Mandatory for all fields, Set the target table Requested for email value using a script


Flow designer
Name: TrackerFlow Fulfillment
Used:
Description: Steps to approve and fulfill Trackerflow requests. Trigger is record created. 
Action 1: Update the state field to Approval awaiting and the assigned to field to Beth anglin.
Action 2: Ask for approval. Manager: Fred Luddy. Employee: Adela Cervantsz.





