# ⚙️ Airtable ERP/CRM & Ops Automation

> A production-ready collection of **14 native Airtable automations** across HR, CRM, Operations, and Finance — paired with **6 role-specific interfaces** designed to centralize studio management and eliminate manual workflows.

---

## 📌 What This Project Is

This repository documents the internal automation infrastructure of a yoga studio built entirely on **Airtable** — without external ETL tools or code. The system covers the full operational lifecycle: from hiring and contract management, to lead qualification and client onboarding, to class scheduling and revenue tracking.

Each automation is triggered by real business events — a contract expiring, a lead converting, a class completing — and responds by updating records, creating entries across linked tables, and logging activity. The result is a self-maintaining operational backbone that keeps data clean, teams aligned, and nothing falling through the cracks.

> ⚠️ **Data Privacy Note:** All datasets are synthetically generated. Names, contact details, and financial figures are fictional and used for demonstration purposes only.

---

## 🗄️ Database Schema

The automation system operates across **7 core Airtable tables**, each serving a distinct role in the operational pipeline:

| Table | Role |
|---|---|
| `Stuff & Teachers` | Central HR record — contracts, specializations, approval status |
| `Classes` | Class catalog — types, qualified teachers, capacity |
| `Session` | Schedule — individual class instances, rooms, attendance |
| `Inbound Leads` | CRM intake — all incoming inquiries before conversion |
| `Clients` | Converted clients — lifecycle, segments, LTV |
| `Partners` | Partner directory — companies, contacts |
| `Transactions` | Financial records — payments linked to clients and subscription plans |

> 📊 **Entity Relationship Diagram**
> *(diagram coming soon — see `/assets/diagrams/`)*

![Database Schema](./assets/diagrams/erd-schema.png)

---

## 🏗️ Repository Structure

```
airtable-erp-crm-ops-automation-no-code/
│
├── README.md                          ← You are here
│
├── hr-staff-management/
│   └── README.md                      ← Contract Renewal Pipeline + Teacher → Class Assignment
│
├── crm-lead-management/
│   └── README.md                      ← Lead Migration Pipeline + Activity Log
│
├── operations-scheduling/
│   └── README.md                      ← Recurring Sessions + Event → Calendar Sync
│
├── finance-transactions/
│   └── README.md                      ← New Client Transaction Sync
│
├── interfaces/
│   ├── README.md                      ← Interface Map — all 6 hubs
│   └── interface-map.md               ← Pages, stakeholders, automation links
│
└── assets/
    └── diagrams/                      ← Flow diagrams and screenshots
```

---

## ⚡ Automation Overview

**14 native Airtable automations** across 4 operational domains:

### 📁 HR & Staff Management — 6 automations
| # | Automation | Trigger | Action |
|---|---|---|---|
| 1 | [HR] Auto-start Renewal | `Update on Contract(Renew)` field updated | Sets `CDD_Renewal Progress → In Discussion`, logs timestamp |
| 2 | Done: Auto-mark Renewal as Done | `CDD_End_Date` updated + conditions met | Marks renewal cycle as Done |
| 3 | Non-Renewal: Auto-Close | Record matches Termination conditions | Closes record as Done, logs reason, archives |
| 4 | [HR] Renewal: Finalize & Reset | `CDD_Renewal Progress = Done` | Clears notes, resets to Not Started |
| 5 | Teacher Approval to Class Workflow | New teacher approved + Specialization filled | Links teacher to `Classes.Qualified Teachers` |
| 6 | Update Teacher Sync Class | `Specialization` field updated | Re-syncs teacher across all linked Classes |

### 📁 CRM & Lead Management — 5 automations
| # | Automation | Trigger | Action |
|---|---|---|---|
| 7 | LEAD MIGRATION: Clients | Lead `Positive` + `Contact_Type = Client` | Creates record in `Clients` |
| 8 | LEAD MIGRATION: Partners | Lead `Positive` + `Contact_Type = Partner` | Creates record in `Partners` |
| 9 | LEAD MIGRATION: Stuff | Lead `Positive` + `Contact_Type = Hiring_Stuff` | Creates record in `Stuff & Teachers` |
| 10 | LEAD MIGRATION: Teachers | Lead `Positive` + `Contact_Type = Yoga_Teacher` | Creates record in `Stuff & Teachers` with Contact Type = Yoga_Teacher |
| 11 | Lead Management: Archive Notes | `Notes` + `Next_Step_Date` both filled | Appends to activity log, clears Notes on due date |

### 📁 Operations & Scheduling — 2 automations
| # | Automation | Trigger | Action |
|---|---|---|---|
| 12 | Recurring Sessions Generator | Session `Completed` + `Recurring = ✅` | Creates next session +7 days via `DATEADD` formula |
| 13 | Sync Event to Studio Calendar | Campaign `In Progress` + `Event/Workshop` type | Creates linked Session record in studio calendar |

### 📁 Finance & Transactions — 1 automation
| # | Automation | Trigger | Action |
|---|---|---|---|
| 14 | SYNC TRANSACTIONS TO NEW CLIENT | Form `New Client Registration & Sale` submitted | Creates linked `Transactions` record with Client + Subscription Plan |

---

## 🖥️ Interface Map

**6 role-specific Airtable interfaces** built to surface the right data to the right people:

| Interface | Purpose | Stakeholders |
|---|---|---|
| **Studio HR Hub** | 360° team management — from hiring to contract renewals | HR, Admin |
| **Studio Operations Hub** | Centralize monthly scheduling, class oversight, and teacher management | HR, Admin |
| **Check-in & Sales Hub** | Front-desk operations — client registration, plan sales, and session check-ins | Marketing, Admin |
| **Marketing Ops Hub** | Campaign management, partnership tracking, and event coordination | Marketing |
| **Sales Ops Hub** | Lead qualification funnel, client portfolio health, and retention metrics | Sales |
| **Web Operations Hub** | Website content management — events, gallery, and AI-translated media sync | Marketing |

---

## 🔗 Tech Stack

| Layer | Tool | Role |
|---|---|---|
| Database & Automations | **Airtable** | Single source of truth — all data, logic, and automations live here |
| Interfaces | **Airtable Interfaces** | Role-based operational dashboards |
| Media CDN | **Cloudinary** | Image and video hosting for website gallery |
| Website Sync | **GitHub API** | Events and gallery JSON pushed to website repo |
| ETL & External Sync | **Make (Integromat)** | Analytics data sync to Google Sheets → Looker Studio |

---

## 📂 Detailed Documentation

| Section | Description |
|---|---|
| [HR & Staff Management](./hr-staff-management/README.md) | Contract Renewal Pipeline + Teacher → Class Assignment |
| [CRM & Lead Management](./crm-lead-management/README.md) | Lead Migration Pipeline + Activity Log Automation |
| [Operations & Scheduling](./operations-scheduling/README.md) | Recurring Sessions + Event → Calendar Sync |
| [Finance & Transactions](./finance-transactions/README.md) | New Client Transaction Sync |
| [Interface Map](./interfaces/README.md) | All 6 hubs — pages, stakeholders, and automation links |

---

*Documentation in progress. Screenshots and flow diagrams will be added per section.*
