# 📁 Finance & Transactions

> **1 native Airtable automation** triggered by a form submission — instantly creating a linked transaction record when a new client is registered and a subscription plan is sold.

**Contents:** [🖥️ Interface](#interface) · [⚡ Automation Overview](#automation-overview) · [👤 User Workflows](#user-workflows) · [🎬 Demo](#demo) · [🔬 Technical Deep Dive](#technical-deep-dive)

---

## What This Module Does

From a business perspective, this module solves one critical front-desk problem:

**A sale is never registered without a transaction.** When an admin registers a new client and sells them a subscription plan at the front desk, a transaction record is created automatically the moment the form is submitted — linked to both the client and the plan. The admin fills in one form; the system handles both the client record and the financial entry simultaneously.

Without this automation, the registration and the sale would need to be entered separately — creating a gap where a client could be onboarded but their payment never recorded.

---

<a id="demo"></a>
## 🎬 Demo

> 📹 *Walkthrough video — coming soon.*
> Screenshots and step-by-step demo for the New Client Registration & Sale form and transaction sync flow will be added here.

---

<a id="interface"></a>
## 🖥️ Interface

The automation is triggered and the post-registration workflow is managed entirely through one interface.

### Check-in & Sales Hub

The front-desk workspace. Admins use this interface for all client-facing operations — registrations, plan sales, event sign-ups, and attendance check-ins.

| Page | What the user does here | Automations triggered |
|---|---|---|
| **🆕 New Client Registration & Sale** | Registers a first-time client and processes their initial plan purchase or event ticket. Fills in client details, selects subscription plan, submits the form. | SYNC TRANSACTIONS TO NEW CLIENT (1) |
| **📋 Existing Client Sale** | Processes plan renewals and upgrades for clients already in the system. Client record already exists — no new record is created. | — |
| **📅 Existing Client Event Registration** | Links an already-registered client to a specific event attendance list. Used after new client registration if the client attended an event. | — |
| **✅ Class / Event Check-in** | Logs attendance for regular sessions and events. Used after registration to record who actually attended. | — |
| **🏠 Overview** | Daily front-desk summary — expected check-ins, recent sales, and new registrations. | — |

---

<a id="automation-overview"></a>
## ⚡ Automation Overview

1 automation covering one transactional pipeline:

**New Client Transaction Sync (automation 1)** — a form-triggered creation. When the **New Client Registration & Sale** form is submitted, a client record appears in `Clients` and the automation immediately creates a matching record in `Transactions` — linking the client to their purchased subscription plan. Both records are created in a single admin action.

| # | Automation | Trigger | Source Table | Destination Table | Interface |
|---|---|---|---|---|---|
| 1 | SYNC TRANSACTIONS TO NEW CLIENT | Form `New Client Registration & Sale` submitted | `Clients` | `Transactions` | Check-in & Sales Hub → New Client Registration & Sale |

---

<a id="user-workflows"></a>
## 👤 User Workflows

### New Client Registration & Sale

```
1. New client arrives at the front desk (walk-in or event attendee)

2. Admin opens Check-in & Sales Hub → New Client Registration & Sale

3. Admin fills in the form:
   → First name, Last name, Email, Phone
   → Acquisition Source (how they heard about the studio)
   → Subscription Plan (which plan they are purchasing)
   → If event attendee: selects Event Special Ticket

4. Admin submits the form
   → Client record created in Clients table
   → [1] SYNC TRANSACTIONS fires instantly
   → Transaction record created in Transactions:
      ├── Client → linked to the new Client record
      └── Subscription Plan → linked to the selected plan
   → Client is now active and their purchase is recorded

5a. If client attended an event:
    → Admin goes to Existing Client Event Registration
    → Finds the event card
    → Links the new client to the event attendance list

5b. If client is attending a regular class:
    → Admin goes to Class / Event Check-in
    → Registers client attendance for the session
```

### Returning Client Sale

```
Returning client purchases a new or renewed plan:
   → Admin opens Check-in & Sales Hub → Existing Client Sale
   → Selects existing client record
   → Processes the new plan purchase manually
   → This does NOT trigger the automation
     (client record already exists — no form submission)
```

---

<a id="technical-deep-dive"></a>
## 🔬 Technical Deep Dive

### Tables Involved

```mermaid
erDiagram
    CLIENTS {
        int Client_id PK
        string Client_User_name
        string First_name
        string Last_name
        string Email
        string Phone_number
        date Acquisition_Date
    }

    TRANSACTIONS {
        int ID_Transaction PK
        decimal Amount_Paid
        date Date_transaction
        int Remaining_Classes
        int Total_Classes
        int Visits_Used
    }

    SUBSCRIPTION_PLANS {
        int Plan_ID PK
        string Plan_Name
        decimal Price
        int Classes_Included
        int Validity_Months
    }

    CLIENTS ||--o{ TRANSACTIONS : "makes"
    SUBSCRIPTION_PLANS ||--o{ TRANSACTIONS : "applied to"
```

---

### New Client Transaction Sync — Flow

```
Administrator submits form:
"New Client Registration & Sale Classes/Events"
                ↓
Client record created in Clients table
                ↓
[1] SYNC TRANSACTIONS TO NEW CLIENT fires
                ↓
New record created in Transactions:
    ├── Client        → linked to new Client record
    └── Subscription Plan → linked to selected plan
```

---

### Automation 1 — SYNC TRANSACTIONS TO NEW CLIENT

**Trigger:** Form submitted — `New Client Registration & Sale Classes/Events`
**Table:** `Clients`

**Action:** Creates record in `Transactions`:

| Field | Value |
|---|---|
| `Client` | Linked to `Client_User_name` from submitted form |
| `Subscription Plan` | Linked to `Subscription_Plan` selected in form |

**What this replaces:** Admin manually creating a transaction record after registering a new client — a step that was often missed, leaving sales unrecorded.

---

### Form: New Client Registration & Sale Classes/Events

This is an **internal administrator form** used at the front desk when a new client walks in for the first time or registers at an event.

**Form fields:**

| Field | Description |
|---|---|
| First name, Last name | Client identity |
| Email, Phone | Contact details |
| Acquisition Source | How the client heard about the studio |
| Subscription Plan | Which plan they are purchasing — drives the linked transaction |
| Event Special Ticket | Selected instead of a plan if the client is an event attendee |

---

### Key Fields

| Field | Table | Type | Description |
|---|---|---|---|
| `Client_User_name` | `Clients` | Text | Unique client identifier — used to link the transaction |
| `Subscription Plan` | `Transactions` | Linked record | References `Subscription_Plans` table |
| `Amount_Paid` | `Transactions` | Currency | Payment amount — populated from plan price |
| `Total_Classes` | `Transactions` | Number | Classes included in the purchased plan |
| `Remaining_Classes` | `Transactions` | Formula | `Total_Classes - Visits_Used` |
| `Visits_Used` | `Transactions` | Rollup | COUNT of linked attendance records |
| `Date_transaction` | `Transactions` | Date | Date of purchase |
| `Plan_Name` | `Subscription_Plans` | Text | Name of the membership plan |
| `Price` | `Subscription_Plans` | Currency | Plan price — referenced in transaction |
| `Classes_Included` | `Subscription_Plans` | Number | Total classes per plan |
| `Validity_Months` | `Subscription_Plans` | Number | Plan duration in months |

---



*[← Back to main README](./README.md)*
