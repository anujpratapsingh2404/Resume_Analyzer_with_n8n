🚀 Workflow Overview

The entire process is triggered by a form submission and is broken down into four main phases:

Phase 1: Pre-Screening & Data Acquisition
 1. On Form Submission: Triggers the workflow and captures applicant data (Name, Email, Resume file).
 2. Duplicate Check: Uses the Google Sheets and Function nodes to search the database for applications submitted within the last 6 months.
 3. Conditional Routing: A Switch node terminates the workflow and sends a "Applied Recently" email if a recent duplicate is found.
 4. File Handling: The resume is uploaded to Google Drive and the JD is downloaded and extracted.

Phase 2: AI Analysis (Core Logic)
 1. AI Agent (Powered by Gemini API): Receives the text of the Resume and the Job Description.
 2. Structured Output Parser: Crucially, this node forces the AI's response into a strict JSON object, ensuring fields like overall_fit_score are present and correct.

Phase 3: Decision & Communication
 1. Route by Score (Switch): Checks the overall_fit_score output by the AI Agent.
 2. Send Success Email: Sends a congratulatory email to candidates scoring high (e.g., $\geq 70$).
 3. Send Rejection Email: Sends a regret email to candidates scoring low.

Phase 4: Logging and Database
 1. Database Sheet: Logs the full details of every application (including the final AI score and decision) into the main Google Sheet.
 2. Shortlist Candidates: Logs only the high-scoring candidates to a separate, dedicated Google Sheet for HR review.


 🔑 Authentication Requirements
 To run this workflow successfully, you must have the following credentials 
 set up in your n8n instance:

 Service,            Credential Name,                   Required Access
Google Drive,        [Your Drive Credential Name],      Read/Write access to the folder where resumes are stored.
Google Sheets,       [Your Sheets Credential Name],     Read/Write access to the CV_analyser document.
Gmail,               [Your Gmail Credential Name],      Send Email permission.
AI Agent,            Gemini API,                        Requires an API Key for access to the Gemini model.

⚙️ Before Execution
  1. Import: Import the [Your Workflow Name].json file into your n8n editor.

  2. Credentials: Update all nodes (Google Drive, Sheets, Gmail, AI Agent) with your valid credentials.

  3. Google Sheets: Ensure the CV_analyser spreadsheet exists and has the necessary column headers (e.g., Email Address, Date Processed, AI Rating, Resume Link).

  4. Job Description: Ensure the JD file path referenced by the Job Description node is correct.

  5. Function Node: Verify that the column name in the Function node for the date check ('Date Processed') exactly matches your sheet header.

✅ After Execution
Review the following outputs to confirm successful execution:

  1. Gmail Inbox: Verify that the applicant received the appropriate email (Success, Rejection, or Applied Recently).

  2. Google Sheets (CV_analyser): Confirm that the applicant's data, including the AI score and the direct Resume Link, was logged correctly.

  3. Google Sheets (Shortlist Candidates): If the candidate scored high, confirm their data was logged into the separate shortlist sheet  


