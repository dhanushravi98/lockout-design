# Changelog

## v0.9.11 (draft)
- New: Stat card, Activity row, Device counts and Facility row (all In review), in the Cards category. Facility row is one school in a district list (logo, name, attention link, alert pill, device counts, recent lockdowns, View) and rearranges itself as its own width shrinks; Device counts collapses from four counts to two plus a +2 button to a single N devices button; Stat card is a headline number with an optional action badge; Activity row is one line of a Recent activity list.
- New screen: screens/District Home.dc.html, for the district admin login, built from these components. School logo and name header with My requests and Report an issue; Secure Mode banner with a pulsing alert pill on the school row; news banner; stat cards; Facilities and Recent activity. Attention items open a list with a Raise request action that opens the Service Request overlay on its review step with the devices chosen; submitted requests appear under My requests and in Recent activity, and the school row shows a request-raised pill that collapses to a check after 10 seconds.
- Sidebar and Nav item: items can be disabled (Assessments and Emergency Prep are disabled on this screen; Reports is removed from the district menu).
- Profile card: gear button and an unclipped last-login line.
- Link button: new tone prop ('warning'). Pill: text centred.
- Service Request Classic: new props preset and onSubmit.

## v0.9.10 (draft)
- New: Toast (In review). Pop-up confirmation with a round navy icon, title, optional second line and close button; floating places it at the top centre of the page.
- New: Mode button (In review). Round outlined icon button that grows into a primary Button with a label and green dot while a mode is on (Maintenance Mode).
- New: Rich text field (In review). One-line comment box that grows while typing and collapses when left; toolbar for bold, italic, underline and lists; paperclip for images only, 3 MB each.
- Icon button docs: weight and disabled props listed.
- New screen: screens/School Screen - Maintenance Mode.dc.html. Maintenance Mode on Tablet List, Smart Boot, Pull Box and Smart Light adds Checked, Replaced and Comments columns.

## v0.9.9 (draft)
- New: Contact card (In review). One contact with role, Primary or Secondary pill, name, phone and an outline Edit button; edit mode takes the fields as children, stacks Save and Cancel and puts Delete in the bottom right corner.
- New: Notice (In review). One-line success or error feedback bar with an optional close button.
- Button: new variant "danger" (fill #fef2f2, text #b91c1c, border #fecaca) for destructive actions.
- BREAKING (visual): Text field default is now a rectangle with 8px corners (was a pill). Pill stays available as shape="pill". Fill is white; hover darkens only the border; focused fields fill #eaeef5. Hover and focus are tracked inside the component, which also fixes a border that stayed dark after hover.
- Pill: new size "xs".
- Section header: keeps a light fill while its section is open.
- Modal shell and Confirm dialog: new prop confirmVariant; delete confirmations use "danger".

## v0.9.8 (draft)
- Compact School screen for 13-inch laptops (new screens/School Screen Adjusted.dc.html). Breadcrumb, school strip, tabs row, search and filter row and pagination footer are shorter. Device count removed from the school strip. Secondary links sit edge to edge.
- Tab: padding 11px 14px 12px becomes 6px 14px 7px; tab bar top padding 10px becomes 7px.
- Pagination: padding 14px 20px becomes 11px 20px.
- New: Link button (In review). The quiet secondary link with an icon for Site Map, Trainings, Emergency Prep and Assessments. Icon turns solid and a light background appears on hover.
- Compact values for the School screen are listed in Foundations.

## v0.9.7 (draft)
- New: Modal shell (Approved). The frame for dialogs: backdrop, white box, title with optional icon, scrolling body, footer with Cancel and a primary button.
- Confirm dialog and the Add Wi-Fi network and Add contact dialogs are built on Modal shell. Fields inside dialogs are 44px (the Confirm dialog password field was 46px).
- Still to build: dropdown with checkboxes, review layout. The Facilities List modal in the Dispatch screen is still inline; move it onto Modal shell later.

## v0.9.6 (draft)
- Library: removed the duplicate queued "Modals and overlays" tab (the real category with Confirm dialog stays). Still to build: modal shell, dropdown with checkboxes, review layout.
- All components Approved. The 10 that were In review (Icon button, Section header, Confirm dialog, Sidebar, Nav item, Nav divider, Sidebar toggle, Profile card, Avatar, Tooltip) are now signed off in their docs/*.json. Every library component is Approved.
- Screens: School Screen updated to the current design (Button, Icon, Text field, Icon button, Section header, Confirm dialog). The Dispatch menu item opens the Dispatch screen.
- Dispatch: Facilities List modal is an accordion (districts open one at a time).
- School Screen, Facility Information: four collapsible sections that open one at a time (Facility overview, Facility details, Direct contacts, Wi-Fi networks). Section headers are 50px (compact). Inline edit with Cancel and Save per section, Direct contacts edit per contact (Role, Name, Phone) with add and delete, Wi-Fi networks with add, show password (verify dialog) and update password. Every save shows a confirmation bar that closes after 10 seconds. Saved values are held in the page only.
- Section header: new compact prop (50px row); the action button shows only while the section is open.

## v0.9.5 (draft)
- New: Icon button, Section header, Confirm dialog (all In review).
- TextField: new type prop ('text' | 'password'). Not breaking.
- New screen: Dispatch (screens/Dispatch Screen.dc.html) with the Facilities List modal, grouped by district in collapsible Section headers. Each district shows 10 facilities in a scrolling box with a Show 10 more button.
- Screens: School Screen and the Report Issue overlay updated to the current design. They import library components (Button, Icon, Text field, Icon button, Section header, Confirm dialog). The Dispatch menu item opens the Dispatch screen.
- support.js: components can load from another folder (component-dir meta) and from a path. Not breaking.
- Confirm dialog docs: playground now has a password field placeholder control (on by default), so the password field shows in the preview.
- Text field docs: playground gains Type (text / password) and Value controls, so the password option can be seen in the preview.
- Housekeeping: README registry wording, docs/index.json version, removed components/INSTRUCTIONS.md.

## v0.9.3 to v0.9.4 (draft)
The Navigation components below were In review here and are Approved as of v0.9.6.
- Sidebar (new component). The main menu: logo and product name, Nav item groups split by Nav dividers, Profile card at the foot and Sidebar toggle on the right edge. 264px expanded, 80px collapsed. In the new Navigation category.
- Nav item (new component). One menu destination (icon and label, fully rounded row) with an active state; collapsed it is a 44px icon-only circle with its label in a Tooltip. In the Navigation category.
- Nav divider (new component). A thin decorative rule between groups of nav items. In the Navigation category.
- Sidebar toggle (new component). The flap on the sidebar's right edge that collapses and expands the menu. In the Navigation category.
- Profile card (new component). The signed-in person at the foot of the sidebar: Avatar, full name and last login; collapsed shows only the Avatar. In the Navigation category.
- Avatar (new component). A person's initials in a navy circle (36px in the sidebar profile, 44px on the user card). In the Navigation category.
- Tooltip (new component). A one-line label for an icon-only control, shown on hover and keyboard focus. In the Navigation category.
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
