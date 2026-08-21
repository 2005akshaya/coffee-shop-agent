# ☕ Coffee Shop Agent

An AI-powered coffee shop operations assistant built with **Google Cloud Run, Google ADK, Gemini on Vertex AI, and Google Sheets**.

The agent analyzes historical POS data, correlates demand patterns with graduation schedules, identifies potential operational bottlenecks, and recommends staffing actions. After receiving manager approval, it automatically creates and updates a task list in Google Sheets.

> This project is an extended implementation inspired by Google's [Cloud Run Personal Agent - Coffee Shop Codelab](https://codelabs.developers.google.com/codelabs/cloud-run/cloud-run-personal-agent-coffee-shop).

---

## 🚀 Overview

University graduation weekends can create unpredictable demand spikes in coffee shops. Certain beverages may become significantly more popular, customer wait times can increase, and additional staff may be required during specific ceremony periods.

The **Coffee Shop Agent** acts as an AI-powered operations assistant that helps managers prepare for these situations.

The agent:

1. Reads historical POS data from Google Sheets.
2. Analyzes historical product demand and operational patterns.
3. Correlates product spikes with graduation ceremonies.
4. Compares historical patterns with the upcoming graduation schedule.
5. Identifies potential wait-time and staffing bottlenecks.
6. Recommends actionable operational tasks.
7. Requests manager approval before modifying the task list.
8. Creates a `TODO-2026` spreadsheet tab when required.
9. Writes the approved tasks directly into Google Sheets.

The result is a conversational AI assistant that transforms historical business data into actionable operational planning.

---

## ✨ Key Features

### 📊 Historical POS Data Analysis

The agent reads historical transaction and operational data from the `Sheet1` tab of a Google Spreadsheet.

The data includes information such as:

- Date
- Time
- Ceremony
- Product
- Quantity
- Wait time
- Number of cashiers working

This allows the agent to identify patterns in customer demand and service pressure.

---

### ☕ Product Demand Analysis

The agent analyzes demand patterns around important beverage categories, including:

- Cold Brew
- Alternative Milk beverages
- Extra Espresso

These historical demand patterns are correlated with graduation ceremonies to identify periods that may require additional operational support.

---

### 🎓 Graduation Schedule Correlation

The manager provides the upcoming graduation schedule directly through the chat interface.

The agent compares the provided schedule with historical POS patterns to identify:

- Expected product demand spikes
- Busy ceremony periods
- Potential wait-time bottlenecks
- Staffing requirements
- Operational support opportunities

---

### 🤖 Gemini-Powered Business Reasoning

Gemini running through **Google Vertex AI** performs the analysis and determines which operational actions should be recommended.

The agent reasons over the historical spreadsheet data and the graduation schedule provided by the manager.

---

### 👤 Human-in-the-Loop Approval

The agent does not automatically modify the operational task list.

Instead, it first presents its recommendations and asks the manager for confirmation.

For example:

```text
Would you like me to add these tasks to the spreadsheet?
```

Only after the manager approves the recommendations does the agent perform spreadsheet write operations.

This provides a safety layer between AI-generated recommendations and real operational changes.

---

### 📋 Automatic TODO Sheet Management

After approval, the agent creates a dedicated:

```text
TODO-2026
```

spreadsheet tab if it does not already exist.

The approved operational tasks are then written into that tab.

---

### ☁️ Google Cloud Run Deployment

The application is containerized and deployed to **Google Cloud Run**.

Cloud Run provides:

- Managed container execution
- Automatic HTTPS
- Serverless deployment
- Application scalability
- Integration with Google Cloud services

---

## 🏗️ Architecture

```text
                         ┌─────────────────────┐
                         │       Manager       │
                         │     Web Chat UI     │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     Google Cloud    │
                         │       Run           │
                         │    FastAPI Server   │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      Google ADK     │
                         │        Agent        │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Gemini / Vertex   │
                         │    AI Reasoning     │
                         └──────────┬──────────┘
                                    │
                    ┌───────────────┴────────────────┐
                    │                                │
                    ▼                                ▼
          ┌─────────────────────┐        ┌──────────────────────┐
          │    Google Sheets    │        │   Spreadsheet Tools  │
          │                     │◄──────►│                      │
          │ Historical POS Data │        │ Read / Create /       │
          │ TODO-2026           │        │ Update                │
          └─────────────────────┘        └──────────────────────┘
```

---

## 🧰 Technology Stack

| Component | Technology |
|---|---|
| Programming Language | Python |
| Web Framework | FastAPI |
| ASGI Server | Uvicorn |
| Agent Framework | Google ADK |
| Large Language Model | Gemini |
| AI Platform | Google Vertex AI |
| Deployment Platform | Google Cloud Run |
| Data Store | Google Sheets |
| Containerization | Docker |
| Cloud Management | Google Cloud CLI |

---

## 📁 Project Structure

```text
coffee-mgr-agent/
│
├── main.py
├── Dockerfile
├── requirements.txt
├── README.md
└── .gitignore
```

### `main.py`

Contains the main application logic, including:

- FastAPI application
- Google ADK agent
- Gemini / Vertex AI configuration
- Google Sheets integration
- Spreadsheet tools
- WebSocket chat handling
- Agent execution loop

### `Dockerfile`

Defines the container image used to package and deploy the application to Cloud Run.

### `requirements.txt`

Contains the Python dependencies required by the application.

### `.gitignore`

Prevents sensitive and unnecessary files from being committed to the repository.

---

## 🔧 Spreadsheet Tools

The agent uses dedicated tools for interacting with Google Sheets.

### `read_spreadsheet_values`

Reads data from a specified spreadsheet range.

This is used to retrieve historical POS data before the agent performs its analysis.

---

### `create_spreadsheet_tab`

Creates a new spreadsheet tab when required.

For the graduation planning workflow, this is used to create:

```text
TODO-2026
```

---

### `update_spreadsheet_values`

Writes the approved operational tasks into the spreadsheet.

The spreadsheet operations are separated into individual tools so that the AI reasoning process and external data modifications remain clearly defined.

---

## 🔄 Agent Workflow

The complete workflow is:

```text
┌───────────────────────────────┐
│ 1. Manager provides schedule  │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ 2. Read historical POS data   │
│    from Google Sheets         │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ 3. Gemini analyzes historical │
│    demand and wait times      │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ 4. Correlate patterns with    │
│    graduation schedule        │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ 5. Generate staffing and      │
│    operational recommendations│
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ 6. Ask manager for approval   │
└───────────────┬───────────────┘
                │
                ▼
        ┌───────────────┐
        │   Approved?   │
        └───────┬───────┘
                │ Yes
                ▼
┌───────────────────────────────┐
│ 7. Create TODO-2026 if needed │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ 8. Write approved tasks       │
│    into Google Sheets         │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│ 9. Confirm completion         │
└───────────────────────────────┘
```

---

## 💬 Example Interaction

### Manager

```text
Here is the graduation schedule for 2026.

[Graduation schedule...]

Analyze the historical POS data and recommend
staffing tasks for the upcoming ceremonies.
```

### Agent

The agent reads the historical POS data and identifies patterns such as beverage demand spikes and increased wait times around specific ceremonies.

It may recommend tasks such as:

```text
1. Schedule a Support Barista for Saturday evening
   at the College of Liberal Arts.

2. Schedule a Support Barista for Sunday evening
   at the College of Engineering.

Would you like me to add these tasks to the spreadsheet?
```

### Manager

```text
yes
```

### Agent

The agent creates or updates:

```text
TODO-2026
```

and writes the approved tasks into the spreadsheet.

It then confirms that the tasks have been successfully added.

---

## 🔐 Configuration

The application uses the following environment variables:

```text
SPREADSHEET_ID
GOOGLE_GENAI_USE_VERTEXAI
GOOGLE_CLOUD_PROJECT
GOOGLE_CLOUD_LOCATION
```

Example:

```bash
export SPREADSHEET_ID="your-spreadsheet-id"
export GOOGLE_GENAI_USE_VERTEXAI="TRUE"
export GOOGLE_CLOUD_PROJECT="your-google-cloud-project"
export GOOGLE_CLOUD_LOCATION="your-region"
```

### Environment Variable Description

| Variable | Description |
|---|---|
| `SPREADSHEET_ID` | Google Spreadsheet used for POS data and operational tasks |
| `GOOGLE_GENAI_USE_VERTEXAI` | Enables Gemini through Vertex AI |
| `GOOGLE_CLOUD_PROJECT` | Google Cloud project ID |
| `GOOGLE_CLOUD_LOCATION` | Google Cloud region used by the application |

---

## 🔒 Security

Never commit sensitive credentials or secrets to GitHub.

Do **not** commit:

- Service account credential files
- `.env` files
- API keys
- Access tokens
- Private keys
- Authentication credentials

The repository includes a `.gitignore` to help prevent sensitive files from being accidentally committed.

---

## ☁️ Deploying to Google Cloud Run

Make sure the Google Cloud CLI is installed and authenticated.

Set the required environment variables:

```bash
export REGION="your-region"
export SERVICE_ACCOUNT_ADDRESS="your-service-account"
export SPREADSHEET_ID="your-spreadsheet-id"
```

Deploy the application:

```bash
gcloud run deploy coffee-shop-agent \
  --source . \
  --region=$REGION \
  --service-account=$SERVICE_ACCOUNT_ADDRESS \
  --set-env-vars="SPREADSHEET_ID=$SPREADSHEET_ID,GOOGLE_GENAI_USE_VERTEXAI=TRUE,GOOGLE_CLOUD_PROJECT=$GOOGLE_CLOUD_PROJECT,GOOGLE_CLOUD_LOCATION=$REGION" \
  --allow-unauthenticated \
  --memory=1Gi \
  --timeout=300
```

Cloud Run builds the container from the project source and deploys it as a managed service.

---

## 🧪 Local Development

Clone the repository:

```bash
git clone https://github.com/2005akshaya/coffee-shop-agent.git
cd coffee-shop-agent
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Configure the required environment variables:

```bash
export SPREADSHEET_ID="your-spreadsheet-id"
export GOOGLE_GENAI_USE_VERTEXAI="TRUE"
export GOOGLE_CLOUD_PROJECT="your-project-id"
export GOOGLE_CLOUD_LOCATION="your-region"
```

Run the application:

```bash
python main.py
```

The application uses the `PORT` environment variable provided by Cloud Run and defaults to:

```text
8080
```

---

## 🛡️ Design Principles

### Human-in-the-Loop

The agent requires explicit manager approval before modifying operational tasks.

This prevents the AI from silently changing business operations.

### Tool-Based External Actions

Spreadsheet operations are exposed through dedicated tools:

```text
read_spreadsheet_values
create_spreadsheet_tab
update_spreadsheet_values
```

This provides a clear boundary between AI reasoning and external actions.

### Analysis Before Execution

The agent follows a two-stage process:

```text
Historical Data
      ↓
AI Analysis
      ↓
Recommendations
      ↓
Manager Approval
      ↓
Spreadsheet Modification
```

This ensures that recommendations are reviewed before they become operational tasks.

---

## 📈 Example Use Case

Consider a university graduation weekend where multiple ceremonies take place throughout the day.

Historical POS data shows that:

- Certain beverages experience demand spikes after specific ceremonies.
- Wait times increase during those periods.
- Additional staffing reduces operational pressure.

The Coffee Shop Agent can use those historical patterns to recommend staffing support for similar periods in the upcoming graduation schedule.

For example:

```text
Historical Pattern
        ↓
Ceremony + Product Spike
        ↓
High Wait Time
        ↓
Upcoming Similar Ceremony
        ↓
Staffing Recommendation
        ↓
Manager Approval
        ↓
TODO-2026
```

---

## 🔮 Future Improvements

Potential future extensions include:

- Real-time POS system integration
- Automated demand forecasting
- Advanced staffing optimization
- Historical trend visualization
- Google Cloud Monitoring integration
- Automated operational alerts
- Google Calendar integration
- Manager authentication
- Role-based access control
- Multi-location coffee shop support
- Persistent conversation history
- Task prioritization
- Automated daily operational reports

---

## 📚 Reference

This project is based on and extends Google's Cloud Run Coffee Shop Personal Agent Codelab:

**Google Cloud Codelab**

https://codelabs.developers.google.com/codelabs/cloud-run/cloud-run-personal-agent-coffee-shop

The original codelab demonstrates the foundation for building and deploying a personal AI agent on Google Cloud Run.

This repository extends that concept into a graduation-weekend coffee shop operations assistant with:

- Historical POS analysis
- Graduation schedule correlation
- AI-generated staffing recommendations
- Human approval before actions
- Google Sheets integration
- Automatic TODO sheet creation
- Automated task updates
- Cloud Run deployment

---

## 👨‍💻 Author

**2005akshaya**

GitHub:  
https://github.com/2005akshaya

---

## ⭐ Project

If you find this project useful or interesting, consider giving the repository a ⭐ on GitHub.

Built with:

**Google Cloud ☁️ + Gemini 🤖 + Google ADK 🧠 + FastAPI ⚡ + Google Sheets 📊**
