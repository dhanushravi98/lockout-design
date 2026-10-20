# Changelog

## v0.9.6 (draft)
- All components Approved. The 10 that were In review (Icon button, Section header, Confirm dialog, Sidebar, Nav item, Nav divider, Sidebar toggle, Profile card, Avatar, Tooltip) are now signed off in their docs/*.json. Every library component is Approved.
- New: Icon button, Section header, Confirm dialog (all In review).
- TextField: new type prop ('text' | 'password'). Not breaking.
- New screen: Dispatch (screens/Dispatch Screen.dc.html) with the Facilities List modal, grouped by district in collapsible Section headers. Districts open one at a time (accordion). Each district shows 10 facilities in a scrolling box with a Show 10 more button.
- Screens: School Screen and the Report Issue overlay updated to the current design. They import library components (Button, Icon, Text field, Icon button, Section header, Confirm dialog). The Dispatch menu item opens the Dispatch screen.
- School Screen, Facility Information: four collapsible sections that open one at a time (Facility overview, Facility details, Direct contacts, Wi-Fi networks). Section headers are 50px (compact). Inline edit with Cancel and Save per section, Direct contacts edit per contact (Role, Name, Phone) with add and delete, Wi-Fi networks with add, show password (verify dialog) and update password. Every save shows a confirmation bar that closes after 10 seconds. Saved values are held in the page only.
- Section header: new compact prop (50px row); the action button shows only while the section is open.
- support.js: components can load from another folder (component-dir meta) and from a path. Not breaking.
- Housekeeping: README registry wording, docs/index.json version, removed components/INSTRUCTIONS.md.

## v0.9.5 (draft)
- New: Icon button, Section header, Confirm dialog (all In review).
- TextField: new type prop ('text' | 'password'). Not breaking.
- New screen: Dispatch (screens/Dispatch Screen.dc.html) with the Facilities List modal, grouped by district in collapsible Section headers. Each district shows 10 facilities in a scrolling box with a Show 10 more button.
- Screens: School Screen and the Report Issue overlay updated to the current design. They import library components (Button, Icon, Text field, Icon button, Section header, Confirm dialog). The Dispatch menu item opens the Dispatch screen.
- support.js: components can load from another folder (component-dir meta) and from a path. Not breaking.
- Confirm dialog docs: playground now has a password field placeholder control (on by default), so the password field shows in the preview.
- Text field docs: playground gains Type (text / password) and Value controls, so the password option can be seen in the preview.
- Housekeeping: README registry wording, docs/index.json version, removed components/INSTRUCTIONS.md.

## Unreleased
- Sidebar (In review): new component. The main menu: logo and product name, Nav item groups split by Nav dividers, Profile card at the foot and Sidebar toggle on the right edge. 264px expanded, 80px collapsed. In the new Navigation category.
- Nav item (In review): new component. One menu destination (icon and label, fully rounded row) with an active state; collapsed it is a 44px icon-only circle with its label in a Tooltip. In the Navigation category.
- Nav divider (In review): new component. A thin decorative rule between groups of nav items. In the Navigation category.
- Sidebar toggle (In review): new component. The flap on the sidebar's right edge that collapses and expands the menu. In the Navigation category.
- Profile card (In review): new component. The signed-in person at the foot of the sidebar: Avatar, full name and last login; collapsed shows only the Avatar. In the Navigation category.
- Avatar (In review): new component. A person's initials in a navy circle (36px in the sidebar profile, 44px on the user card). In the Navigation category.
- Tooltip (In review): new component. A one-line label for an icon-only control, shown on hover and keyboard focus. In the Navigation category.
- Icons: the hand-drawn SVG icons inside the shared components now use the Icon component (Phosphor). Checkbox check (check), Chip and Dropzone remove (x), Dropzone upload (upload-simple, regular), Pagination per-page chevron (caret-down) and Prev / Next (caret-left / caret-right, were text ‹ ›), User card counter (caret-up) and open button (caret-right), Icon explorer category dropdown (caret-down). Sizes and colors unchanged; small controls use bold to match the old 2.4 to 3px strokes. Pagination Prev / Next gain a 4px gap between icon and text. Props unchanged. Screens not changed.
- Playgrounds: new icon picker control (search and pick from the Phosphor icon library only). Used by Button, Chip, Pill and Icon.
- Icon picker: icons in the field and the grid are shown in Phosphor fill.
- Button, Chip and Pill: icons are now drawn in Phosphor fill (was regular), matching the picker. Size and color unchanged.
- Icon: fixed icons sitting 1px high beside text (Button, Chip, Pill). The glyph is now a block exactly `size` px square, so its wrapper no longer adds 2px of line space below it. Sizes and colors unchanged.
- BREAKING: Button, Chip and Pill `icon` now takes an icon library name (e.g. "wrench") instead of SVG path data, and draws it with the Icon component (Phosphor regular, same 18px / 14px size, color follows label). No screens imported these components, so none needed changes.
- Button: fixed real hover, pressed and focus, which never showed. support.js does not fill {{ }} values in style-hover/active/focus, so the rules were empty. Button now sets those states with fixed classes (same values).
- Button playground: added a State control (default, hover, pressed, focus). Icon controls are now Icon (Off, On), then Icon position and Icon name, shown only when Icon is On.
- Chip and Pill playgrounds: Leading icon (Off, On), then Icon name when On.
- Playgrounds: a control can set "when": "<prop>" to show only while that control is on.
- Component Library: content column now spans the full width beside the sidebar (was capped at 920px).
- Button: renamed variants filled, outlined, ghost to primary, secondary, tertiary. Values and states unchanged.

## v0.9.2 (draft)
- Icons: Icon component (Phosphor, six weights) and Icon explorer (search, weight, size, color, category, copy code).

## v0.9.1 (draft)
- Chips, pills and badges rebuilt as four components: Chip (clickable: toggle, removable, icon, count), Chip group (single or multi select), Pill (read-only text or status, optional dot or icon), Badge (count or dot, inline or corner).
- BREAKING: Pill no longer takes `count` or `onClick`. Use Badge for counts and Chip for anything clickable.
- Library category renamed to "Chips, pills and badges".
- Chip: fixed lost right padding after toggling removable off, and lost border color after leaving a forced state. Padding and border are now set without mixing shorthand and longhand. Values unchanged.

## v0.9.0 (draft)
- Component Library: Foundations (color scales), Buttons, Pills and chips, Form fields (text field, text box, checkbox, radio, drag and drop), Tabs, Cards (user card), Tables and pagination.
- Shared components in components/: Button, Pill, Chip, TextField, TextArea, Checkbox, Radio, Dropzone, Tab, UserCard, DataTable, Pagination.
- Screens: School Screen, Service Request overlay, User Management.
- Emails: service request (with and without requests), suggestion.
- Not yet in the library: Modals and overlays, Icons, device card.
