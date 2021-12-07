# P0Analyzer-startup
## Startup Installation
1. Make sure you are running Windows 10 with the latest updates installed
2. Install Microsoft Excel (Office 365 Subscription)
3. Install Microsoft Word (Office 365 Subscription)
4. Install UiPath Studio and UiPath Assistant
   - https://docs.uipath.com/installation-and-upgrade/docs/studio-install-studio
5. Install Git
   - https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
6. Install Chrome Browser
   - https://www.google.com/chrome/
7. Install Uipath Chrome Browser Extension
   - UiPath Home -> Tools -> UiPath Extensions -> Chrome Extension
8. Install Adobe Acrobat Reader DC
   - https://get.adobe.com/reader/

## Git Clone Source Repository
1. Open up Command Prompt and navigate to a desired directory
   In the Command Prompt, run:
   - git clone https://github.com/QC-Automation-Suite/P3-p0analyzer

## General UiPath Orchestrator Instructions
1. Login to UiPath studio inside the application.
2. Create a machine key/add a machine
   1. Login to Orchestrator
   2. Click Orchestrator in the left navbar
   3. Click Tenant in the top left
   4. Click "Add machine" -> Machine Template
   5. Add a Template Name
   6. Click provision
3. Copy your machine key
   1. Login to Orchestrator
   2. Click Orchestrator in the left navbar
   3. Click Tenant in the top left
   4. Click Machines
   5. Click on the three dots on the right and click edit machine
   6. Copy and paste your machine key to a temporary document
4. Copy your orchestrator URL
   1. Login to Orchestrator
   2. Click Orchestrator in the left navbar
   3. Copy the URL up until /orchestrator_/ to a temporary document:
       - Example:https://cloud.uipath.com//DefaultTenant/orchestrator_/
   4. Launch UiPath Assistant
      1. Click on your initials
      2. Click Sign In
      3. Click More Options
      4. Enter in your Orchestrator URL from #11.
      5. Enter in your Machine Key from #10.

## P0Analyzer Orchestrator Specific Instructions
1. Login to Orchestrator
   1. Click on Orchestrator in the Top Left
   2. Click on Storage Buckets in the navbar on the top right
   3. Create a folder/storage bucket named "OutputFiles"
   4. Create a folder/storage bucket named "Repos"
   5. Create a folder/storage bucket named “Reports”
   6. Upload your "P0TesterAnalyzer.xlsx" file to the Repos folder/storage bucket in Orchestrator
      1. Login to Orchestrator
      2. Click on Orchestrator in the Top Left
      3. Click on Storage Buckets in the navbar on the top right
      4. Click Repos
      5. Click Upload New File and upload your "P0TesterAnalyzer.xlsx".

## Publishing the P0Anazlyer to Orchestrator