
# Customer Sentiment Analysis – n8n Workflow

## Overview
This repository contains a **production-ready n8n workflow** for automated **Customer Sentiment Analysis** using **AI, Vector Databases, and Google Sheets logging**.

The workflow receives customer messages via a webhook, analyzes sentiment using an LLM with Retrieval-Augmented Generation (RAG), stores embeddings in Pinecone for historical context, logs results in Google Sheets, and sends Slack alerts on failures.

This setup is suitable for **customer support automation, feedback analysis, CRM enrichment, and AI-powered monitoring systems**.

---

## Key Features
- Real-time sentiment analysis (Positive / Negative / Neutral)
- AI-powered reasoning using Google Gemini
- Context-aware analysis with Pinecone vector search
- Automatic logging to Google Sheets
- Slack alerts for error handling
- Scalable and extensible architecture

---

## Workflow Architecture

**Trigger → AI Processing → Vector Storage → Logging → Alerts**

1. Webhook receives customer messages
2. Messages are split and processed individually
3. AI analyzes sentiment using RAG
4. Embeddings stored and queried from Pinecone
5. Sentiment results saved in Google Sheets
6. Errors reported via Slack

---

## Node-by-Node Explanation

### 1. Webhook Trigger
- **Type:** Webhook (POST)
- **Purpose:** Receives incoming customer messages
- **Endpoint:** `/customer-sentiment-analysis`
- **Expected Payload Example:**
```json
{
  "body": {
    "text": "I am very unhappy with the service"
  }
}
```

---

### 2. Split Out
- Splits multiple messages into individual items
- Ensures each message is analyzed independently

---

### 3. Google Gemini Chat Model
- **Model:** gemini-flash-lite-latest
- Acts as the main LLM for sentiment reasoning

---

### 4. Window Memory
- Maintains short-term conversational context
- Improves accuracy for follow-up or repeated messages

---

### 5. Pinecone Vector Tool
- Retrieves historical customer feedback
- Enables context-aware sentiment analysis

---

### 6. RAG Agent
- Combines:
  - LLM (Gemini)
  - Vector search (Pinecone)
  - Memory buffer
- Outputs sentiment with reasoning
- Response format:
```
Positive | Negative | Neutral
<reasoning>
```

---

### 7. Edit Fields (Set Node)
- Extracts sentiment label from AI output
- Stores it as `SentimentScore`

---

### 8. Google Sheets – Append Row
- Logs processed data:
  - Date
  - Original Text
  - Sentiment
  - Status
  - AI Output
- Enables reporting and analytics

---

### 9. Pinecone Insert
- Stores message embeddings
- Builds a long-term knowledge base

---

### 10. Slack Alert
- Sends alerts to Slack on workflow errors
- Ensures operational visibility

---

## Prerequisites
Before importing the workflow, ensure you have:

- n8n (self-hosted or cloud)
- Google Gemini API credentials
- Pinecone account & index
- Google Sheets OAuth credentials
- Slack OAuth credentials

---

## Setup Instructions

1. Clone this repository
2. Import the workflow JSON into n8n
3. Configure credentials:
   - Google Gemini (PaLM API)
   - Pinecone API
   - Google Sheets OAuth
   - Slack OAuth
4. Update:
   - Pinecone index name
   - Google Sheet ID
   - Slack channel
5. Activate the workflow

---

## Usage

Send a POST request to the webhook URL:
```bash
- curl -X POST <WEBHOOK_URL> -H "Content-Type: application/json" -d '{"body":{"text":"The support team was very helpful"}}'
```
- copy your <WEBHOOK_URL> after opening webhook node and past in above link at <WEBHOOK_URL> (B/W POST and -H) it looks like this 

- example: 

       curl -i -X POST https://usamapu.app.n8n.cloud/webhook-test/customer-sentiment-analysis -H "Content-Type: application/json" -d "{\"text\": \"The customer service was excellent, but the shipping took too long.\"}"

- now excute workflow (your work flow must be in listing mode), then open cmd in your mechine and then past final curl with your webhook url and hit enter youe workflow excute successfully 

The workflow will:
- Analyze sentiment
- Store embeddings
- Log results
- Handle alerts automatically

---

## Sample Output

     Date               Original Text                  Sentiment    Output                                     Status 
    2026-02-07 10:39   The service was excellent...    Neutral     Neutral. Conflicting sentiments found...   Processed

---

## Use Cases
- Customer support monitoring
- Product feedback analysis
- CRM sentiment enrichment
- AI-powered helpdesk automation
- Voice of Customer (VoC) systems

---

## Scalability & Extensions
- Add CRM integrations (HubSpot, Salesforce)
- Trigger WhatsApp or Email alerts for negative sentiment
- Build dashboards on top of Google Sheets or BigQuery
- Extend to multilingual sentiment analysis

---

## Status
**Production-ready & tested**

---

## Author
**Abrar Hussain**

If you found this useful, feel free to ⭐ the repository.
