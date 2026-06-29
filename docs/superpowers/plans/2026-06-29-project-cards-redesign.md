# Project Cards Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Enhance project cards with emoji icons, a large ghost number, category-colored top borders, and category-tinted hover glows.

**Architecture:** Pure HTML + CSS changes. JS data array gets an `icon` field per project; the card template renders two new elements (`card-icon`, `card-ghost`); style.css gets new rules for icons, ghost numbers, top borders, and per-category hover glows.

**Tech Stack:** Vanilla HTML, CSS, JS. No build step, no dependencies.

## Global Constraints

- No new files
- No new dependencies
- No changes to grid, header, filter bar, or footer
- No JS logic changes (filtering must still work)
- Existing category color values must be used verbatim (see spec)

---

### Task 1: Add icon data and update card template in `index.html`

**Files:**
- Modify: `index.html` (projects array + card HTML template)

**Interfaces:**
- Produces: each card `<a>` contains `.card-icon` (emoji span) and `.card-ghost` (decorative number span), used by Task 2's CSS

- [ ] **Step 1: Add `icon` field to every project object**

In `index.html`, update the `projects` array so every object has an `icon` property. Replace the existing array with:

```js
const projects = [
  { n: 1,  name: 'Weather App',          slug: '1. Weather App',                    cat: 'api',         icon: '🌤️'  },
  { n: 2,  name: 'To-do List',           slug: '2. To-do List',                     cat: 'interactive', icon: '✅'  },
  { n: 3,  name: 'Quiz App',             slug: '3. Quiz App',                       cat: 'interactive', icon: '🧠'  },
  { n: 4,  name: 'Password Generator',   slug: '4. Random Password Generator',      cat: 'tool',        icon: '🔐'  },
  { n: 5,  name: 'Notes App',            slug: '5. Notes App',                      cat: 'tool',        icon: '📝'  },
  { n: 6,  name: 'Age Calculator',       slug: '6. Age Calculator',                 cat: 'tool',        icon: '🎂'  },
  { n: 7,  name: 'Quote Generator',      slug: '7. Quote Generator',                cat: 'api',         icon: '💬'  },
  { n: 8,  name: 'QR Code Generator',    slug: '8. QR Code Generator',              cat: 'tool',        icon: '📱'  },
  { n: 9,  name: 'Toast Notification',   slug: '9. Toast Notification for Website', cat: 'ui',          icon: '🔔'  },
  { n: 10, name: 'Music Player',         slug: '10. Music Player',                  cat: 'media',       icon: '🎵'  },
  { n: 11, name: 'Stopwatch',            slug: '11. Stopwatch',                     cat: 'tool',        icon: '⏱️'  },
  { n: 12, name: 'Calculator',           slug: '12. Calculator',                    cat: 'tool',        icon: '🧮'  },
  { n: 13, name: 'Popup',               slug: '13. Popup',                         cat: 'ui',          icon: '💭'  },
  { n: 14, name: 'Password Toggle',      slug: '14. Password Toggle',               cat: 'ui',          icon: '👁️'  },
  { n: 15, name: 'Dark Mode',            slug: '15. Dark Mode',                     cat: 'ui',          icon: '🌙'  },
  { n: 16, name: 'Form Validation',      slug: '16. Form Validation',               cat: 'forms',       icon: '📋'  },
  { n: 17, name: 'Image Gallery',        slug: '17. Image Gallery',                 cat: 'media',       icon: '🖼️'  },
  { n: 18, name: 'Email Subscription',   slug: '18. Email Subscription Form',       cat: 'forms',       icon: '📧'  },
  { n: 19, name: 'Password Strength',    slug: '19. Password Strength Indicator',   cat: 'forms',       icon: '🔒'  },
  { n: 20, name: 'Text to Speech',       slug: '20. Text-to-speech Converter',      cat: 'media',       icon: '🔊'  },
  { n: 21, name: 'Coming Soon Page',     slug: '21. Website Coming Soon',           cat: 'ui',          icon: '🚀'  },
  { n: 22, name: 'Image Transition',     slug: '22. Image Transition',              cat: 'media',       icon: '🎞️'  },
  { n: 23, name: 'Mini Calendar',        slug: '23. Mini Calendar',                 cat: 'tool',        icon: '📅'  },
  { n: 24, name: 'Select Menu',          slug: '24. Select Menu Design',            cat: 'ui',          icon: '🔽'  },
  { n: 25, name: 'Circular Progress Bar',slug: '25. Circular Progress Bar',         cat: 'ui',          icon: '⭕'  },
  { n: 26, name: 'Product Page',         slug: '26. Product Page Design',           cat: 'ui',          icon: '🛍️'  },
  { n: 27, name: 'Crypto Website',       slug: '27. Cryptocurrency Website',        cat: 'api',         icon: '₿'   },
  { n: 28, name: 'Digital Clock',        slug: '28. Digital Clock',                 cat: 'tool',        icon: '🕐'  },
  { n: 29, name: 'Drag and Drop',        slug: '29. Drag and Drop',                 cat: 'interactive', icon: '🧩'  },
  { n: 30, name: 'Image Search Engine',  slug: '30. Image Search Engine',           cat: 'api',         icon: '🔍'  },
];
```

- [ ] **Step 2: Update the card HTML template**

Find the `grid.innerHTML = projects.map(...)` block and replace the template string with:

```js
grid.innerHTML = projects.map((p, i) => `
  <a href="${encodeURIComponent(p.slug)}/index.html"
     class="card"
     data-cat="${p.cat}"
     style="--i:${i}"
     aria-label="${p.name}">
    <div class="card-top">
      <span class="card-number">${String(p.n).padStart(2, '0')}</span>
      <span class="card-cat cat-${p.cat}">${catLabels[p.cat]}</span>
    </div>
    <span class="card-icon" aria-hidden="true">${p.icon}</span>
    <div class="card-bottom">
      <span class="card-name">${p.name}</span>
      <span class="card-arrow" aria-hidden="true">&#8599;</span>
    </div>
    <span class="card-ghost" aria-hidden="true">${String(p.n).padStart(2, '0')}</span>
  </a>
`).join('');
```

- [ ] **Step 3: Visual check**

Open `index.html` in a browser (e.g. `open index.html` or via Live Server). Verify:
- Each card shows its emoji icon between the top row and the project name
- A large faint number is visible in the bottom-right of each card
- Filtering still works (click API, Tool, etc.)

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add icon and ghost number elements to project cards"
```

---

### Task 2: Add CSS for icons, ghost number, top borders, and tinted hover glows

**Files:**
- Modify: `style.css`

**Interfaces:**
- Consumes: `.card-icon` and `.card-ghost` spans produced by Task 1
- Consumes: `data-cat` attribute on `.card` (already exists)

- [ ] **Step 1: Increase card min-height to accommodate icon**

Find `.card` in `style.css`. Change `min-height: 148px` to `min-height: 172px`.

Also update the responsive rule at `@media (max-width: 640px)`:
Change `.card { min-height: 124px; ... }` to `min-height: 148px`.

- [ ] **Step 2: Add `.card-icon` and `.card-ghost` styles**

Add these rules after the `.card:hover .card-arrow` block:

```css
/* icon */
.card-icon {
  font-size: 28px;
  line-height: 1;
  display: block;
  margin: 8px 0;
  position: relative;
  z-index: 1;
}

/* ghost number */
.card-ghost {
  position: absolute;
  bottom: -8px;
  right: 12px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 90px;
  font-weight: 700;
  color: #fff;
  opacity: 0.04;
  line-height: 1;
  pointer-events: none;
  user-select: none;
  z-index: 0;
  transition: opacity 0.25s ease;
}

.card:hover .card-ghost {
  opacity: 0.08;
}
```

- [ ] **Step 3: Add category-colored top borders**

Add after the ghost number rules:

```css
/* category top borders */
.card[data-cat="api"]         { border-top: 2px solid rgba(56, 189, 248, 0.5); }
.card[data-cat="tool"]        { border-top: 2px solid rgba(167, 139, 250, 0.5); }
.card[data-cat="ui"]          { border-top: 2px solid rgba(251, 146, 60, 0.5); }
.card[data-cat="forms"]       { border-top: 2px solid rgba(244, 114, 182, 0.5); }
.card[data-cat="media"]       { border-top: 2px solid rgba(251, 191, 36, 0.5); }
.card[data-cat="interactive"] { border-top: 2px solid rgba(74, 222, 128, 0.5); }
```

- [ ] **Step 4: Update base `.card:hover` and add category-tinted hover glows**

Find the existing `.card:hover` rule:

```css
.card:hover {
  transform: translateY(-2px);
  border-color: var(--border-hover);
  background: var(--card-hover);
  box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5), 0 0 0 1px rgba(74, 222, 128, 0.08);
}
```

Remove the `box-shadow` line from it (the per-category rules below will own it). Result:

```css
.card:hover {
  transform: translateY(-2px);
  border-color: var(--border-hover);
  background: var(--card-hover);
}
```

Then add after the top border rules:

```css
/* category-tinted hover glows */
.card[data-cat="api"]:hover         { box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5), 0 0 24px rgba(56, 189, 248, 0.18); }
.card[data-cat="tool"]:hover        { box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5), 0 0 24px rgba(167, 139, 250, 0.18); }
.card[data-cat="ui"]:hover          { box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5), 0 0 24px rgba(251, 146, 60, 0.18); }
.card[data-cat="forms"]:hover       { box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5), 0 0 24px rgba(244, 114, 182, 0.18); }
.card[data-cat="media"]:hover       { box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5), 0 0 24px rgba(251, 191, 36, 0.18); }
.card[data-cat="interactive"]:hover { box-shadow: 0 12px 40px rgba(0, 0, 0, 0.5), 0 0 24px rgba(74, 222, 128, 0.18); }
```

- [ ] **Step 5: Visual check**

Reload `index.html` in the browser. Verify:
- Each card has a colored top border matching its category
- Hovering a card shows a glow that matches the category color (blue for API, purple for Tool, orange for UI, pink for Forms, yellow for Media, green for Interactive)
- Ghost number is subtly visible and brightens on hover
- Emoji icons are clearly visible on every card
- Filtering still works correctly
- No layout breakage at mobile sizes (resize browser to 640px wide)

- [ ] **Step 6: Commit**

```bash
git add style.css
git commit -m "feat: add icon, ghost number, category borders and tinted hover glows to cards"
```
