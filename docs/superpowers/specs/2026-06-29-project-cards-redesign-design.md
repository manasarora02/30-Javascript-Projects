# Project Cards Redesign — Design Spec

**Date:** 2026-06-29  
**Scope:** `index.html` + `style.css` only. No new files, no new dependencies.

## Goal

Make the project cards more attractive by adding: emoji icons (content richness), a large ghost number (decorative depth), a 2px category-colored top border, and a category-tinted hover glow.

## Changes

### 1. JS data — add `icon` field

Each project object in the `projects` array gets an `icon` string (emoji). Example:

```js
{ n: 1, name: 'Weather App', slug: '...', cat: 'api', icon: '🌤️' }
```

### 2. Card HTML template

Two new elements added to the card `<a>`:

- `<span class="card-icon">${p.icon}</span>` — between `card-top` and `card-bottom`
- `<span class="card-ghost" aria-hidden="true">${String(p.n).padStart(2,'0')}</span>` — absolutely positioned decorative bg number

### 3. CSS — `style.css`

**Ghost number:**
```css
.card-ghost {
  position: absolute;
  bottom: -8px; right: 12px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 90px; font-weight: 700;
  color: #fff; opacity: 0.04;
  line-height: 1; pointer-events: none;
  user-select: none; z-index: 0;
  transition: opacity 0.25s ease;
}
.card:hover .card-ghost { opacity: 0.08; }
```

**Icon:**
```css
.card-icon {
  font-size: 28px; line-height: 1;
  margin: auto 0;
  position: relative; z-index: 1;
}
```

**Category top border** (6 rules, one per category):
```css
.card[data-cat="api"]         { border-top: 2px solid rgba(56,189,248,0.5); }
.card[data-cat="tool"]        { border-top: 2px solid rgba(167,139,250,0.5); }
.card[data-cat="ui"]          { border-top: 2px solid rgba(251,146,60,0.5); }
.card[data-cat="forms"]       { border-top: 2px solid rgba(244,114,182,0.5); }
.card[data-cat="media"]       { border-top: 2px solid rgba(251,191,36,0.5); }
.card[data-cat="interactive"] { border-top: 2px solid rgba(74,222,128,0.5); }
```

**Category-tinted hover glow** (6 rules — replaces the always-green shadow on hover):
```css
.card[data-cat="api"]:hover         { box-shadow: 0 12px 40px rgba(0,0,0,0.5), 0 0 24px rgba(56,189,248,0.15); }
.card[data-cat="tool"]:hover        { box-shadow: 0 12px 40px rgba(0,0,0,0.5), 0 0 24px rgba(167,139,250,0.15); }
.card[data-cat="ui"]:hover          { box-shadow: 0 12px 40px rgba(0,0,0,0.5), 0 0 24px rgba(251,146,60,0.15); }
.card[data-cat="forms"]:hover       { box-shadow: 0 12px 40px rgba(0,0,0,0.5), 0 0 24px rgba(244,114,182,0.15); }
.card[data-cat="media"]:hover       { box-shadow: 0 12px 40px rgba(0,0,0,0.5), 0 0 24px rgba(251,191,36,0.15); }
.card[data-cat="interactive"]:hover { box-shadow: 0 12px 40px rgba(0,0,0,0.5), 0 0 24px rgba(74,222,128,0.15); }
```

Also update `.card:hover` to drop the hardcoded green from `box-shadow` (the per-category rules take over).

## What's not changing

- No layout/grid changes
- No font changes
- No filter bar or header changes
- No JS logic changes (filtering still works as-is)
- No new files
