![alt text: revature logo](images/revature_logo.PNG)
---

# QC Report Helper Read Me

---

## Overview

The QC Report Helper automation is here to help make the process of finishing and filling out information regarding QC and the different batches easier.  Not only will this robot make it easier to fill out information, it will also make it faster and much more organized when filling out said information.

This automation designed to store and implement data processing following the _Dispatcher-Peformer_ automation model. Stored data will be extracted from data given by Caliber and Survey Monkey. This data will automatically fill the required fields of a survey in MS Forms; excluding three required manual inputs. At its’ end, the automation will create a pdf file and send it in an email.

---

## MVP Features

- [x] Extract data from Caliber and populate that data into the MS Form Report
- [x] Extract data from the SurveyMonkey PowerBI Dashboard and populate that data into the MS Form Report
- [x] Identify concerning associates based on QC grade trends:
	* Rules: 1 = red, 2 = yellow, 3 = green, 4 = blue
	* Flags: Any reds, 2+ reds in a row, no greens, any certain grade that is below a benchmark for QC
	* Limitation: Each blue cancels out a red
- [x] Send automated emails upon completion of tasks with attached PDF copies of any report documents to the user
- [x] Should be able to allow user to enter notes of their own into the form after populating fields

### Stretch Goal Features

- Create a summary of survey feedback from the SurveyMonkey PowerBI Dashboard into the form

---

## Automation Inputs

- `in_auditor` (website input that selects desired information from automation)
- `in_email` (website input that that selects desired email to send PDF from automation)
- SurveyMonkey data (Excel workbook)
- Caliber (API)

---

## Automation Outputs

- `Report.Helper.Dispatcher` sends a Dictionary to Orchestrator Queue
- `Report.Helper.Performer` processes the queue items and enters the result into to MS Forms
- PDF Document is generated (saved printout of MS Forms output)

---

## UiPath Studio Project Dependencies (minimum package version requirements)

- UiPath.System.Activities
  - v21.10.2
- UiPath.UIAutomationActivities
  - v21.10.3
- UiPath.Mail.Activities
  - v1.12.3
- UiPath.Excel.Activities
  - v2.11.4
- UiPath.WebAPI.Activities 
  - v1.9.2