# Instructions for AI coding assistants

You are building production UI from design files in this repo.

## Read first
1. `components/Component Library.dc.html` and the data in `docs/`: every component, its props, exact values, status and where it is used.
2. The screen file you are building, for layout and behavior.
3. `components/*.dc.html`: reference implementations of the shared components.

## Rules
- Build each library component once in the product's own component system, then compose screens from them. Do not copy inline styles into screens.
- Use the values in the library exactly: colors, sizes, radius, padding, font sizes. Do not round or substitute.
- Brand navy is `#25395c`. Buttons, pills, badges and the pill-shaped Text field use a fully rounded corner (999px). Other radii (Text box 12px, cards 16px, tables 14px) are in each component's values. Do not apply 999px to anything else.
- Do not use components marked Deprecated or In review.
- If a screen needs something the library lacks, stop and list what is missing. Do not invent a variant.
- Demo data (names, rooms, MAC addresses, request numbers) is placeholder. Wire real data.
- Icons: use the Icon component and the icon names listed in the library (Icons category, `docs/icon.json`). Do not draw SVGs or invent icon names. Tab icons for devices (tablet, boot, pull box, light, alerts, school) are image files in `uploads/`.

- The design repo is read-only for developers. Never edit or commit files in it. Report missing or wrong values instead.
- Focus ring: show it only for keyboard focus (CSS :focus-visible). Do not add focus styles that appear after a mouse click. Text fields are the exception; they change fill while focused.

## Prompt to start a screen
"Build the screen in `<file>` using the components in `components/Component Library.dc.html` and the data in `docs/`. Follow `AGENTS.md`. First list the library components the screen uses and any that are missing from our codebase, then build the missing ones, then the screen."
