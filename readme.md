# N8N Upwork Automation Workflow

An automation system built using **n8n** to streamline and automate Upwork-related processes using API-based integrations.

## Repository Architecture
```
n8n-upwork-automation/
├── Upwork-Automation.json
├── .gitignore
  └── .env.example
├── demo.md
├── readme.md
└── report.md
```

## Project Overview

This project automates repetitive Upwork tasks using **n8n workflows**.  
The automation logic is exported as a JSON file and can be directly imported into an n8n instance.

The repository contains:
- Workflow configuration
- Setup documentation
- Demo reference
- Detailed project report

---

## Setup Instructions

### Prerequisites
- Running n8n instance (local or self-hosted)
- Required API credentials

---

### Step 1: Import Workflow
1. Open the n8n Editor UI
2. Select **Import workflow**
3. Upload `Upwork-Automation.json`
4. Save the workflow

---

### Step 2: Environment Configuration
1. Create a `.env` file in the project root
2. Define all required environment variables

```env
UPWORK_API_KEY=
UPWORK_CLIENT_ID=
UPWORK_CLIENT_SECRET=
Refer to .env.example for variable names.
```
### Step 3: Credential Mapping
Open Credentials in n8n
Create credentials for the APIs used in the workflow
Attach credentials to the corresponding workflow nodes

---

### Step 4: Workflow Activation
Open the imported workflow
Validate node configurations
Activate the workflow

---

Step 5: Execution
Trigger the workflow using the configured trigger node
Monitor execution logs in n8n
Verify workflow output

---
