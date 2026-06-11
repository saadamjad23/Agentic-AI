# AI-Powered Feedback Classification & Routing (n8n Workflow)

This repository contains an n8n workflow designed to automate the collection, analysis, and routing of user feedback. By leveraging AI to categorize incoming messages, this automation ensures that your team can efficiently handle complaints, celebrate complements, and track feature requests without manual triage. It is fully compatible with both cloud-hosted and local instances of n8n.

## 🚀 Workflow Architecture

Here is the step-by-step breakdown of how the workflow operates:

1. **Trigger (Feedback Form)**: An n8n Form Trigger captures the user's submission, collecting their Full Name, Email, Contact Info, and Feedback.
2. **AI Analysis (Google Gemini)**: The workflow utilizes an AI Agent powered by the Google Gemini Chat Model to interpret the feedback text. It strictly classifies the input into one of three categories:
   - `1. Complaint`
   - `2. Complement`
   - `3. Feature Addition Request`
3. **Data Merging & Routing**: A Merge node combines the original form data with the AI's classification. A Switch node then directs the data down one of three paths.
4. **Storage (Airtable)**: Based on the classification, the user's details and feedback are logged into specific Airtable tables within your base.
5. **Notifications (Slack & Gmail)**: 
   - Relevant Slack channels (`#feedback_complaint`, `#feedback_complement`, `#feedback_request`) receive an instant alert containing the user's name, email, and message.
   - **Escalation**: If the feedback is categorized as a *Complaint*, an additional escalation email is automatically sent via Gmail.

## 🛠️ Prerequisites

To run this workflow, you will need the following accounts and credentials configured in your n8n environment:

* **Google Gemini (PaLM) API**: For the AI Agent classification.
* **Airtable Personal Access Token**: To write records to your feedback base.
* **Slack App / Bot Token**: With permissions to post to specific feedback channels.
* **Gmail OAuth2**: To send complaint escalation emails.

## 📥 Installation & Setup

1. **Import the Workflow**:
   - Open your n8n workspace.
   - Go to **Workflows** -> **Add Workflow**.
   - Click the **...** (Options) menu in the top right and select **Import from File**.
   - Select the `Feedback-Automation.json` file.
2. **Configure Credentials**:
   - Double-click each node marked with an alert/warning icon to connect your credentials (Airtable, Slack, Gmail, and Google Gemini).
3. **Customize the Nodes**:
   - **Airtable Nodes**: Update the Base ID and Table IDs to match your specific Airtable setup. Ensure your tables have columns for `Full Name`, `Email`, `Contact Info`, and `Feedback`.
   - **Slack Nodes**: Update the `Channel ID` fields to match your actual Slack channel IDs (e.g., replace `C0B7TBB76AG` with your target complaint channel).
   - **Gmail Node**: Update the `Send To` email address to your preferred support or management inbox.
4. **Activate**:
   - Toggle the workflow to **Active** in the top right corner.
   - Your form is now live and ready to process incoming feedback!

## 🤝 Customization

You can easily modify the AI Agent's prompt to add more categories (e.g., "Bug Report" or "Billing Question") and add corresponding routing paths in the Switch node.
