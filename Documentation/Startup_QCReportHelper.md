![alt text: revature logo](images/revature_logo.PNG)
---

# QC Report Helper Startup

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
- UiPath License (recommended: Enterprise License)
- UiPath Studio (minimum version: v2021.10.3) ([_download link_](https://docs.uipath.com/installation-and-upgrade/docs/studio-install-studio))
	* Must be connected to the _Production Orchestrator Tenant_ `TenantA` in the _UiPath Organization_ `OrganizationA`. [VERIFY]
		- __ie__ to the _Orchestrator URL_: `https://cloud.uipath.com/[ OrganizationA ]/[ TenantA ]/orchestrator_/`. [VERIFY]
- UiPath Assistant
	* Must be connected to the _Production Orchestrator Tenant_ `TenantA` in the _UiPath Organization_ `OrganizationA`. [VERIFY]
		- __ie__ to the _Orchestrator URL_: `https://cloud.uipath.com/[ OrganizationA ]/[ TenantA ]/orchestrator_/`. [VERIFY]
- Adobe Acrobat Reader DC ([_download link_](https://get.adobe.com/reader/))

---
---

## Clone the source Repository from GitHub

1. Open up Command Prompt and navigate to a desired local directory
2. In the Command Prompt, run:
3. git clone git clone `https://github.com/QC-Automation-Suite/P3-QC`

_P3-QC Repo_ contains:
- `Report.Helper.Dispatcher` UiPath Studio Process.
- `Report.Helper.Performer` UiPath Studio Process.
- `QcAutomationSuite.uiapp` UiPath App from the folder: `./QcAutomationSuite/`.
- `Dummy QC Schedule Copy.xlsx` Excel file in the folder: `./Data/QcReportHelper/`
- `Dummy_Survey_Responses.xlsx` Excel file in the folder: `./Data/QcReportHelper/`

---
---

## Automation and UiPath App Setup in UiPath Organization (cloud.uipath.com)

---

### Publish the Processe to Tenant

1. Publish the `Report.Helper.Dispatcher` and `Report.Helper.Performer` to `TenantA` from UiPath Studio as  Packages.
2. Verify that the Processes have been published to `TenantA` as a Packages of the same name.
3. Add the Processes into an appropriate Folder.

> __Recommended Folder to house the Processes (Tenant-level)__:
> `QcReportHelper` Folder to house the Process: `Report.Helper.Dispatcher` and `Report.Helper.Performer`.

---

### Configure the Folder resources in `TenantA`

The automation makes use of the following Folder resources:
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

> __Recommended Folder for these resources__:
> Put the above-mentioned resources for the _QC Report Helper_ automation into the `QcReportHelper` Folder.

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
		* Identify each the two Processes: `Report.Helper.Dispatcher` and `Report.Helper.Performer`. For each of these:
			- Click `Replace`.
			- A Window pops up with two vertical panels: _Processes_ and _Process Details_.
		* The _Processes_ panel displays all of the Folders in _TenantA_ as expandable menu items, with their respective Processes as sub items.
		* Locate and select the matching Process in the appropriate Folder.
		* Click `Replace`.
4. Publish the App: In the top right corner, click `Publish`.

---
---

## Using the QcAutomationSuite UiPath App

---

### Run the QcAutomationSuite UiPath App from UiPath Assistant

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

### Executing the Automation from the Web App

The automation requires certain inputs to be entered by the user. These are:
- __Auditor Email__: This is the email address to which the PDF reports generated by any of the 3 automations is to be sent.
	* It is to be entered in an _input_ element of _text_ type (_textbox_).
	* Must be provided before the execution of this automation.
- __Auditor Name__: The name of the Auditor (user) executing the automations from this web app.
	* It is to be selected in an _input_ element of _select_ type (_single selection dropdown_).
	* Must be selected before the execution of this automation.

The execution of this automation is (_QC Report Helper_) is mapped to a _Button_ element. Clicking a button results in the execution of the corresponding automation via UiPath Assistant (in tandem with Orchestrator). The status of the execution is displayed at the bottom of the page.

---
---

## Known Bugs or Limitations:

- Restricted portability as a result of certain kinds of Web Browser automation _Selectors_.
- Hard-coded values.