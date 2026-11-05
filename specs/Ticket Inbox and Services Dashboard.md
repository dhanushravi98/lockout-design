# Ticket Inbox and Services Dashboard: functional specification

Screen files: `screens/Services Dashboard.dc.html` (home, source name "Services Dashboard v1") and `screens/Ticket Inbox.dc.html`.
Audience: developers and the AI model that writes the code. The screen files are the visual reference; this document explains behaviour and rules. Where they disagree, ask the designer.
All data in the files is demo data held in the page. Replace it with real data.

## 1. Purpose and users

The **support team** (role: super admin, the receiving side) works the service requests that schools raise on the School Requests page (see `School Requests - Self Service.md`). This spec covers self-service schools. Under-contract behaviour is not part of this work.

- **Services dashboard**: the home screen of Services. A quick picture of what is new, open and late, and who is busy.
- **Ticket inbox**: the full list of requests, a summary panel and a full request page where support works a request.

## 2. Request data

Same request as on the school page (see section 4 of that spec), plus: `district`, the school's district; `created` (timestamp); events can be internal (`internal: true`), which only support sees. Statuses: Created, In progress, Action needed, Resolved, Cancelled. Progress line: **Created, In progress, Resolved** (stage 1 to 3). Assignee is a name from the support team, or "Unassigned"; it is not a step.

## 3. Services dashboard

Left menu (Services highlighted), then:
1. Header: "SERVICES", title "Service requests", help line, and a **Create request** button (opens a dialog to raise a request on behalf of a school: facility, device type, issue, notes).
2. Three stat cards: **New requests** (status Created), **Open requests** (not Resolved or Cancelled), **Overdue** (open for 3 days or more). Pressing a card opens the inbox filtered to it.
3. **Recent requests**: the 5 newest, with id, school, district, issues, age and status pill. "View all" opens the inbox. Pressing a row opens that request in the inbox.
4. Two columns: **Overdue** (list of overdue requests, count pill, "Nothing is overdue." when empty) and **Workload by person** (open requests per support person as a bar and a number); then **Activity** (latest events: raised, assigned, asked, resolved).

Rules: overdue = open and (now - created) >= 3 days; age text is "N h" under a day, otherwise "N days". All counts are over all requests.

## 4. Ticket inbox

### 4.1 List
- A **Back to Services** link button (left arrow) above the title returns to the Services dashboard.
- Three stat cards on top: **Waiting on school** (Action needed), **In progress**, **Overdue**.
- Search box (request id, school, device names) on the first row; status chips with counts on the second: **All, Created, Action needed, In progress, Resolved**. One status chip is selected. There is no "Assigned to me" filter.
- Table columns: **ID, School / district and contract, Issues and devices, Age, Assignee, Status**. Age turns amber at 3 days or more while open. "Unassigned" is grey italic. Whole row opens the summary panel.
- Pagination: 5 rows per page, choose 5, 10 or 25; footer fixed at the bottom; header and rows scroll together sideways under 720px.

### 4.2 Summary panel (right, 520px)
Header: id, status pill, school and district, raised by and when, on-site contact; expand button (opens the full request page) and close.
Body in clear sections: Progress line (with the school's question when Action needed); Devices and issues (grouped by type); Location of the fault; Activity (Timeline, attachment previews that open a preview dialog); then two tabs: **Message to school** (message box, attach-a-file link, **Send message**, **Ask for a reply**, and for self-service schools the option **Also ask for a visit time**) and **Internal note** (only support sees; **Save note**).
Footer: **Close** and one primary next-step button.

### 4.3 Full request page
Back link, id, status pill, one line of context (school, district, raised, assigned to), the primary next-step button. Left: Progress, Devices and issues, Location of the fault, Activity. Right: Details, Assigned to (chips), **Message to the school** (hidden when Resolved or Cancelled), **Internal note** (its own card below Assigned to). Opening the inbox with `?r=SR-1048&f=1` shows a request's full page directly.

### 4.4 Actions and status moves
| Request is | Primary button | Result |
|---|---|---|
| Created | Start work | Assigns it to the person pressing (if unassigned), status In progress. |
| In progress | Mark resolved | Status Resolved, stage 3. |
| Action needed | none (waiting for the school) | A "Demo: school replied" button stands in for the school's answer; remove it in the real build. |
| Resolved, Cancelled | none | Read only. |

- **Assigned to**: choosing a name sets the assignee and adds "Assigned to {name}" (or "Unassigned"). It does not change the status.
- **Ask for a reply**: adds "Asked the school: {text}" (default "Please reply when you can."), sets status Action needed and stores the question; with **Also ask for a visit time** ticked, the school sees the visit-time block.
- **Send message**: adds "Message to the school: {text}" (or "Sent an attachment"); status does not change.
- **Save note**: adds an internal event (grey dot) that never reaches the school.
- Every action adds an event with author and time. Events are append-only.

### 4.5 Attachments
One file per message: images, PDF, Word, Excel or CSV, 3 MB maximum. An attachment is an event; the preview (thumbnail or chip) opens a preview dialog. Same rules as the school page.

## 5. What the school sees of support's actions
Status changes, assignment, messages, questions and attachments appear in the school's Activity. Internal notes never do. A question puts the school's request in Action needed and shows the amber banner "{id} needs your reply" on the school page.

## 6. What the demo fakes (build for real)
| Demo | Real build |
|---|---|
| Requests held in the page and shared with a second tab through DemoSync (`demo-sync.js`, files named Demo *) | Server data and live updates. Do not build DemoSync. |
| "Demo: school replied" | The school's real answer. |
| Support team names, "Akhila Sai" as the signed-in user | Real users and sign-in. |
| Timestamps relative to a fixed date | Real timestamps. |
| Files kept in memory | Upload to storage; enforce the 3 MB and type limits on the server. |

## 7. Not in this work
Under-contract schools, the calendar view and parts to order (removed), district-level requests page.
