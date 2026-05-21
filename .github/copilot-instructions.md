# GitHub Copilot Instructions — n8n Automation Platform
# Author: Ramalakshmi Mani (Rama) | Senior Cloud & Platform Engineer @ BMW TechWorks
# Purpose: Custom Copilot instructions for n8n workflow automation repo

---

## 🚫 Things Copilot Should NEVER Do

- Never suggest hardcoded credentials or API keys
- Never suggest deprecated n8n node types
- Never suggest synchronous HTTP calls without timeout
- Never generate workflows without error handling
- Never use `console.log` in production code — use structured logging
- Never suggest storing sensitive data in workflow variables

## 🧠 Project Context

This repository contains n8n workflow automations, AI agent configurations, and integrations
used across enterprise platform engineering and cross-team collaboration at BMW TechWorks.

Stack: n8n, Python, JavaScript, REST APIs, Webhooks, AI Agents, Azure, AWS

---

## ⚙️ General Coding Standards

- Always use **descriptive variable names** — no single letter variables except loop counters
- Add **inline comments** for every non-obvious logic block
- Follow **DRY principle** — reuse functions, avoid duplicating logic
- Always handle **errors and exceptions** — never leave empty catch blocks
- Use **environment variables** for all secrets, URLs, and credentials — never hardcode
- Prefer **async/await** over callbacks in JavaScript/Node.js

---

## 🔐 Security Rules (Non-Negotiable)

- **NEVER hardcode** API keys, passwords, tokens, or connection strings
- Always reference secrets via **environment variables** or **secret managers**
- All external HTTP calls must use **HTTPS only**
- Validate and **sanitize all inputs** before processing in workflow nodes
- Use **least privilege** principle for all service accounts and API tokens
- When generating n8n credentials, always add a comment: `# Store in n8n Credentials Manager`

---

## 🤖 n8n Workflow Standards

- Every workflow must have a **clear description** in the workflow notes field
- Use **sticky notes** in workflows to explain complex logic
- Node naming convention: `[Action] - [Resource]` e.g. `Get - Jira Tickets`, `Send - Slack Alert`
- Always add **error handling nodes** (Error Trigger) for production workflows
- Use **Set nodes** to normalize data before passing between nodes
- Prefer **Function nodes** over Code nodes for reusability
- Always add a **Wait node** between bulk API calls to avoid rate limiting
- Webhook URLs must be documented in README

---

## 🧩 AI Agent Workflow Standards

- Every AI agent workflow must define a clear **system prompt** with role and boundaries
- Always set **max iterations** on AI agent loops to prevent infinite runs
- Use **structured output parsers** when AI response feeds into downstream nodes
- Add **fallback logic** when AI response is empty or malformed
- Document the **model used** (e.g. GPT-4o, Claude Sonnet) in workflow sticky notes
- Never pass **raw PII data** to AI models — anonymize first

---

## 🐍 Python Scripts (used in n8n Code nodes)

- Use **Python 3.11+** syntax
- Always include **type hints** on function signatures
- Use **requests** library for HTTP calls — always set timeout parameter
- Structure: imports → constants → functions → main logic
- Example:
  ```python
  import requests
  from typing import Optional

  API_URL: str = "https://api.example.com"
  TIMEOUT: int = 30

  def fetch_data(endpoint: str, token: str) -> Optional[dict]:
      """Fetch data from API endpoint."""
      try:
          response = requests.get(
              f"{API_URL}/{endpoint}",
              headers={"Authorization": f"Bearer {token}"},
              timeout=TIMEOUT
          )
          response.raise_for_status()
          return response.json()
      except requests.RequestException as e:
          print(f"Error fetching data: {e}")
          return None
  ```

---

## 🌐 JavaScript / Node.js (used in n8n Function nodes)

- Always use **const/let** — never var
- Use **optional chaining** (`?.`) and **nullish coalescing** (`??`) where applicable
- Always return data in n8n expected format: `return items.map(item => ({ json: item }))`
- Example:
  ```javascript
  const items = $input.all();

  return items.map(item => {
    const data = item.json;
    return {
      json: {
        id: data?.id ?? 'unknown',
        status: data?.status ?? 'pending',
        processedAt: new Date().toISOString()
      }
    };
  });
  ```

---

## 📁 Folder Structure Convention

```
n8n-workflows/
├── .github/
│   └── copilot-instructions.md   ← You are here
├── workflows/
│   ├── ai-agents/                ← AI agent workflows
│   ├── integrations/             ← Third-party integrations
│   ├── automations/              ← Day-to-day automations
│   └── templates/                ← Reusable workflow templates
├── scripts/
│   ├── python/                   ← Python helper scripts
│   └── javascript/               ← JS helper functions
├── docs/
│   └── workflow-registry.md      ← List of all workflows + purpose
├── .env.example                  ← Sample env variables (no real values)
└── README.md
```

---

## 📝 Commit Message Convention

Follow **Conventional Commits**:
- `feat:` new workflow or feature
- `fix:` bug fix in workflow
- `refactor:` restructuring without behavior change
- `docs:` documentation updates
- `chore:` maintenance tasks

Examples:
```
feat: add AI agent workflow for Jira ticket triage
fix: handle empty response in Slack notification workflow
docs: update webhook URLs in README
```

---

## 🚫 Things Copilot Should NEVER Do

- Never suggest hardcoded credentials or API keys
- Never suggest deprecated n8n node types
- Never suggest synchronous HTTP calls without timeout
- Never generate workflows without error handling
- Never use `console.log` in production code — use structured logging
- Never suggest storing sensitive data in workflow variables
