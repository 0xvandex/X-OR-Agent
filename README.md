# OpenCode Cybersecurity Agent

An AI-powered cybersecurity agent framework built on top of **OpenCode**, designed to assist with authorized security testing, reconnaissance, vulnerability analysis, exploitation planning, and security reporting.

The project combines specialized AI agents with reusable cybersecurity Skills and shared engagement state to create a modular security-testing workflow.

> **Important:** This project is intended for authorized security testing, bug bounty programs, CTFs, labs, and environments where you have explicit permission to test.

---

## Overview

The goal of this project is to turn OpenCode into a cybersecurity-focused multi-agent system.

Instead of relying on one general-purpose agent, the framework uses specialized agents for different security domains.

An **Operator** coordinates the engagement and delegates tasks to specialized agents.

```text
                         ┌─────────────────┐
                         │     Operator    │
                         │  Orchestration  │
                         └────────┬────────┘
                                  │
              ┌───────────────────┼───────────────────┐
              │                   │                   │
              ▼                   ▼                   ▼
        ┌───────────┐       ┌────────────┐      ┌────────────┐
        │  Recon    │       │ Exploitation│      │  Reporting │
        │  Agents   │       │   Agents    │      │   Agents   │
        └─────┬─────┘       └──────┬─────┘      └──────┬─────┘
              │                    │                    │
              └────────────────────┼────────────────────┘
                                   ▼
                         ┌──────────────────┐
                         │ Shared Engagement│
                         │      State       │
                         └──────────────────┘
```

---

# Architecture

The project is divided into three main layers:

### Agents

Agents define **who is responsible for a task**.

They provide:

* Role
* Responsibilities
* Decision making
* Task delegation
* Coordination
* Security boundaries

Examples:

```text
ai-recon
web-hunter
api-security
bizlogic-hunter
code-auditor
exploit-chainer
bug-bounty
operator
```

---

### Skills

Skills define **how a security task should be performed**.

A Skill contains the methodology, workflow, reasoning process, checks, and expected outputs for a specific security problem.

Examples:

```text
hunt-idor
hunt-oauth
hunt-jwt-crypto
hunt-ssrf
hunt-xss
hunt-race-condition
hunt-business-logic
hunt-source-leak
hunt-shadow-api
hunt-spa-api
```

Agents receive explicit permission to use the Skills relevant to their role.

This keeps the agent architecture separate from the actual security methodology.

---

### Pentest State

The `pentest/` directory contains shared engagement data.

It is used for information that should persist between agents and stages of an engagement.

```text
pentest/
├── finding.db
├── knowledge/
├── scripts/
└── scope.json
```

This allows agents to work from shared findings instead of keeping important discoveries isolated inside individual conversations.

---

# Repository Structure

```text
.
├── agents/
│   ├── operator.md
│   ├── ai-recon.md
│   ├── web-hunter.md
│   ├── api-security.md
│   ├── bizlogic-hunter.md
│   ├── code-auditor.md
│   ├── exploit-chainer.md
│   ├── bug-bounty.md
│   └── ...
│
├── skills/
│   ├── recon/
│   ├── web/
│   ├── api/
│   ├── auth/
│   ├── hunt-idor/
│   ├── hunt-xss/
│   ├── hunt-ssrf/
│   ├── hunt-oauth/
│   ├── hunt-business-logic/
│   ├── hunt-race-condition/
│   └── ...
│
├── pentest/
│   ├── finding.db
│   ├── knowledge/
│   ├── scripts/
│   └── scope.json
│
├── AGENTS.md
├── opencode.jsonc
└── README.md
```

---

# Agent Model

The framework currently contains specialized agents covering multiple areas of offensive security.

Examples include:

| Agent                | Responsibility                              |
| -------------------- | ------------------------------------------- |
| `operator`           | Engagement orchestration                    |
| `ai-recon`           | Reconnaissance and attack-surface discovery |
| `recon-advisor`      | Recon strategy and prioritization           |
| `web-hunter`         | Web application vulnerabilities             |
| `api-security`       | API security testing                        |
| `bizlogic-hunter`    | Business logic and authorization flaws      |
| `code-auditor`       | Source-code security analysis               |
| `vuln-scanner`       | Automated vulnerability discovery           |
| `exploit-guide`      | Exploitation methodology                    |
| `exploit-chainer`    | Vulnerability chaining                      |
| `payload-crafter`    | Payload and exploit construction            |
| `privesc-advisor`    | Privilege escalation                        |
| `lateral-movement`   | Lateral movement analysis                   |
| `cloud-security`     | Cloud security                              |
| `container-breakout` | Container and Kubernetes security           |
| `llm-redteam`        | LLM/AI security                             |
| `osint-collector`    | OSINT collection                            |
| `detection-engineer` | Detection and defensive analysis            |
| `bug-bounty`         | Bug bounty workflow and reporting           |

The Operator is responsible for deciding which specialized agent should handle a task.

---

# Skill System

Skills are intentionally separated from agents.

For example, an agent may have access to:

```text
hunt-idor
hunt-auth-bypass
hunt-oauth
hunt-jwt-crypto
hunt-session
hunt-business-logic
hunt-race-condition
```

But the agent is not required to blindly execute every Skill.

Instead, the Skill system gives the agent a toolbox of specialized methodologies.

This allows the agent to reason about:

```text
What do I know?
        ↓
What should I test?
        ↓
Which Skill applies?
        ↓
What information should I obtain?
        ↓
What should I investigate next?
```

---

# Permission Model

Agents use explicit Skill permissions.

Example:

```yaml
permission:
  skill:
    "*": "deny"
    "recon": "allow"
    "web2-recon": "allow"
    "hunt-subdomain": "allow"
    "hunt-source-leak": "allow"
    "hunt-spa-api": "allow"
```

The default policy is:

```text
Deny everything
        ↓
Allow only relevant Skills
```

This prevents every agent from automatically having access to every methodology.

---

# Engagement Workflow

A typical authorized engagement follows this general flow:

```text
1. Define Scope
       ↓
2. Reconnaissance
       ↓
3. Attack Surface Mapping
       ↓
4. Vulnerability Hypothesis
       ↓
5. Specialized Testing
       ↓
6. Validation
       ↓
7. Evidence Collection
       ↓
8. Impact Assessment
       ↓
9. Reporting
```

The Operator coordinates the workflow and delegates individual tasks to specialized agents.

---

# Example

An engagement might begin with:

```text
User
 │
 ▼
Operator
 │
 ├──► AI Recon
 │       │
 │       ├── recon
 │       ├── web2-recon
 │       ├── hunt-subdomain
 │       └── hunt-source-leak
 │
 ├──► Web Hunter
 │       │
 │       ├── hunt-idor
 │       ├── hunt-auth-bypass
 │       └── hunt-session
 │
 ├──► API Security
 │       │
 │       ├── hunt-oauth
 │       ├── hunt-jwt-crypto
 │       └── hunt-graphql
 │
 └──► Bug Bounty
         │
         ├── triage-validation
         ├── evidence-hygiene
         └── report-writing
```

Agents can build on discoveries made during earlier stages through the shared engagement state.

---

# Scope & Safety

This framework should only be used against systems where testing is explicitly authorized.

Before an engagement begins, define:

* Target scope
* Out-of-scope assets
* Allowed testing techniques
* Rate limits
* Authentication boundaries
* Data-handling restrictions
* Reporting requirements

The agent should respect the engagement rules throughout the entire workflow.

### Sensitive Data

If testing unexpectedly exposes data belonging to another user:

```text
STOP
↓
Do not enumerate
↓
Do not exfiltrate
↓
Preserve minimal evidence
↓
Report according to the engagement policy
```

If remote access or code execution is obtained unexpectedly:

```text
STOP
↓
Do not pivot
↓
Do not escalate privileges
↓
Preserve minimal evidence
↓
Report immediately
```

---

# Account Handling

Authenticated testing should use a dedicated authorized test account whenever possible.

The framework should not automatically request or store personal credentials.

If an engagement requires account creation:

```text
No authorized account
        ↓
Ask the user for approval
        ↓
Create/use a dedicated test account
        ↓
Test only within authorized boundaries
```

Credentials should never be hard-coded into agent definitions, Skills, or repository files.

---

# Evidence

Security findings should contain enough information to reproduce and validate the issue.

A useful finding should include:

```text
Target
Vulnerability
Affected endpoint
Attack type
Prerequisites
Steps to reproduce
Requests
Responses
Impact
Evidence
Authorization context
Data-access status
Recommended remediation
```

Evidence should be collected with the minimum amount of sensitive information necessary.

---

# Design Principles

The project follows several core principles.

### 1. Specialized Agents

A focused agent is easier to control and reason about than one giant agent responsible for everything.

### 2. Reusable Skills

Security methodologies should be reusable across multiple agents.

### 3. Explicit Permissions

Agents should only receive the Skills relevant to their responsibilities.

### 4. Shared Knowledge

Important discoveries should persist outside individual agent conversations.

### 5. Evidence-Driven Testing

Potential vulnerabilities should be validated with reproducible evidence rather than assumptions.

### 6. Scope First

The framework should understand the engagement scope before performing security testing.

### 7. Human Oversight

The AI assists with security research and decision making. The human remains responsible for authorization, scope, and final actions.

---

# Current Focus

The project is primarily focused on:

* Web application security
* API security
* Authentication and authorization
* IDOR / BOLA / BFLA
* OAuth / JWT / SAML
* Business logic vulnerabilities
* Race conditions
* SSRF
* XSS
* SQL/NoSQL injection
* File upload vulnerabilities
* Deserialization
* GraphQL / gRPC
* SPA/API attack surfaces
* Source leaks
* Cloud security
* Container security
* LLM security
* Bug bounty workflows
* Evidence and vulnerability reporting

---

# Status

This project is under active development.

The current priority is improving:

* Agent coordination
* Skill routing
* Shared engagement knowledge
* Finding management
* Evidence handling
* Autonomous planning
* Vulnerability validation
* Bug bounty reporting

The goal is not simply to create a collection of prompts.

The goal is to build a **coherent cybersecurity workflow where specialized agents can reason, share discoveries, and work together throughout an authorized engagement.**

---

# Disclaimer

This project is provided for educational and authorized security-testing purposes.

Do not use it to access, modify, disrupt, or extract data from systems without explicit authorization.

You are responsible for complying with applicable laws, program rules, contracts, and terms of service.

---

## Author

**Vandex**

Cybersecurity Researcher & AI Security Enthusiast
# 🤖 Offensive Agents — AI-Powered Penetration Testing Framework

An offensive security framework built on [OpenCode](https://opencode.ai) featuring a primary orchestration agent and 24 specialized sub-agents for authorized penetration testing, bug bounty programs, and CTF challenges.

> ⚠️ **Legal Disclaimer:** This framework is intended exclusively for authorized security testing, bug bounty programs, and controlled lab environments (HTB, TryHackMe, PortSwigger Labs). The user is solely responsible for ensuring proper authorization before testing any target.

---

## 🏗️ Architecture
```
Operator (Primary Agent)**
└── **24 Specialized Sub-Agents**
    ├── **Reconnaissance:** `recon-advisor`, `osint-collector`, `ai-recon`, `vuln-scanner`
    ├── **Web & API:** `web-hunter`, `api-security`, `bizlogic-hunter`, `bug-bounty`, `code-auditor`
    ├── **Network & Infra:** `network-attacker`, `ad-attacker`, `cloud-security`, `container-breakout`, `database-attacker`
    ├── **Exploitation:** `payload-crafter`, `exploit-guide`, `exploit-chainer`, `attack-planner`, `lateral-movement`, `privesc-advisor`
    └── **Specialized:** `llm-redteam`, `malware-analyst`, `detection-engineer`, `ctf-solver`
```
---

## ✨ Features

* **Smart Delegation** — Operator automatically routes tasks to the right sub-agent.
* **Scope Enforcement** — Every agent validates targets against `scope.json` before execution.
* **Findings Database** — SQLite DB with 8 tables tracking hosts, services, vulns, chains.
* **Knowledge Engine** — Persistent write-up and technique storage across engagements.
* **Skill Loading** — Each sub-agent loads its `SKILL.md` for specialized methodology.
* **MCP Integration** — Burp Suite, Playwright, Notion, Filesystem, Memory, GitHub.
* **OPSEC Tagging** — Every command tagged `QUIET` / `MODERATE` / `LOUD`.
* **Attack Chaining** — Automated multi-stage exploit path building.

---

## 🚀 Quick Start

### Requirements
* [OpenCode](https://opencode.ai) installed
* Node.js 18+
* An LLM API key (OpenRouter, OpenAI, Anthropic, etc.)

### Installation Steps

```bash
# 1. Clone the repo:
git clone https://github.com/Monesgoda/offensive-agents
cd offensive-agents

# 2. Copy config to OpenCode directory:
cp -r . ~/.config/opencode/

# 3. Configure your settings:
nano ~/.config/opencode/opencode.jsonc
# (Add your API key and model)

# 4. Set your engagement scope:
nano ~/.config/opencode/pentest/scope.json
# (Add your authorized target)

# 5. Initialize the findings database:
bash ~/.config/opencode/pentest/scripts/init_db.sh

# 6. Launch OpenCode:
opencode
```

---

## ⚙️ Configuration

### `opencode.jsonc`
```json
{
  "model": "YOUR_MODEL_HERE",
  "fallback": ["YOUR_FALLBACK_MODEL"],
  "mcp": {
    "filesystem": { ... },
    "memory": { ... },
    "github": { ... }
  }
}
```

### `pentest/scope.json`
```json
{
  "engagement": "YOUR_ENGAGEMENT_NAME",
  "target": "TARGET_DOMAIN",
  "in_scope": [],
  "out_of_scope": [],
  "rules": {
    "max_rate_rps": 10,
    "no_destructive_actions": true,
    "no_dos": true,
    "require_scope_check": true,
    "require_user_approval": true,
    "passive_first": true
  }
}
```

---

## 📁 Structure

```text
~/.config/opencode/
├── opencode.jsonc          ← Main config (API keys, MCP servers)
├── AGENTS.md               ← Agent index and documentation
├── agents/                 ← Global agents (loaded by OpenCode)
│   ├── operator.md         ← Primary orchestration agent
│   ├── recon-advisor.md    ← + 23 sub-agents
│   └── ...
└── pentest/                ← Shared framework layer
    ├── scope.json          ← Engagement scope rules
    ├── findings.db         ← SQLite findings database (auto-created)
    ├── memory.jsonl        ← Persistent memory graph
    ├── schema.sql          ← DB schema
    ├── agents/             ← Sub-agent source files
    │   ├── recon-advisor/
    │   │   ├── agent.md
    │   │   └── SKILL.md
    │   └── ...
    ├── knowledge/          ← Vulnerability knowledge engine
    │   ├── api/
    │   ├── web/
    │   ├── bypasses/
    │   └── ...
    └── scripts/
        ├── init_db.sh
        └── doctor.sh
```

---

## 🤝 Contributing

PRs welcome. If you add a new sub-agent:
1. Create `pentest/agents/[agent-name]/agent.md` + `SKILL.md`
2. Add the merged version to `agents/[agent-name].md`
3. Register it in `AGENTS.md` and `operator.md`

---

## 📺 Video Walkthrough

> 🎬 Full setup and demo video coming soon on YouTube.

---

## 📄 License

**MIT License** — Use responsibly and only on authorized targets.
