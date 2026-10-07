# THE LIST OF DOOM

## Data model

```js
Item {
  id: string,
  text: string,
  note: string,
  done: boolean,
  due: string,      // YYYY-MM-DD or ""
  collapsed: boolean,
  children: Item[]
}

List {
  id: string,
  title: string,
  items: Item[]
}
```

## Component / module structure

- `index.html` (single-file app)
  - Sidebar: list switcher, new list, theme toggle, undo
  - Toolbar: list title, quick add, search, filters, import/export
  - Tree renderer: recursive nested checklist UI (3+ levels)
  - Progress renderer: list-level + parent-level leaf-only progress bars
  - Import modal: file upload/drag-drop, AI parsing, preview/edit/regenerate/confirm
  - State + helpers: localStorage persistence, checkbox propagation, reorder, indent/outdent, undo

## Full code

The full implementation is in:
- `/home/runner/work/THE-LIST/THE-LIST/index.html`

## How progress is calculated

Progress is computed from **leaf tasks only** (items with no children), so parent tasks are never double-counted.  
For any subtree: `doneLeaves / totalLeaves * 100`.
- Parent `done` status auto-syncs from children (`all children done => parent done`).
- Parent counters show `x/y done` using descendant leaves.
- Bars animate width changes and change color by threshold:
  - `< 33%` red
  - `< 66%` amber
  - `>= 66%` green
- A small celebration toast appears when a list reaches 100%.

## Setup / run

No build step required.

1. Open `/home/runner/work/THE-LIST/THE-LIST/index.html` in a browser.
2. (Recommended) use a local static server for file imports:
   - `python3 -m http.server 8080`
   - open `http://localhost:8080`
3. Data is saved in `localStorage`.
4. For AI import on unstructured files, enter your API key/endpoint/model in the import modal.

## Publish on GitHub Pages

This repository includes a workflow at `.github/workflows/deploy-pages.yml` that deploys the site on every push to `main`.

1. In GitHub, open **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **GitHub Actions**.
3. Push to `main` (or run the workflow manually from the **Actions** tab).
4. Your app will be published at `https://<your-username>.github.io/THE-LIST/`.
