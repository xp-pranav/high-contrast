# High Contrast — Specialty Coffee Bar

A single-page, retail-only website for **High Contrast**, a corner espresso bar in Collingwood, Melbourne. Neo-brutalist / editorial visual language ("designed loud, brewed louder") built as a fast, dependency-free static site.

> All copy, prices, hours and photos are **sample placeholders** — see [`customize.md`](./customize.md) for a full guide on swapping them for real content.

## Features

- **Hero** with a clickable 3-slide photo gallery, "Est." sticker and quick-facts strip
- **Scrolling marquee** of brand phrases (CSS animation, seamless loop)
- **Story / Standouts / House rules / What to expect** editorial sections
- **Full menu** with working category filters (`All · Espresso · Milk · Filter · Cold · Not coffee`), a live item counter and an empty-state message
- **Visit** section with opening hours, phone/directions buttons and an embedded Google Map
- **Reviews** section with aggregate rating, review cards and a "Review us on Google" CTA
- **Sticky header** with shadow-on-scroll, mobile hamburger nav with Escape-to-close
- **Scroll-reveal** animations that respect `prefers-reduced-motion`
- Fully responsive layout with no horizontal overflow

## Tech stack

- **HTML5** — semantic landmarks (`header`, `nav`, `main`, `section`, `article`, `footer`)
- **CSS3** — custom properties (design tokens), CSS grid + flexbox, keyframe animations
- **Vanilla JavaScript (ES6)** — no libraries, no framework, no build step
- **Google Fonts** — Archivo, Archivo Black, Unbounded, Space Mono
- **Unsplash** — hot-linked placeholder photography

## File structure

```
.
├── index.html      # all content & markup (the page)
├── css/
│   └── styles.css  # design tokens, layout, components, animations, responsive rules
├── js/
│   └── main.js     # hero gallery, menu filter, mobile nav, header, reveal-on-scroll
├── customize.md    # owner's guide to editing every part of the site
└── README.md       # this file
```

## Getting started

There's nothing to install or build. To view the site:

```sh
# option 1 — just open the file
open index.html        # macOS
# or double-click index.html on Windows/Linux

# option 2 — serve it locally (recommended, lets you reload with no caching)
python3 -m http.server
# then open http://localhost:8000
```

Editing is done directly in a text editor — save and refresh the browser. No compilation step exists.

## How it works

### Design tokens (`css/styles.css`)
All colors, fonts, spacing and shadows are defined once at the top in `:root`:

```css
--paper / --paper-2 / --ink / --muted / --card / --accent / --accent-2
--font-display / --font-brand / --font-body / --font-mono
--gutter / --max / --header-h
--line / --shadow / --shadow-sm / --shadow-xs
```

Changing a token recolours or re-spaces the entire site instantly.

### Interactions (`js/main.js`)
1. **Footer year** — auto-fills `#year` with the current year
2. **Sticky header** — toggles `.is-scrolled` after 8px of scroll
3. **Mobile nav** — opens/closes `#primary-nav`, locks body scroll, closes on link click or Escape
4. **Hero gallery** — swaps the hero `<img>` from a `slides` array (synced with the 1/2/3 buttons)
5. **Menu filter** — reads `data-category` on tabs and menu items, updates the live count
6. **Reveal-on-scroll** — adds `.is-visible` to `[data-reveal]` elements via `IntersectionObserver`

### Menu filtering model
Filter tabs and menu items are paired through matching `data-category` values, so categories can be added or renamed in `index.html` alone — no JavaScript changes required.

## Customizing

The full owner's manual — images, colors, fonts, text, menu, pricing, hours/contact, reviews, navigation, animations, SEO and deployment — is in **[`customize.md`](./customize.md)**.

## Accessibility & performance

- Skip-to-content link, semantic landmarks and a labelled mobile nav toggle
- `aria-pressed` filter tabs with arrow-key support, `aria-live` item counter, `aria-current` gallery buttons
- Descriptive `alt` text on all images
- `prefers-reduced-motion` fully respected (animations auto-disable)
- Lazy-loaded below-the-fold images with `width`/`height` attributes to avoid layout shift
- Solid `.js`-independent fallbacks (menu items still list correctly without JS)

## Deployment

The site is static — deploy the folder as-is to any static host (Netlify, Vercel, Cloudflare Pages, GitHub Pages, or plain FTP). Relative paths (`css/styles.css`, `js/main.js`) work unchanged.

## License

Not specified. Content and photography in this repository are sample placeholders.
