# Scheduled AI News Digest (n8n Workflow)

This n8n workflow completely automates the process of gathering, analyzing, and delivering the latest AI news. It acts as an intelligent news aggregator that pulls from major AI publications, uses a local AI agent to summarize and rank the stories, and emails you a beautifully formatted HTML digest every morning.

## 🚀 Workflow Architecture

Here is how the automated news digest works step-by-step:

1. **Scheduling**: A Schedule Trigger kicks off the workflow automatically every day at 7:00 AM.
2. **RSS Aggregation**: The workflow simultaneously fetches the latest articles from four major RSS feeds:
   - OpenAI News
   - Google AI Blog
   - TechCrunch AI
   - MIT Artificial Intelligence
3. **Data Filtering**: 
   - A Merge node combines all the fetched articles into a single list.
   - An `If` node filters out older news, ensuring only articles published within the last 24 hours proceed.
   - A Code node removes any duplicate articles based on the headline.
   - A Limit node caps the processing batch to the top 5 most recent articles to prevent overloading the local LLM.
4. **AI Analysis & Parsing (Ollama + LangChain)**: 
   - The core of the workflow is an AI Agent powered by a local Ollama model (`qwen3:8b`). 
   - It reads each article's content and is strictly instructed via a Structured Output Parser to return a specific JSON schema containing: Title, Category, an Importance Score (1-10), a brief Summary, Industry Impact, and Tags.
5. **Ranking & Formatting**: 
   - A Code node sorts the analyzed articles by their AI-generated `importance_score` in descending order, ensuring the biggest news is at the top.
   - Another Code node generates a clean, responsive HTML template and injects the AI-structured data.
6. **Delivery**: Finally, a Gmail node emails the finished HTML digest to your inbox.

## 🛠️ Prerequisites

To run this workflow effectively, particularly since it relies on local AI inference, you will need the following setup:

* **n8n Environment**: A running instance of n8n (v1.0+ recommended for LangChain node support).
* **Ollama (Local AI)**: You must have Ollama installed and running on your network. Ensure you have pulled the required model by running `ollama run qwen3:8b` in your terminal.
* **Gmail Account**: An active Gmail account with OAuth2 configured in n8n for sending the final email.

## 📥 Installation & Setup

1. **Import the Workflow**:
   - Open your n8n workspace.
   - Go to **Workflows** -> **Add Workflow**.
   - Click the **...** (Options) menu in the top right and select **Import from File**.
   - Select the `Scheduled AI News Digest.json` file.
2. **Configure Credentials**:
   - **Ollama Node**: Double-click the "Ollama Chat Model" node and connect it to your Ollama API credentials. Ensure the Base URL is correct (e.g., `http://localhost:11434`).
   - **Gmail Node**: Double-click the "Send a message" node and select or create your Gmail OAuth2 credentials.
3. **Customize the Nodes**:
   - **Email Destination**: In the Gmail node, update the `Send To` field from the placeholder email to your actual email address.
   - **RSS Feeds (Optional)**: You can easily add, remove, or change the RSS Feed Read nodes to pull from your preferred tech blogs. Just make sure to connect them to the Merge node!
   - **Schedule (Optional)**: Adjust the Schedule Trigger node if you prefer the digest at a different time of day.
4. **Activate**:
   - Toggle the workflow to **Active** in the top right corner. The workflow will now run automatically on your defined schedule.

## 🧠 Why Local AI?
By utilizing Ollama instead of a cloud provider like OpenAI, this workflow analyzes daily news completely free of API costs and ensures your reading habits and data remain entirely private on your local machine.
