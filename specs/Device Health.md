# Device health: functional specification

Screen: `screens/District Home - Device Health.dc.html` (the District Home with device health). District admin login, one district. All data in the screen is demo data held in the page; replace it with real data. The screen file is the visual reference; this document explains behaviour.

## 1. Purpose
The backend polls the devices (tablets, Smart Boots, Pull Boxes, DomeBLE) for battery and connectivity and stores the result. The portal shows the latest stored result per facility. A district admin can also start a **health check**: the backend asks every device (or every device in one facility) for fresh data. A check is slow (about an hour for the district; one facility is shorter), so the portal never waits for it. It shows the last saved data, labelled with its age, and follows the check in the background.

## 2. What the page shows
- **Stat cards (one row of five):** Facilities, Healthy (devices with no issue), Low batteries (opens the list of devices), Offline (opens the list), Recent lockdowns. Low batteries and Offline are action cards.
- **Facilities table:** the library Facility row for each school, then a **health row** directly under it (section 3). On this page the Facility row hides its attention link ("N low batteries") and its "All devices healthy" pill (Facility row prop showHealth false); the health row carries that information instead. Rows alternate white and light grey; hover fills both together.
- **Header of the Facilities table:** a **Do a health check** primary split button (navy, one unit in two parts with a refresh icon). The main part starts a district check; the chevron part opens the settings pane (see Automatic health check). It is right-aligned with the chevron buttons of the rows below it (20px from the right edge, 14px below 730px wide). The label becomes "Health check" when the card is under about 360px wide and "Checking..." while a check runs.

## 3. The health row (under each facility)
All the health data (label, time, counts) is grouped on the right, next to the two action buttons, at any width; the left side of the row is empty. The left edge lines up with the logo above (20px padding, 14px below 730px wide) and the right edge with the View button. The left content wraps onto a second line when the row is narrow; the buttons stay on the right. Vertically centred, 10px above and below. Left to right:
1. **Label:** "Device health" in bold navy.
2. **All devices healthy:** when no device is low on battery or offline, a green Pill "All devices healthy" (size s, dot) replaces the counts.
3. **Time:** clock icon and "2 min ago" / "2 h ago" (tooltip "Last updated"). Amber and bold with the tooltip "Last updated more than 24 hours ago." when the data is 24 hours or older.
4. **Counts, icon and number only:** green check = healthy, amber battery = low battery, red wifi-slash = offline. Zero counts are left out. Hover shows the full label ("3 low batteries").
5. **Result (10 seconds only):** after a check finishes, a green check and "All 140 answered" or "62 of 64 answered", then it disappears. Success feedback is never permanent; the time ("just now") is the lasting record.
6. **Right group (actions only):** a round icon button **Check this facility** (refresh icon) and a round chevron that opens the details.

### Devices missed during a check
A device can be skipped while a check collects data (it did not answer in time). The backend reports these per facility after a run. The portal logs them: the health row gets a grey question-mark count ("2 were missed in the last check" on hover) and the healthy count excludes them; the expanded details get a **Missed in the last check** section, with a grey Pill (no icon), the sentence "These devices did not answer in the check at 11:45 AM. Their last known values are kept.", and a line per device with its name, room and last known value ("Battery 41% · last seen yesterday, 3:20 PM"). The result line after a run reads "62 of 64 answered". The log stays until the next check replaces it. In the demo Bath Career Center misses two devices (Smart Boot 09 and Smart Light 11) every time a check completes.

### Tooltips
Every status has a hover tooltip: the counts (healthy, low battery, offline, missed), the time ("Time since the last saved result", or the step description while a check runs), the 10 second result, each type tile, the Low battery / Offline pills in the list and the Missed pill. The tooltips explain what the status means and what to do.

### Expanded details
The View button is replaced by a round outline chevron button next to the school name (Facility row prop viewMode="chevron"), which opens the school page. The recent lockdowns count is a line of text below the school name ("3 recent lockdowns", 13px #4b5563) and the device counts box is right aligned at every width. The logo is 68px (the Facility row prop logoSize). The details content fills the full width of the row and scales with it: four type tiles side by side, and the device lines run the full width.
Pressing anywhere on the facility row or its health row opens and closes the details. Buttons, links and pills inside the row keep their own action and do not toggle it.
Each facility (row, health row and details) is separated from the next by a 2px #c4cad4 divider. When a row is expanded nothing around it moves, and the hover fill is off for that facility. A divider appears between the facility row and the health row, which then acts as the header of the details; the details follow below it.
Full width of the row, 20px side padding, no boxes. A grid of the four device types (icon, name, "N healthy", and "N low battery · N offline" in amber when there are problems), then one plain line per device with its icon, name, room and area, "last seen ...", and a pill (Low battery with the percentage = warning, Offline = error); the last line has no divider below it. Under the list, a yellow **Needs attention** pill on the left and a **Report an issue** button on the right, which opens the existing Report an issue overlay with those devices chosen. Healthy facilities say "No devices need attention at this facility."

### Automatic health check (settings pane)
The chevron part of the split button opens a settings pane (320px, anchored under the button; closes on an outside click or Cancel). It lets the user schedule one automated health check per day for the whole district: a **Time of day** field (native time input; the check always runs every day, default 2:00 AM), the line "Next automatic check: Nov 18, 2026 · 2:00 AM", and **Cancel** / **Save** (Save is disabled until the time changes). Saving shows a green notice for 10 seconds ("Saved. A health check will run every day at 2:00 AM."). The chevron stays usable while a check runs; only the main part is disabled. The schedule must be stored by the backend per district; the demo keeps it in the page. The automatic check follows the same steps and progress card as a manual one. The portal needs the backend to poll on this schedule, which does not exist yet.

## 4. The check and its steps
Two levels: the whole district (button in the table header) or one facility (button on its health row). Only one check runs at a time for the district; while one runs every check button is disabled and the header button's tooltip says "A check is already running."

The backend reports one step at a time. The portal shows it in the progress card and in each health row of the facilities being checked:
| Step | Meaning | Row text | Card pill |
|---|---|---|---|
| waiting | No tablet has taken the check yet. A wait on a previous device can delay it. | Waiting for a tablet | Waiting |
| assigned | One tablet has taken the check and is loading the device list. | Loading the device list | Assigned |
| collecting | The tablet is preparing and collecting device replies. | Collecting device replies | Collecting |
| paused | Work is stopped temporarily. Saved replies are kept. | Paused. Saved replies are kept | Paused (warning) |
| saving | The tablet is saving the results. | Saving results | Saving |
| completed | Saving has finished. Some or all devices may have replied. | just now, plus the result for 10 s | none |
| failed | The check stopped; the reason says why. | The row keeps the last saved values | none |
While running, the facility rows keep showing the old numbers. All the facilities in the check update together when it completes.

### Progress card (floating, bottom right, 340px)
Title ("Refreshing device health" or "Refreshing {facility}"), the step pill, the step description, a progress bar with no counter (only during collecting, saving and paused): an 8px rounded bar, filled navy #25395c, whose unfilled part is a gentle left-to-right gradient from #e8ecf3 to #f6f8fb, "Started at 11:45 AM. A check usually takes about an hour.", "You can leave this page. We will show a notice when it finishes." The × hides it and leaves a small pill at the bottom right ("Device health check: Collecting") that reopens it.

### A check that takes too long
Past the usual time (the demo uses a toggle; the real threshold is yet to be agreed) the card adds an amber box: "Taking longer than usual. You can leave this page. We will tell you when it finishes." The progress must not show 100% until the backend says completed.

## 5. The status belongs to the district
Never keep check state in the browser. The backend holds one record per district: step, scope (district or facility), started time, devices answered, reason on failure. The page reads it when it opens and while a check runs, so a user who leaves and returns, or opens another tab, sees the same state. There is one district admin account, so who started a check is not shown. The demo keeps it in session storage as a stand-in.

## 6. Suggested backend contract (adapt to your system)
- `GET /district/health`: per facility {updatedAt, counts by type {total, low, offline}, devices needing attention [{id, type, name, location, kind, batteryPercent, lastSeen}], missed devices from the last check [{id, type, name, location, lastKnown}], unreachableCount}.
- `GET /district/health/check`: the current check {step, scope, startedAt, devicesChecked, devicesTotal, long (bool), reason?} or none.
- `POST /district/health/check` {scope: "district" | facilityId}: starts a check; rejected while one is running.
- Poll the check status every few seconds while one runs; stop when step is completed or failed, then reload health.

## 7. What the demo fakes (build for real)
| Demo | Real build |
|---|---|
| A 30 second run (8 seconds for one facility) with steps by percentage | Steps from the backend; real duration |
| (Removed) pause, long-run and fail controls | The backend sends paused, long and failed; the page already shows those states |
| Health data, device lists, times | Real data |
| Check state in sessionStorage | Backend record (section 5) |
| The 90 minute long-run threshold | Agree a real threshold |
| The failure reason text | The backend's reason |

## 8. Not built
The backend run on the saved schedule (the settings pane stores the time, but the backend does not poll on a schedule yet), a history of past checks (built and removed; the log code remains but nothing shows it), email or header notices for a finished check on other pages, a cooldown after a check. The header buttons "My requests" and "Report an issue" reuse the District Home behaviour; "My requests" and the "View my tickets" link open `My Requests.dc.html`, which is not in this repo yet (point them at your requests page).

## 9. Library components used
Facility row (with the new logoSize and viewMode props), Stat card (with the new compact prop), Activity row, Pill (with icon and dot), Button, Icon button, Link button, Notice, Modal shell, Icon, Sidebar. The progress bar in the card is built in the screen (8px bar with a gradient track, no counter), because the library Progress bar always shows a counter. The health row and the expanded details are built in the screen, not in the library yet; build them as a Health row component if more screens need them.
