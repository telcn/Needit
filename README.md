# TrackerFlow

The TrackerFlow application will manage and track requests for Human Resources, Legal, Facilities, and one other department (to be determined) in an automated way, using an easy-to-use user interface that is consistent with other corporate software.

# Tables


# Client-side Scripts
Uses: 
1. Alerts, confirm, message
2. Populate a form field in response to another field's value
3. Validate form data
4. Hide/Show/Highlight fields or sections in the form
5. Place cursor in the form field on load

Types: 
1. Client scripts 
2. UI policies

Client Scripts:
Types:
1. onLoad-  execute script logic when forms are loaded. Used to **manipulate** a form's appearance or content. Can impact form load time. 
2. onChange- execute script logic when a particular field's value changes. Used to **respond** to field values of interest and to modify another field's value or attributes. Eg. if the State field's value changes to Closed Complete, generate an alert and make the Description field mandatory.
3. onSubmit- execute script logic when a form is submitted. Used to **validate** field values. Eg. validate if the entered email is valid.


g_form
Uses:
1. Retrieve a field value on a form
2. Hide a field
3. Make a field mandatory
4. Write a message on a form or a field
5. Add/Remove fields to a choice list
6. 

eg. 
alert(g_form.getValue('short_description'));
g_form.addInfoMessage('This is some info');


g_user
Uses:
1. To retrieve the user's first name, full name, last name, user ID, user name
2. Determine if the user has a particular role

alert("g_user.firstName = " + g_user.firstName 
     + ", \n g_user.lastName = " + g_user.lastName 
     + ", \n g_user.userName = " + g_user.userName 
     + ", \n g_user.userID = " + g_user.userID);
     
 g_user.hasRole('client_script_admin');
 g_user.hasRoleExactly('client_script_admin');


Client Script 1:
onChange
To do: Populate the _What needed_ choice list values based on the value in the _Request type_ field
Name: NeedIt Request Type Options
Script:

function onChange(control, oldValue, newValue, isLoading, isTemplate) {
   if (isLoading || newValue === '') {
      return;
   }

   //Type appropriate comment here, and begin script below
	
	
	var whatneeded = g_form.getValue('u_what_needed');
    
    // Clear all of the choices from the What needed field choice list
    g_form.clearOptions('u_what_needed');
    
    // If the value of the Request type field is hr, add
    // two hr choices and other to the What needed field choice list
    if(newValue == 'hr'){
      g_form.addOption('u_what_needed','hr1','Human Resources 1');
      g_form.addOption('u_what_needed','hr2','Human Resources 2');
      g_form.addOption('u_what_needed','other','Other');
    }
    // If the value of the Request type field is facilities, add
    // two facilities choices and other to the What needed field
    // choice list
    if(newValue == 'facilities'){
      g_form.addOption('u_what_needed','facilities1','Facilities 1');
      g_form.addOption('u_what_needed','facilities2','Facilities 2');
      g_form.addOption('u_what_needed','other','Other');
    }
    // If the value of the Request type field is legal, add
    // two legal choices and other to the What needed field
    // choice list
    if(newValue == 'legal'){
      g_form.addOption('u_what_needed','legal1','Legal 1');
      g_form.addOption('u_what_needed','legal2','Legal 2');
      g_form.addOption('u_what_needed','other','Other');
    }
    
    // If the form is loading and it is not a new record, set the u_what_needed value to the
    // value from the record before it was loaded
    if(isLoading && !g_form.isNewRecord()){
      g_form.setValue('u_what_needed', whatneeded);
    }
   
}


Client Script 2:
onLoad
To do: Populate the _Requested for_ field
Name: TrackerFlow Set Requested for
Script:

function onLoad() {
   //Type appropriate comment here, and begin script below
   if(g_form.isNewRecord()){
        g_form.setValue('u_requested_for',g_user.userID);
    }
}




Debugging client scripts:
1.  jslog("This message is from jslog().");   
2.  try{
    helloWorld();
  }
  catch(err){
    jslog('A JavaScript runtime error occurred: ' + err.message);
  }
3. console.log("This message was written by console.log().");




UI Policies:

UI Policy Actions:
Uses:
1. Make fields mandatory/visible/read-only

Go to UI policy -> enter the table name & short description -> save -> UI Policy Action related list appears


UI Policy Scripts: use the client-side API to execute script logic based on whether the UI Policy condition tests true or false.


UI Policy 1:
If _other_ option is selected in _What needed_ field, then make the OTHER field visible and mandatory.
Steps: Go to UI policy -> New -> Select table -> Add condition- if _what is_ field is _other_ -> save -> UI Action related List appears -> New -> Select field as _other_ -> Make visible as true -> save


UI Policy 2: Script
Go to advanced view related list -> In if true script -> g_form.showFieldMsg('u_other','Briefly explain what you need.','info');


## Server side scripting
1. Business rules
2. Script includes: classless, class, extend glideajax
3. API: glidesystem, gliderecord, glidedatetime


Uses:
1. update field values based on data received by querying db
2. determine if user has specific role
3. send email, generate and respond to events
4. date time
5. initiate integration and API calls
6. REST messages: send and retrieve results
<br>
#### Business rules
Business Rules are server-side logic that execute when database records are queried, updated, inserted, or deleted. 
BR executes: before, after, async, display
The current object's properties are all the fields for a record and all the GlideRecord methods.
The previous object's properties are also all fields from a record and the GlideRecord methods

Before BR: execute their logic before a database operation occurs. 
After BR: execute their logic just after a db opertaion and before the form is rendered to the user
Async BR: execute their logic on a different thread. Eg. Rest msgs
Disply BR: execute their logic when a form loads and a record is loaded from the database. The purpose is to populate the g_scratchpad object. 
This object is used to pass data to client side from the server side.

Business Rules often use the current and previous objects in their script logic.
The current object's properties are all the fields for a record and all the GlideRecord methods.
The previous object's properties are also all fields from a record and the GlideRecord methods. The property values are the values for the record fields when they were loaded from the database and before any changes were made.

Dot walking: 
Eg: current.u_requested_for.company.latitude
Here, the u_requested_for contains the sys_id of a record in u_requested_for table & by dot-waling, we can access the field values of that table

## server side api:

1. GlideSystem


