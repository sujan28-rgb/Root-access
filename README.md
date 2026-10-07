# Sentinel — Offline AI Incident Investigator

> **Investigate. Explain. Respond. Privately.**

Sentinel is an **offline AI-powered cybersecurity incident investigator** that analyzes locally collected computer evidence, detects suspicious activity, reconstructs incident timelines, and uses a **locally running Gemma 4 E4B model** to explain what happened and why it matters.

Unlike cloud-based security copilots, Sentinel keeps sensitive security data **entirely on the user's machine**. No security logs or evidence need to be sent to an external AI service.

---

## 🚨 Problem

Cybersecurity investigation often involves sensitive information such as:

- Authentication logs
- Process activity
- Network connections
- File activity
- System events
- User information

Sending this information to cloud-based AI systems can create privacy and security concerns.

At the same time, raw security logs are difficult for non-security professionals to understand.

Sentinel addresses both problems by combining:

**Deterministic security analysis + Local AI reasoning**

---

## 💡 Solution

Sentinel takes locally available computer evidence and processes it through several stages:

```text
Local Evidence
      ↓
Evidence Parser
      ↓
Detection Engine
      ↓
Event Correlation
      ↓
Structured Security Evidence
      ↓
Local Gemma 4 E4B
      ↓
AI Analysis
      ↓
Incident Timeline + Dashboard + Report
```

The entire AI inference pipeline runs locally.

---

## 🎯 Key Features

### 1. Local Evidence Analysis

Sentinel can analyze locally available:

- Authentication logs
- Process/activity logs
- File activity
- Network connection logs
- System events

The system does not require Internet access to analyze the evidence.

---

### 2. Automated Suspicious Activity Detection

A deterministic Python-based detection engine identifies suspicious patterns such as:

- Repeated authentication failures
- Unusual login activity
- Suspicious process execution
- Abnormal file activity
- Unusual network connections
- Temporally correlated events

Example:

```text
47 failed login attempts
        ↓
Same source IP
        ↓
Within 90 seconds
        ↓
Successful login
        ↓
HIGH-RISK EVENT
```

---

### 3. Event Correlation

Individual events are correlated to determine whether they may belong to the same incident.

Example:

```text
10:31:02 — Suspicious executable created
10:31:05 — PowerShell launched
10:31:11 — Network connection established
10:31:18 — Credential-related file accessed
10:31:24 — Administrator login
```

Sentinel can combine these events into a single incident timeline rather than treating them as unrelated alerts.

---

### 4. Local AI Investigation

Relevant structured evidence is passed to a **locally running Gemma 4 E4B model**.

The model provides:

- Threat classification
- Severity assessment
- Natural-language explanation
- Evidence-based reasoning
- Recommended investigation steps

Example:

```text
THREAT LEVEL: HIGH

LIKELY ACTIVITY:
Credential-guessing / brute-force activity

EVIDENCE:
• 47 failed authentication attempts
• Same source IP
• 90-second time window
• Successful login followed the failures

ANALYSIS:
The clustered authentication failures followed by
a successful login are consistent with credential
guessing activity.

RECOMMENDED INVESTIGATION:
Review the successful session and commands executed
after authentication.
```

---

### 5. "Why?" Explanation

Sentinel allows users to understand why an event was classified as suspicious.

Example:

```text
WHY WAS THIS MARKED HIGH RISK?

✓ 47 failed attempts
✓ Same source IP
✓ 90-second time window
✓ Successful login afterward

These observations triggered the
high-risk classification.
```

This makes security analysis understandable even to users without a cybersecurity background.

---

### 6. Incident Timeline

Sentinel converts raw events into an easy-to-understand timeline.

```text
10:31:02 ── Failed authentication
      │
10:31:05 ── Suspicious process created
      │
10:31:11 ── Network connection
      │
10:31:18 ── Credential access
      │
10:31:24 ── Successful login
      │
      ▼
   HIGH RISK
```

---

### 7. Incident Report

Sentinel can generate a structured incident report containing:

- Incident ID
- Severity
- Incident type
- Evidence
- Timeline
- AI analysis
- Recommended investigation steps

---

# 🧠 System Architecture

```text
                    LOCAL COMPUTER EVIDENCE
                  /          |          \
                 /           |           \
              Logs       Processes      Files
                 \           |           /
                  \          |          /
                   └─────────┬─────────┘
                             ↓
                    ┌─────────────────┐
                    │  Evidence       │
                    │  Parser         │
                    │  Python         │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │ Detection Engine │
                    │ Rules + Analysis │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │ Event Correlation│
                    └────────┬────────┘
                             ↓
                    Structured Evidence
                             ↓
                    ┌─────────────────┐
                    │     Ollama      │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │  Gemma 4 E4B    │
                    │   Local Model   │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │   AI Analysis   │
                    │ Explanation     │
                    │ Recommendations │
                    └────────┬────────┘
                             ↓
                    ┌─────────────────┐
                    │    Dashboard    │
                    │ Timeline        │
                    │ Evidence        │
                    │ Reports         │
                    └─────────────────┘
```

---

# 🔒 Offline-First Architecture

Sentinel is designed so that the core investigation process does not require an Internet connection.

```text
                INTERNET
                   ❌
                   │
                   X

        ┌──────────────────────────┐
        │      USER MACHINE       │
        │                          │
        │  Evidence                │
        │     ↓                    │
        │  Python                  │
        │     ↓                    │
        │  Detection Engine        │
        │     ↓                    │
        │  Ollama                  │
        │     ↓                    │
        │  Gemma 4 E4B             │
        │     ↓                    │
        │  RTX 4050                │
        │     ↓                    │
        │  Dashboard               │
        │                          │
        └──────────────────────────┘
```

No security evidence needs to leave the machine.

### Offline Demonstration

During the demonstration:

1. Disable Wi-Fi.
2. Load a prepared security evidence dataset.
3. Run Sentinel.
4. Analyze the evidence.
5. Generate the incident timeline.
6. Ask the local AI for an explanation.
7. Display the final report.

The system continues functioning because the AI model is running locally.

---

# 🤖 Local AI

### Model

**Gemma 4 E4B**

### Runtime

**Ollama**

### Hardware

**NVIDIA RTX 4050 Laptop GPU**

The local model is responsible primarily for:

- Reasoning over detected evidence
- Explaining suspicious activity
- Generating human-readable incident summaries
- Providing investigation recommendations
- Converting technical findings into understandable language

The deterministic detection layer remains separate from the LLM.

---

# 🛠️ Technology Stack

| Component | Technology |
|---|---|
| Local AI | Gemma 4 E4B |
| AI Runtime | Ollama |
| AI Hardware | NVIDIA RTX 4050 |
| Backend | Python |
| API | FastAPI |
| Detection | Python |
| Frontend | React |
| Database | SQLite |
| Visualization | Chart.js / React visualization |
| Version Control | Git + GitHub |

---

# 📁 Project Structure

```text
sentinel/
│
├── backend/
│   ├── main.py
│   ├── parser/
│   ├── detection/
│   ├── correlation/
│   ├── ai/
│   └── reports/
│
├── frontend/
│   ├── src/
│   ├── components/
│   └── pages/
│
├── data/
│   ├── sample_logs/
│   └── test_incidents/
│
├── tests/
│
├── README.md
├── requirements.txt
└── .gitignore
```

---

# ⚙️ Installation

## Prerequisites

- Python 3.11+
- Git
- Node.js
- Ollama
- NVIDIA GPU recommended for primary deployment

---

## 1. Install Ollama

Install Ollama for your operating system.

Verify:

```bash
ollama --version
```

---

## 2. Download the Local Model

```bash
ollama pull gemma4:e4b
```

Test:

```bash
ollama run gemma4:e4b
```

---

## 3. Clone the Repository

```bash
git clone <repository-url>
cd sentinel
```

---

## 4. Create Python Environment

```bash
python -m venv .venv
```

### Windows

```bash
.venv\Scripts\activate
```

### macOS/Linux

```bash
source .venv/bin/activate
```

---

## 5. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## 6. Start the Backend

```bash
uvicorn backend.main:app --reload
```

---

## 7. Start the Frontend

```bash
cd frontend
npm install
npm run dev
```

---

# 🔄 Example Workflow

### Input

```text
47 failed authentication attempts
from the same IP address
within 90 seconds
followed by a successful login.
```

### Detection Engine

```text
Event Type: Authentication anomaly
Attempts: 47
Source: 192.168.1.25
Duration: 90 seconds
Successful login: Yes

Risk: HIGH
```

### Local AI

Gemma 4 E4B analyzes the structured evidence.

### Output

```text
Potential credential-guessing activity.

The high number of authentication failures from a
single source within a short time period, followed
by a successful login, warrants investigation.

Recommended action:
Review the successful session and associated activity.
```

---

# 🧪 Testing Offline Mode

To verify that the project does not depend on cloud AI:

```text
1. Start Sentinel
2. Disable Wi-Fi
3. Load sample evidence
4. Run analysis
5. Generate AI explanation
```

The complete analysis should continue to function without Internet access.

---

# 👥 Team Responsibilities

### AI / Local Inference

- Ollama setup
- Gemma integration
- Prompt engineering
- Structured AI output

### Security Analysis

- Log parsing
- Detection rules
- Event correlation
- Risk scoring

### Frontend

- Dashboard
- Incident timeline
- Evidence visualization
- AI explanation interface

### Integration / QA

- API integration
- Testing
- Sample datasets
- Documentation
- Demo preparation

---

# 🚀 Future Improvements

If additional development time is available:

- Real-time system monitoring
- More detection rules
- Additional evidence sources
- Interactive incident graphs
- Natural-language investigation queries
- Automated report generation
- Additional local AI models
- Explainable risk scoring
- Cross-platform evidence collectors

---

# ⚠️ Scope & Disclaimer

Sentinel is an **incident investigation and analysis prototype**, not a replacement for a professional Security Operations Center, antivirus product, or endpoint detection and response platform.

Its findings should be treated as investigative guidance and verified by a security professional.

---

# 🏆 Hackathon Focus

Sentinel is designed around the Hack Day's local-AI requirement:

### Local

Security evidence remains on the user's machine.

### Open Source

The system is built around open-source development tools and locally deployed models.

### AI

Gemma 4 E4B provides local reasoning and explanation.

### Hardware

AI inference runs on the team's own hardware.

### Explainable

Every alert is accompanied by the evidence that contributed to the classification.

---

## License

This project is licensed under the MIT License - see the LICENSE file for details.
