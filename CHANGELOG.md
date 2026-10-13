# Changelog

## Unreleased
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
