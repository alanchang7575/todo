# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

A deliberately simple to-do tracker: plain HTML, CSS and JavaScript, no frameworks, no build step, no package manager, no tests, no git (this applies to the to-do app itself; see `ccc-learner-pack/` below for a separate project that does have tests). Run it by opening `index.html` in a browser (double-click works; localStorage works on `file://` in Chrome).

- `index.html` is the whole app (inline `<style>` and `<script>`). Keep it as a single file.
- `mockup.html` is a preview page that embeds `index.html` in two iframes drawn as an iPhone 15 (393x852) and a Samsung Galaxy S24 (360x780). Open it in the same browser as the app so the iframes show the real saved items.

## Architecture (index.html)

- **State:** a `todos` array of `{ id, text, category, due, done }`, saved to localStorage key `todos` by `save()`. The theme is stored under key `theme`. All storage access is wrapped in try/catch so the app still works when storage is blocked.
- **Rendering:** `render()` rebuilds the whole list from `todos` and the category filter. Every state change is: mutate `todos`, call `save()`, call `render()`. `editingId` marks which row is in edit mode, and `row(t)` returns either the edit view or the display view.
- **Categories:** the fixed list is `CATEGORIES` (Home, Office). `categoryList()` also includes any other category found on saved items, so older data is never hidden. `fillCategories()` populates the add form, the edit row and the filter.
- **Theme:** `document.documentElement.dataset.theme` (`light` or `dark`) drives CSS rules on `:root[data-theme=...]`. The default comes from `prefers-color-scheme`.
- **Overdue:** an item is overdue when `due < today()` and it is not done. `today()` returns local-time YYYY-MM-DD, the same format as the date input, so plain string comparison works.

## Conventions

- Build DOM with `createElement` and `textContent`, never `innerHTML`, so typed text is never interpreted as markup.
- Mobile support relies on the viewport meta tag, flex-wrap layout and `font-size: 16px` on form controls (prevents iOS focus zoom). Keep those when changing styles.
- Edit and Delete are emoji buttons (✏️ 🗑️) with `title` and `aria-label` set through `button(label, onClick, name)`.
- The user asked for a minimal app, so don't add features that were not requested.

## ccc-learner-pack/ (separate from the to-do app)

Training material that happens to live in this folder. Don't apply the to-do app's rules to it, and don't touch it when working on `index.html`.

- `kopi-loyalty/` is a small Node (18+) bill calculator for a Singapore café, with its own `package.json`, tests and `CLAUDE.md`. Read that `CLAUDE.md` before changing it. Run tests from inside that folder with `npm test`.
- `brand/`, `receipts/`, `contacts_messy.csv` and the PDF guide are sample inputs for exercises.
