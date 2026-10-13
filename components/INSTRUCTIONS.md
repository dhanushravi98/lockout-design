# v0.9.2: Icons

Copy these files into your local lockout-design repo, keeping the folder names. Overwrite when asked.

```
components/Icon.dc.html                new
components/IconExplorer.dc.html        new
docs/icon.json                         new
docs/index.json                        replaces the old one
components/LibraryEntry.dc.html        replaces the old one
components/Component Library.dc.html   replaces the old one
```

Why three are replaced whole: each needed small edits (an Icons category, an Icon line in the registry, and the explorer slot). Replacing them avoids hand-editing. If you changed those three files yourself since v0.9.1, tell me, or paste the changes by hand:

- docs/index.json: add { "name": "Icons", "explorer": true, "entries": ["icon"] } to categories, and remove the queued "Icons" item.
- LibraryEntry.dc.html: add isIcon lines to the registry (same as the others) and 'Icon' to the component list.
- Component Library.dc.html: add the explorer slot and the explorer flag in select().

## CHANGELOG.md, add at the top

```
## v0.9.2 (draft)
- Icons: Icon component (Phosphor, six weights) and Icon explorer (search, weight, size, color, category, copy code).
```

## Commands

```
git checkout -b library/icons
git add .
git commit -m "v0.9.2: add Icon component and explorer"
git push -u origin library/icons
```

Then open a pull request, or merge to main.

## Check

```
npx serve .
```

Open http://localhost:3000/components/Component%20Library.dc.html, click Icons, and confirm the explorer shows icons and the Icon entry shows its Playground. Needs internet.
