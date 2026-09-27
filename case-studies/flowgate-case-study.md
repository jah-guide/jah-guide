# FlowGate — SLA service-request workflow

**Systems Analyst case study · Bohlokoa (jah-guide)**  
*Northvale Shared Services (composite scenario) · Next.js portfolio prototype*

---

## Problem

Hundreds of IT and operations requests each month arrive via **email, chat, and ad-hoc forms**. Intake is manual, approvals sit in spreadsheets, and operations leadership learns about **SLA breaches only after escalations**.

| Pain | Impact |
|------|--------|
| Fragmented intake | Duplicate work, lost context |
| Informal triage | Wrong priority assigned late |
| Unclear approval path | Requests stall between manager and director |
| Reactive SLA reporting | Breaches discovered after customer impact |

**Vision:** FlowGate standardizes structured intake, ops triage, sequential manager → director approval, and live **On Track / At Risk / Breached** posture on open work.

---

## Stakeholders

| Stakeholder | Interest | Key need |
|-------------|----------|----------|
| Business requester | Timely fulfillment | Status visibility, clear reject reasons |
| Ops analyst | Fair queue, accurate priority | Single queue, triage tools, SLA signals |
| Line manager | Risk control | Approve/reject with context |
| Service director | Policy alignment | Final gate for governed requests |
| Head of Shared Services | Throughput & breach rate | Dashboard of open SLA posture |

*Demo roles:* Ops Analyst, Line Manager, Director (actions exposed on the request detail page for frictionless walkthrough).

---

## To-be process (FlowGate demo)

```
Requester          Ops analyst         Manager           Director
    |                  |                  |                  |
    v                  v                  v                  v
[Intake form] --> [Triage & route] --> [Approve/Reject] --> [Approve/Reject]
    |                  |                  |                  |
    +------------------+------------------+------------------+
                       |
                 [SLA engine: P1 4h / P2 24h / P3 72h]
                       |
              On Track | At Risk (≤25% left) | Breached
```

**States:** `submitted` → triage → `pending_manager` → `pending_director` → `approved` | `rejected`  
**Resolution for SLA:** workflow completion (approved or rejected), not full fulfillment.

---

## Sample functional requirements

| ID | Requirement | Priority |
|----|-------------|----------|
| FR-01 | Capture title, description, category, priority, requester, department; start SLA clock | Must |
| FR-05 | Triage **Submitted** requests → route to manager (`pending_manager`) + timeline event | Must |
| FR-06 | Manager approve/reject in **Manager approval** gate | Must |
| FR-04 | Classify open SLA: **On Track**, **At Risk** (≤25% time left), **Breached** | Must |
| FR-12 | Control board: counts for open, awaiting approval, at risk, breached | Should |

Full set: [`flowgate/docs/03-requirements.md`](https://github.com/jah-guide/flowgate/blob/main/docs/03-requirements.md)

---

## Traceability snippet

Requirements link to UI/code and acceptance tests (portfolio evidence).

| Req | Summary | Build | Test |
|-----|---------|-------|------|
| FR-01 | Capture request | `/requests/new`, `createRequestAction` | AT-01 |
| FR-05 | Triage | `triageRequest`, `RequestActions` | AT-05 |
| FR-06 | Manager gate | `approveRequest` / `rejectRequest` | AT-06, AT-07 |
| FR-04 | SLA badges | `SlaBadge`, `sla.ts` | AT-04, AT-09 |
| FR-12 | Dashboard | `src/app/page.tsx` | AT-04 |

Matrix: [`docs/08-traceability-matrix.md`](https://github.com/jah-guide/flowgate/blob/main/docs/08-traceability-matrix.md)

---

## UI (prototype)

Dark **ops control board** — readable SLA posture without exporting spreadsheets.

| Surface | What reviewers see |
|---------|-------------------|
| **Control board (`/`)** | Stat cards (open, approval queue, at risk, breached); recent activity feed |
| **Request queue (`/requests`)** | Newest-first table; status, priority, SLA badge, countdown; search & filters |
| **New request (`/requests/new`)** | Structured intake (category, P1–P3, department) |
| **Detail (`/requests/[id]`)** | Description, SLA panel (due, elapsed bar), audit **timeline**, triage/approve/reject actions |

*Screenshot placeholders for PDF:* control board stats · queue with SLA badges · detail timeline + approval panel.

---

## Deliverables & links

| Artefact | Location |
|----------|----------|
| Working demo | [github.com/jah-guide/flowgate](https://github.com/jah-guide/flowgate) |
| Analysis pack | `docs/` (context, RACI, process, data model, acceptance tests) |
| Printable case study (HTML) | [jah-guide/case-studies/flowgate-case-study.html](https://github.com/jah-guide/jah-guide/blob/main/case-studies/flowgate-case-study.html) |

*Spec-driven delivery: requirements → traceability matrix → acceptance tests → Next.js increment.*
