# 🤖 AI-Powered Autonomous Feedback Routing Engine

![n8n](https://img.shields.io/badge/n8n-Workflow-FF6C37?style=for-the-badge&logo=n8n&logoColor=white)
![AI](https://img.shields.io/badge/AI-Groq%20%7C%20Llama%203-000000?style=for-the-badge)
![Telegram](https://img.shields.io/badge/Telegram-Bot%20API-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)

An enterprise-grade, autonomous workflow built in **n8n** to manage customer feedback for beauty and personal care salons (specifically designed for Aurox Software). The system leverages Large Language Models (LLMs) to analyze raw customer feedback, categorize it, and route it to the appropriate channels in real-time, backed by a robust global error-handling architecture.

## 🚀 Key Features & Capabilities

* **Agentic Data Routing:** Ingests unstructured customer feedback via Webhooks and processes it through an LLM (Groq Llama 3) to enforce strict JSON schema outputs (Sentiment, Category, Summary, Action Required).
* **Real-time Anomaly Detection (Human-in-the-loop):** Identifies critical/negative feedback and instantly dispatches structured alert messages to management via Telegram Bot API for immediate intervention.
* **Automated KPI Analytics:** Routes positive or neutral feedback directly into Google Sheets, creating a continuous data pipeline for performance monitoring and salon analytics.
* **Enterprise Error Handling:**
  * **Node-Level:** Implements custom "Retry on Fail" logic (3 retries with custom intervals) for API rate-limits or temporary outages.
  * **Global-Level:** Features a dedicated, secondary `Global Error Handler` workflow. If a critical failure occurs, it intercepts the crash data and alerts the developer via Telegram with exact technical logs (`execution.id`, `node.name`, `error.message`).

## 🏗️ Architecture Flow

1. **Trigger:** `Webhook Node` receives POST request containing customer feedback.
2. **AI Processing:** `Basic LLM Chain Node` (connected to Groq Chat Model) parses the prompt and forces a structured JSON output.
3. **Data Parsing:** `Code Node (JavaScript)` parses the stringified AI response into a usable JSON object.
4. **Routing (IF Node):**
   * `TRUE (Action Required):` Triggers Telegram Node -> Sends urgent alert.
   * `FALSE (Positive/Neutral):` Triggers Google Sheets Node -> Appends a new row for analytics.
5. **Fallback:** If any node fails permanently, the `Global Error Handler` workflow is triggered.

## 🛠️ How to Install & Run

1. Clone this repository.
2. Open your n8n instance.
3. Go to **Workflows** -> **Import from File** and upload the `aurox_feedback_engine.json` file.
4. Import the `global_error_handler.json` workflow and set it to **Active**.
5. Update the Node Credentials:
   * **Groq API:** Add your Groq API Key.
   * **Telegram:** Add your Bot Token and Chat ID.
   * **Google Sheets:** Authenticate your Google account and select your target spreadsheet.
6. Set the main workflow to **Active** and update your external systems to point to the new Production Webhook URL.

## 👨‍💻 Author

**Faruk Burak ÖĞRETİCİ**  
Full Stack Developer & Software Specialist  
*(Feel free to reach out for discussions regarding backend integrations, AI workflows, or .NET/React architectures)*
