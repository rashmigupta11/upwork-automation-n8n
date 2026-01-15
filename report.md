# Project Report: n8n Upwork Automation

## Overview

This project implements a production-ready n8n workflow to automatically scrape recent Upwork job postings, analyze them using an LLM, score and prioritize them based on relevance, store structured results in Airtable, and send a consolidated email alert for high-priority opportunities.

The original workflow was designed to use the **OpenAI GPT-4o API**. However, due to the absence of an active paid OpenAI API billing account, the OpenAI node could not be executed. To ensure uninterrupted progress and successful completion of the assignment, the workflow was adapted to use the **Groq LLM API** instead.

All AI-scored results are successfully written to Airtable, and high-priority jobs are delivered via a single aggregated email.

---

## Issues Identified & Fixed

### 1. OpenAI API Access & Model Substitution

**Issue:**  
The workflow initially relied on the OpenAI GPT-4o API, but execution failed due to unavailable API credits and billing restrictions.

**Fix:**  
Replaced the OpenAI Chat Model with the **Groq Chat Model (openai/gpt-oss-120b)**, while preserving:
- The original prompt structure
- The required JSON output format
- Compatibility with downstream nodes (Airtable + email)

This allowed the workflow to remain fully functional without altering the overall design.

---

### 2. API Rate Limiting & Model Overload

**Issue:**  
When processing multiple job descriptions simultaneously, the Groq API occasionally returned overload or rate-limit errors, especially when handling 8–10 jobs in a single execution.

**Fix:**  
Implemented a **Loop Over Items** node configured with:
- **Batch size = 1**
- A **Wait node (5 seconds)** between iterations

This ensured jobs were processed sequentially, stabilizing AI calls and eliminating rate-limit failures.

---

### 3. Data Loss Between Nodes (Missing Job Fields)

**Issue:**  
After AI analysis, original job fields (title, description, URL) were missing in Airtable because the AI node output replaced upstream data.

**Fix:**  
Resolved by explicitly referencing upstream node data using n8n expressions such as:

```text
{{ $('Loop Over Items').item.json.title }}

## Mandatory Enhancements Implemented

### 1. High-Priority Job Email Aggregation
**Enhancement:**  
Instead of sending one email per job, all high-priority jobs (score 8–10) are aggregated into a single email digest per workflow run.

**Challenges Faced:**  
- Initial implementation triggered multiple emails (one per job)


**Solution:**  
- Collected high-priority jobs after AI scoring
- Used a JavaScript Code node to aggregate results
- Generated a single HTML email containing all relevant jobs
- Sent one consolidated email via Gmail

---

### 2. Skill-Based Scoring Using Custom Variables 
**Enhancement:**  
Introduced deterministic skill scoring in n8n before AI analysis to improve reliability and reduce hallucinations.

**Custom Variables Added:**
- `n8n_score`
- `automation_score`
- `ai_score`
- `custom_skill_score`

**Challenges Faced:**  

- Scores were biased toward irrelevant job priority

**Solution:**  
- Preserved upstream job data explicitly
- Enforced strict AI rules in the prompt
- Balanced scoring logic to allow Low / Medium / High outputs

---



