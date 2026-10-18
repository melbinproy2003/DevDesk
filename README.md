# DevDesk

**Distributed Issue & Service Management Platform**

DevDesk is a backend-focused platform that helps software teams report, organize, assign, track, and resolve technical issues in one place. It covers bugs, service incidents, and engineering tasks, with comments, audit history, and asynchronous notifications.

This project is designed to go beyond basic CRUD — focusing on secure REST APIs, role-based access, reliable background jobs, testing, containerization, CI/CD, and observability.

---

## Why DevDesk?

When issue reports, assignments, and updates live across disconnected tools or informal messages, teams lose context, miss updates, and struggle to see who owns what.

DevDesk addresses this with:

- A centralized issue workflow
- Role-based access control
- Searchable issue records with filtering and pagination
- An audit trail of important changes
- Automated email notifications via background workers

---

## Roles

| Role | Responsibility |
|------|----------------|
| **Reporter** | Creates issues and follows progress |
| **Administrator** | Manages users, assigns issues, oversees activity |
| **Developer** | Works assigned issues, updates status, adds comments |

Permissions are enforced by the backend, not only the UI.

---

## Core Features

- **Authentication & users** — registration, login, password hashing, JWT-style tokens, RBAC
- **Issue management** — create, assign, prioritize, update status, resolve, and close
- **Comments** — discussion history tied to each issue
- **Audit trail** — who changed what, and when
- **Notifications** — async email jobs on assignment and status changes (Redis + BullMQ)
- **Search & filters** — by status, priority, assignee, project, and date
- **Observability** — health checks, structured logs, metrics, and distributed tracing

---

## Architecture

Three independently deployable backend components:

```
┌─────────────────┐     ┌──────────────────────┐     ┌─────────────────────┐
│ Auth Service    │     │ Issue Management     │     │ Notification Worker │
│ register/login  │     │ issues, comments,    │     │ BullMQ consumer →   │
│ tokens & roles  │     │ audit history        │     │ email delivery      │
└─────────────────┘     └──────────────────────┘     └─────────────────────┘
         │                         │                            │
         └────────────┬────────────┘                            │
                      ▼                                         ▼
                 MongoDB                                    Redis
                      ▲
                      │
                   Nginx (reverse proxy)
```

---

## Tech Stack

| Layer | Technologies |
|-------|----------------|
| Runtime & API | Node.js, TypeScript, Express.js |
| Data | MongoDB |
| Jobs | Redis, BullMQ |
| Testing | Jest, Supertest |
| Ops | Docker, Docker Compose, Nginx, GitHub Actions |
| Observability | OpenTelemetry, Jaeger, Prometheus, Grafana |
| Cloud | AWS or Microsoft Azure |

---

## Example Workflow

1. A reporter creates an issue via `POST /api/v1/issues`
2. An administrator assigns a developer
3. The change is written to audit history and a notification job is queued
4. The notification worker sends email (with retries on temporary failure)
5. Logs, metrics, and traces help diagnose the full path

---
