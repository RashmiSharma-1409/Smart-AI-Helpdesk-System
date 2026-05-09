<p align="center">
  <img src="static/images/hero.png" alt="TicketEasy Banner" width="600"/>
</p>

<h1 align="center">🎫 TicketEasy — AI-Powered IT Helpdesk</h1>

<p align="center">
  <strong>by Team TechSlayers</strong> &nbsp;|&nbsp; Smart Innovation Hackathon (SIH) Project
</p>

<p align="center">
  <a href="#features">Features</a> •
  <a href="#architecture">Architecture</a> •
  <a href="#tech-stack">Tech Stack</a> •
  <a href="#getting-started">Getting Started</a> •
  <a href="#role-hierarchy">Role Hierarchy</a> •
  <a href="#project-structure">Project Structure</a> •
  <a href="#contributing">Contributing</a>
</p>

---

## 📖 Overview

**TicketEasy** is an intelligent, enterprise-grade IT helpdesk system built for the **Smart India Hackathon (SIH)**. It automates ticket classification, priority scoring, sentiment detection, and agent routing using AI — drastically reducing response times and manual triage overhead.

Designed for organizations like **PGCIL (PowerGrid Corporation of India)**, TicketEasy aggregates support requests from multiple channels and routes them to the best available agent using a load-balanced algorithm — all powered by NVIDIA NIM (LLaMA 3.1 8B Instruct).

---

## ✨ Features

### 🤖 AI-Powered Ticket Triage
- **Auto-Classification** — Tickets are automatically sorted into `Software`, `Hardware`, `Network`, or `Database` categories using NLP.
- **Priority Scoring** — AI determines `High`, `Medium`, or `Low` priority based on content analysis.
- **Sentiment Detection** — Detects 10 sentiments (`Furious`, `Frustrated`, `Urgent`, `Sad`, `Confused`, `Neutral`, `Happy`, `Grateful`, `Sarcastic`, `Professional`) to flag critical tickets.
- **Suggested Resolution Steps** — AI generates actionable troubleshooting steps for each ticket.

### 💬 Conversational AI Chatbot
- Embedded chatbot on every page for instant IT support.
- **Smart Intent Detection** — Automatically raises tickets, redirects to dashboards, or attempts to resolve issues in-chat.
- **Context-Aware** — Maintains conversation history for coherent multi-turn dialogue.
- **Quick Action Buttons** — One-click shortcuts for common tasks (Raise Ticket, Check Status, Password Reset).

### ⚖️ Intelligent Agent Routing
- **Least-Loaded Algorithm** — Assigns tickets to the agent with the fewest active (open/in-progress) tickets.
- **Category-Aware** — Only routes to agents specialized in the ticket's category.
- **Tie-Breaking** — Uses experience (resolved ticket count) as a secondary factor.

### 👥 Role-Based Access Control (RBAC)
- Four-tier role hierarchy: `Super Admin` → `Manager` → `Agent` → `Employee`.
- Each role sees only what they need — granular dashboard views and permissions.

### 📧 Email Notifications
- Automated email alerts on ticket creation, assignment, and status changes.
- Daily summary reports sent to the Super Admin at 6:00 PM.

### 📊 Dashboard Analytics
- Real-time stats: Total, Open, In-Progress, and Resolved tickets.
- Priority breakdown (High / Medium / Low) for agents and managers.
- Complete ticket history with filtering for employees.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        FRONTEND                             │
│  Jinja2 Templates • Inter Font • Glassmorphism UI           │
│  Landing → Auth → Employee Home → Raise Ticket → History    │
│  Agent Dashboard → Super Admin Dashboard → Ticket Detail    │
│  Embedded AI Chatbot (every page)                           │
├─────────────────────────────────────────────────────────────┤
│                     FLASK BACKEND                           │
│  Routes → Decorators (login_required, agent_required)       │
│  Session Auth • CORS • Background Scheduler                 │
├──────────────┬──────────────┬───────────────────────────────┤
│  AI ENGINE   │   CHATBOT    │   ROUTING ENGINE              │
│  NVIDIA NIM  │   LLaMA 3.1  │   Least-Loaded Balancer       │
│  (OpenAI SDK)│   Multi-turn │   Category-Aware Assignment   │
├──────────────┴──────────────┴───────────────────────────────┤
│                      DATA LAYER                             │
│  SQLite (SQLAlchemy ORM) • Redis (Round-Robin State)        │
│  Models: User, Ticket, Category, AdminCategories            │
└─────────────────────────────────────────────────────────────┘
```

---

## 🛠️ Tech Stack

| Layer        | Technology                                                  |
| ------------ | ----------------------------------------------------------- |
| **Backend**  | Python 3.10+, Flask, Flask-SQLAlchemy, Flask-Login           |
| **AI/NLP**   | NVIDIA NIM API (LLaMA 3.1 8B Instruct), OpenAI Python SDK   |
| **Database** | SQLite (dev), Redis (routing state)                          |
| **Frontend** | Jinja2, HTML5, CSS3 (Inter font, glassmorphism), Vanilla JS  |
| **Email**    | SMTP (Gmail), APScheduler for daily reports                  |
| **Validation** | Pydantic v2 for structured AI output parsing               |
| **Security** | Werkzeug password hashing, session-based auth, dotenv        |

---

## 🚀 Getting Started

### Prerequisites

- **Python 3.10+**
- **Redis** (optional — used for round-robin state; app works without it)
- **NVIDIA NIM API Key** ([Get one here](https://build.nvidia.com/)) — optional, app falls back to manual mode without it

### 1. Clone the Repository

```bash
git clone https://github.com/Rajeevkulkarni1111/TicketEasy_Tech_Slayers.git
cd TicketEasy_Tech_Slayers
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv

# Windows
.venv\Scripts\activate

# macOS / Linux
source .venv/bin/activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables

Create a `.env` file in the project root:

```env
# AI Configuration
NVIDIA_API_KEY=your_nvidia_nim_api_key
NVIDIA_MODEL_NAME=meta/llama-3.1-8b-instruct

# Redis (optional)
REDIS_URL=redis://localhost:6379

# Flask
FLASK_SECRET_KEY=your_random_secret_key_here

# Admin
SUPER_ADMIN_EMAIL=admin@example.com

# Email Notifications (Gmail App Password)
EMAIL_ADDRESS=your_email@gmail.com
EMAIL_PASSWORD=your_gmail_app_password
```

> **Note:** For Gmail, you need to generate an [App Password](https://support.google.com/accounts/answer/185833) (2FA must be enabled).

### 5. Initialize the Database & Run

```bash
# Start the application (auto-creates DB and seed categories)
python app.py
```

The app will be available at **http://localhost:5000**

### 6. (Optional) Create a Super Admin

```bash
python create_super.py
```

### 7. (Optional) Create a Department Manager

```bash
python setup_admin.py
```

---

## 🔐 Role Hierarchy

| Role            | Capabilities                                                                      |
| --------------- | --------------------------------------------------------------------------------- |
| **Super Admin** | Full system access, view all tickets across all categories, daily reports          |
| **Manager**     | View all tickets within their assigned category/department, manage agents          |
| **Agent**       | View and resolve only tickets assigned to them, update status and remarks          |
| **Employee**    | Raise tickets, track ticket status, view history, use AI chatbot for quick support |

```
Super Admin ──► Manager ──► Agent ──► Employee
   (God Mode)    (Dept Head)  (Worker)   (End User)
```

---

## 📁 Project Structure

```
TicketEasy_Tech_Slayers/
├── app.py                  # Main Flask application & routes
├── ai_engine.py            # NVIDIA NIM AI ticket analysis engine
├── chatbot.py              # Conversational AI chatbot module
├── models.py               # SQLAlchemy data models (User, Ticket, Category)
├── extensions.py           # Flask extensions & password utilities
├── requirements.txt        # Python dependencies
├── .env                    # Environment variables (not committed)
├── .gitignore              # Git ignore rules
│
├── setup_admin.py          # Script to create department managers
├── create_super.py         # Script to create super admin user
├── admin_operations.py     # Admin utility operations
├── migrate_db.py           # Database migration helper
├── migrate_sih.sql         # SQL migration script
├── final_fix.sql           # SQL fix script
│
├── static/
│   ├── css/
│   │   └── style.css       # Global stylesheet
│   └── images/
│       └── hero.png        # Landing page hero image
│
├── templates/
│   ├── base.html           # Base layout with header, footer & chatbot
│   ├── landing.html        # Public landing page
│   ├── auth.html           # Login/Register router
│   ├── login.html          # Login form
│   ├── register_employee.html
│   ├── register_agent.html
│   ├── agent_login.html
│   ├── employee_home.html  # Employee dashboard
│   ├── emp_history.html    # Employee ticket history
│   ├── raise_ticket.html   # New ticket form (AI-analyzed)
│   ├── agent_dashboard.html
│   ├── super_dashboard.html
│   └── ticket_detail.html  # Individual ticket view & update
│
└── instance/
    └── sih_helpdesk.db     # SQLite database (auto-generated)
```

---

## 🧠 How the AI Works

### Ticket Analysis Pipeline

```
User submits ticket
       │
       ▼
┌──────────────────┐
│  ai_engine.py    │
│                  │
│  Subject + Desc  │──► NVIDIA NIM (LLaMA 3.1 8B)
│                  │         │
│  Pydantic v2     │◄────────┘
│  Validation      │
│                  │
│  Output:         │
│  • Category      │
│  • Priority      │
│  • Sentiment     │
│  • Steps         │
└──────┬───────────┘
       │
       ▼
  Least-Loaded Router
  assigns to best Agent
       │
       ▼
  Email notifications
  sent to Agent + User
```

### Chatbot Flow

```
User message ──► Intent Detection (regex)
                      │
              ┌───────┴────────┐
              │                │
        Special Intent    General Query
        (raise ticket,    (sent to LLaMA 3.1
         dashboard,        with conversation
         login)            history)
              │                │
              ▼                ▼
         Redirect URL     AI Response
```

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

---

## 📜 License

This project was developed as part of the **Smart India Hackathon (SIH)** by **Team TechSlayers** Lead by [**Rajeev Kulkarni**](https://github.com/Rajeevkulkarni1111).

---

## 👨‍💻 Team TechSlayers

Built with ❤️ for the Smart India Hackathon.

<p align="center">
  <strong>TicketEasy</strong> — Smarter Support, Faster Solutions.
</p>
