# 🗺️ Interface Map

> **6 role-specific Airtable interfaces** designed to surface the right data to the right people — each built around a specific operational domain and stakeholder group.

---

## Overview

| Interface | Purpose | Stakeholders | Automations Managed |
|---|---|---|---|
| [Studio HR Hub](#studio-hr-hub) | 360° team management — hiring to contract renewals | HR, Admin | 6 |
| [Studio Operations Hub](#studio-operations-hub) | Monthly scheduling, class oversight, teacher management | HR, Admin | 1 |
| [Check-in & Sales Hub](#check-in--sales-hub) | Front-desk operations — registration, sales, check-ins | Marketing, Admin | 1 |
| [Marketing Ops Hub](#marketing-ops-hub) | Campaign management, partnerships, event coordination | Marketing | 1 |
| [Sales Ops Hub](#sales-ops-hub) | Lead qualification funnel, client portfolio, retention | Sales | 5 |
| [Web Operations Hub](#web-operations-hub) | Website content — events, gallery, AI-translated media | Marketing | — |

---

## Studio HR Hub

> A 360° operational view of the entire team — from initial hiring and onboarding through to contract renewals and offboarding.

**Stakeholders:** HR Manager, Studio Admin

**Automations managed here:** 6
- [HR] Auto-start Renewal
- Done: Auto-mark Renewal as Done
- Non-Renewal: Auto-Close
- [HR] Renewal: Finalize & Reset
- Teacher Approval to Class Workflow
- Update Teacher Sync Class

### Pages

| Page | Description |
|---|---|
| 🚀 HR Control Center | High-level overview of all HR processes — active staff, pending renewals, onboarding pipeline, and absence summary. Entry point for all HR operations. |
| 📂 Staff Directory | Full staff record cards — contact details, contract type, specializations, approval status. Editing specializations here triggers Teacher → Class sync automations. |
| ✍️ Staff Absence Form | Internal form for logging sick leave, holidays, and planned absences. |
| 📋 Staff Absence Reporting | Overview of all logged absences — past, current, and upcoming. Used to plan substitutions and track patterns. |
| ⏳ New Staff Onboarding | Pipeline for managing new hires from offer acceptance through to system setup and first session. |
| 🔄 Contract Renewal Management | CDD contract renewal pipeline. Kanban board across four stages: Not Started → In Discussion → Contract Sent → Done. All 4 renewal automations are triggered by actions taken on this board. |

---

## Studio Operations Hub

> A multi-page interface designed to centralize monthly scheduling, class oversight, and teacher management — providing real-time visibility into availability, absences, and session coordination.

**Stakeholders:** HR Manager, Studio Admin

**Automations managed here:** 1
- Recurring Sessions Generator

### Pages

| Page | Description |
|---|---|
| Overview | Summary of daily and weekly operations — session count, room utilization, teacher availability, and upcoming absences. |
| Schedule & Operations | Master session list with filtering by date, teacher, room, and status. Used for operational oversight and issue resolution. |
| Monthly Studio Planner | Interactive calendar view for creating and managing sessions. This is where sessions are created, the `Recurring` toggle is set, and `Session_Status` is updated to Completed — triggering the Recurring Sessions Generator automation. |
| Teacher Profiles | Detailed teacher cards — contact info, specializations, active classes, and session history. Updating specializations here triggers the Teacher Sync automation. |
| Leave Registration | Form for logging teacher leave requests — sick days, holidays, and planned absences. |
| Absence Tracker | Real-time view of who is currently absent, on leave, or has upcoming planned time off. Used to identify sessions that need a substitute teacher. |

---

## Check-in & Sales Hub

> A streamlined front-desk interface dedicated to client registration, subscription sales, and session attendance management.

**Stakeholders:** Marketing Manager, Studio Admin

**Automations managed here:** 1
- SYNC TRANSACTIONS TO NEW CLIENT

### Pages

| Page | Description |
|---|---|
| Overview | Daily front-desk summary — expected check-ins, recent sales, and new registrations. |
| Existing Client Sale | Form for processing subscription renewals and plan upgrades for clients already in the system. Does not trigger the transaction automation — client record already exists. |
| New Client Registration & Sale | Form for registering first-time clients and processing their initial plan purchase or event ticket. **Submitting this form triggers the SYNC TRANSACTIONS automation.** |
| Existing Client Event Registration | Board of scheduled events with attendee management. Used after registration to link a client to a specific event. |
| Class / Event Check-in | Attendance registration form for regular sessions and events. Used to log who actually attended vs. scheduled. |

---

## Marketing Ops Hub

> A multi-page workspace designed to centralize campaign management, partnership tracking, and event coordination — aligning promotional activities with studio scheduling.

**Stakeholders:** Marketing Manager

**Automations managed here:** 1
- Sync Event to Studio Calendar

### Pages

| Page | Description |
|---|---|
| Overview | Marketing performance summary — active campaigns, upcoming events, recent leads, and partner activity. |
| Monthly Studio Planner | Shared calendar view — allows marketing to align campaign timing with the class schedule. Read-only overview of sessions. |
| Marketing Campaigns Intake | Form for registering new campaigns. Filling in `Event_Date` and selecting `Event/Workshop` type here prepares the record for the calendar sync automation. |
| Event Lifecycle Manager | Kanban pipeline across all campaign statuses: Concept → Planning → In Progress → Completed. **Moving a campaign to In Progress triggers the Sync Event to Studio Calendar automation.** |
| Add New Partner | Form for registering a new partner — company details, contact person, and partnership type. |
| Partner Directory | Full directory of studio partners with contact details, active campaigns, and collaboration history. |

---

## Sales Ops Hub

> A multi-page interface designed to manage the entire client lifecycle — from lead qualification through to portfolio health and retention monitoring.

**Stakeholders:** Sales Manager

**Automations managed here:** 5
- LEAD MIGRATION: Clients
- LEAD MIGRATION: Partners
- LEAD MIGRATION: Stuff
- LEAD MIGRATION: Teachers
- Lead Management: Archive Notes to Activity Log

### Pages

| Page | Description |
|---|---|
| Overview | Sales performance summary — lead counts by status, recent conversions, and key retention metrics. Quick links to essential Airtable resources and documentation. |
| Lead Management Board | Kanban board across all qualification statuses: MQL → Not Reached → SQL → Positive → Rejected. Lead cards surface contact details, lead type, original message, notes, and next step date. All 5 CRM automations are triggered by actions taken here. |
| Client Portfolio & Health Insights | Client segmentation dashboard — identifies 💎 VIP, ⭐ Regular, ⚠️ At-Risk, and 💀 Churned clients. Tracks subscription health, visit frequency, and LTV across the full client base. |

---

## Web Operations Hub

> A multi-page control center for managing all website content — from publishing studio events to curating the automated Instagram gallery feed.

**Stakeholders:** Marketing Manager

**Automations managed here:** None (managed via Make scenarios — see [yoga-ops-analytics-pipeline-portfolio](../))

### Pages

| Page | Description |
|---|---|
| Overview | Website content status — published events, gallery items, and pending AI translations requiring review. |
| Website Events Manager | Form and board for creating and publishing events to the website. Controls `Display_on_Website` toggle and manages AI-generated multilingual titles (RU / EN / FR). |
| Events Board | Full overview of all event records — both published and unpublished. Used to manage visibility and review content across all statuses. |
| Instagram Gallery Manager | Gallery management board for Instagram content synced automatically via Make. Allows review of AI-generated titles, category assignment, and toggling visibility for the live website gallery. |

---

## Automation → Interface Map

Full cross-reference of which automation is triggered from which interface page:

| Automation | Interface | Page |
|---|---|---|
| [HR] Auto-start Renewal | Studio HR Hub | Contract Renewal Management |
| Done: Auto-mark Renewal as Done | Studio HR Hub | Contract Renewal Management |
| Non-Renewal: Auto-Close | Studio HR Hub | Contract Renewal Management |
| [HR] Renewal: Finalize & Reset | Studio HR Hub | Contract Renewal Management |
| Teacher Approval to Class Workflow | Studio HR Hub | Staff Directory |
| Update Teacher Sync Class | Studio HR Hub / Studio Operations Hub | Staff Directory / Teacher Profiles |
| LEAD MIGRATION: Clients | Sales Ops Hub | Lead Management Board |
| LEAD MIGRATION: Partners | Sales Ops Hub | Lead Management Board |
| LEAD MIGRATION: Stuff | Sales Ops Hub | Lead Management Board |
| LEAD MIGRATION: Teachers | Sales Ops Hub | Lead Management Board |
| Lead Management: Archive Notes | Sales Ops Hub | Lead Management Board |
| Recurring Sessions Generator | Studio Operations Hub | Monthly Studio Planner |
| Sync Event to Studio Calendar | Marketing Ops Hub | Event Lifecycle Manager |
| SYNC TRANSACTIONS TO NEW CLIENT | Check-in & Sales Hub | New Client Registration & Sale |

---

*[← Back to main README](./README.md)*
