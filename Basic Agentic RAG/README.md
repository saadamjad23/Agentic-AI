# Agentic RAG Pipeline: Ingestion & Retrieval

This repository contains a two-part n8n workflow system designed to build a complete Retrieval-Augmented Generation (RAG) architecture. It seamlessly handles both the ingestion of documents into a vector database and the intelligent retrieval of that data via an AI chat agent. 

Optimized for environments running a local instance of n8n, this setup leverages Ollama for local embeddings to keep processing secure and self-contained, while utilizing Google Gemini for advanced agentic reasoning.

## 🚀 Workflow Architecture

This solution is split into two complementary workflows:

### 1. Agentic RAG Database (Data Ingestion)
This workflow automates the process of watching for new documents and embedding them into your vector store.
* **Trigger (Google Drive)**: Polls a specific Google Drive folder every minute for newly created files.
* **Download & Load**: Downloads the newly detected file and processes it using the Default Data Loader, extracting the binary data and file name.
* **Local Embeddings (Ollama)**: Utilizes a locally hosted Ollama model (`qwen3-embedding:0.6b`) to generate high-quality text embeddings.
* **Vector Storage (Qdrant)**: Upserts the embedded documents into a Qdrant vector database collection (named dynamically based on the file ID) for efficient future retrieval.

### 2. RAG Retrieval (Chat Agent)
This workflow provides a conversational interface for users to query the ingested data.
* **Trigger (n8n Chat)**: Initiates a session when a message is received through the native n8n chat interface.
* **Memory Management**: Uses a Window Buffer Memory to maintain conversational context across multiple interactions.
* **AI Agent (Google Gemini)**: The core reasoning engine powered by the Google Gemini Chat Model.
* **Retrieval Tool (Qdrant & Ollama)**: The Agent is equipped with the Qdrant Vector Store as a tool. When a user asks a relevant question (e.g., regarding "ethics"), the Agent queries Qdrant using the same local Ollama embeddings to fetch the exact context needed to formulate an accurate answer.

## 🛠️ Prerequisites

To run these workflows, you need the following integrations configured in your n8n workspace:

* **Google Drive API (OAuth2)**: To monitor folders and download files.
* **Ollama (Local)**: A running instance of Ollama with the `qwen3-embedding:0.6b` model pulled.
* **Qdrant API**: Access to a Qdrant cluster (cloud or local) to store and retrieve vectors.
* **Google Gemini (PaLM) API**: For the conversational AI Agent.

## 📥 Installation & Setup

1. **Import the Workflows**:
   - In n8n, go to **Workflows** -> **Add Workflow**.
   - Click the **...** menu, select **Import from File**, and upload both JSON workflow files.
2. **Configure Credentials**:
   - Double-click the nodes with warning icons to attach your Google Drive, Ollama, Qdrant, and Google Gemini credentials.
3. **Customize Ingestion**:
   - In the **Google Drive Trigger**, update the `Folder To Watch` to the specific ID of your target drive folder.
4. **Customize Retrieval**:
   - In the **Qdrant Vector Store** tool node (in the RAG Retrieval workflow), update the `Tool Description` to accurately reflect the domain of your documents so the AI knows exactly when to trigger it. Ensure the Qdrant Collection ID matches your ingestion target.
5. **Activate**:
   - Toggle both workflows to **Active**. Drop a file into your Google Drive folder, wait a minute, and then test the Chat interface!
