// --- Configuration Section ---
// --- MODIFY VALUES BELOW ---

// TODO: Replace with the actual ID of your Google Spreadsheet
const SPREADSHEET_ID = "Replace Google Sheet ID here";

// TODO: Replace with the exact name of the sheet tab containing the data
const SHEET_NAME = "Sheet1"; // Adjust if your sheet tab has a different name

// Column indices based on your sheet (A=0, B=1, C=2, etc.)
// --- Verify these match your actual sheet ---
const SUBJECT_COL_INDEX = 0;      // Column A: Subject
const TASK_COL_INDEX = 1;         // Column B: Task
const NAME_COL_INDEX = 2;         // Column C: Name
const MANAGER_COL_INDEX = 3;      // Column D: Manager name
const TARGET_DATE_COL_INDEX = 4;  // Column E: Target date
const RECIPIENT_COL_INDEX = 5;    // Column F: Recipient
const CC_COL_INDEX = 6;           // Column G: Cc

// Number of days before the deadline to send the reminder
const REMINDER_DAYS_BEFORE = 7;

// --- END OF CONFIGURATION ---


// Sends deadline reminder emails based on dates in the configured Google Sheet.
function sendDeadlineRemindersUpdated() {
  // Calculate the target date for sending reminders
  const today = new Date();
  today.setHours(0, 0, 0, 0); // Normalize today's date to midnight

  const reminderDate = new Date(today);
  reminderDate.setDate(today.getDate() + REMINDER_DAYS_BEFORE);

  const formattedReminderDate = Utilities.formatDate(reminderDate, Session.getScriptTimeZone(), "yyyy-MM-dd");
  Logger.log(`Checking for reminders due on: ${formattedReminderDate}`);

  let sheet;
  try {
    const ss = SpreadsheetApp.openById(SPREADSHEET_ID);
    sheet = ss.getSheetByName(SHEET_NAME);
    if (!sheet) {
      throw new Error(`Sheet "${SHEET_NAME}" not found.`);
    }
  } catch (e) {
    Logger.log(`ERROR accessing sheet: ${e}`);
    return; // Stop execution
  }

  // Data starts from row 2 (assuming row 1 has headers)
  const startRow = 2;
  if (sheet.getLastRow() < startRow) {
    Logger.log(`No data found in sheet "${SHEET_NAME}".`);
    return;
  }
  const dataRange = sheet.getRange(startRow, 1, sheet.getLastRow() - startRow + 1, sheet.getLastColumn());
  const data = dataRange.getValues();

  let emailsSentCount = 0;

  // Process each row from the sheet
  data.forEach(function(row, index) {
    const currentRowInSheet = startRow + index;

    // Extract data using column indices
    const subjectLine = row[SUBJECT_COL_INDEX];
    const task = row[TASK_COL_INDEX];
    const name = row[NAME_COL_INDEX];
    const managerName = row[MANAGER_COL_INDEX];
    const targetDateValue = row[TARGET_DATE_COL_INDEX];
    const recipient = row[RECIPIENT_COL_INDEX];
    const ccRecipient = row[CC_COL_INDEX];

    // Basic validation
    if (!recipient || !targetDateValue || recipient.toString().trim() === '' || !(targetDateValue instanceof Date)) {
       if (!(targetDateValue instanceof Date) && targetDateValue !== '') {
         Logger.log(`Row ${currentRowInSheet}: Skipping - Invalid date value: ${targetDateValue}`);
      }
      return; // Skip row
    }

    try {
      // Normalize the target date to midnight
      const targetDate = new Date(targetDateValue);
      targetDate.setHours(0, 0, 0, 0);

      // Compare normalized dates
      if (targetDate.getTime() === reminderDate.getTime()) {
        // --- Match found! Prepare and send email ---
        const formattedDueDate = Utilities.formatDate(targetDate, Session.getScriptTimeZone(), "MMMM dd, yyyy");
        Logger.log(`Row ${currentRowInSheet}: Match found for ${formattedDueDate}. Sending email.`);

        const emailSubject = `Reminder: ${subjectLine || task} - Due in ${REMINDER_DAYS_BEFORE} days (${formattedDueDate})`;
        const emailBody = buildHtmlEmailBodyUpdated_NoColor(subjectLine, task, targetDate, name, managerName);

        const mailOptions = {
          to: recipient.toString().trim(),
          subject: emailSubject,
          htmlBody: emailBody
        };

        if (ccRecipient && ccRecipient.toString().trim() !== '') {
          mailOptions.cc = ccRecipient.toString().trim();
        }

        MailApp.sendEmail(mailOptions);
        emailsSentCount++;
        Logger.log(`--> Email SENT to ${mailOptions.to} ${mailOptions.cc ? '(CC: ' + mailOptions.cc + ')' : ''}`);

      } // End date match check

    } catch (e) {
      Logger.log(`Row ${currentRowInSheet}: ERROR processing row. Task: "${task || 'N/A'}". Error: ${e}`);
    }
  }); // End data.forEach

  Logger.log(`Script finished. Sent ${emailsSentCount} reminder emails.`);
}


// Builds the HTML email body (No color styles)
function buildHtmlEmailBodyUpdated_NoColor(subjectLine, task, targetDate, name, managerName) {
  const formattedDueDate = Utilities.formatDate(targetDate, Session.getScriptTimeZone(), "MMMM dd, yyyy");

  // HTML email body template - COLOR STYLES REMOVED
  let html = `
    <html>
      <body style="font-family: Arial, sans-serif; line-height: 1.6;">
        <h2>Task Reminder</h2>
        <p>This is an automated reminder for the following task:</p>
        <table style="border-collapse: collapse; width: 100%; border: 1px solid #ddd; margin-bottom: 15px;">
          <tr>
            <td style="padding: 8px; border: 1px solid #ddd; width: 120px;"><strong>Subject:</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">${subjectLine || "(No Subject Provided)"}</td>
          </tr>
          <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Assigned To:</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">${name || "(No Name Provided)"}</td>
          </tr>
          <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Manager:</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">${managerName || "(No Manager Provided)"}</td>
          </tr>
          <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Task Details:</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;">${task ? task.replace(/\n/g, '<br>') : "(No Task Details Provided)"}</td>
          </tr>
          <tr>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong>Target Deadline:</strong></td>
            <td style="padding: 8px; border: 1px solid #ddd;"><strong><span>${formattedDueDate}</span></strong> (Due in ${REMINDER_DAYS_BEFORE} days)</td>
          </tr>
        </table>
        <p>Please ensure this task is prioritized and completed before the target date.</p>
        <br>
        <p style="font-size: 0.9em;"><em>This is a test email sent by Vivek Vibhuti. Please do not reply directly.</em></p>
      </body>
    </html>`;
  return html;
}
