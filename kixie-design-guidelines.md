# Kixie Alert Center — Design Guidelines

This document describes the visual and interaction design system used by the **Alert Center** experience in this project (`index.html`). Use it when extending the UI or aligning new screens with the same product language.

---

## 1. Design principles

- **Clarity first:** Dense operational data (severity, region, time) stays scannable; hierarchy uses weight, size, and color—not decoration alone.
- **Light workspace, dark navigation:** The main work area is bright neutrals; wayfinding lives in a saturated navy sidebar.
- **Severity is semantic:** Critical / Urgent / Info / Resolved map to fixed colors everywhere (dots, list labels, detail badges, timeline accents).
- **Actions are explicit:** Primary actions are filled; secondary actions are outline or neutral; destructive or exceptional flows use distinct affordances (e.g. audit card, modal copy).
- **Accessible by default:** Focus states, landmarks, modal focus return, and descriptive labels are part of the design—not an afterthought.

---

## 2. Design tokens (CSS variables)

These are defined on `:root` and should remain the single source of truth for new components.

### 2.1 Color

| Token | Role | Typical use |
|--------|------|-------------|
| `--sidebar-bg` `#1e1e40` | Sidebar background | Left navigation |
| `--sidebar-text` `#e8eaf0` | Sidebar primary text | Nav labels, active item |
| `--sidebar-muted` `#9aa3b5` | Sidebar secondary | Inactive items, section labels |
| `--page-bg` `#f8f9fa` | App canvas | Behind list column, action rail |
| `--surface` `#ffffff` | Cards, modals, detail | Panels on top of page bg |
| `--border` `#e5e7eb` | Hairlines & outlines | Column borders, inputs, pills |
| `--text` `#1c1b1f` | Body text | Titles, paragraphs |
| `--muted` `#79747e` | Supporting text | Meta, timestamps, labels |
| `--primary` `#4163ed` | Brand / app chrome | Topbar primary buttons, links in chrome |
| `--primary-hover` `#3554c9` | Primary hover | |
| `--critical` `#e5484d` | Severity: Critical | Dots, list severity line |
| `--urgent` `#f97316` | Severity: Urgent | |
| `--info` `#3b82f6` | Severity: Info | |
| `--resolved` `#10b981` | Severity: Resolved | |

**Additional fixed colors in components (not all tokenized):**

- **Selection / filter “on” state:** Background `#e8f0fe`, border/text tied to `--primary`.
- **Recommendation box:** Background `#eff6ff`, border `#bfdbfe`, label text `--primary`, body `#1e3a5f`.
- **Success / resolved celebration:** Greens `#4caf50` (icon ring), `#e8f5e9` / `#c8e6c9` (surfaces), text `#1b5e20`, `#558b2f`.
- **Audit trail (override):** Background `#fffbeb`, border `#fde68a`, text `#713f12` / headings `#92400e`.
- **Modal backdrop:** `rgba(15, 23, 42, 0.45)`.
- **Indigo action stack (recommended actions column):** Primary stacked CTAs use indigo `#6366f1` / hover `#4f46e5` (distinct from `--primary` topbar blue—intentional hierarchy).

### 2.2 Typography

| Setting | Value |
|---------|--------|
| Font stack | `"Segoe UI", system-ui, -apple-system, sans-serif` (`--font`) |
| Base size | `14px` (`body`) |
| Base line height | `1.45` |

**Patterns:**

- **Page title (topbar):** `1.125rem`, weight `600`.
- **Detail title:** `1.5rem`, weight `600`.
- **Section / column labels:** `0.6875rem`–`0.65rem`, weight `600`–`700`, `letter-spacing` ~`0.06em`–`0.08em`, often **uppercase** (`text-transform: uppercase` where specified).
- **Alert card title:** `0.9375rem`, weight `700`.
- **Pills / small UI:** `0.75rem`–`0.8125rem`.

### 2.3 Radius & elevation

| Token / pattern | Value |
|-----------------|--------|
| `--radius` | `8px` — default for cards, inputs, severity tiles |
| Small controls | `4px`–`6px` — badges, small buttons, dots |
| Pills (filters) | `999px` (fully rounded) |
| Detail severity badge | `20px` pill shape |
| Modals | `10px` panel, shadow `0 20px 50px rgba(0,0,0,0.18)` |
| Toasts (current) | `16px`, light shadow |

---

## 3. Layout shell

| Region | Width / behavior |
|--------|------------------|
| Sidebar | `220px`, fixed, `flex-shrink: 0` |
| Alert list column | `340px`, scrollable list |
| Detail column | `flex: 1`, `min-width: 280px`, scrollable |
| Actions column | `300px`, `flex-shrink: 0`, background `#f8f9fa` (page-bg–like) |

**Workspace:** Horizontal flex, full viewport height (`100vh`), `min-height: 0` on flex children to allow internal scrolling.

**Topbar:** White surface, bottom border `--border`, padding `0.65rem 1.25rem`, title + optional meta + filter toolbar + actions aligned end (`margin-left: auto`).

---

## 4. Severity system

### 4.1 Semantic levels

| Level | Token | List label style | Detail badge |
|-------|--------|------------------|----------------|
| Critical | `--critical` | Red, uppercase micro label + dot | Light red fill, dark red text, border |
| Urgent | `--urgent` | Orange | Amber / orange badge |
| Info | `--info` | Blue | Blue badge |
| Resolved | `--resolved` | Green | Green badge |

**Dots:** `6px` circles in list and summary row; summary uses classes `dot-c`, `dot-u`, `dot-i`, `dot-r`.

**Selection:** Selected alert row = light blue background `#e8f0fe`, **4px** left border `--primary`.

### 4.2 Copy

- Severity labels in UI are **uppercase** where shown as badges (`CRITICAL`, `URGENT`, etc.).
- Sentence case is used in paragraphs and timeline titles.

---

## 5. Core components

### 5.1 Filter toolbar

- **Role:** `toolbar`, `aria-label` describes purpose (e.g. “Filter alerts by severity”).
- **Control:** Pill buttons; active state = `is-on` (border `--primary`, background `#e8f0fe`, text `--primary`).
- **Optional:** Leading color dot per severity filter.

### 5.2 Alert list rows

- Full-width **button** per row (keyboard and SR friendly).
- Structure: severity line → title → impact pill → meta line.
- **Impact pill:** Neutral gray `#e5e7eb`, small radius `4px`.
- **Transient success (restart):** Green block below row with circular check, title + subtitle (auto-dismiss timing is a behavior spec, not a visual token).

### 5.3 Detail panel

- Labels: uppercase micro, muted.
- **Grid:** Two columns for Region / Service / optional Status; keys uppercase muted, values `0.875rem`.
- **Event explanation:** Muted label; body `#374151`, comfortable line height `~1.55`.

### 5.4 Activity timeline

- Vertical rail: `2px` line `#e5e7eb`, dots `8px` with light border.
- Time: `0.75rem` muted.
- Title: `0.875rem`, weight `500`.
- Optional body block: `#f9fafb` background, left accent border; color by event type (escalation = primary, override = critical red, restart = resolved green).

### 5.5 Right column — recommendations & actions

- **SYSTEM RECOMMENDS** callout: Blue-tinted box (`rec-box`), uppercase micro label in `--primary`.
- **Button stack:** Full width, `0.55rem` vertical padding, `6px` radius, `0.8125rem` font.
  - **Primary:** Indigo filled (`btn-restart` pattern).
  - **Secondary:** White + border (escalate / neutral).
  - **Override:** Distinct outline (e.g. orange accent in current CSS—treat as “caution / exception” affordance).
- **Audit trail card:** Yellow-amber panel when an override exists; key/value rows, uppercase section heading.
- **Quick info:** Top border separator, label/value rows muted vs default text.

### 5.6 Modals (Escalate & Override)

- **Backdrop:** Dark scrim, centered flex, `z-index` high layer (`10000`).
- **Panel:** Max width `440px`, white, `10px` radius, heavy shadow, `1px` border.
- **Header:** Title + optional subtitle; close control top-right.
- **Escalate:** Context block in gray `#f3f4f6`, team cards as selectable tiles (selected = primary border + light blue fill + subtle ring).
- **Override:** 2×2 severity grid, large textarea (muted background until focus), footer actions right-aligned.
- **Primary modal actions:** Indigo to match column primary actions (`btn-escalate-primary` / `btn-ov-submit`).

### 5.7 Resolved states

- **Inline (detail):** Green panel with circular check icon, bold title “Alert Resolved”, supporting line in green-muted tone.
- **Right column success:** Large ring + check, centered copy, optional `aria-live`.

### 5.8 Toasts

- Fixed **top-right**, max width clamped to viewport.
- Flex row: optional leading mark/icon, text block, dismiss control.
- Typography: title line weight `600`, secondary line muted `#49454f`.
- Enter/exit: short opacity + translate transition.

---

## 6. Interaction & motion

- **Hover:** Subtle background shifts on rows, outline buttons, team cards, severity cards (`#fafafa`, border `#cbd5e1`).
- **Selected tiles:** Primary border + `#eff6ff` fill + light outer ring (`box-shadow` with primary alpha).
- **Disabled:** `opacity: 0.45`, `cursor: not-allowed`.
- **Transitions:** ~`0.15s` on borders/shadows for cards; ~`0.2s` on toast visibility.

Prefer **`prefers-reduced-motion`** for future motion-heavy additions (not globally implemented in the current file).

---

## 7. Accessibility (product expectations)

Aligned with current implementation:

- Skip link to `#main-content`; main landmark with `tabindex="-1"` for programmatic focus.
- Filter toolbar **not** exposed as tabs unless full tab semantics are implemented; use **toolbar** + **pressed** state on filter buttons.
- Alert rows expose a concise **`aria-label`**; selected row uses **`aria-current`**.
- Modals: `aria-modal`, labelled titles, described-by where needed; **focus return** to invoking control on close; **Tab** cycles within the active dialog.
- Decorative icons: **`aria-hidden="true"`** on SVGs that duplicate text.
- Live regions: detail panel and toasts use appropriate **`aria-live`** / **`aria-hidden`** when hidden.

---

## 8. Content & tone

- **Alert titles:** Short, specific, title case (e.g. “SSO Authentication Failure”).
- **System recommendation:** Imperative, operational (“Restart authentication service”, “Increase pool size…”).
- **Timeline:** Timestamp first line, human-readable action second; attribution (“by system”, “by you”) on a tertiary line when present.
- **Audit / override:** Factual fields (who, when, from → to, reason); no marketing copy.

---

## 9. File & maintenance note

- **Canonical tokens** live in `:root` inside `index.html`. When adding new screens, **extend the token set** instead of hard-coding one-off hex values, unless mapping a truly new semantic (then add a variable with a comment).
- This document should be updated when **severity colors**, **primary/indigo split**, or **column widths** change.

---

*Last aligned with the Alert Center implementation in this repository.*
