**Google Sheet Task Reminder Script**

This Google Apps Script sends email reminders based on target dates listed in a Google Sheet. It checks daily (when triggered) for tasks due a specific number of days in the future and sends a formatted HTML email to recipients and CC addresses listed in the sheet.

**Functions**

Reads task data from a specified Google Sheet.
Sends reminders a configurable number of days before the target date.
Includes task details, assigned person, manager, and due date in the email.
Sends emails to specified recipients and CCs.
Uses HTML formatting for emails.

**Setup**

1.  Create Google Sheet: Create a Google Sheet to hold the task data. Ensure it has the columns specified below. Get the Sheet ID from its URL (the long string between `/d/` and `/edit`).
2.  Create Apps Script: Create a new Apps Script project (e.g., via script.google.com or from within the Sheet via Extensions > Apps Script).
3.  Copy Code: Copy the code from `Code.gs` (or your `.gs` file name) in this repository into your Apps Script project's editor.
4.  Copy Manifest: Copy the content of `appsscript.json` from this repository into your Apps Script project's `appsscript.json` file (ensure it's visible via Project Settings).
5.  Configure Script Variables: Update the configuration section at the top of the script file (`Code.gs`):
    `SPREADSHEET_ID`: Replace `"YOUR_SPREADSHEET_ID_HERE"` with the actual ID of your Google Sheet.
    `SHEET_NAME`: Ensure this matches the exact name of the sheet tab containing the data (e.g., `"Sheet1"`).
    Verify `"_COL_INDEX` variables match your sheet columns (A=0, B=1, etc.).
    Set `REMINDER_DAYS_BEFORE` to how many days before the deadline the reminder should be sent.
6.  Grant Permissions: Run the `sendDeadlineRemindersUpdated` function once manually from the editor. You will be prompted to grant the necessary permissions (accessing Sheets, sending email). Review and allow them.
   
**Set Up Trigger**
   
    In the Apps Script editor, click the "Triggers" icon (alarm clock on the left).
    Click ""+ Add Trigger".
    Configure the trigger:
        Choose which function to run: `sendDeadlineRemindersUpdated`
        Choose which deployment should run: `Head`
        Select event source: `Time-driven`
        Select type of time based trigger: `Daily timer`
        Select time of day: Choose a suitable time (e.g., `8am to 9am`).
        Error notification settings: Choose how often you want to be notified if the script fails.
9. Click "Save". You might be asked to authorize again.

**Google Sheet Structure**

The script expects the Google Sheet specified by `SHEET_NAME` to have the following columns (order matters for the default `_COL_INDEX` values):

1. ""Column A:"" Subject (Email subject line part)
2. ""Column B:"" Task (Task details, can include line breaks)
3. ""Column C:"" Name (Name of the person assigned the task)
4. ""Column D:"" Manager name (Name of the manager)
5. ""Column E:"" Target date (The deadline date - Must be formatted as a valid date recognized by Google Sheets)
6. ""Column F:"" Recipient (Email address(es) for the 'To' field, comma-separated if multiple)
7. ""Column G:"" Cc (Email address(es) for the 'Cc' field, comma-separated if multiple)

Data should start from row 2 (assuming row 1 has headers).
