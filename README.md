# LockOut USA design repo

Design source for the LockOut USA admin app. Files ending in .dc.html are Design Components: they open directly in a browser and are the reference developers build from.

## Open it

The library page loads the shared components from separate files, and browsers block that when a page is opened by double-click. Serve the folder:

```
npx serve .
```

then open `http://localhost:3000/components/Component%20Library.dc.html`. Or publish the repo with GitHub Pages and open it there.

## Structure

```
components/                 Shared components, one file each
  Component Library.dc.html The library page (open this one)
  LibraryEntry.dc.html      Shows one library entry from its data file
docs/                       One .json data file per component, plus index.json and foundations.json
screens/                    School Screen, Service Request overlay, User Management
emails/                     Service request and suggestion email templates
reference/                  Older spec pages, to be folded into the library
uploads/                    Icons and logo
support.js                  Runtime for the .dc.html files
AGENTS.md / CLAUDE.md       Rules for AI coding assistants
CHANGELOG.md                What changed in each version
```

## Status labels

- Approved: signed off. Build to spec.
- In review: do not build yet.
- Deprecated: do not use in new work; the replacement is named in the library.

## Adding a component

1. `components/NewThing.dc.html`: the component.
2. `docs/new-thing.json`: its library entry (copy an existing one: description, status, controls, states, props, values).
3. `docs/index.json`: add the id to its category.
4. `components/LibraryEntry.dc.html`: add one line to the marked registry for the new component name.

## Working on it

1. Branch: `design/<name>` for screens, `library/<name>` for components.
2. Change the component file, then update its entry in components/Component Library.dc.html and CHANGELOG.md in the same commit.
3. Open a pull request. A reviewer checks the library entry matches the component.
4. Merge and tag a version, for example v0.9.1.

## Building a screen in the product

Give the AI assistant the screen file, AGENTS.md and the library. Starter prompt is in AGENTS.md.
