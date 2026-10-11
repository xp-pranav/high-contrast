# Customizing "High Contrast" — an owner's manual

This guide explains **every way** a customer can change the website without touching any build tooling. The site is plain, static HTML + CSS + vanilla JavaScript — you edit text files and reopen the page in a browser. No frameworks, no compilation, no database.

## The three files you'll touch

| File | What lives in it |
| --- | --- |
| `index.html` | Every word, image URL, price, menu item, hour and link on the page (the content/markup). |
| `css/styles.css` | The "design system": colors, fonts, spacing, layout and all visual styling. |
| `js/main.js` | The behaviour: hero photo gallery, menu filtering, mobile menu, scroll animations. (Also the three hero image URLs.) |

**How to edit:** open any file in a plain text editor (VS Code, Sublime, even TextEdit in plain mode) and save. **How to preview:** double-click `index.html` to open it in a browser — that's it. (For a local server with auto-reload, run `python3 -m http.server` in this folder and open the printed URL.)

> Tip: almost everything below can be found by searching the file for the exact current text (e.g. search `#FF4B1F` for the orange). Use your editor's "Find" rather than hunting by line number.

---

## 1. Quick reference — find it fast

| I want to change… | Look in | Search for |
| --- | --- | --- |
| The orange/cream/yellow palette | `css/styles.css` (top) | `--accent`, `--paper`, `--ink` |
| Fonts | `index.html` + `css/styles.css` | `fonts.googleapis`, `--font-display` |
| Business name / page title | `index.html` `<head>` and header | `HIGH CONTRAST`, `<title>` |
| The hero headline & blurb | `index.html` | `hero__title`, `hero__desc` |
| Any photo | `index.html` (and `js/main.js` for the hero) | `images.unsplash.com` |
| The menu / prices | `index.html` | `menu-item`, `$5.50` |
| Open hours | `index.html` (in **3 places**) | `15:00` |
| Phone number | `index.html` (in **2 places**) | `+61 3 9000` |
| Email | `index.html` footer | `hello@highcontrast.coffee` |
| Instagram link | `index.html` footer | `instagram.com` |
| Address & the map | `index.html` visit/footer | `42 Laneway Walk`, `output=embed` |
| The reviews / rating | `index.html` | `reviews`, `4.9 / 5` |
| Section order / nav links | `index.html` | `nav__list`, `id="` |

---

## 2. Brand basics

These define the site's name and identity.

| What | Where | Current value |
| --- | --- | --- |
| Browser-tab title | `index.html` line ~6 `<title>` | `High Contrast — Specialty Coffee Bar · Collingwood, Melbourne` |
| Search description (meta) | `index.html` line ~7 `meta[name="description"]` | a one-line summary |
| Browser chrome color | `index.html` line ~8 `meta[name="theme-color"]` | `#F3EEE4` |
| Favicon (tab icon) | `index.html` line ~9 `<link rel="icon">` | a tiny SVG data-URI (orange/dark/cream square) |
| Header wordmark | `index.html` `<span class="brand__name">` | `HIGH CONTRAST` |
| Logo mark | `index.html` `<span class="brand__mark">` (styled square) | CSS in `styles.css` (`.brand__mark`) |
| Footer wordmark | `index.html` `<p class="footer__wordmark">` | `HIGH CONTRAST` |
| Footer legal line | `index.html` `<span class="footer__bottom">` | `High Contrast Coffee Co.` |
| "Est." sticker | `index.html` hero | `Est. 2019` |
| Business number chip | `index.html` hero | `№ 4 espresso bar` |
| Founded / roast facts | `index.html` story meta + experience | `2019`, `15kg`, `Tue & Fri` |

**To change the favicon** with a real logo file: replace the `href="data:image/svg+xml,…"` on the `<link rel="icon">` line with `href="favicon.svg"` (or `.png`/`.ico`), and add that file into the project folder.

---

## 3. Colors (design tokens)

All colors are defined once at the top of `css/styles.css` as CSS variables. Editing the value here recolours the whole site instantly.

```css
:root {
  --paper:   #F3EEE4;   /* page background — warm bone   */
  --paper-2: #EAE1CF;   /* secondary background — deeper bone */
  --ink:     #16130F;   /* text & borders — espresso black */
  --muted:   #6B6257;   /* secondary/small text            */
  --card:    #FFF8EC;   /* card background — milk          */
  --accent:  #FF4B1F;   /* primary accent — crema orange   */
  --accent-2:#FFC61A;   /* secondary accent — sunflower    */
}
```

- Change `--accent` to change the buttons, highlights, star ratings, selection color and focus rings.
- Change `--paper` / `--ink` to flip the light/dark base.
- To go **dark mode**, swap `--paper` and `--ink` values and adjust `--card`/`--muted` to suit.

The chunky offset shadow thickness and border width are also tokens:

```css
--line: 2px solid var(--ink);
--shadow: 8px 8px 0 var(--ink);
--shadow-sm: 4px 4px 0 var(--ink);
```

---

## 4. Fonts

Fonts are loaded from Google Fonts in `index.html` (line ~13):

```html
<link href="https://fonts.googleapis.com/css2?family=Archivo:wght@400;500;600;700;800&family=Archivo+Black&family=Unbounded:wght@400;600;800&family=Space+Mono:wght@400;700&display=swap" rel="stylesheet" />
```

Their roles are mapped in `css/styles.css`:

```css
--font-display: 'Archivo Black';  /* big display headings */
--font-brand:   'Unbounded';       /* logo/wordmark        */
--font-body:    'Archivo';         /* paragraphs           */
--font-mono:    'Space Mono';      /* labels, prices, kickers, buttons */
```

**To switch a font:**
1. In the Google Fonts URL, replace the family name (e.g. swap `Archivo` → `Inter`) and its weights.
2. Update the matching `--font-*` variable in `styles.css`. Keep the weight numbers you actually use (Archivo is used at 400–800).

**To use an uploaded/self-hosted font** (no Google dependency): put the font files in the project, add an `@font-face` rule at the top of `styles.css`, and point the `--font-*` variables at that family name.

---

## 5. Changing text, section by section

All the written content lives in `index.html`. Each section is a `<section>` with an `id`. Below, the "find" strings are the current copy you can search to jump straight there.

### Hero (top of page)
- **Eyebrow:** `<p class="eyebrow">` — "Specialty espresso bar — Collingwood, Melbourne"
- **Headline:** `<h1 class="hero__title">` — three lines ("Coffee, / served / loud."). Each line is a `<span class="hero__line">`.
- **Sub-copy:** `<p class="hero__desc">` — "Single-origin espresso, roasted twice a week…"
- **Buttons:** `<a class="btn btn--solid">` ("See the menu") and `<a class="btn btn--ghost">` ("Find us").
- **Quick facts under headline:** `<dl class="hero__meta">` — "Open / Roast / Seats". ⚠️ "Open 7:00 — 15:00" here must stay in sync with the Visit and Footer hours (see §8).

### Scrolling marquee (`<div class="marquee">`)
Six phrases repeated twice: *Single origin ✳ Roasted twice a week ✳ Walk-ins only ✳ Light roast ✳ No syrups ✳ Hello, neighbour*. Edit each `<span>` (keep the two `<div class="marquee__group">` blocks identical so the loop is seamless).

### Story (`id="story"`)
- **Kicker:** `01 — The short version`
- **Big statement:** `<h2 class="story__statement">` (the `<mark>` highlights, `<em>` italics).
- **Body paragraph:** `<p class="story__body">`.
- **Facts ("Founded / Roasted / Milk / Roster"):** `<dl class="story__meta">` — e.g. `2019`, `In-house · 15kg`, `Oat · Almond · Soy · Full`, `Walk-ins only`.

### Standouts / featured (`id="drinks"`)
- **Heading:** `02 — The standouts` + "What we're pouring".
- **Each drink** is an `<article class="feature">` containing: `feature__index` (№ 01…), `feature__name`, `feature__type`, `feature__desc`, `feature__tags` (the `.tag` chips), and `feature__price`. There are four, plus a `.bun` card ("The cardamom bun"). To edit a price/name/description, change the text inside these elements.

### Menu (`id="menu"`)
- **Heading + note:** `03 — The full menu`, plus `menu__note` ("Milk alternatives +$0.50… Beans available by the 250g bag.").
- **Filter tabs:** `menu__tabs` (`All / Espresso / Milk / Filter / Cold / Not coffee`).
- **Items:** all inside `<ul class="menu-list" id="menu-list">` as `<li class="menu-item" data-category="…">`. See §7 for how to add/remove/pricing and the `data-category` logic.

### House rules (`id="principles"`)
- `04 — House rules`, title "Three things we won't compromise on.", and three `.principle` cards (number + title + body).

### What to expect (`id="experience"`)
- `05 — What to expect`, title "Four ways to do High Contrast.", four `.exp__tile` cards (Sit / Take / Buy / Learn), and the `.exp__roast` footer line.

### Visit (`id="visit"`)
- `06 — Come say hi`, `.visit__address`, "Get directions" / "Call the bar" buttons, the `.hours` list, and the map `<iframe>`. ⚠️ Hours live here too (see §8).

### Reviews (`id="reviews"`)
- See §9.

### Footer
- Four columns: **Find us** (address + directions), **Hours**, **Menu** (in-page links), **Say hi** (email / phone / Instagram). The `© <span id="year">` line auto-updates each January via JavaScript — no manual year edit needed.

---

## 6. Images — swapping every photo

Photos currently load from **Unsplash** (hot-linked URLs). Every `<img>` in `index.html` has an `src="https://images.unsplash.com/photo-XXXX..."`. That's all you change to swap a picture.

### The full list of current photos

| Location | Element / where | Current Unsplash ID |
| --- | --- | --- |
| Hero — slide 1 (barfront) | `js/main.js` `slides` array **and** the `<img id="hero-img">` in `index.html` | `photo-1554118811-1e0d58224f24` |
| Hero — slide 2 (short black) | `js/main.js` `slides` array | `photo-1495474472287-4d71bcdd2085` |
| Hero — slide 3 (flat white) | `js/main.js` `slides` array | `photo-1509042239860-f550ce710b93` |
| Story — the room | `index.html` `.story__img--a` | `photo-1521017432531-fbd92d768814` |
| Story — roasting beans | `index.html` `.story__img--b` | `photo-1447933601403-0c6688de566e` |
| Standout № 01 Flat Post | `index.html` `feature` 1 | `photo-1541167760496-1628856ab772` |
| Standout № 02 Filter | `index.html` `feature` 2 | `photo-1522992319-0365e5f11656` |
| Standout № 03 Long black | `index.html` `feature` 3 | `photo-1442512595331-e89e73853f31` |
| Standout № 04 Espresso tonic | `index.html` `feature` 4 | `photo-1521302200778-33500795e128` |
| Cardamom bun | `index.html` `.bun__media` | `photo-1555507036-ab1f4038808a` |

### Method A — keep using Unsplash (quickest)
1. Pick a photo on [Unsplash](https://unsplash.com).
2. Copy its share/source URL or the `images.unsplash.com/photo-…` URL.
3. Append sizing params so it's cropped correctly: `?q=80&w=1200&h=900&auto=format&fit=crop` (adjust `w`/`h` to the aspect ratio noted in the table below).
4. Replace the existing `src="…"`. Also update `alt="…"` to describe your new photo (important for accessibility/SEO).

### Method B — use your own photos (recommended long-term)
1. Create an `images/` folder next to `index.html`.
2. Drop your files in (e.g. `images/bar-front.jpg`).
3. Replace `src="https://images.unsplash.com/…"` with `src="images/bar-front.jpg"`.
4. Keep file sizes sane: **1200–1600px wide for full-width shots, ~800px for thumbnails**, JPEG/WebP, ideally under ~200 KB each so the page loads fast.
5. Update the matching `alt` text and keep `width`/`height` attributes matching your image's real dimensions (this prevents layout shift).

### ⚠️ The hero has a special "two places" rule
The first hero photo is stored in **two files** — the visible `<img id="hero-img">` in `index.html` (the initial image) **and** the `slides` array in `js/main.js` (where clicking the 1/2/3 buttons pulls from). If you change one, change the other, or the gallery buttons will snap back to the old photo when clicked.

The `slides` array in `js/main.js` looks like:

```js
const slides = [
  { src: 'https://images.unsplash.com/photo-1554118811-1e0d58224f24?…', alt: 'The High Contrast espresso barfront…' },
  { src: 'https://images.unsplash.com/photo-1495474472287-4d71bcdd2085?…', alt: 'A short black poured…' },
  { src: 'https://images.unsplash.com/photo-1509042239860-f550ce710b93?…', alt: 'A flat white…' }
];
```

**To add or remove a hero slide:** edit that array, and in `index.html` add/remove a matching gallery button inside `.hero__gallery`:

```html
<button class="gallery-btn" data-slide="2" aria-current="false" aria-label="Show photo 3 of 3">3</button>
```

(`data-slide` must match the array position, starting at `0`; update each button's `aria-label` "…of N" and number.)

### Recommended image aspect ratios (approx.)
| Image | Ratio |
| --- | --- |
| Hero slides | 4:5 (portrait) |
| Story thumbnails | 4:3 / 1:1 |
| Standout cards | 4:3 |
| Bun card | 5:4 |

The `fit=crop` param means Unsplash auto-crops to whatever `w`/`h` you set, so mismatched ratios won't break the layout — they just crop.

### Alt text (don't skip it)
Every image has an `alt="…"`. Update it whenever you swap a photo — it's read aloud by screen readers, shown if the image fails, and used by search engines.

---

## 7. The menu — items, prices, categories

All drinks live in `index.html` inside `<ul class="menu-list" id="menu-list">`. Each item is one `<li>` and looks like this:

```html
<li class="menu-item" data-category="milk">
  <div class="menu-item__main">
    <h4 class="menu-item__name">Flat White</h4>
    <p class="menu-item__desc">Double ristretto, velvet microfoam, one pour.</p>
    <ul class="menu-item__notes">
      <li class="tag">The house pour</li>
    </ul>
  </div>
  <span class="menu-item__price mono">$5.50</span>
</li>
```

- **Change a name / description / price:** edit `menu-item__name`, `menu-item__desc`, and `menu-item__price`.
- **Add a note chip:** add another `<li class="tag">…</li>` inside `menu-item__notes`.
- **Remove an item:** delete its whole `<li class="menu-item">…</li>`.
- **Add an item:** copy any `<li class="menu-item">…</li>`, paste it into the correct category block, edit the text.

### How the filter knows where each item goes
Each `<li class="menu-item">` carries a `data-category` value, and each tab button carries the matching value:

```html
<button class="menu__tab" data-category="espresso" aria-pressed="false">Espresso</button>
```

The active categories are: **`espresso` · `milk` · `filter` · `cold` · `notcoffee`**.

- **Add an item to a category:** give it the right `data-category` (e.g. `data-category="cold"`).
- **Add a new category:** add a new `<button class="menu__tab" data-category="tea">Tea</button>` in `menu__tabs`, then add items with `data-category="tea"`. Filter and live count update automatically — no JS change needed.
- **Rename a tab label:** change the text between the `<button>…</button>` tags (keep the same `data-category` value).

> ⚠️ The `data-category` must **exactly match** between the tab and its items (case-sensitive, no spaces).

### The item counter
- The live count (`<p class="menu__count" id="menu-count">20 items</p>`) is recalculated by JavaScript on every tab click — no manual update needed.
- The hard-coded `20 items` is only the no-JS fallback; update it if you change the item total.
- The empty-state message lives in `<p class="menu-empty" id="menu-empty">` (shown only if a category has zero items).

### The "Standouts" cards are separate
The four featured drinks in `id="drinks"` are **not** driven by the filter — they're hand-built `<article class="feature">` blocks. If you change a standout's name/price, update the matching item in the full menu list (and vice-versa) so they don't contradict.

---

## 8. Hours, contact & location — edit all, keep them in sync

These details are **hard-coded in more than one place**. Change them everywhere or sections will disagree.

### Opening hours — 3 places
1. **Hero** quick-fact `<dl class="hero__meta">`: the `Open` / `Roast` rows (e.g. `7:00 — 15:00`, `Tue & Fri`).
2. **Visit** `<dl class="hours">`: three rows — `Mon — Fri`, `Sat — Sun`, `Public holidays`.
3. **Footer** `Hours` column: three `<li>` lines (`Mon — Fri · 7:00 — 15:00`, etc.).

> Note: the hero currently shows only weekday hours (`7:00 — 15:00`) while Visit/Footer also list weekend (`8:00 — 16:00`) and public-holiday (`Closed`) hours. Make these consistent with the real opening times.

### Phone — 2 places
- Visit: `<a class="btn btn--ghost" href="tel:+61390000000">Call the bar</a>`
- Footer: `<a href="tel:+61390000000">+61 3 9000 0000</a>`

Update both `tel:` links **and** the visible text.

### Email — 1 place
- Footer: `<a href="mailto:hello@highcontrast.coffee">hello@highcontrast.coffee</a>` (placeholder).

### Address — 2 places (+ map)
- Visit: `<p class="visit__address">42 Laneway Walk<br>Collingwood, VIC 3066<br>Melbourne</p>`
- Footer "Find us": `42 Laneway Walk, Collingwood VIC 3066`.

### Google Map — 1 place
- `<iframe … src="https://maps.google.com/maps?q=Collingwood+VIC+Australia&z=15&output=embed" …>` in Visit. Change the `q=` part to your real address (URL-encoded, `+` for spaces) and adjust `z=` zoom (higher = closer). For your exact Google Business pin, copy Google Maps' "Embed a map" `src` and paste it here.

### "Get directions" links — 2 places
- Visit button and Footer "Get directions →" both use `https://www.google.com/maps/dir/?api=1&destination=…`. Update the `destination=` value.

### Instagram — 1 place
- Footer: `<a href="https://instagram.com" target="_blank" rel="noopener">Instagram</a>` — replace the URL (and optionally the label, e.g. `@highcontrast`).

---

## 9. Reviews section

The reviews live in `index.html` at `<section class="reviews" id="reviews">`.

- **Aggregate rating:** `<span class="reviews__stars">★★★★★</span>` + `<span class="reviews__score mono">4.9 / 5 · 200+ reviews</span>` — edit `4.9`, `/ 5`, and `200+` to your real figures.
- **Each review** is an `<article class="review">` containing:
  - `<p class="review__stars">★★★★★</p>` — for a partial score, end with a dimmed star: `★★★★<span class="review__off">★</span>` (the 4-star example does this).
  - `<blockquote class="review__quote">"…"</blockquote>` — the quote.
  - `<footer class="review__meta">` → `<span class="review__name">` and `<span class="review__order mono">` (what they ordered).
- **Add a review:** copy an `<article class="review">…</article>` block, paste before the closing `</div>` of `.reviews__grid`, edit the text. The grid reflows automatically.
- **Remove a review:** delete its `<article>…</article>`.
- **"Review us on Google →" button:** the link is currently a generic Google Maps search. Replace `href` with your real Google review URL (Google Business Profile → "Ask for reviews" → copy the short link, e.g. `g.page/[name]/review`).

> Real customer reviews convert better than placeholder copy — replace the sample names/quotes with genuine ones you've collected.

---

## 10. Navigation & reorganising sections

- **Header nav** is `<ul class="nav__list">`. Each link is `<a class="nav__link" href="#story">Story</a>` — the `href` points at a section's `id` (`#story`, `#drinks`, `#menu`, `#visit`, `#reviews`).
- **Footer menu** is the `Menu` column — same `#id` links.
- **Add a nav item:** add a link in the header nav (and optionally footer) pointing at an existing section `id`.
- **Rename a section heading:** edit its `<h2>…</h2>` and `<p class="kicker">…</p>`.
- **Reorder sections:** move a whole `<section …>…</section>` block and renumber the kicker labels (`01 —`, `02 —`, …). In-page `#` links keep working because they reference `id`s, not position.
- **Remove a section:** delete its `<section>…</section>` **and** its nav + footer link(s).
- Keep `id` values unchanged unless you also update every link that points at them.

---

## 11. Animations & interactivity (what `js/main.js` does)

`js/main.js` runs these behaviours. You usually don't need to touch it:

1. **Footer year** — auto-fills `#year`. Nothing to change.
2. **Sticky header shadow** — adds `.is-scrolled` after 8px of scrolling.
3. **Mobile menu** — opens/closes `#primary-nav` via the hamburger (`[data-nav-toggle]`).
4. **Hero gallery** — swaps the hero image from the `slides` array (see §6). Edit only when adding/removing slides.
5. **Menu filter** — reads `data-category` on tabs and items (see §7).
6. **Reveal-on-scroll** — any element with `data-reveal` fades/slides in when scrolled into view. To animate a new element, add `data-reveal`; to stop animating one, remove it. (Users with "reduce motion" enabled already see everything un-animated automatically.)

### Disabling animations
- **One element:** remove its `data-reveal` attribute.
- **Everything:** delete/comment the `/* ---------- Reveal on scroll ---------- */` block in `js/main.js` and remove `data-reveal` attributes.

### Tuning animation
- **Reveal trigger/feel:** in `js/main.js`, the observer uses `{ threshold: 0.12, rootMargin: '0px 0px -8% 0px' }`. The fade speed is in `css/styles.css` under `[data-reveal]` / `.is-visible` (`transition` duration).
- **Marquee speed:** `css/styles.css` under `.marquee__track` (`animation:`).

---

## 12. SEO, social sharing & analytics

- **Page title:** `<title>` in `index.html` (the tab text + Google result headline).
- **Meta description:** `meta[name="description"]` in `index.html` (the Google result snippet).
- **Social preview cards:** currently there are no Open Graph / Twitter tags. To show a branded card when the link is shared on Facebook/X/Slack, add these inside `<head>`:

```html
<meta property="og:title" content="High Contrast — Specialty Coffee Bar" />
<meta property="og:description" content="Single-origin espresso, roasted twice a week in Collingwood, Melbourne." />
<meta property="og:image" content="images/og-cover.jpg" />
<meta property="og:url" content="https://yourdomain.com/" />
<meta name="twitter:card" content="summary_large_image" />
```

(Use a 1200×630 image. Change `https://yourdomain.com/` to your real URL.)

- **Analytics:** not installed by default. To add Google Analytics / Plausible / Fathom, paste its `<script>` snippet just before `</head>` (for GA) or before `</body>`.
- **Favicon:** see §2.

---

## 13. Publishing / deployment

The site is static — put the three files (`index.html`, `css/`, `js/`) on any static host:

- **Netlify / Vercel / Cloudflare Pages:** drag-and-drop the folder, or connect a Git repo. Free, auto-HTTPS, custom domain.
- **GitHub Pages:** push the folder to a repo → Settings → Pages → deploy from branch.
- **Any shared web host:** upload via FTP/SFTP — the `index.html` at the web root is what loads.

If you add an `images/` folder, upload that too. Relative paths in the code (`css/styles.css`, `js/main.js`, `images/…`) work as-is on all of the above.

---

## 14. Step-by-step recipes

### A. Change the opening hours
1. In `index.html`, find `15:00` (or the `.hours` block).
2. Update the three hour rows in the **Visit** `<dl class="hours">`.
3. Update the **Footer** `Hours` column.
4. Update the **Hero** `Open`/`Roast` quick-facts so they match.

### B. Add a new drink to the menu
1. In `index.html`, find an existing `<li class="menu-item" data-category="milk">…</li>`.
2. Copy it, paste a new one, change the name/description/price/tag, and set the right `data-category`.
3. (Optional) If it's a new category, also add a matching `menu__tab` button.
4. (Optional) Update the fallback `20 items` count.

### C. Replace the hero photo
1. In `index.html`, replace the `src` on `<img id="hero-img">` with the new URL/path and update `alt`.
2. In `js/main.js`, update the first entry in the `slides` array with the same `src` and `alt`.

### D. Change the brand accent colour
1. In `css/styles.css`, change `--accent: #FF4B1F;` to your hex (e.g. a brand green `#1F7A4D`).
2. That's it — buttons, highlights, stars, focus rings and selection all update.

### E. Update the phone number
1. In `index.html`, find `tel:+61390000000` (two places).
2. Change both `href="tel:…"` values and the visible label text.

---

## 15. Pre-launch checklist

- [ ] Page `<title>` and meta description reflect the real business.
- [ ] Opening hours match in **Hero + Visit + Footer** (3 places).
- [ ] Phone (2 places) and email are real.
- [ ] Address, map `iframe` `q=`, and both "Get directions" links point at the real location.
- [ ] Instagram link points at the real account.
- [ ] Prices and menu items are current (incl. the Standouts cards + full menu agreeing).
- [ ] All placeholder/review copy replaced with real words; **Review us on Google** uses the real review URL.
- [ ] All photos are correct and every `alt` text updated; hero image matched in `index.html` + `js/main.js`.
- [ ] Brand colours/fonts verified (if changed).
- [ ] Opened in a browser: check on mobile width, tablet, and desktop — no horizontal scroll, nav works, menu tabs filter, hero gallery buttons work, reviews reveal on scroll.
- [ ] No broken images (right-click → "Open image in new tab" to verify each one).

---

### Need something not covered here?
Ask for a specific edit — e.g. "add a gallery section", "make the header transparent", "add a booking button" — and it can be done in the same three files.
