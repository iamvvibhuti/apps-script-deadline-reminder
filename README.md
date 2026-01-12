# Google Sheets Deadline Reminder

This repository contains a Google Apps Script that sends automated
email reminders based on deadlines stored in a Google Sheet.

The script is intended for simple task tracking use cases where
reminders need to be sent a fixed number of days before a target date.

---

## What the script does

On each run, the script:

1. Reads task data from a configured Google Sheet
2. Checks the target deadline date for each row
3. If the deadline is exactly **N days away**, an email reminder is sent
4. Supports:
   - To and CC recipients
   - Custom email subject
   - HTML formatted email body

Rows with missing or invalid data are skipped safely.

---

## Sheet structure

The script expects the following columns (starting from column A):

| Column | Field |
|------|------|
| A | Subject |
| B | Task details |
| C | Name |
| D | Manager |
| E | Target date |
| F | Recipient email |
| G | CC email (optional) |

Row 1 should contain headers.  
Data starts from row 2.

---

## Configuration

Update the values at the top of the script:

```js
const SPREADSHEET_ID = "YOUR_GOOGLE_SHEET_ID";
const SHEET_NAME = "Sheet1";
const REMINDER_DAYS_BEFORE = 7;
