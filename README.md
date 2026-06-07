# TenderFlow — Tender & Proposal Management System

A professional, lightweight Python web application for managing tender proposals across a structured 6-stage workflow.

## Tech Stack

| Layer       | Technology              | Why                                           |
|-------------|-------------------------|-----------------------------------------------|
| Backend     | FastAPI                 | Async, fast, auto docs, clean DX              |
| Templating  | Jinja2                  | Server-side rendering, no JS build step       |
| Database    | SQLite + SQLAlchemy     | Zero-config locally, swap to Postgres for prod|
| Frontend    | Tailwind CSS + Alpine.js| Utility-first CSS, lightweight reactivity     |
| Auth        | JWT (HTTP-only cookies) | Secure, stateless, no session storage needed  |
| Scheduler   | APScheduler             | Background deadline scan + notifications      |
| Email       | smtplib / fastapi-mail  | Standard SMTP, Gmail-compatible               |
| WhatsApp    | WhatsApp Business API   | Meta Cloud API integration                    |

## Quick Start

### 1. Set up environment

```bash
cd tender_system
python -m venv venv
venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

### 2. Configure environment

```bash
cp .env.example .env
# Edit .env with your settings (email, WhatsApp, secret key)
```

### 3. Run the app
(Set-ExecutionPolicy -Scope Process -ExecutionPolicy RemoteSigned) ; (& c:\Projects\tender_system\.venv\Scripts\Activate.ps1)
cd C:\Projects\tender_system && python -m venv venvcd C:\Projects\tender_system && .\venv\Scripts\activate && python main.py

```bash
uvicorn main:app --reload --port 8000
# OR
python main.py
```

Open: **http://localhost:8000**

### 4. Seed demo data (optional)

```bash
curl -X POST http://localhost:8000/dev/seed
```

Demo login credentials:
- Admin:        `admin@tenderflow.com` / `password123`
- Bid Manager:  `sara@tenderflow.com`  / `password123`
- Contributor:  `omar@tenderflow.com`  / `password123`

## Project Structure

```
tender_system/
├── main.py                    # App entry point, scheduler, seed endpoint
├── config.py                  # Settings (from .env)
├── database.py                # SQLAlchemy engine and session
├── models.py                  # All ORM models (User, Tender, Stage, Task, etc.)
├── auth.py                    # JWT tokens, password hashing, auth dependencies
├── requirements.txt
├── .env.example
│
├── routers/
│   ├── auth_router.py         # Login, signup, logout
│   └── tenders.py             # Dashboard, tender CRUD, stage & task updates
│
├── services/
│   └── notifications.py       # Internal alerts, email, WhatsApp, deadline scanner
│
└── templates/
    ├── base.html              # Sidebar layout, nav, countdown JS
    ├── auth/
    │   ├── login.html
    │   └── signup.html
    └── dashboard/
        ├── index.html         # Tender list with filters and stats
        ├── tender_detail.html # Stage management, task tracking, audit log
        ├── tender_form.html   # Create tender with stage configuration
        └── notifications.html
```

## Workflow — 6 Stages

| # | Stage               | Purpose                                         |
|---|---------------------|-------------------------------------------------|
| 1 | Preliminaries       | Document review, site visit, sub-contractor list|
| 2 | Work Packages       | Supplier quotes, work breakdown structure        |
| 3 | Pricing             | Cost build-up, margin, risk analysis            |
| 4 | Technical Proposal  | Method statements, programme, CVs               |
| 5 | Commercial Offer    | Cover letter, forms, bonds, contract conditions  |
| 6 | Printing & Submitting | Compilation, printing, delivery               |

Stages can be individually **disabled** for tenders where they don't apply. Disabled stages are excluded from progress calculations and deadline countdowns.

## Key Features

- ✅ Secure JWT authentication (HTTP-only cookies)
- ✅ Role-based access: Admin / Bid Manager / Contributor / Viewer
- ✅ 6-stage proposal workflow with skip/disable capability
- ✅ Task tracking within each stage with cascade completion
- ✅ Deadline countdowns (live, updates every minute)
- ✅ Internal notification system
- ✅ Email notifications (SMTP)
- ✅ WhatsApp Business API integration (placeholder)
- ✅ Audit log for all changes
- ✅ Background scheduler for deadline warnings (7d / 3d / 1d)
- ✅ Demo data seed endpoint
- ✅ FastAPI auto-docs at `/api/docs`

## Notifications

Deadline warnings fire automatically at **7 days**, **3 days**, and **1 day** before a stage due date.
Each trigger sends:
1. An internal dashboard notification
2. An email to the assigned user
3. A WhatsApp message (if phone number is set and API is configured)

## Deploying to Production

1. Replace SQLite with PostgreSQL:
   ```
   DATABASE_URL=postgresql://user:pass@host/dbname
   ```
2. Use a strong `SECRET_KEY` (32+ random chars)
3. Disable `DEBUG=False`
4. Run behind Nginx + Gunicorn:
   ```bash
   gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
   ```
5. Set `BASE_URL` to your domain for WhatsApp/email links

## API Documentation

Visit `http://localhost:8000/api/docs` (available in DEBUG mode only).
