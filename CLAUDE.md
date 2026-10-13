# LockOut USA design repo

Follow AGENTS.md in this folder.

- components/Component Library.dc.html, fed by docs/*.json, is the single source of truth for every value (color, size, radius, padding, states). Do not guess values.
- Shared components live in components/. Screens in screens/ are built from them.
- When you change a component, update its entry in docs/<name>.json (props, values, states, controls) in the same commit and add a line to CHANGELOG.md.
- A new component is four edits: components/<Name>.dc.html, docs/<name>.json, a line in docs/index.json, and three edits in the registry in components/LibraryEntry.dc.html (see the registry check below).
- Do not add a new color, size or variant. If something is missing, list it and ask.
- Status labels: Approved is final, In review is not to be built, Deprecated is not to be used.
- Never edit files in a way that changes a component's props without updating every screen that imports it.

## After adding a new component: registry check (required)

A new component is registered in three places in `components/LibraryEntry.dc.html`. All three are required. If one is missing, its library entry is blank and no error is shown.

1. Playground block: add `<sc-if value="{{ isName }}"><dc-import name="Name" dc-props="{{ live }}" ...></dc-import></sc-if>` next to the `REGISTRY` comment.
2. Grid block: add the same kind of `<sc-if>` using `dc-props="{{ it.props }}"`.
3. Name list: add `'Name'` to the array that sets the `isX` flags.

Also check:
- The `component` field in `docs/<name>.json` matches `Name` exactly, including capitals.
- `docs/index.json` lists the component in a category.

Before finishing a new component, run this check using the real component name (never a placeholder) and report the result in this form:

| Component | Playground | Grid | Name list | docs `component` matches | In docs/index.json |
|---|---|---|---|---|---|

Every cell must say yes. If any cell says no, fix it before committing.