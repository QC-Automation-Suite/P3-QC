![alt text: revature logo](images/revature_logo.PNG)
---

# QC Automation Suite - Startup

---
---

## Definitions

- _OrganizationA_: The UiPath Organization being used.
- _TenantA_: The Tenant in _OrganizationA_.

---
---

## Software & License Prerequisites

- Windows 10 with latest updates
- Microsoft Excel (Office 365 Subscription)
- Microsoft Word (Office 365 Subscription)
- UiPath Licesne (recommended: Enterprise License)
- UiPath Studio ([_download link_](https://docs.uipath.com/installation-and-upgrade/docs/studio-install-studio))
	* Must be connected to the _Production Orchestrator Tenant_ `TenantA` in the _UiPath Organization_ `OrganizationA`. [VERIFY]
		- __ie__ to the _Orchestrator URL_: `https://cloud.uipath.com/[ OrganizationA ]/[ TenantA ]/orchestrator_/`. [VERIFY]
- UiPath Assistant
	* Must be connected to the _Production Orchestrator Tenant_ `TenantA` in the _UiPath Organization_ `OrganizationA`. [VERIFY]
		- __ie__ to the _Orchestrator URL_: `https://cloud.uipath.com/[ OrganizationA ]/[ TenantA ]/orchestrator_/`. [VERIFY]
- Git ([_installation guide_](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git))
- Chrome Browser ([_download link_](https://www.google.com/chrome/))
- Uipath Chrome Browser Extension
	* UiPath Home -> Tools -> UiPath Extensions -> Chrome Extension
- Adobe Acrobat Reader DC ([_download link_](https://get.adobe.com/reader/))

---
---

## Clone the source Repository from GitHub

1. Open up Command Prompt and navigate to a desired local directory
2. In the Command Prompt, run:
3. git clone `https://github.com/QC-Automation-Suite/P3-QC`

_P3-QC Repo_ contains:
- `QcSchedulerAssistant` UiPath Studio Process.
- `Report.Helper.Dispatcher` UiPath Studio Process.
- `Report.Helper.Performer` UiPath Studio Process.
- `P0Analyzer` UiPath Studio Process.
- `QcAutomationSuite.uiapp` UiPath App from the folder: `./QcAutomationSuite/`.
- `QCSchedule.xlsx` Excel file in the folder: `./Data/QcSchedulerAssistant/`
- `Dummy QC Schedule Copy.xlsx` Excel file in the folder: `./Data/QcReportHelper/`
- `Dummy_Survey_Responses.xlsx` Excel file in the folder: `./Data/QcReportHelper/`

---
---

## Automation and UiPath App Setup in UiPath Organization (cloud.uipath.com)

There are 3 Automations to deploy to _TenantA_:
1. _QC Scheduler Assistant_, consisting of the Process: `QcSchedulerAssistant`.
2. _QC Report Helper_, consisting of the Processes: `Report.Helper.Dispatcher`, and `Report.Helper.Performer`.
3. _P0 Analyzer_, consisting of the Process: `P0Analyzer`.

---

### Publish the Processes to Tenant

1. Publish each of the 4 Processes to `TenantA` from UiPath Studio as Packages.
2. Verify that the 4 Processes have been published to `TenantA` as 4 Packages of the same name.
3. Add the 4 Processes appropriately into an appropriate Folder-structure.

> __Recommended Folder-structure  to house the Processes for the 3 Automations (all at Tenant-level)__:
> - `QcSchedulerAssistant` Folder to house the Process: `QcSchedulerAssistant`.
> - `QcReportHelper` Folder to house the Processes: `Report.Helper.Dispatcher`, and `Report.Helper.Performer`.
> - `P0Analyzer` Folder to house the Process: `P0Analyzer`.

---

### Configure the Folder-structure and Folder resources in `TenantA`

Each of the 3 Automations makes use of the following Folder resources:
1. __QC Scheduler Assistant__:
	- `Schedules` : Storage Bucket
		* Configuration: Upload the file `QCSchedule.xlsx` from the cloned `P3-QC` Repo folder `./Data/QcSchedulerAssistant`.
    	- `Gmail` : Asset of _Credential_ type
        	* Configuration: Enter the email id and password of the __Gmail__ account that is to be used to send a copy of the final PDF report generated by this automation.
2. __QC Report Helper__:
	- `Excel Sheets` : Storage Bucket
		* Configuration: Upload the following files from the cloned `P3-QC` Repo folder `./Data/QcReportHelper/`:
			- `Dummy QC Schedule Copy.xlsx`
			- `Dummy_Survey_Responses.xlsx`
	- `JustAQueue` : Queue
		* Configuration:
			- Select `Yes` for the property: `Auto Retry`.
			- Enter `1` for the property: _Max # of retries_.
	- `Report Helper Queue Trigger` : Trigger of _Queue_ type.
		* Configuration:
			- Select the `Report.Helper.Performer` Process for the _Process Name_ property.
			- Set the _Job priority_ property to `Inherited`.
			- Set the _Runtime license_ property to `Production (Unattended)`.
			- Select the Queue `JustAQueue` into the _Queue_ property.
			- Enter `1` for the property: _Minimum number of items to trigger the first job_.
			- Enter `10` for the property: _Maximum number of pending and running jobs allowed simultaneously_.
			- Enter `1` for the property: _Another job is triggered for each x new item(s)_.
			- In the _Execution Target_ property-section:
				* Select the appropriate Tenant User `Account` and `Machine` that have been allocated an _Unattended License_.
3. __P0 Analyzer__:
	- `Repos` : Storage Bucket
	- `Reports` : Storage Bucket
	- `OutputFiles` : StorageBucket
	- `EmailCred` : Asset of _Credential_ type.
		* Configuration: Enter the email id and password of the __Gmail__ account that is to be used to send a copy of the final PDF report generated by this automation.

> __Recommended setup for these resources in the Folder-structure__:
> - Put the above-mentioned resources for the _QC Scheduler Assistant_ automation(s) into the `QcSchedulerAssistant` Folder:
> - Put the above-mentioned resources for the _QC Report Helper_ automation(s) into the `QcReportHelper` Folder:
> - Put the above-mentioned resources for the _P0 Analyzer_ automation(s) into the `P0Analyzer` Folder:

__NOTE__ (_regarding sender email account(s)_): The Gmail account(s) used for the above Assets (`Gmail` and `EmailCred`) must have the Security setting _Less secure app access_ enables. In order to enable this setting:
1. Sign in to `https://myaccount.google.com/` with the account.
2. Navigate to the `Security` tab.
3. Locate the _Less secure app access_ box.
4. Click `Turn on access`.

---

### Configure the `QC Automation Suite` UiPath App into the _OrganizationA_

1. Go to `Apps` in the cloud.uipath.com when logged into _OrganizationA_
2. Import the UiPath App (`QCAutomationSuite.uiapp`) from the cloned _P3-QC_ folder `./QcAutomationSuiteWebApp/`:
	- Click `Build a new app`.
	- Give the `Name`: `QC Automation Suite`.
	- Click `Import from file`.
	- Navigate to `.uiapp` file : ("QCAutomationSuite.uiapp").
	- Click `Create`.
3. Configure Orchestrator Dependencies:
	- In the Left Panel of the UiPath App Studio:
		* Navigate to and expand the expandable menu item `Orchestrator`. It should have the Symbol: `!` in a Red Circle, next to it.
		* There should be four subitems, each corresponding to one of the 4 Processes, each with the same Symbol next to it: `!` in a Red Circle, next to it.
		* For each, right click on it.
			- Click `Replace`.
			- A Window pops up with two vertical panels: _Processes_ and _Process Details_.
		* The _Processes_ panel displays all of the Folders in _TenantA_ as expandable menu items, with their respective Processes as sub items.
		* Locate and select the Matching Process in the appropriate Folder.
		* Click `Replace`.
4. Publish the App: In the top right corner, click `Publish`.

---
---

## Using the QcAutomationSuite _UiPath App_

---

### Run the QcAutomationSuite _UiPath App_ from _UiPath Assistant_

- Run from UiPath Assistant:
	1. Open UiPath Assitant.
	2. In the `Home` tab, navigate to the expandable section: `Apps`.
	3. In this section, locate the App with the name `QC Automation Suite`.
	4. Click `Run`. (_play_ button)
- Run from UiPath Cloud:
	1. Sign in to URL: `https://cloud.uipath.com/[ OrganizationA ]/apps_/[ TenantA ]`
		* must be signed in as a User with access to _TenantA_ with atleast `Automation User` permission granted for the web app.
	2. Locate the App with the name `QC Automation Suite`.
	3. Click `Run`. (_play_ button)

---
### Entering Inputs into the Web App

Each of the automations require certain inputs to be entered by the user. These are:
- __Auditor Email__: This is the email address to which the PDF reports generated by any of the 3 automations is to be sent.
	* It is to be entered in an _input_ element of _text_ type (_textbox_).
	* Must be provided before the execution of the `QC Report Helper`, `QC Scheduler`, and `P0 Analyzer` automations.
- __Auditor Name__: The name of the Auditor (user) executing the automations from this web app.
	* It is to be selected in an _input_ element of _select_ type (_single selection dropdown_).
	* Must be selected before the execution of the `QC Report Helper` and `QC Scheduler` automations.
- __Project 0 Requirements URL__: This is the GitHub URL to the `ReadMe.md` file with project requirements in the _Project 0_ for which student repository submissions are to be analyzed (assisted by `P0Analyzer`).
	* Example: `https://github.com/[ batch organization ]/trainer-code/wiki/P0-Requirements.md`.
	* This _ReadMe.md_ must contain a _MVP Features_ section.
	* It is to be entered in an _input_ element of _text_ type (_textbox_).
	* Must be provided before the execution of the `P0 Analyzer` automation.
- __Repos List__: This is a `.xlsx` file containing a table of all the Associate GitHub repositories to be analyzed.
	* The file must consist of a single _sheet_ containing a single table with the columns: `Name`, `UserName`, and `Repo URL`.
	* It is to be uploaded using a _File Selecter_ element, followed by clicking the button `Upload to Storage Bucket`.
	* Must be provided before the execution of the `P0 Analyzer` automation.

---

### Executing the Automations

The execution of each of these 3 automations (`P0 Analyzer`, `QC Report Helper`, `QC Scheduler`) is mapped to a _Button_ element. Clicking a button results in the execution of the corresponding automation via UiPath Assistant (in tandem with Orchestrator). The status of the execution is displayed at the bottom of the page.

---
---


## Known Bugs or Limitations:

- __General__:
	* A better implementation of automated email sending is needed. The current iteration makes use of Gmail account(s) with the _Less secure app access_ Security setting enabled. Despite this, certain security features of Gmail may prevent execution of this automated email feature from multiple machines.
- __QC Scheduler Assistant__:
	* The step at the end of the automation where a Microsoft Word document is opened (and used to generate a PDF) must be followed by procedure that closes the Microsoft Word document.
	* Hard-coded values.
- __QC Report Helper__:
	* Restricted portability as a result of certain kinds of Web Browser automation _Selectors_.
	* Hard-coded values.
- __P0 Analyzer__:
	* The User executing this automation must not interact with the GUI of their machine while the automation is running. This is because the automation makes use of _Hardware Event_ type UI automation.
	* Hard-coded values.