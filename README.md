


[![PR](https://img.shields.io/badge/PRs-Welcome-<COLOR>.svg)][pullreq-url]
[![Maintenance](https://img.shields.io/badge/Maintained%3F-Yes-<COLOR>.svg)](https://github.com/telcn/TrackerFlow)
[![Made with](https://img.shields.io/badge/Made%20with-Javascript-<COLOR>.svg)](https://www.javascript.com/)
[![License](https://img.shields.io/badge/Licence-MIT-blue.svg)](https://github.com/telcn/ChocolateBucket/blob/master/LICENSE)

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://github.com/telcn/TrackerFlow">
    <img src="https://i.imgur.com/x27u2ty.png" alt="Logo" width="280" height="180">
    
  </a>

  <h3 align="center">TrackerFlow</h3>

  <p align="center">
    Manage and track requests for different corporate services
    <br />
    <a href="https://teamtelstra-my.sharepoint.com/:v:/g/personal/niyati_datta_team_telstra_com/EXkmYtaFlrJKrEUsJqkEINIBPbIuCrZrGjoi1tHHZrk1Nw?e=kUuglM"><strong></strong></a>
    <br />
    <br />
    <a href="https://github.com/othneildrew/Best-README-Template"></a>
    ·
    <a href="https://github.com/telcn/TrackerFlow/issues">Report Bug</a>
    ·
    <a href="https://github.com/telcn/TrackerFlow/issues">Request Feature</a>
  </p>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#roadmap">Roadmap</a></li>
    <li><a href="#contributing">Contributing</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
    <li><a href="#acknowledgments">Acknowledgments</a></li>
  </ol>
</details>



<!-- ABOUT THE PROJECT -->
## About The Project


The TrackerFlow application will manage and track requests for Human Resources, Legal, Facilities, and one other department (to be determined) in an automated way, using an easy-to-use user interface that is consistent with other corporate software




### Built With

* [JavaScript](https://www.javascript.com/)
* [ServiceNow](https://www.servicenow.com/)


<p align="right">(<a href="#top">back to top</a>)</p>



<!-- GETTING STARTED -->
## Getting Started
Follow the below instructions for setting the project up on your local machine.

### Prerequisites

Get a ServiceNow personal developer instance [here](https://developer.servicenow.com/dev.do#!/home)

### Installation

1. Clone the repo
   ```sh
   git clone https://github.com/telcn/TrackerFlow.git
   ```
2. Import the repo in ServiceNow Studio

<p align="right">(<a href="#top">back to top</a>)</p>



## Usage

**Table Name**: x_705716_trackerfl_trackerflow </br>
**Config**: x_705716_trackerfl_trackerflow.CONFIG </br>
**List**: x_705716_trackerfl_trackerflow.LIST</br>
**Users**: Beth Anglin and Zane Sulikowski</br>
**Admin**:  Fred Luddy and Alissa Mountjoy </br>
</br>

**Client Scripts: **

There are two main client scripts:
1. Name: **TrackerFlow Request Type Options**
Used: g_form.getValue(), g_form.clearOptions(), g_form.setValue(), g_form.isNewRecord()
Description: Change the "What needed" field based on  "Request type" field. Eg: If what needed is Legal, then change the Request type choices as Legal
2. Name: **TrackerFlow Set Requested for**
Used:  g_form.setValue(), g_form.isNewRecord()
Description: Set the Requested for to the currently logged in user for new records. Users can change the field value



**Business Rules:**
1. Name: **NeedIt When needed field date**
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

</br>

**Script Includes**
</br>
1. Name:  **validateEmailAddress**
Used: string.match() function to match 2 strings
Description: On demand Script Include to validate email address syntax using regular expressions.
Name of the Business Rule used to call the script include: Email Address Syntax Validate
Used: validateEmailAddress(current.u_requested_for_email) to call the script include, gs.addErrorMessage()

</br>
2. Script include for extending a class 
Although most ServiceNow classes are extensible, the most commonly extended classes are: GlideAjax, LDAPUtils & Catalog
Eg. Extending the Glide Ajax class using script include
Name of the script include:  **GetEmailAddress**
Used: GlideRecord("sys_user"), userRecord.get(this.getParameter('sysparm_userID'))</br>
Description: The GlideAjax class is used by client-side scripts to send data to and receive data from the ServiceNow server. The client-side script passes parameters to the Script Include. The Script Include returns data as XML or a JSON object. The client-side script parses data from the response and can use the data in subsequent script logic.</br>
Requested for is a referenced field i.e a record from the users table. In the form, we want to populate the email field (on clie</br>nt side) using info
from the users table (email field). This data transfer from server to client side is done by using the glide ajax class. 
The script include is client callable. We call the script include on the client side to get email of the user from the server side to client side.

Name of Client Script used to call the GetEmailAddress script include: TrackerFlow Populate Email Field
Used: Created an object of GlideAjax('GetEmailAddress'), getEmailAddr.addParam('sysparm_name','getEmail'),  getEmailAddr.getXML() to send 
request to the server

3. Utilities script include</br>
Most applications store util functions in a utilities script include. Call them on server side. The SI is a class with different methods.
To use them, create an object of the SI class in the server script and call the required method.
Name of SI: **TrackerFlowUtils**</br>


Other Server side scripts:</br>
Script Action: respond to events</br>
Scheduled Script Execution: Run a report and email it</br>
UI Actions: Adding buttons</br>
Notification Email Script</br>
Scripted REST APIs: Request sent or received through web services</br>
UI Page Processing Script: Script that is executed once UI page is submitted</br>
Transform Map Script: Data import</br>
</br>

**Flow designer**

Name: **TrackerFlow Fulfillment** </br>
Description: Steps to approve and fulfill Trackerflow requests. Trigger is record created. </br>
Action 1: Update the state field to Approval awaiting and the assigned to field to Beth anglin.</br>
Action 2: Ask for approval. Manager: Fred Luddy. Employee: Adela Cervantsz.</br>






<!-- ROADMAP -->
## Roadmap

- [x] Client Scripts
- [x] Server Scripts
- [x] Roles and Groups
- [x] Scheduled Jobs
- [x] Events
- [x] Flow 
    
    

See the [open issues](https://github.com/telcn/TrackerFlow/issues) for a full list of proposed features (and known issues).

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- CONTRIBUTING -->
## Contributing

Any contributions you make are **greatly appreciated**.

If you have a suggestion that would make this better, please fork the repo and create a pull request. You can also simply open an issue with the tag "enhancement".

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- LICENSE -->
## License

Distributed under the MIT License. See `LICENSE` for more information.

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- CONTACT -->
## Contact

Chinmay Nehate 

Project Link: [https://github.com/telcn/TrackerFlow](https://github.com/telcn/TrackerFlow)

<p align="right">(<a href="#top">back to top</a>)</p>



<!-- ACKNOWLEDGMENTS -->
## Acknowledgments



* [ServiceNow Docs](https://docs.servicenow.com/)
* [Basico](https://www.basicoservicenowlearning.in/)
* [Saas with ServiceNow](https://www.saaswithservicenow.in/)


<p align="right">(<a href="#top">back to top</a>)</p>



<!-- MARKDOWN LINKS & IMAGES -->

[pullreq-url]: https://github.com/telcn/TrackerFlow/pulls

