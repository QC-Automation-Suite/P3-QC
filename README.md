![alt text: revature logo](Documentation/images/revature_logo.PNG)
---

# QC Automation Suite Read Me

---

## Overview

The _QC Automation Suite_ currently provides 3 automations to be used internally at Revature to automate aspects of the QC (Quality Control) process. QC Audits and batch showcases have to be scheduled on a weekly basis, and must be scheduled with respect to certain rules (concerning Auditor availability and skill set). The _QC Scheduler Assistant_ automates this entire process. Similarly, on a weekly basis, QC Auditors must enter QC assessment data for their assigned batches into an MS Form. The _QC Report Helper_ automates most of this processes, efficiently managing the processing of Batch and QC assessment data from multiple sources. The P0 Analyzer automation assists the Auditor with the regular task of assessing projects of assigned batches. It checks for MVP requirement satisfaction, and looks for potential signs of plagarism in the projects. The suite also consists of a web app of the same name which serves as a visual interface for the QC user to easily run any of these automations with specified inputs.

---

## MVP Features

- [x] Single web page that functions as a visual interface allowing users to run the automations included in this suite.
- [x] Generate a PDF report and send it as an attachment via email, for each automation.

### QC Scheduler Assistant

- [x] Assign quality auditors to batches based on their skillsets.
- [x] Identify secondary auditors based on their skillsets. 
- [x] Track PTO Status of trainers and QC analysts.
- [x] Remove batches that are past their end date.

### QC Report Helper

- [x] Extract data from Caliber and populate that data into the MS Form Report
- [x] Extract data from the SurveyMonkey PowerBI Dashboard and populate that data into the MS Form Report
- [x] Identify concerning associates based on QC grade trends:
	* Rules: 1 = red, 2 = yellow, 3 = green, 4 = blue
	* Flags: Any reds, 2+ reds in a row, no greens, any certain grade that is below a benchmark for QC
	* Limitation: Each blue cancels out a red
- [x] Should be able to allow user to enter notes of their own into the form after populating fields

### P0 Analyzer

- [x] Take in Project 0 Specifications
	* Read from the GitHub URL to the _project requirements_ `ReadMe.md` file from GitHub for the desired batch's Project 0.
	* Example: `https://github.com/[ batch organization ]/trainer-code/wiki/P0-Requirements.md`.
- [x] Execute and process the output of `git diff` command for each distinct pair of Project 0 GitHub repositories in the batch.
- [x] Generate a ranked list comprising of each Project 0 repository in the batch with respect to the analysis.
- [x] Find the Mean Similarity (index) for the batch with respect to the `git diff` command outputs.

### Stretch Goal Features

- Automatically schedule showcase for batches. (__QC Scheduler Assistant__)
- Create a summary of survey feedback from the SurveyMonkey PowerBI Dashboard into the form. (__QC Report Helper__)

---

## Technology Stack

- UiPath (Studio, Orchestrator, Assistant, Apps)
- VB.NET
- Git
