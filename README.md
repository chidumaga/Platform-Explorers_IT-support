# TechCare Solutions – IT Support Ticketing System
> A Microsoft Power Platform solution to centralise IT support requests, enforce SLA tracking, automate escalations, and deliver real-time management reporting.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Problem Statement](#problem-statement)
- [Solution Summary](#solution-summary)
- [Stakeholders & Roles](#stakeholders--roles)
- [Power Platform Components](#power-platform-components)
- [Data Model (ERD)](#data-model-erd)
- [Business Requirements](#business-requirements)
- [SLA Priority Matrix](#sla-priority-matrix)
- [Getting Started](#getting-started)
- [Prerequisites](#prerequisites)
- [Environment Setup](#environment-setup)
- [Project Structure](#project-structure)
- [Relationship Types Reference](#relationship-types-reference)
- [Contributing](#contributing)
- [Contact](#contact)

---

## Project Overview

TechCare Solutions receives **200+ IT support requests weekly** via email and phone. With no centralised system in place, the organisation experiences:

- Missed SLAs due to lack of visibility
- Duplicated work across agents
- No automated escalation or alerts
- Frustrated staff and poor resolution transparency
- No management reporting or performance data

This project delivers a fully integrated, low-code solution built on the **Microsoft Power Platform**, using Dataverse as the single source of truth for all IT support activity.

---

## Problem Statement

| Issue | Impact |
|---|---|
| No centralised ticket queue | Requests lost or forgotten |
| Email & phone requests untracked | No audit trail or SLA enforcement |
| No agent assignment logic | Duplicated or unowned work |
| No escalation automation | SLA breaches go unnoticed |
| No reporting | Blind management decisions |

---

## Solution Summary

The TechCare IT Support Ticketing System provides:

- **Centralised intake** – all email and phone requests converted to tracked tickets automatically
- **SLA enforcement** – priority-based due dates calculated at ticket creation
- **Automated alerts** – Teams and Outlook notifications before and at SLA breach
- **Self-service portal** – end users submit and track their own requests
- **Role-based access** – users only see what their role permits
- **Real-time dashboards** – SLA compliance, volume trends, and agent performance
- **AI triage chatbot** – Copilot Studio bot handles first-line FAQs and pre-fills tickets

---

## Stakeholders & Roles

| Stakeholder | Role | Access Level |
|---|---|---|
| IT Manager | Oversees SLAs, reports, escalations | Admin / Full access |
| IT Support Agent | Creates, owns, and resolves tickets | Agent – assigned tickets only |
| End User (Staff) | Submits requests, tracks own tickets | Self-service portal |
| Team Lead | Assigns tickets, monitors queue | Supervisor access |
| Finance / Ops Director | Views dashboards and SLA compliance | Read-only reporting |

---

## Power Platform Components

| Component | Purpose |
|---|---|
| **Microsoft Dataverse** | Central data store – all tables, business rules, column-level security |
| **Power Apps (Model-Driven)** | Agent and manager desk – queue views, SLA timers, full ticket management |
| **Power Apps (Canvas)** | End-user self-service app – submit requests, track status, add comments |
| **Power Automate** | Email-to-ticket flow, SLA breach alerts, escalation logic, round-robin assignment |
| **Power BI** | SLA compliance reports, agent performance dashboards, volume analytics |
| **Power Pages** | External-facing portal for staff without a Microsoft 365 licence |
| **Copilot Studio** | AI chatbot for first-line triage, FAQ handling, and ticket pre-fill |
| **Connectors** | Microsoft Teams, Outlook, and SharePoint integration |

---

## Data Model (ERD)

The solution uses **7 Dataverse tables**. All relationships are enforced at the data layer.

```
USER ──────────────────────┐
  │  (submits)             │ (assigned to)
  │ 1:N                    │ 1:N
  ▼                        ▼
TICKET ──── 1:1 ──── SLA_RECORD
  │
  ├── 1:N ──── TICKET_COMMENT
  ├── 1:N ──── ATTACHMENT
  ├── N:1 ──── CATEGORY
  └── N:1 ──── TEAM

USER ◄──── M:N ────► TEAM
```

### Tables at a Glance

| Table | Key Columns | Relationship |
|---|---|---|
| `USER` | UserId (PK), FullName, Email, Role, IsActive | Parent of TICKET, COMMENT |
| `TICKET` | TicketId (PK), TicketRef, Status, Priority, Channel, SLADueDate, SLABreached | Core entity |
| `SLA_RECORD` | SLAId (PK), TicketId (FK), ResponseTargetHrs, ResolutionTargetHrs, IsBreached | 1:1 with TICKET |
| `CATEGORY` | CategoryId (PK), CategoryName, DefaultPriorityLevel | 1:N with TICKET |
| `TEAM` | TeamId (PK), TeamName, TeamEmail, TeamLeadId (FK) | 1:N with TICKET |
| `TICKET_COMMENT` | CommentId (PK), TicketId (FK), AuthorId (FK), IsInternal | 1:N with TICKET |
| `ATTACHMENT` | AttachmentId (PK), TicketId (FK), FileName, FileType, FileSizeKB | 1:N with TICKET |

---

## Business Requirements

### Functional
- Capture all IT support requests from email and phone into a single queue
- Assign unique ticket reference numbers and priority levels (P1–P4)
- Auto-calculate SLA due dates based on priority at ticket creation
- Support manual and automated (round-robin) agent assignment
- Allow agents to add internal notes and external comments
- Provide an end-user self-service portal for submission and status tracking
- Detect and flag potential duplicate tickets
- Deliver management dashboards with SLA compliance, volumes, and agent metrics
- Trigger automated escalation when tickets approach or exceed SLA deadlines

### Non-Functional
- Accessible on desktop and mobile devices
- Role-based access control enforced at the data layer
- Integration with Microsoft Teams and Outlook for all notifications
- Built entirely on Microsoft Power Platform (low-code / no-code)
- Target uptime: **99.5%** during business hours (07:00–19:00, Mon–Fri)

---


---

## SLA Priority Matrix

| Priority | Label | Response Target | Resolution Target | Example Scenario |
|---|---|---|---|---|
| P1 | Critical | 1 hour | 4 hours | System outage, complete service failure |
| P2 | High | 2 hours | 8 hours | Key system slow/degraded, single user blocked |
| P3 | Medium | 4 hours | 24 hours | Non-urgent issue, workaround available |
| P4 | Low | 8 hours | 72 hours | General request, how-to question |

---

## Getting Started

### Prerequisites

Before deploying this solution, ensure you have:

- A **Microsoft 365** tenant with Power Platform access
- One of the following licences:
  - Power Apps Per User Plan, or
  - Microsoft 365 E3/E5 with Power Apps seeded licences
- **Power BI Pro** or Premium Per User licence for embedded reporting
- **Dataverse** environment (not the default environment — create a dedicated one)
- System Administrator or Environment Maker role in Power Platform
- A shared support inbox configured in Exchange / Outlook

### Environment Setup

1. **Create a Dataverse environment**
   - Navigate to [Power Platform Admin Centre](https://admin.powerplatform.microsoft.com)
   - Create a new environment: `TechCare-Prod` (type: Production)
   - Enable Dataverse when prompted

2. **Import the solution package**
   - Go to [make.powerapps.com](https://make.powerapps.com)
   - Select your environment → Solutions → Import
   - Upload `TechCare_Solution.zip`
   - Follow the connection reference prompts for Outlook and Teams connectors

3. **Configure connection references**
   - Outlook 365 – connect to the shared support inbox
   - Microsoft Teams – connect to the IT support team channel
   - SharePoint – connect to the document library for attachments (optional)

4. **Activate Power Automate flows**
   - `TC-Flow-EmailToTicket` – monitors the support inbox
   - `TC-Flow-SLABreachAlert` – runs every 15 minutes
   - `TC-Flow-EscalationAssign` – runs every 30 minutes
   - `TC-Flow-ConfirmationEmail` – triggered on ticket creation

5. **Set environment variables**
   - `SupportInboxEmail` – shared mailbox address
   - `TeamChannelId` – Teams channel ID for escalation alerts
   - `DefaultTeamLeadId` – fallback Team Lead user ID

6. **Assign security roles**
   - Assign `TC – IT Agent` role to support staff
   - Assign `TC – End User` role to all staff
   - Assign `TC – IT Manager` role to managers and team leads

---

## Project Structure

```
TechCare_Solution/
│
├── DataModel/
│   ├── Tables/                  # Dataverse table definitions (JSON)
│   └── Relationships/           # Entity relationship configs
│
├── Apps/
│   ├── ModelDrivenApp/          # Agent desk Power App
│   └── CanvasApp/               # End-user self-service app
│
├── Flows/
│   ├── TC-Flow-EmailToTicket
│   ├── TC-Flow-SLABreachAlert
│   ├── TC-Flow-EscalationAssign
│   └── TC-Flow-ConfirmationEmail
│
├── Reports/
│   └── TechCare_Dashboard.pbix  # Power BI report file
│
├── CopilotStudio/
│   └── TechCare_Bot/            # Chatbot topic definitions
│
├── Docs/
│   ├── TechCare_PowerPlatform_Solution.docx   # Full solution design document
│   └── README.md                              # This file
│
└── TechCare_Solution.zip        # Importable solution package
```

---

## Relationship Types Reference

| Type | Notation | Meaning | Dataverse Implementation |
|---|---|---|---|
| One-to-One | 1:1 | One record in A maps to exactly one record in B | Lookup column with unique constraint |
| One-to-Many | 1:N | One record in A maps to many records in B | Lookup (FK) column on the child table |
| Many-to-Many | M:N | Many records in A map to many records in B | Junction table or built-in N:N relationship |

---

## Contributing

1. Fork or clone this repository
2. Create a feature branch: `git checkout -b feature/your-feature-name`
3. Make your changes and test in a non-production Dataverse environment
4. Export the updated solution as **unmanaged** for development, **managed** for deployment
5. Submit a pull request with a clear description of changes and test evidence

> All changes must be validated against the 8 acceptance criteria before a pull request is approved.

---

## Contact

| Role | Name | Contact |
|---|---|---|
| Project Owner | TechCare IT Manager | it-manager@techcare.com |
| Power Platform Lead | TechCare IT Team | itsupport@techcare.com |
| Documentation | TechCare Solutions | techcare.com |

---
