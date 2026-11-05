# Self-service service requests: system overview

Audience: the developer and the AI agent that builds this system. Read this first, then the two screen specs:
- `specs/School Requests - Self Service.md` (the school admin page)
- `specs/Ticket Inbox and Services Dashboard.md` (the support team pages)

The screen files are the visual reference. All data in them is demo data held in the page. Where a screen and a spec disagree, ask the designer.

## 1. What the system does

A school that is **not under a LockOut service contract** ("self service") looks after its own devices (tablets, Smart Boots, Pull Boxes, Smart Lights). When a device is broken or its battery is low, the school either fixes it itself or **reports an issue** to the LockOut support team. Support works the request from creation to resolution and talks to the school through the request. A technician visit is arranged inside the same request when the school cannot fix the device.

Scope of this work: self-service schools, school level. Under-contract schools (LockOut opens requests automatically and schedules services), district-level request pages and the old calendar and parts-to-order ideas are not part of it.

## 2. The two sides

| | School side | Support side |
|---|---|---|
| Who | School admin (one school) | Support team (super admin role) |
| Goal | Report issues, follow progress, answer questions | Triage, assign, work and resolve requests |
| Screens | School Requests page with the Report an issue overlay | Services dashboard (home) and Ticket inbox |
| Sees | Only its own school's requests, public events only | Every school's requests, public and internal events |

One request object is shared by both sides. The school creates it; support changes its status, assignee and questions; both add comments and attachments.

## 3. Lifecycle of a request

Three steps: **Created, In progress, Resolved**. Two more statuses sit outside the line: **Action needed** (support is waiting for the school; the line stays on In progress, shown amber) and **Cancelled**. The assignee is a field, not a step.

```
School: Report an issue
      |
   Created ---- school can Cancel (only here) --> Cancelled
      |  support: Start work (assigns if nobody yet)
   In progress <------ school answers ------+
      |  \                                   |
      |   support: Ask for a reply  --> Action needed
      |  support: Mark resolved
   Resolved ---- school can Reopen --> Created
```

Rules:
- Only support moves a request to In progress, Action needed or Resolved. Only the school cancels or reopens.
- A school's comment, answer or visit-time suggestion on an Action needed request moves it back to In progress.
- A Resolved or Cancelled request accepts no comments, attachments or visit-time suggestions.
- Assigning someone changes the assignee only; it never changes the status.

## 4. A typical day, end to end

1. The school admin presses **Report an issue**, picks devices (grouped by type, with zone and search filters), chooses one or more issues per device (or "Other comments" with text), adds an on-site contact (name and phone, required) and optional comments and photos, reviews the summary and submits.
2. The request appears as **Created** on the school's Requests tab and on the support dashboard ("New requests", Recent requests) and in the inbox.
3. Support opens it, presses **Start work**: it is assigned to that person and becomes **In progress**. The school sees "Assigned to {name}" and the new status in its Activity.
4. Support needs information: types a question and presses **Ask for a reply**. Status becomes **Action needed**. The school sees an amber banner ("{id} needs your reply") and the question on the request. For a technician visit support ticks "Also ask for a visit time", and the school gets a block to suggest a date and one of three time windows.
5. The school replies (comment, optional attachment) or suggests a time. The request returns to **In progress**.
6. Support presses **Mark resolved**. The school sees Resolved and can **Reopen** it (back to Created) if the problem returns.

At any time support can add an **internal note** that the school never sees, and can send a plain message to the school without changing the status.

## 5. Data model

**Request**: id (SR-1048), school (facility) and district, raised by, on-site contact (name, phone), status, stage (1 to 3, 0 when cancelled), assignee, question text (while Action needed), askTime flag (visit-time block), created time, devices[], events[].
**Device in a request**: name, type, location (room and area), issues[], other (free text).
**Event** (append-only): text, author (school person, "Support team" or a name, "System"), time, colour dot, internal flag, optional attachment {name, size, url}.
Never edit or delete an event. Removing a photo or cancelling adds an event.

Issue lists: tablets have Tablet Damaged, Tablet not responding, Replace charger, Other comments. All other devices have Device not functioning, Device malfunctioning, Replace Battery, Other comments.

Attachments: one file per comment or message; images, PDF, Word, Excel or CSV; 3 MB maximum. Enforce on the server as well as in the page. An attachment is an event; images show as a thumbnail and open a preview dialog, other files show as a chip.

## 6. What each side may do

| Action | School | Support |
|---|---|---|
| Create a request | Yes (Report an issue) | Yes, on behalf of a school (Create request on the dashboard) |
| Start work, assign, mark resolved | No | Yes |
| Ask a question / visit time | No | Yes |
| Answer, suggest a time | Yes | No |
| Comment with attachment | Yes | Yes (message to the school) |
| Internal note | Not visible | Yes |
| Cancel (Created only) / Reopen (Resolved only) | Yes | No |
| See other schools' requests | No | Yes |

## 7. Pages and where they connect

- School Requests: Requests tab (list, filters, Fix it yourself guides), Needs attention tab (devices with an open request or an unreported fault; a Report button opens the overlay on its review step with the device chosen), History log tab (every event). Request summary panel and full request page.
- Services dashboard: three stat cards (New requests, Open requests, Overdue), Recent requests, Overdue, Workload by person, Activity. Rows open the request in the inbox.
- Ticket inbox: stat cards (Waiting on school, In progress, Overdue), search and status chips, table, summary panel, full request page, Back to Services link.
- The school's "Needs attention" Report button and the overlay's `preset` and `onSubmit` are the only ties between the page and the overlay; see the school spec, section 11.

## 8. Notifications (not on the screens, build them)

- Email the school admin when support asks a question or changes the status.
- Email or notify support when a school creates a request, cancels, reopens, or answers a question.
Wording and channels are open; ask the designer.

## 9. What the demo fakes

| Demo | Build for real |
|---|---|
| Requests held in the page | Server storage, paginated and filtered on the server |
| A second tab or file linked through DemoSync (`demo-sync.js`, files named Demo * and Both *), "?demo=1", "Demo: school replied" | Real live updates and the school's real answer. Do not build DemoSync. |
| Signed-in user "Akhila Sai", support names | Real sign-in and roles |
| Fixed dates and "Just now" | Real timestamps |
| Files in memory | File storage with size and type checks |
| Fix it yourself guides, Needs attention list | Help content; device health data (low battery, offline) not yet reported |

## 10. Rules for the AI agent

- Build each library component once in the product's own system and compose screens from it. Use the library values exactly (colors, sizes, radius, states). Brand navy #25395c; buttons, pills and chips are fully rounded.
- Use exactly the statuses, labels and strings in the specs.
- Keep the pipeline at three steps. Do not add steps or statuses without asking.
- Do not invent a variant of a component. If something is missing, stop and list it.
- Keep school and support views separate: internal notes and other schools' data never reach the school side.
- Ask the designer when the spec and the screen disagree or when behaviour is not written down.
