# School Requests (Self Service): functional specification

Screen file: `screens/School Requests - Self Service.dc.html`
The file also contains a block for a two-tab demo (`DemoSync`, `toCanon`, `fromCanon`, `?demo=1`, the sync banner). It is not part of the product; ignore it and do not build it.
Audience: developers and the AI model that writes the production code. The screen file is the visual reference; this document explains behaviour, data and rules. Where the two disagree, ask the designer.

All data in the screen file (requests, devices, names, dates) is demo data held in the page. Replace it with real data; nothing is saved when the page reloads.

## 1. Purpose and users

A school administrator at a school that is **not under a LockOut service contract** ("self service") uses this page to:
- report broken or low-battery devices to the LockOut support team, and ask for a technician visit when they cannot fix it themselves;
- follow each report from creation to resolution;
- answer questions from support, add comments and photos;
- see which devices need attention, and report them.

Self-service schools replace parts (batteries, chargers) themselves, or request a technician. LockOut does **not** open requests on its own for these schools (an under-contract school would get automatic low-battery requests; that is a different page, not in this spec).

Role: School admin. Scope: one school (the demo school is Bath Middle School). Everything on the page is limited to that school.

## 2. Layout

Full-height page: left menu (Sidebar) and a content area.

Content area, top to bottom:
1. **Header row**: small grey school name (BATH MIDDLE SCHOOL), title "My requests", one line of help text ("Replace parts yourself, or request a technician visit when you need one.") and the primary button **Report an issue** (right).
2. **Three tabs** (Tab component, icon + label): **Requests**, **Needs attention** (with a count badge until opened), **History log**. The selected tab's icon is solid.
3. Tab content (sections 4 to 6).

Overlays on top of the page:
- **Summary panel** (Side panel) for one request, from the right, dimming the whole window including the menu.
- **Full request page**, which replaces the tab content and sits beside the menu.
- **Report an issue overlay** (screens/Service Request Classic.dc.html, embedded).
- **Cancel confirmation** (Confirm dialog, red confirm button).

## 3. Library components used

Sidebar, Tab, Button, LinkButton, Chip, Pill, TextField, Pagination, IconButton, Icon, ConfirmDialog, SidePanel, ProgressSteps, Timeline, CollapsibleBar, CommentBox, TextArea (inside CommentBox). Values (colors, sizes, radii, states) are in each component's entry in the library; do not copy inline styles. See AGENTS.md.

## 4. Data model

### Request (ticket)
| Field | Type | Notes |
|---|---|---|
| id | string | "SR-1048". Created by the Report an issue overlay. |
| fac | string | Facility name. Always the current school on this page. |
| status | enum | Created, In progress, Action needed, Resolved, Cancelled. |
| stage | 0 to 3 | Position in the three-step line; see below. Cancelled is 0. |
| assignee | string | "Unassigned" until support assigns someone. Shown as "Assigned to {name}" in the header and Details; it is not a separate step. |
| by | string | Person who raised it. |
| contact | [name, phone] or null | On-site contact for a technician visit. |
| ask | string | The question support asked while status is Action needed. |
| askTime | boolean | True when support is asking the school to suggest a visit time. |
| when | string | Date and time raised; the table shows only the date part. |
| devices | Device[] | One or more. |
| photos | { id, name, url }[] | Photos added to the request. |
| log | Event[] | Everything that happened, oldest first. |

### Device (inside a request)
name ("Smart Boot 05"), type (Tablet, Smart Boot, Pull Box, Smart Light), loc (room and area, or "Location not given"), issues (string[]), other (free text when "Other comments" was chosen).

### Event (history entry)
text, by ("You", "Support team", "System"), dot (colour: navy #25395c for the school's own actions, amber #92600a for status changes and questions, green #12b76a for resolved, grey #9aa1ac for cancelled or removed), ts (sortable timestamp), time (display string, "Just now" for new events), file ({ name, size, url }, optional).

### Issue labels
Tablets: Tablet Damaged, Tablet not responding, Replace charger, Other comments.
All other devices: Device not functioning, Device malfunctioning, Replace Battery, Other comments.

## 5. Statuses and the progress line

The progress line has three steps: **Created, In progress, Resolved**. Who the request is assigned to is shown beside the title ("Assigned to Priya Nair" or "Not assigned yet"), not as a step. There is no separate "Out for service" step; a technician visit happens while the request is In progress.

| Status | Pill variant | Stage (steps reached) | Meaning |
|---|---|---|---|
| Created | tint | 1 | Raised, support has not started. |
| In progress | info | 2 | Support owns it and is working on it, including a technician visit. |
| Action needed | warning | 2 (current step shown amber, label "Action needed") | Support is waiting for the school. |
| Resolved | success | 3 | Done. |
| Cancelled | tint | 0 (no progress line) | Withdrawn by the school. |

Steps before the current one show a check; the current step is bold; later steps are grey. When status is Action needed the current step (2) turns amber and reads "Action needed". Answering moves the request back to In progress.

Allowed transitions made by the school on this page:
- Created -> Cancelled (button "Cancel request", only while Created).
- Resolved -> Created (button "Reopen request"; adds "You reopened this request" and "Status changed to Created").
- Action needed -> In progress when the school comments, or suggests a visit time.
All other transitions (assigning, In progress, Resolved, and Action needed itself) are made by support in the Ticket inbox. The page only displays them. See `specs/Ticket Inbox and Services Dashboard.md`.

## 6. Tab: Requests

### 6.1 Banner
When any request has status Action needed, an amber banner shows above the table: "{id} needs your reply" (or "{n} requests need your reply") with the text "The support team is waiting for your reply before they continue." and a **Reply now** button that opens the first such request's summary panel.

### 6.2 Fix it yourself bar
A Collapsible bar (icon wrench, title "Fix it yourself", hint "3 guides"), **closed by default**. Open: three guide rows, each with a title, a line of effort and parts ("About 5 minutes · Replacement battery needed") and a "View steps >" Link button (right chevron). Footer note: "Cannot fix it yourself? Use Report an issue above." The guides are static content; "View steps" has no action yet.

Guides: Replace a Smart Boot battery; Replace a tablet charger; Restart a Pull Box.

### 6.3 Filter row
- Search box, placeholder "Search by ID or device name". Matches the request id or any device name in the request, case-insensitive, anywhere in the text. Typing resets to page 1.
- Status chips with counts: **All, Open, Action needed, Resolved, Cancelled**. One is selected (All at start). "Open" means every status except Resolved and Cancelled. Selecting a chip resets to page 1. Counts are of all requests, ignoring the search text.

### 6.4 Table
Columns: **ID** (72px, bold navy), **Issues and devices**, **Raised**, **Status** (pill).
- Issues and devices: first line is the distinct issues joined with " · ", cut at 40 characters with "..." (full text in the tooltip); second line is "N device(s)".
- Raised: the date part only ("Oct 31", "Today").
- Whole row is clickable and opens the summary panel for that request.
- Empty result: "No requests match these filters."
- Minimum table width 620px; below that it scrolls sideways. The header and rows stay aligned. The rows area scrolls vertically; the header, filters and pagination stay fixed.

### 6.5 Pagination
Pagination component, **5 rows per page** at start, user can choose 5, 10 or 25. Choosing a size or changing filters resets to page 1. The same page size applies to the Needs attention table. Footer shows the record range and page numbers. Footer stays at the bottom of the page.

## 7. Tab: Needs attention

Title row: "Devices that need attention at Bath Middle School" and a count of devices.
Table columns: **Device** (device icon + name), **Location**, **Reason**, **Status**.
A device appears here when it has an open request, or when it has a known fault that nobody has reported.

- **Device with an open request** (status not Resolved or Cancelled): Status shows a blue pill with the request id ("SR-1048"), tooltip "Request SR-1048 is open". Pressing the row opens that request's summary panel. Reason is the request's issues for that device.
- **Device not reported yet**: Status shows a **Report** button (secondary, warning icon). Pressing it opens the Report an issue overlay straight on its review step with this device and its likely issue already chosen (see 11). The row itself is not clickable. Examples in the demo: Smart Boot 08 "Low battery alert", Pull Box 01 "Offline since Oct 30".
- Order: not-reported devices first (they need the school to act), then reported ones, each group alphabetical by device name.
- Pagination as in 6.5.
- The tab shows a small count badge (the number of not-reported devices) until the tab is opened once; after that the badge disappears.
- Once a request is raised for a device it moves from "Report" to the request-id pill.

## 8. Tab: History log

A single list of every event of every request, newest first.
- Search box "Search the history". Matches request id, facility name and event text.
- Chips by who did it: **All, You, Support team, System**, with counts.
- Columns: **Date and time**, **Request** (id, pressing opens the summary), **Event** (text; "· Attachment: name" when the event has a file), **By**. A small dot in the event's colour precedes the text.
- Empty result: "No events match these filters."

## 9. Request details

Pressing a request opens the **summary panel** (Side panel, 520px, from the right). The expand button in its header opens the **full request page**. Both show the same information and actions.

### 9.1 Summary panel
Header: id (22px bold), status pill, then three lines: facility, "Raised {when}", "Assigned to {name}" or "Not assigned yet" (bold). Buttons: expand (outline) and close.
Body, top to bottom:
1. **Progress line** (hidden when Cancelled).
2. **Question box** when status is Action needed: "The support team asked: {ask}" (amber).
3. **DEVICES AND ISSUES**: one row per device **type** (not per device): the device icon, type name, "N devices", and the issues of that type as tint pills.
4. **LOCATION OF THE FAULT**: "On-site contact: {name · phone}" and one row per device: map pin, name, location, and "Other comments: {text}" if given.
5. **ACTIVITY**: Timeline, newest first. An event can carry one attachment, shown under its text as a preview: an image as a thumbnail, any other file as a chip with its name and size. Pressing a preview opens a preview dialog (Modal shell, no footer buttons) with the full image, or the file name and size, and a close button. There is no separate Photos section: photos and files are attachments in the activity.
6. **Suggest a time for the visit** block (section 10) when it applies.
7. **Add a comment** (section 9.3) when the request is open.
Footer: left, **Cancel request** (red, only while Created) or **Reopen request** (only while Resolved); right, **Close** and **Add comment** (primary, disabled until there is text or a file).

### 9.2 Full request page
Back button "Back to requests", id, status pill, the same actions at the top (Cancel request or Reopen request), then cards: **Progress**, **Devices and issues** (grouped by type as above), **Location of the fault**, **Activity** (with attachment previews), **Details** (Facility, District, Raised by, On-site contact, Assigned to, Raised, Last update, Devices), **Add a comment**. Its address can carry `?r=SR-1048` to open a request directly (used by links from other screens). Closing the page returns to the previous tab.

### 9.3 Comments and attachments
- Comment box: label "Add a comment" (panel) or "Comment" (full page), placeholder "Write your comment".
- One optional file per comment: images, PDF, Word, Excel or CSV; **3 MB maximum**. Errors, exact text: "This file type is not supported." and "This file is larger than 3 MB." The file is not added when there is an error. A chosen file shows as a chip with name, size and a remove button.
- **Add comment** is enabled when there is text or a file. Sending adds an event with text "You commented: {text}" (or "You added an attachment" when only a file), author "You", and the file chip. If the status was Action needed it returns to its stage's normal status ("Status changed to In progress") and the question box disappears. The box and file are cleared.
- Comments are not allowed when status is Resolved or Cancelled (the box is hidden).

### 9.4 Attachments in the activity
Every attachment is an event in the activity. The school adds one with a comment (9.3) or with the Attach a file link button, a secondary link style button under the comment box. Each file is checked: images, PDF, Word, Excel or CSV, **3 MB maximum**; errors as in 9.3. Photos added in the Report an issue overlay become the first events of the new request. Support's attachments appear in the same feed with "Support team" as the author. Not available when Resolved or Cancelled.

### 9.5 Cancel and reopen
- **Cancel request** (only while Created) opens a confirmation: title "Cancel this request?", message "Request SR-1048 will be withdrawn and the support team will not work on it.", buttons "Keep request" and a red "Cancel request". Confirming sets status Cancelled, clears any question, and adds "You cancelled this request" (grey).
- **Reopen request** (only while Resolved) sets status Created, stage 1, and adds two events.

## 10. Suggest a time for the visit

Shown only when `askTime` is true and the status is Action needed: the technician is ready and needs a time from the school.
Block title "Suggest a time for the visit": a date field and three time chips (**Morning, 8 AM to 12 PM**, **Afternoon, 12 to 4 PM**, **Late afternoon, 4 to 6 PM**), one selectable. **Send suggestion** is enabled once a date and a slot are chosen. Sending adds "You suggested a visit time: Nov 6, Morning, 8 AM to 12 PM", then "Status changed to {normal status for the stage}", clears the question and hides the block.
The school can suggest a time only after the technician asks. Self-service schools only: under-contract visits are scheduled by LockOut.

## 11. Report an issue overlay (integration)

The page embeds `screens/Service Request Classic.dc.html` with:
- `open`: true while the overlay is showing; `on-close`: closes it.
- `preset` (optional): array of `{ id, facility, issues[] }`. When present the overlay opens on its review step with those devices already chosen and issues set (used by the Report button on Needs attention).
- `onSubmit(payload)`: called when the person presses Submit Request. Payload: `{ id, devices (count), facilities[], issues[], deviceList[{ id, name, type, loc, fac, issues[], other }], comments, contact{ name, phone }, photos[{ name, url }], by }`.

On submit the page adds a request at the top of the list: status **Created**, stage 1, assignee "Unassigned", facility forced to this school, devices built from deviceList (name, type, location, issues, other), photos, and a log of "You raised this request" plus "You wrote: "{comments}"" when comments exist. The overlay then shows its own "Request sent" confirmation.
Issues, photos and comments entered in the overlay are the same rules as in section 9 (images up to 3 MB for photos).

## 12. Navigation

Menu items: Home (District Home), School, Dispatch, Texting, User Management, Services (this page, highlighted), Assessments and Emergency Prep (disabled), Trainings, User Logs. Press behaviour in the demo: Home, Texting, Trainings and School open their screens; the others do nothing yet.

## 13. Rules the code must keep

- Everything is scoped to one school. A request raised here belongs to this school.
- A request in a final state (Resolved, Cancelled) accepts no comments, photos or time suggestions.
- Only the school's own actions are written by this page (comments, photos, cancel, reopen, time suggestion). Status changes made by support arrive from the server.
- Events are append-only. Never edit or delete an event; removing a photo adds an event.
- "Open" in the filters means not Resolved and not Cancelled.
- Needs attention never lists a device twice; a device with an open request shows its id, not a Report button.
- Dates: show "Today" for today, otherwise "Mon D"; times in 12-hour format ("3:02 PM").
- Counts in the chips are over all requests; search and page do not change them.
- The summary panel dims the whole window; the full request page does not cover the menu.

## 14. What the demo fakes (build for real)

| Demo | Real build |
|---|---|
| Seven requests held in the page | Load requests for the school from the server, paginated and filtered on the server for large lists. |
| Status moves made by support | Receive them from the support system and add events. |
| "Just now" and demo dates | Use real timestamps. |
| Needs attention list (two fixed devices) | Derive from device health (low battery, offline) that nobody has reported. |
| Files kept in memory | Upload to storage; keep name, size and a link. Enforce the 3 MB and type limits on the server too. |
| Contact name and phone | Read from the school's contact record. |
| Guides in Fix it yourself | Content from a help system; "View steps" opens the guide. |
| Notifications | Email the school when support asks a question or changes status; not shown on this page. |

## 15. Accessibility

- Table rows and Needs attention rows are clickable: give them role button or link, keyboard focus and Enter to open.
- Chevron and icon-only buttons have text labels (for example "Open full page", "Close", "Remove attachment").
- Status is never shown by color alone: every status has its text label.
- The summary panel traps focus while open and closes on Escape and on a press on the dimmed area.
- Respect reduced motion for the Collapsible bar slide.

## 16. Not on this page

- Under-contract behaviour (Next service line, automatic low-battery requests, no Fix it yourself bar): see the Under Contract page.
- The super admin inbox where support works the requests.
- District-level view of requests for many schools.
