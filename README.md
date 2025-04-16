**Google Sheet Task Deadline Reminder Scripts**

This Google Sheets script does an automation reminder on Google Sheet, taking reminders that are a specific number of days away from a given target date in the sheet. It checks daily (when triggered) and sends a properly formatted HTML email, texts, and other necessary communications to the listed recipients and CC IDs in the Google sheet.

**Capabilities**

Reads from a defined Google Sheet containing a list of specified tasks.
Sends reminders a configurable number of days before the target date.
Includes task summary, assigned personnel, manager, and due date within the email.
Sends emails to defined users and CCs.
Uses HTML formatting for emails.

**Integration Instructions**

1.	Build Google Sheet: Start by creating a Google sheet that will have task data, making sure it contains the columns detailed below. The sheet ID that can be extracted from its URL (the long string lying between /d/ and /edit).
2.	Build Apps Script: Either go to script.google.com or go to the sheet and click on Extensions > Apps Script > Create a new Apps Script project.
3.	Copy the Code: Go to your Apps script project editor, and copy the link Code.gs (or your .gs file name) from this repository.
4.	Copy the Manifest: Ensure that appsscript.json is visible in project settings and copy the content of this repository into the appsscript.json file that will go to the project.
5.	Set Up Script Variables: Edit the configuration portion at the beginning of the script file (Code.gs):
“SPREADSHEET_ID: Substitute accordingly with the correct value for your Google Sheet: “YOURSPREADSHEETID_HERE”
“SHEET_NAME: Verify, this is exactly how the name of your data tab sheet in the workbook looks like (ex. “Sheet1”)
Check that "_COL_INDEX variable corresponds to the columns in your sheet (A=0, B=1 …).
Adjust REMINDER_DAYS_BEFORE to the amount of days pre-deadline reminder should be sent.
6.	Grant Permissions: Manually execute from the editor the function sendDeadlineRemindersUpdated once. You will need to review permissions first (Sheet access, Email access). Accept them after reviewing.

**Set Up Trigger**

In the Apps Script console, hit the "Triggers" tab (clock icon on the left).
Hit “+ Add Trigger”.
Set the following for the trigger:
Select which function to execute: sendDeadlineRemindersUpdated
Select which deployment should run: Head
Select event source: A new dropdown - Choose from “Time driven”
Select type of time based trigger: Daily timer
Select time of day: Set a desired time like 8am to 9am
Pick time of day: For when Daily timer event will occur
Error notification settings: Determine how you want to be notified when the script fails and possibly spam’d you.
7.	Click “Save”. Depending on user settings you may need to approve again.

**Structure of Google Sheet**

The script anticipates that the Google Sheet denoted by SHEET_NAME will at least include the following columns (the order matters for the default _COL_INDEX values):
1.	”Column A:” Subject (A part of Email subject line)
2.	”Column B:” Task (Details of the task. Possible multiline)
3.	”Column C:” Name (The name of the assignee)
4.	”Column D:” Manager name (The name of the manager)
5.	”Column E:” Target date (set a goal for the date – Must be in a format recognized by Google Sheets as a date)
6.	”Column F:” Recipient (Email address(es) for the ‘To’ field. Multiple addresses should be comma separated)
7.	”Column G:” Cc (Email address(es) for the ‘Cc’ field. Multiple addresses should be comma separated)

   
Data should be entered starting from row 2 (assume row 1 contains the header).
