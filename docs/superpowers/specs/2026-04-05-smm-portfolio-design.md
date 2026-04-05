# SMM Portfolio — Єлизавета & Власта

## Overview

Portfolio website for two SMM specialists — Єлизавета and Власта. The site showcases their team, services, project cases, results, content formats, equipment, and provides a CTA to contact via Instagram Direct. Ukrainian language only.

## Tech Stack

- **Astro** — static site generator, zero JS by default, SSG
- **Tailwind CSS** — utility-first styling
- **GSAP** — scroll-triggered animations, parallax, counters, stagger effects
- **Deployment** — Vercel or Netlify (static)

## Design Direction

### Color Palette — Dark Violet

- Background: `#0a0a14` (deep dark), `#12121f` (card dark), `#1a1a2e` (section dark)
- Primary: `#7C3AED` (violet), `#4C1D95` (deep violet)
- Accent: `#C084FC` (light violet), `#E9D5FF` (lavender)
- Text: `#ffffff` (headings), `#e0e0e0` (body), `#888888` (muted)
- Gradients: `linear-gradient(135deg, #1a1a2e, #2D1B69, #4C1D95)`

### Layout — Hybrid Scroll + Horizontal Showcases

Vertical scroll for main content with horizontal carousels for cases and equipment sections. GSAP animations throughout. Not snap-scroll — smooth natural scrolling with scroll-triggered reveals.

### Responsive Breakpoints

- Desktop: >= 1024px
- Tablet: 768px - 1023px
- Mobile: < 768px

All grids collapse appropriately. Horizontal carousels become swipeable on touch devices.

## Team

- **Єлизавета** — контент-стратег, копірайтер, сценарист (photo: `639502413_18559706272003876_4003531176310318002_n.jpg`)
- **Власта** — дизайнер, сторісмейкер, візуальний стратег (photo: `621575140_18501353014076769_8075981825132043635_n.jpg`)

Brand name: "Єлизавета & Власта" (no studio name, just their names).

## Sections (8 total)

### 1. Hero

Full-width opening screen with animated purple gradient background and floating particles (GSAP). Shows:

- Both specialists' photos (circular, with violet glow border)
- Heading: "Єлизавета & Власта"
- Subheading: "SMM, що масштабує ваш бізнес"
- CTA button: "Написати в Instagram" → links to Instagram Direct
- GSAP: parallax layers, floating particles, fade-in on load

### 2. About — Хто ми та що пропонуємо?

Two person cards side by side (stacked on mobile):

- Each card: photo, name, role description
- Brief text about their approach: "Ми не просто знімаємо відео — ми впроваджуємо чітку систему, яка перетворює глядачів на клієнтів"
- GSAP: scroll-triggered fade + slide-in, hover glow on photos

### 3. Services — Повний цикл проекту під ключ

Process flow visualization:

- Flow: Аналіз ЦА → Стратегія → Контент → Воронка продажів
- GSAP: animated arrows connecting steps, sequential reveal
- Service cards grid (4 columns → 2 → 1):
  - Контент-стратегія
  - Фото/Відеозйомка
  - Дизайн та верстка
  - Ведення акаунтів
- GSAP: stagger card entrance on scroll

### 4. Cases — Наш досвід, кейси та експертиза

**Horizontal carousel** (the key differentiator from classic layout). Each case is a large clickable card that links to the project's Instagram page.

Projects:

| Project | Instagram | Followers |
|---------|-----------|-----------|
| Coffee Place №1 | https://www.instagram.com/coffeeplace___1/ | 2,873 |
| Ресторан Затишний дім | https://www.instagram.com/zatyshnyjdim.official/ | 2,076 |
| Бюстьє | https://www.instagram.com/biustie/ | 42,100 |
| Food Place №1 | https://www.instagram.com/foodplace___1/ | 1,480 |

Each card shows:

- Project logo/screenshot (from provided Instagram screenshots)
- Project name
- Follower count
- Instagram link icon (↗)
- Opens in new tab on click

GSAP: stagger reveal on scroll-into-view, hover scale effect.
Touch: swipeable carousel on mobile/tablet.
Desktop: scroll arrows or drag.

### 5. Results — Віральний контент — результати

Animated counter statistics (placeholder values to be replaced later):

- `218K+` — переглядів рілсів
- `93.4K` — охоплення
- `500+` — нових підписників

GSAP: countUp animation triggered on scroll (numbers animate from 0 to target value). Cards with subtle violet glow.

### 6. Formats — Формати та типи контенту

Three format cards:

- **Backstage & Product** — живий контент про продукт та команду
- **Atmospheric** — естетика, яка продає
- **Воронки** — створюємо шлях клієнта від перегляду reels до покупки в direct

GSAP: fade-in cards on scroll.

### 7. Equipment — Наша система та обладнання

**Horizontal carousel** of equipment items:

- Камера Canon
- Мікрофони DJI Mic Mini
- iPhone 15 Pro Max
- iPhone 14 Pro
- Figma
- CapCut

Subtext: "ми переносимо нашу відпрацьовану систему на вашу нішу, щоб отримати прогнозований результат, а не випадкові перегляди"

GSAP: float-in from different directions, subtle hover glow.

### 8. CTA — Зв'язатися з нами

Final conversion section:

- Heading: "Готові масштабувати ваш бізнес?"
- Large CTA button: "Написати в Instagram Direct"
- Button links to Instagram Direct
- Gradient background (deep violet)
- GSAP: pulsing glow on button, gradient animation

## Animations Summary

All animations are GSAP-powered and scroll-triggered:

| Section | Animation |
|---------|-----------|
| Hero | Parallax layers, floating particles, fade-in on load |
| About | Scroll-triggered fade + slide, hover glow on photos |
| Services | Animated flow arrows, stagger card entrance |
| Cases | Stagger reveal, hover scale, horizontal carousel |
| Results | CountUp numbers from 0, triggered on scroll |
| Formats | Fade-in cards |
| Equipment | Float-in from sides, horizontal carousel |
| CTA | Pulsing glow button, gradient animation |

## Assets

Photos to be placed in `public/images/`:

- Team photos: Єлизавета and Власта (from provided images)
- Project screenshots: Coffee Place, Затишний дім, Бюстьє, Food Place (from provided Instagram screenshots)

## Project Structure (Astro)

```
portfolio/
├── public/
│   └── images/           # team photos, project screenshots
├── src/
│   ├── layouts/
│   │   └── Layout.astro  # base layout with dark theme
│   ├── components/
│   │   ├── Hero.astro
│   │   ├── About.astro
│   │   ├── Services.astro
│   │   ├── Cases.astro           # horizontal carousel
│   │   ├── Results.astro
│   │   ├── Formats.astro
│   │   ├── Equipment.astro       # horizontal carousel
│   │   └── CTA.astro
│   ├── styles/
│   │   └── global.css    # Tailwind + custom styles
│   └── pages/
│       └── index.astro   # single page, all sections
├── astro.config.mjs
├── tailwind.config.mjs
└── package.json
```

## Out of Scope

- English language version
- Contact form
- Blog/news section
- Telegram/other messengers integration
- CMS or admin panel
- Analytics (can be added later via Vercel Analytics)
