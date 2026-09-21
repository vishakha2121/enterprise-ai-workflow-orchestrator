<!-- 
  ═══════════════════════════════════════════════════════════════
  ENTERPRISE AI WORKFLOW ORCHESTRATOR - README.md
  ═══════════════════════════════════════════════════════════════
-->

<div align="center">

# 🚀 Enterprise AI Workflow Orchestrator

### *Build workflows with AI. Execute them with confidence. Approve them with control.*

An intelligent automation platform where AI agents dynamically build, optimize, and execute complex business workflows with automatic recovery and human-in-the-loop approval.

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.109+-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18+-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://reactjs.org)
[![LangGraph](https://img.shields.io/badge/LangGraph-0.1+-1C3C3C?style=for-the-badge&logo=langchain&logoColor=white)](https://langchain.com)
[![Temporal](https://img.shields.io/badge/Temporal-1.22+-000000?style=for-the-badge&logo=temporal&logoColor=white)](https://temporal.io)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://postgresql.org)
[![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
[![Made with ❤️](https://img.shields.io/badge/Made%20with-❤️-red.svg?style=flat-square)](https://github.com/vishakha2121)

[Features](#-key-features) • [Architecture](#-architecture) • [Quick Start](#-quick-start) • [Tech Stack](#-tech-stack) • [Documentation](#-documentation) • [Demo](#-demo)

</div>

---

## 📌 Overview

**Enterprise AI Workflow Orchestrator** is a next-generation business process automation platform that reimagines how organizations design, execute, and govern complex workflows.

Unlike traditional BPM systems that require rigid, pre-defined process definitions, this platform leverages **autonomous AI agents** to dynamically construct workflows from natural-language business intents.

Users simply describe what they want — *"Approve this invoice if amount < $5000 and vendor is trusted"* — and the system:

1. 🧠 **Plans** — AI agents decompose the intent into executable steps
2. ⚙️ **Executes** — Temporal durably runs each step with automatic retries
3. 👤 **Pauses** — For human approval at critical decision points
4. 🔄 **Recovers** — From failures using intelligent fallback strategies
5. 📊 **Visualizes** — Everything via a beautiful React dashboard in real-time

> **In short:** It's the **Figma for business workflows** — AI generates them, Temporal runs them, humans approve them.

---

## ✨ Key Features

<table>
<tr>
<td width="50%">

### 🤖 AI-Powered Workflow Generation
- **Multi-agent graph** built with LangGraph
- **Planner Agent** decomposes intent into steps
- **Executor Agent** invokes tools intelligently
- **Validator Agent** enforces business rules
- **Recovery Agent** handles failures
- Powered by **Google Gemini 1.5 Flash** (free tier)

</td>
<td width="50%">

### ⚡ Durable Execution
- Built on **Temporal** for reliability
- Survives crashes, restarts, network failures
- **Automatic retries** with exponential backoff
- Long-running workflows (days/weeks)
- Event-sourced state management
- **Signals & queries** for external control

</td>
</tr>
<tr>
<td width="50%">

### 👤 Human-in-the-Loop Approval
- Workflows **pause** at critical decision points
- **Real-time WebSocket** notifications
- Approve/Reject via intuitive dashboard
- Complete **approval history & audit trail**
- **Resumes seamlessly** after approval
- Role-based approver assignment

</td>
<td width="50%">

### 📊 BPMN 2.0 Support
- **Import** existing BPMN XML diagrams
- **Drag-and-drop** BPMN editor (bpmn-js)
- Convert **BPMN ↔ internal workflow**
- **Export** AI-generated workflows to BPMN
- Industry-standard process notation
- Interoperable with Camunda, Signavio, etc.

</td>
</tr>
<tr>
<td width="50%">

### 🎨 Modern UI/UX
- Beautiful **dark-themed** dashboard
- **Real-time updates** via WebSocket
- Interactive **execution timeline**
- **Drag-and-drop** workflow builder
- Agent chat playground
- Fully **responsive** design
- Loading skeletons + toast notifications

</td>
<td width="50%">

### 🔐 Enterprise-Grade Security
- **JWT authentication** with refresh tokens
- **Bcrypt** password hashing
- **Role-based access control** (RBAC)
- Complete **audit logging**
- SQL injection prevention (ORM)
- **Rate limiting** per user
- CORS whitelist

</td>
</tr>
</table>

---

## 🏗️ Architecture

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                       🌐 REACT FRONTEND (Vite)                      │
│  ┌──────────┐ ┌──────────────┐ ┌───────────┐ ┌──────────────────┐  │
│  │Dashboard │ │Workflow List │ │BPMN Editor│ │Approval Queue    │  │
│  └──────────┘ └──────────────┘ └───────────┘ └──────────────────┘  │
│  ┌──────────┐ ┌──────────────┐ ┌───────────┐ ┌──────────────────┐  │
│  │Executions│ │ Agent Chat   │ │  Settings │ │  Real-time WS    │  │
│  └──────────┘ └──────────────┘ └───────────┘ └──────────────────┘  │
└──────────────────────────────┬──────────────────────────────────────┘
                               │ REST API + WebSocket
┌──────────────────────────────▼──────────────────────────────────────┐
│                     🚀 FASTAPI BACKEND                              │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  API Layer (v1)                                             │   │
│  │  /auth  /workflows  /executions  /approvals  /agents  /bpmn│   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  🤖 AI Agent Engine (LangGraph)                             │   │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐    │   │
│  │  │ Planner  │─▶│ Executor │─▶│Validator │─▶│ Recovery │    │   │
│  │  └──────────┘  └──────────┘  └──────────┘  └──────────┘    │   │
│  │         │           │             │             │           │   │
│  │         ▼           ▼             ▼             ▼           │   │
│  │   [Gemini]    [Tools]      [Rules]      [Fallback]         │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  ⚡ Workflow Engine (Temporal)                              │   │
│  │  ┌────────────────────┐    ┌─────────────────────┐         │   │
│  │  │ Dynamic Workflow   │    │  Activities         │         │   │
│  │  │ - Execute Step     │◀──▶│  - Agent Activity   │         │   │
│  │  │ - Wait Approval    │    │  - Step Activity    │         │   │
│  │  │ - Retry Failed     │    │  - Notify Activity  │         │   │
│  │  └────────────────────┘    └─────────────────────┘         │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  📊 BPMN Processing                                         │   │
│  │  XML Parser → Element Mapper → Workflow Converter           │   │
│  └─────────────────────────────────────────────────────────────┘   │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │  🔌 WebSocket Manager (Real-time Broadcast)                 │   │
│  └─────────────────────────────────────────────────────────────┘   │
└──────────┬──────────────────────┬───────────────────────┬──────────┘
           │                      │                       │
    ┌──────▼──────┐    ┌──────────▼─────────┐    ┌───────▼────────┐
    │ PostgreSQL  │    │  Temporal Server   │    │  Gemini API    │
    │   (Data)    │    │  + Temporal UI     │    │  (LLM Cloud)   │
    └─────────────┘    └────────────────────┘    └────────────────┘
```

### Workflow Lifecycle

```
User Intent → AI Planning → Workflow Creation → Temporal Execution
     │              │               │                    │
     │              │               │                    ▼
     │              │               │         ┌─────────────────┐
     │              │               │         │ Step Execution  │
     │              │               │         └────────┬────────┘
     │              │               │                  │
     │              │               │         ┌────────▼────────┐
     │              │               │         │  Success?       │
     │              │               │         └────────┬────────┘
     │              │               │              ┌───┴───┐
     │              │               │            Yes      No
     │              │               │             │        │
     │              │               │             │    ┌───▼────┐
     │              │               │             │    │Retry/  │
     │              │               │             │    │Recovery│
     │              │               │             │    └───┬────┘
     │              │               │             │        │
     │              │               │    ┌────────▼────────▼───┐
     │              │               │    │ Approval Required?  │
     │              │               │    └────────┬────────────┘
     │              │               │        ┌────┴────┐
     │              │               │       Yes       No
     │              │               │        │         │
     │              │               │   ┌────▼────┐    │
     │              │               │   │ PAUSE   │    │
     │              │               │   │Notify WS│    │
     │              │               │   │Wait Sig │    │
     │              │               │   └────┬────┘    │
     │              │               │        │         │
     │              │               │   ┌────▼─────────▼───┐
     │              │               │   │  Next Step       │
     │              │               │   └────────┬─────────┘
     │              │               │            │
     │              │               │     ┌──────▼──────┐
     │              │               │     │  Complete   │
     │              │               │     └─────────────┘
```

---

## 🛠️ Tech Stack

### Backend
| Technology | Purpose |
|------------|---------|
| **Python 3.11+** | Core language |
| **FastAPI** | Web framework (async, auto-docs) |
| **SQLAlchemy 2.0** | ORM |
| **Alembic** | Database migrations |
| **Pydantic v2** | Data validation |
| **python-jose** | JWT tokens |
| **passlib[bcrypt]** | Password hashing |
| **PostgreSQL 15** | Primary database |

### AI & Agents
| Technology | Purpose |
|------------|---------|
| **LangGraph** | Multi-agent orchestration |
| **LangChain** | LLM framework |
| **Google Gemini 1.5 Flash** | LLM (free tier) |
| **langchain-google-genai** | Gemini integration |

### Workflow Engine
| Technology | Purpose |
|------------|---------|
| **Temporal** | Durable workflow execution |
| **temporalio** | Python SDK |
| **Temporal UI** | Workflow monitoring |

### BPMN
| Technology | Purpose |
|------------|---------|
| **xml.etree** | XML parsing (backend) |
| **bpmn-js** | BPMN rendering (frontend) |
| **bpmn-js-properties-panel** | BPMN editor |

### Frontend
| Technology | Purpose |
|------------|---------|
| **React 18** | UI library |
| **Vite** | Build tool |
| **Tailwind CSS** | Styling |
| **React Router v6** | Routing |
| **Zustand** | State management |
| **Axios** | HTTP client |
| **React Hook Form** | Form handling |
| **Zod** | Schema validation |
| **Recharts** | Charts |
| **ReactFlow** | Agent graph visualization |
| **Framer Motion** | Animations |
| **React Hot Toast** | Notifications |
| **Lucide React** | Icons |

### DevOps
| Technology | Purpose |
|------------|---------|
| **Docker** | Containerization |
| **Docker Compose** | Multi-container orchestration |
| **Nginx** | Reverse proxy |
| **WebSockets** | Real-time communication |

---

## 🚀 Quick Start

### Prerequisites

Ensure you have the following installed:

| Tool | Version | Check Command |
|------|---------|---------------|
| Python | 3.11+ | `python --version` |
| Node.js | 20+ | `node --version` |
| npm | 10+ | `npm --version` |
| Docker | 24+ | `docker --version` |
| Docker Compose | 2.20+ | `docker compose version` |
| Git | 2.40+ | `git --version` |

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/vishakha2121/enterprise-ai-workflow-orchestrator.git
cd enterprise-ai-workflow-orchestrator
```

### 2️⃣ Get Your Free Gemini API Key

1. Visit [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Click **"Create API Key"**
3. Copy the key — you'll need it in step 4

> 💡 **Free tier:** 60 requests/minute — perfect for development

### 3️⃣ Start Infrastructure (Postgres + Temporal)

```bash
docker-compose up -d postgres temporal temporal-ui
```

**Verify:**
- PostgreSQL: `localhost:5432`
- Temporal UI: http://localhost:8080

### 4️⃣ Setup Backend

```bash
cd backend

# Create virtual environment
python -m venv venv

# Activate (Windows)
venv\Scripts\activate

# Activate (Mac/Linux)
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Copy environment file
cp .env.example .env

# ⚠️ Edit .env and add your GEMINI_API_KEY
```

**Edit `.env`:**
```env
GEMINI_API_KEY=your_actual_gemini_api_key_here
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/workflow_db
TEMPORAL_HOST=localhost:7233
JWT_SECRET_KEY=your_super_secret_key_change_this
```

**Run migrations and seed data:**
```bash
alembic upgrade head
python scripts/seed_db.py
```

**Start FastAPI server:**
```bash
uvicorn app.main:app --reload --port 8000
```

✅ Backend running at http://localhost:8000
✅ API docs at http://localhost:8000/docs

### 5️⃣ Start Temporal Worker (New Terminal)

```bash
cd backend
source venv/bin/activate  # Windows: venv\Scripts\activate
python scripts/run_temporal_worker.py
```

### 6️⃣ Setup Frontend (New Terminal)

```bash
cd frontend

# Install dependencies
npm install

# Copy environment file
cp .env.example .env
```

**Edit `frontend/.env`:**
```env
VITE_API_URL=http://localhost:8000
VITE_WS_URL=ws://localhost:8000/ws
```

**Start dev server:**
```bash
npm run dev
```

✅ Frontend running at http://localhost:5173

### 7️⃣ Open the App

| Service | URL |
|---------|-----|
| 🌐 **Frontend** | http://localhost:5173 |
| 🚀 **Backend API** | http://localhost:8000 |
| 📚 **API Docs** | http://localhost:8000/docs |
| ⚡ **Temporal UI** | http://localhost:8080 |
| 🗄️ **PostgreSQL** | localhost:5432 |

**Default Login:**
```
Email:    admin@example.com
Password: admin123
```

> ⚠️ **Change the default password immediately after first login!**

---

## 📁 Project Structure

```
enterprise-ai-workflow-orchestrator/
│
├── 📄 README.md                          # This file
├── 📄 LICENSE                            # MIT License
├── 📄 .gitignore                         # Git ignore rules
├── 📄 .env.example                       # Environment template
├── 🐳 docker-compose.yml                 # Full stack orchestration
│
├── 📂 backend/                           # Python FastAPI backend
│   ├── 📄 requirements.txt
│   ├── 📄 Dockerfile
│   ├── 📄 alembic.ini
│   ├── 📂 alembic/                       # DB migrations
│   │   ├── env.py
│   │   ├── script.py.mako
│   │   └── versions/
│   │       ├── 001_initial_schema.py
│   │       ├── 002_add_workflow_tables.py
│   │       └── 003_add_approval_tables.py
│   │
│   ├── 📂 app/
│   │   ├── main.py                       # FastAPI entrypoint
│   │   ├── config.py                     # Settings
│   │   ├── dependencies.py               # DI
│   │   │
│   │   ├── 📂 api/v1/                    # REST endpoints
│   │   │   ├── auth.py
│   │   │   ├── workflows.py
│   │   │   ├── executions.py
│   │   │   ├── approvals.py
│   │   │   ├── agents.py
│   │   │   ├── bpmn.py
│   │   │   ├── health.py
│   │   │   └── websocket.py
│   │   │
│   │   ├── 📂 core/                      # Security, logging
│   │   │   ├── security.py
│   │   │   ├── logging.py
│   │   │   ├── exceptions.py
│   │   │   ├── constants.py
│   │   │   └── events.py
│   │   │
│   │   ├── 📂 db/                        # Database setup
│   │   │   ├── base.py
│   │   │   ├── session.py
│   │   │   └── init_db.py
│   │   │
│   │   ├── 📂 models/                    # SQLAlchemy models
│   │   │   ├── user.py
│   │   │   ├── workflow.py
│   │   │   ├── workflow_step.py
│   │   │   ├── execution.py
│   │   │   ├── execution_log.py
│   │   │   ├── approval.py
│   │   │   ├── agent.py
│   │   │   └── audit_log.py
│   │   │
│   │   ├── 📂 schemas/                   # Pydantic schemas
│   │   │   ├── user.py
│   │   │   ├── workflow.py
│   │   │   ├── execution.py
│   │   │   ├── approval.py
│   │   │   ├── agent.py
│   │   │   ├── bpmn.py
│   │   │   └── common.py
│   │   │
│   │   ├── 📂 services/                  # Business logic
│   │   │   ├── auth_service.py
│   │   │   ├── workflow_service.py
│   │   │   ├── execution_service.py
│   │   │   ├── approval_service.py
│   │   │   ├── agent_service.py
│   │   │   ├── bpmn_service.py
│   │   │   └── notification_service.py
│   │   │
│   │   ├── 📂 agents/                    # 🤖 LangGraph agents
│   │   │   ├── graph_builder.py
│   │   │   ├── state.py
│   │   │   ├── 📂 nodes/
│   │   │   │   ├── planner_node.py
│   │   │   │   ├── executor_node.py
│   │   │   │   ├── validator_node.py
│   │   │   │   ├── recovery_node.py
│   │   │   │   └── approval_node.py
│   │   │   ├── 📂 tools/
│   │   │   │   ├── email_tool.py
│   │   │   │   ├── http_tool.py
│   │   │   │   ├── db_tool.py
│   │   │   │   └── slack_tool.py
│   │   │   ├── 📂 prompts/
│   │   │   │   ├── planner_prompt.py
│   │   │   │   ├── executor_prompt.py
│   │   │   │   └── recovery_prompt.py
│   │   │   └── 📂 llm/
│   │   │       └── gemini_client.py
│   │   │
│   │   ├── 📂 temporal/                  # ⚡ Temporal workflows
│   │   │   ├── client.py
│   │   │   ├── worker.py
│   │   │   ├── 📂 workflows/
│   │   │   │   ├── dynamic_workflow.py
│   │   │   │   └── approval_workflow.py
│   │   │   ├── 📂 activities/
│   │   │   │   ├── agent_activity.py
│   │   │   │   ├── step_activity.py
│   │   │   │   ├── notification_activity.py
│   │   │   │   └── recovery_activity.py
│   │   │   └── config.py
│   │   │
│   │   ├── 📂 bpmn/                      # 📊 BPMN processing
│   │   │   ├── parser.py
│   │   │   ├── converter.py
│   │   │   ├── validator.py
│   │   │   └── 📂 templates/
│   │   │       ├── approval_flow.bpmn
│   │   │       ├── invoice_flow.bpmn
│   │   │       └── onboarding_flow.bpmn
│   │   │
│   │   ├── 📂 websocket/                 # 🔌 Real-time
│   │   │   ├── manager.py
│   │   │   └── handlers.py
│   │   │
│   │   └── 📂 utils/
│   │       ├── helpers.py
│   │       ├── validators.py
│   │       └── datetime_utils.py
│   │
│   ├── 📂 tests/
│   │   ├── conftest.py
│   │   ├── test_api/
│   │   ├── test_agents/
│   │   ├── test_services/
│   │   └── test_temporal/
│   │
│   └── 📂 scripts/
│       ├── seed_db.py
│       ├── create_admin.py
│       └── run_temporal_worker.py
│
├── 📂 frontend/                          # React frontend
│   ├── 📄 package.json
│   ├── 📄 vite.config.js
│   ├── 📄 tailwind.config.js
│   ├── 📄 postcss.config.js
│   ├── 📄 index.html
│   ├── 📄 Dockerfile
│   │
│   ├── 📂 public/
│   │   ├── favicon.ico
│   │   ├── logo.svg
│   │   └── manifest.json
│   │
│   └── 📂 src/
│       ├── main.jsx
│       ├── App.jsx
│       ├── index.css
│       │
│       ├── 📂 api/                       # API clients
│       │   ├── axiosClient.js
│       │   ├── authApi.js
│       │   ├── workflowApi.js
│       │   ├── executionApi.js
│       │   ├── approvalApi.js
│       │   ├── agentApi.js
│       │   └── bpmnApi.js
│       │
│       ├── 📂 components/
│       │   ├── 📂 common/                # Reusable UI
│       │   │   ├── Button.jsx
│       │   │   ├── Input.jsx
│       │   │   ├── Card.jsx
│       │   │   ├── Modal.jsx
│       │   │   ├── Loader.jsx
│       │   │   ├── Badge.jsx
│       │   │   ├── Toast.jsx
│       │   │   ├── SkeletonLoader.jsx
│       │   │   └── ConfirmDialog.jsx
│       │   │
│       │   ├── 📂 layout/
│       │   │   ├── Sidebar.jsx
│       │   │   ├── Topbar.jsx
│       │   │   ├── Layout.jsx
│       │   │   └── Breadcrumb.jsx
│       │   │
│       │   ├── 📂 dashboard/
│       │   │   ├── StatsCard.jsx
│       │   │   ├── RecentExecutions.jsx
│       │   │   ├── WorkflowChart.jsx
│       │   │   ├── ApprovalQueue.jsx
│       │   │   └── ActivityFeed.jsx
│       │   │
│       │   ├── 📂 workflows/
│       │   │   ├── WorkflowList.jsx
│       │   │   ├── WorkflowCard.jsx
│       │   │   ├── WorkflowForm.jsx
│       │   │   ├── StepBuilder.jsx
│       │   │   └── WorkflowFilter.jsx
│       │   │
│       │   ├── 📂 bpmn/
│       │   │   ├── BPMNViewer.jsx
│       │   │   ├── BPMNEditor.jsx
│       │   │   └── BPMNToolbar.jsx
│       │   │
│       │   ├── 📂 executions/
│       │   │   ├── ExecutionList.jsx
│       │   │   ├── ExecutionTimeline.jsx
│       │   │   ├── ExecutionLogs.jsx
│       │   │   └── ExecutionStatus.jsx
│       │   │
│       │   ├── 📂 approvals/
│       │   │   ├── ApprovalList.jsx
│       │   │   ├── ApprovalCard.jsx
│       │   │   └── ApprovalModal.jsx
│       │   │
│       │   ├── 📂 agents/
│       │   │   ├── AgentList.jsx
│       │   │   ├── AgentGraph.jsx
│       │   │   └── AgentChat.jsx
│       │   │
│       │   └── 📂 charts/
│       │       ├── LineChart.jsx
│       │       ├── BarChart.jsx
│       │       └── StatusDonut.jsx
│       │
│       ├── 📂 pages/
│       │   ├── Login.jsx
│       │   ├── Register.jsx
│       │   ├── Dashboard.jsx
│       │   ├── Workflows.jsx
│       │   ├── WorkflowCreate.jsx
│       │   ├── WorkflowDetail.jsx
│       │   ├── Executions.jsx
│       │   ├── ExecutionDetail.jsx
│       │   ├── Approvals.jsx
│       │   ├── Agents.jsx
│       │   ├── AgentPlayground.jsx
│       │   ├── BPMNDesigner.jsx
│       │   ├── Settings.jsx
│       │   ├── Profile.jsx
│       │   └── NotFound.jsx
│       │
│       ├── 📂 context/
│       │   ├── AuthContext.jsx
│       │   ├── ThemeContext.jsx
│       │   ├── NotificationContext.jsx
│       │   └── WebSocketContext.jsx
│       │
│       ├── 📂 hooks/
│       │   ├── useAuth.js
│       │   ├── useWorkflows.js
│       │   ├── useExecutions.js
│       │   ├── useApprovals.js
│       │   ├── useWebSocket.js
│       │   ├── useDebounce.js
│       │   └── usePagination.js
│       │
│       ├── 📂 routes/
│       │   ├── AppRoutes.jsx
│       │   ├── PrivateRoute.jsx
│       │   └── PublicRoute.jsx
│       │
│       ├── 📂 store/
│       │   ├── authStore.js
│       │   ├── workflowStore.js
│       │   ├── executionStore.js
│       │   └── uiStore.js
│       │
│       ├── 📂 utils/
│       │   ├── formatters.js
│       │   ├── validators.js
│       │   ├── constants.js
│       │   └── helpers.js
│       │
│       └── 📂 styles/
│           ├── globals.css
│           ├── animations.css
│           └── themes.css
│
├── 📂 database/                          # SQL schemas
│   ├── schema.sql
│   ├── seed_data.sql
│   ├── 📂 migrations/
│   │   ├── 001_init.sql
│   │   ├── 002_workflows.sql
│   │   ├── 003_executions.sql
│   │   ├── 004_approvals.sql
│   │   └── 005_audit.sql
│   └── 📂 erd/
│       └── erd_diagram.png
│
├── 📂 temporal/                          # Temporal config
│   ├── docker-compose.temporal.yml
│   ├── 📂 dynamicconfig/
│   │   └── development.yaml
│   └── README.md
│
├── 📂 docs/                              # Documentation
│   ├── ARCHITECTURE.md
│   ├── API_DOCS.md
│   ├── SETUP_GUIDE.md
│   ├── USER_GUIDE.md
│   ├── AGENT_FLOW.md
│   ├── CONTRIBUTING.md
│   ├── 📂 diagrams/
│   │   ├── architecture.png
│   │   ├── agent_graph.png
│   │   └── workflow_flow.png
│   └── 📂 screenshots/
│       ├── dashboard.png
│       ├── builder.png
│       ├── bpmn.png
│       └── approvals.png
│
├── 📂 nginx/                             # Reverse proxy
│   ├── nginx.conf
│   └── Dockerfile
│
└── 📂 scripts/                           # Utility scripts
    ├── setup.sh
    ├── start-dev.sh
    ├── start-prod.sh
    └── reset-db.sh
```

---

## 🎯 How It Works

### Real-World Example: Invoice Approval

**Step 1: User Describes Intent**

In the dashboard, user types:
> *"Process invoice from Acme Corp for $3,500. Approve if vendor is trusted, otherwise escalate to manager."*

**Step 2: AI Planner Generates Workflow**

The **Planner Agent** (powered by Gemini) produces:
```json
{
  "workflow_name": "Invoice Approval - Acme Corp",
  "steps": [
    {
      "id": 1,
      "name": "Extract invoice data",
      "type": "tool",
      "tool": "ocr_extract",
      "input": {"document_id": "inv_12345"}
    },
    {
      "id": 2,
      "name": "Validate vendor trust score",
      "type": "tool",
      "tool": "db_lookup",
      "input": {"vendor": "Acme Corp", "field": "trust_score"}
    },
    {
      "id": 3,
      "name": "Check approval threshold",
      "type": "condition",
      "condition": "trust_score > 80 AND amount < 5000",
      "on_true": "auto_approve",
      "on_false": "request_manager_approval"
    },
    {
      "id": 4,
      "name": "Request manager approval",
      "type": "approval",
      "requires_approval": true,
      "assignee_role": "manager"
    },
    {
      "id": 5,
      "name": "Send confirmation email",
      "type": "tool",
      "tool": "email_send",
      "input": {"to": "vendor@acme.com", "template": "invoice_approved"}
    }
  ]
}
```

**Step 3: Temporal Executes Durably**

Each step runs as a **Temporal Activity** with:
- ✅ 3 automatic retries
- ✅ Exponential backoff
- ✅ 5-minute timeout
- ✅ Persistent state (survives crashes)

**Step 4: Human Approves**

At step 4, the workflow **pauses**:
- 🔔 WebSocket notification sent to manager's dashboard
- 📧 Email notification sent
- ⏸️ Temporal waits for signal (`