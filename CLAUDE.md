# CLAUDE.md — Kixie Alert Center (for Claude / AI assistants)

Use this file as **fast constraints** when editing this repo. The full spec is **`kixie-design-guidelines.md`** (`npm run guidelines` opens it on macOS).

---

## Project context

- **Product:** Kixie **Alert Center** — operational dashboard: alert list, detail, recommended actions, modals (escalate / override), timeline, toasts.
- **Implementations:** Legacy **`index.html`** (single-file) and/or **`src/`** (Vite + React). **Match tokens and patterns below** regardless of stack.
- **Brand:** Dark navy sidebar, light main workspace; **severity colors are fixed semantics**, not decorative choices.

---

## Non‑negotiables

1. **Use design tokens first** — Prefer `:root` CSS variables in `index.html` (or mirror them in `src/styles/theme.css` / theme). Do not introduce random hex colors for things that already have a role (severity, primary, borders).
2. **Severity mapping** — **Critical** → `--critical` `#e5484d` · **Urgent** → `--urgent` `#f97316` · **Info** → `--info` `#3b82f6` · **Resolved** → `--resolved` `#10b981`. Same meaning for dots, list labels, badges, and timeline accents.
3. **Two blues on purpose** — **`--primary` `#4163ed`** = app chrome / topbar / filters. **Indigo `#6366f1` / `#4f46e5`** = **primary CTAs in the right action column and modal primaries** (stacked “do the thing” buttons). Do not collapse these into one blue without an explicit product decision.
4. **Layout widths** — Sidebar **220px**, alert list **340px**, actions **300px**, detail **flex 1** (min **280px**). Keep **full-height** flex columns with **`min-height: 0`** where scrolling is needed.
5. **Accessibility** — Interactive rows as **buttons** with a clear accessible name; modals **`aria-modal`**, labelled, **return focus** on close; decorative SVGs **`aria-hidden`**. Filters: **toolbar + `aria-pressed`**, not fake tabs without full tab semantics.

---

## Token cheat sheet

| Token | Hex (typical) | Use |
|--------|----------------|-----|
| `--sidebar-bg` | `#1e1e40` | Sidebar |
| `--page-bg` | `#f8f9fa` | Canvas / list bg |
| `--surface` | `#ffffff` | Cards, detail, modals |
| `--border` | `#e5e7eb` | Dividers, inputs |
| `--text` | `#1c1b1f` | Body |
| `--muted` | `#79747e` | Meta, labels |
| `--primary` | `#4163ed` | Chrome / topbar primary actions |
| `--primary-hover` | `#3554c9` | Hover for `--primary` |
| `--critical` / `--urgent` / `--info` / `--resolved` | see above | Severity only |

**Common literals (align if tokenizing):** selection/filter on **`#e8f0fe`**; rec box **`#eff6ff`** / border **`#bfdbfe`**; success panels **`#e8f5e9`**, **`#4caf50`** rings; audit card **`#fffbeb`** / **`#fde68a`**; modal scrim **`rgba(15, 23, 42, 0.45)`**.

**Typography:** `--font` = `"Segoe UI", system-ui, -apple-system, sans-serif` · base **14px**, line-height **1.45** · section labels: small caps / letter-spacing, **uppercase** where the spec says so.

**Radius:** `--radius` **8px** default; filter pills **pill (999px)**; modals **10px**; small controls **4–6px**.

---

## Components — do this

- **List row:** Severity row → title → impact chip → meta; selected = **`#e8f0fe`** + **4px left** primary border.
- **Detail:** Uppercase muted section headers; severity **badge** with semantic background + border (see guidelines).
- **Timeline:** Vertical line **2px** gray; event body block with **left accent** by type (escalation / override / restart).
- **Right column:** “SYSTEM RECOMMENDS” in **rec-box** (blue tint); **full-width** action stack; **audit** = amber card when override data exists.
- **Modals:** Max width **~440px**, white panel, heavy shadow; escalate = context strip + **team tiles**; override = **2×2 severity** + required reason + footer actions.
- **Toasts:** **Top-right**; dismissible; don’t block the whole UI.

---

## Components — avoid

- Tablist **`role="tablist"`** for filters unless you implement full tab semantics (`role="tab"`, selection, panels).
- A **single** “primary blue” for both topbar and action-column primaries — breaks visual hierarchy.
- **Uppercase** body copy or timeline sentences — reserve uppercase for **badges** and **micro-labels** as specified.
- Ornate decoration that competes with **severity** and **density** of operational data.

---

## Content tone

- Alert titles: **short, title case**, specific.
- System recommendations: **imperative**, operational.
- Timeline / audit: **factual** (time, action, attribution); **no marketing fluff**.

---

## When in doubt

1. Open **`kixie-design-guidelines.md`** for tables, spacing, and component detail.
2. Prefer **extending `:root`** (or shared theme) over one-off colors.
3. After UI changes, verify **keyboard**, **focus return from modals**, and **screen reader labels** on new controls.

---

*Keep this file and `kixie-design-guidelines.md` in sync when tokens or layout constants change.*
