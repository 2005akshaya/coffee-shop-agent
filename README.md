# ☕ Coffee Shop Agent

An AI-powered coffee shop operations assistant built with **Google Cloud Run, Google ADK, and Gemini on Vertex AI**.

The agent analyzes historical coffee-shop POS data, combines those insights with a graduation-event schedule, identifies expected operational bottlenecks, and creates actionable tasks in Google Sheets after receiving manager approval.

> Built as an extended implementation of Google's [Cloud Run Coffee Shop Personal Agent Codelab](https://codelabs.developers.google.com/codelabs/cloud-run/cloud-run-personal-agent-coffee-shop).

---

## 🚀 Overview

University graduation weekends can create unpredictable demand spikes in coffee shops. Certain beverages may suddenly become popular, queues can become longer, and staffing requirements can change depending on ceremony schedules.

The **Coffee Shop Agent** helps a manager prepare for these situations by:

1. Reading historical POS data from Google Sheets.
2. Analyzing product demand and operational patterns.
3. Correlating demand spikes with graduation ceremonies.
4. Using the upcoming graduation schedule to predict potential bottlenecks.
5. Recommending operational tasks.
6. Asking the manager for confirmation before making changes.
7. Creating a dedicated `TODO-2026` spreadsheet tab.
8. Writing the approved tasks directly into Google Sheets.

The result is a conversational AI assistant that turns historical business data into actionable operational planning.

---

## ✨ Key Features

### 📊 Historical POS Analysis

The agent reads historical transaction and operational data from the `Sheet1` tab of a Google Spreadsheet.

The analysis considers information such as:

- Date
- Time
- Ceremony
- Product
- Quantity
- Wait time
- Number of cashiers working

This allows the agent to identify patterns in customer demand and service pressure.

### ☕ Product Demand Analysis

The agent specifically analyzes demand patterns around products such as:

- Cold Brew
- Alternative Milk beverages
- Extra Espresso

These historical demand patterns are correlated with graduation ceremonies and used to identify similar situations in the upcoming schedule.

### 🎓 Graduation Schedule Correlation

The manager provides the upcoming graduation schedule directly in the conversation.

The agent compares the schedule against historical POS patterns to identify:

- Expected demand spikes
- Potential busy periods
- Ceremony-related bottlenecks
- Staffing requirements

### 🤖 Gemini-Powered Reasoning

Gemini running through **Vertex AI** performs the business analysis and determines which operational actions should be recommended.

The agent does not require a separate hard-coded prediction pipeline for every schedule. Instead, it reasons over the spreadsheet data and the schedule supplied by the manager.

### 👤 Human Approval

The agent does **not automatically modify the operational task list**.

Instead, it first presents its recommendations and asks the manager for confirmation.

For example:

```text
Would you like me to add these tasks to the spreadsheet?
