## 🛠️ AI Maintenance Concierge: Automated Ticket Classification

[Image of maintenance ticket workflow diagram]

### 💡 Project Overview

The **AI Maintenance Concierge** is an intelligent, low-code solution designed to revolutionize property and facility management by automating the conversion of raw, unstructured user complaints into standardized, actionable maintenance tickets.

Leveraging the **Gemini-2.5-flash-lite** model's natural language processing (NLP) and strict JSON output capabilities, this project eliminates manual triage, speeds up technician dispatch, and ensures consistent data quality across all reported issues. It prioritizes safety by instantly classifying critical issues (like electrical sparks or water leaks) with a **HIGH** priority flag.

-----

### ⚙️ Core Features

  * **Instant Classification:** Automatically maps any text complaint to exactly one of 8 predefined categories (e.g., Plumbing, Electrical, AC/Cooling).
  * **Safety-First Prioritization:** Assigns urgency levels (**HIGH, MEDIUM, LOW**) based on immediate risk to safety or property.
  * **Strict JSON Output:** Guarantees a clean, validated JSON schema for every ticket, making the output directly compatible with modern Computerized Maintenance Management Systems (CMMS).
  * **Persistent Data Logging:** Uses **Pandas** to append structured tickets to a persistent CSV database (`maintenance_log.csv`).
  * **Scalable Architecture:** Built on the Google GenAI SDK, making the core logic easily transferable to a cloud function, microservice, or web application.

-----

### 🚀 Architecture and Data Flow

The project is built on a clear, sequential flow executed within a **Kaggle Notebook** environment:

1.  **Input:** A user submits a raw text complaint (e.g., "The hallway light is wet and smells bad.").
2.  **Agent Processing:** The **Python script** sends the text to the **Gemini-2.5-flash-lite** model.
3.  **System Instruction:** A highly-constrained **System Prompt** forces the model to analyze the text and generate a structured JSON ticket based on the defined categories and priority rules.
4.  **Output Validation:** The model uses **`response_mime_type="application/json"`** to ensure schema adherence.
5.  **Data Persistence:** The Python script uses **Pandas** to convert the validated JSON into a DataFrame row and append it to the central **`maintenance_log.csv`** file.

| Component | Technology | Function |
| :--- | :--- | :--- |
| **Inference Engine** | Gemini-2.5-flash-lite | NLP and Structured Output Generation. |
| **API Interface** | Google GenAI SDK | Secure communication and configuration setup. |
| **Data Handler** | Pandas | CSV creation, appending, and retrieval. |
| **Security** | Kaggle Secrets | Secure storage of the `GOOGLE_API_KEY`. |

-----

### 📋 Ticket Schema (Mandatory JSON Output)

The agent’s primary output is the ticket object, strictly formatted as follows:

```json
{
  "ticket_id": "TKT-xxxxx",
  "issue_summary": "<1-line summary>",
  "category": "<Plumbing/Electrical/etc.>",
  "priority": "<High/Medium/Low>",
  "recommended_action": "<short repair instruction>",
  "technician_required": "Yes/No",
  "estimated_resolution_time": "<hours/days>",
  "risk_level": "<Critical/Warning/Safe>",
  "notes_for_maintenance_team": "<optional useful details>"
}
```

### 📈 Example: Safety Prioritization

| Complaint | Priority (AI Output) | Rationale |
| :--- | :--- | :--- |
| "The electric switch is sparking when touched." | **High** | Risk to safety / Fire hazard. |
| "The toilet flushes slowly but still drains." | **Medium** | Functioning but faulty / Inconvenient. |
| "A handle on the kitchen cupboard is loose." | **Low** | Cosmetic / Minor. |

-----

### ▶️ Getting Started (Kaggle Notebook)

This project requires a Gemini API key configured as a **Kaggle Secret**.

#### **Prerequisites**

1.  Create a **Kaggle Notebook**.
2.  Add your Gemini API Key to Kaggle Secrets, naming the secret **`GOOGLE_API_KEY`**.

#### **Execution Steps**

The notebook is divided into sequential cells. Run them in order:

1.  **Cell 1:** Setup and Configuration. Imports libraries and securely retrieves the `GOOGLE_API_KEY`.
2.  **Cell 2-4:** Definitions. Imports the necessary packages and defines the **`AGENT_INSTRUCTION`** system prompt.
3.  **Cell 5:** Core Functions. Defines `generate_ticket()` (Gemini call) and `save_to_database()` (Pandas call).
4.  **Cell 6:** **Execution Loop.** Sets the test complaint, calls the functions, prints the resulting JSON, saves the data, and verifies the final CSV contents.

<!-- end list -->

```python
# --- EXECUTION LOOP (Cell 6 Snippet) ---
user_input = "The air conditioner in the lobby is dripping water and making a loud grinding noise."
ticket_json = generate_ticket(user_input)
print(json.dumps(ticket_json, indent=2)) # Display JSON
status = save_to_database(ticket_json) # Save to CSV
```

-----

### 🔗 Contact & Repository

| Detail | Value |
| :--- | :--- |
| **Author** | Abhishek Mukhiya |
| **Contact** | abhishekmukhiya9822@gmail.com |
| **Repository** | [Link to this GitHub Repository] |
