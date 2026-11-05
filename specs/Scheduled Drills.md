# Scheduled drills: functional specification

Screen: `School Screen Fac Info.dc.html` (header button **Schedule a drill**). School side only for now; section 7 lists what dispatch needs. All data in the screen is demo data held in the page; replace it with real data.

## 1. Purpose
A school admin schedules a drill to test the alerts (Lockdown, Secure mode, or both) so dispatch knows in advance and does not respond or send resources to the school. Dispatch is only notified; it does not approve anything.

## 2. Entry
Secondary button **Schedule a drill** (calendar icon, 46px high, fully rounded) in the school header, beside Report Issue. It opens one overlay (Modal shell, 720px wide, fixed 660px height, no close button in the title bar, closes with Cancel) with the title "Schedule a drill" ("Edit drill" while editing) and the subtitle "Dispatch is told ahead of time, so they do not need to respond."

## 3. Layout of the overlay
- Title area, fixed.
- Tab bar, fixed: grey bar (`#e8ebf0`, 14px rounded top corners, thin `#e5e7eb` border, 10px padding at the top and sides, the same bar as the devices table tabs on the school page) with two Tab components: **Schedule** (calendar-plus icon; the label reads "Edit drill" while editing) and **Upcoming and past** (list icon, with a count of upcoming drills). The selected tab's icon is solid and the tab joins the panel below. Same structure as the SMS and Email tabs on the Texting page.
- Panel under the tabs (`#fafbfc`, thin border, rounded bottom): **only this panel scrolls**. The title and tab bar never move.
- Footer, Schedule tab only: **Cancel** (secondary) and **Schedule drill** (primary; **Save changes** when editing). The Upcoming and past tab has no footer.
- A green Notice at the top of the panel appears after every action and disappears after 8 seconds or when closed.

## 4. Schedule tab
All fields are required except notes. Errors show after the first press of the primary button, in red under the field.
| Field | Control and rule | Error |
|---|---|---|
| Alert to test | Chips: **Lockdown** (lock icon, red tone) and **Secure mode** (shield icon, yellow tone). Each toggles on its own; choose one or both. Both are tested in the same drill and shown as "Lockdown + Secure mode". | "Choose at least one alert to test." |
| Date and Start time | Native date and time inputs (the browser draws the pickers). The date and time must be in the future, with no minimum notice. Past dates are disabled in the date picker. | "Choose a date and start time in the future." |
| Duration window | Chips, one only: 30 minutes, **1 hour (default)**, 2 hours. | "Choose how long the drill runs." |
| Notes for dispatch | Optional text area, for example the areas involved or the time the alert is sent. | none |

Chip colours (library Chip, `tone` prop): Lockdown is `error` (off: fill `#fef2f2`, text `#b91c1c`, border `#fecaca`; chosen: solid `#b91c1c`, white text). Secure mode is `warning` (off: fill `#fff7e0`, text `#92600a`, border `#f6e4b0`; chosen: solid `#f5c242`, text `#4a3200`, border `#e0a823`). The icon takes the text colour.

Below the fields an info line says: dispatch is told when a drill is scheduled or changed, reminded 24 hours and 1 hour before, and shown a banner while it runs; they do not need to respond or send anyone.

## 5. Upcoming and past tab
Two groups: **UPCOMING** (Scheduled and Running, soonest first) and **PAST** (Completed and Cancelled, latest first). Empty state: "No drills yet. Use the Schedule tab to plan one."
Each row: round icon (lock for Lockdown, shield-check for Secure mode, shield-warning for both), "{type} drill", a status pill, "{date} · {time} · {duration}", and a small grey line saying when dispatch was told ("Dispatch notified Nov 3, 2:10 PM", "Dispatch told of the change ...", "Dispatch told it was cancelled ..."). Notes are not shown here; they are for dispatch.
Row actions:
- Scheduled: **Edit** (outline icon button, pencil) and **Cancel drill** (red icon button).
- Running: **End drill** (primary).
- Completed, Cancelled: none.

## 6. Statuses and behaviour
| Status | Pill | Meaning |
|---|---|---|
| Scheduled | info | Booked, not started. Editable and cancellable. |
| Running | warning | The alert test is in progress. |
| Completed | success | Ended. |
| Cancelled | tint | Withdrawn before it started. |

Moves: Scheduled -> Running -> Completed, and Scheduled -> Cancelled. There is no Start drill button; a drill becomes Running when the alert is sent from the system (not built yet), and the school ends it with **End drill**. Nothing changes by the clock.
- **Schedule:** adds a Scheduled drill (id DR-n, next number), switches to the Upcoming and past tab, shows "Drill scheduled for {date} · {time}. Dispatch has been notified."
- **Edit:** opens the Schedule tab filled with the drill. Save changes updates it and shows "Drill updated. Dispatch has been notified of the change." **Cancel while editing** discards the changes and returns to the Upcoming and past tab with the overlay open; Cancel while scheduling a new drill closes the overlay.
- **Cancel drill:** asks "Cancel this drill?" (Confirm dialog, red confirm button "Cancel drill", secondary "Keep drill"; message names the type and date). Confirming sets Cancelled and shows "Drill cancelled. Dispatch has been told."
- **End drill:** sets Completed and shows "{type} drill ended and marked completed."
- Every schedule, edit, cancel and end updates the drill's "dispatch notified" line with the current date and time.

## 7. Data
Drill: id (DR-103), type (`Lockdown`, `Secure mode` or `Lockdown + Secure mode`), start (timestamp), dur (`30 minutes`, `1 hour`, `2 hours`), notes (text, optional), status, notified (text of the last notice to dispatch). One school per drill. The school is the one whose page is open.

## 8. Library components used
Modal shell, Tab, Chip (with `icon` and the new `tone` prop), Button, Icon button (outline and danger variants), Pill, Text area, Notice, Confirm dialog. Do not copy the screen's inline styles; build each component once and compose.

## 9. What dispatch needs (not built)
- A **Scheduled drill** badge on the school in the dispatch districts and school views, with the date, time and type.
- A notice when a drill is scheduled, changed or cancelled; reminders 24 hours and 1 hour before.
- A banner on the dispatch side while the drill is Running, removed when it ends. Drill alerts must be marked as drills so they are never treated as real events.
- Sending the drill alert from the system, which moves a drill to Running.
