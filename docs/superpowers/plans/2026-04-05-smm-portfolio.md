# SMM Portfolio Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a Dark Violet portfolio website for SMM specialists Єлизавета & Власта with GSAP animations and horizontal carousels.

**Architecture:** Single-page Astro static site. 8 section components assembled in `index.astro`. GSAP ScrollTrigger for all animations. CSS scroll-snap for horizontal carousels (Cases, Equipment). Tailwind 4 via Vite plugin for styling.

**Tech Stack:** Astro 5, Tailwind CSS 4 (`@tailwindcss/vite`), GSAP 3 (ScrollTrigger), TypeScript

---

## File Structure

```
portfolio/
├── public/
│   └── images/
│       ├── team/
│       │   ├── yelyzaveta.jpg        # team photo
│       │   └── vlasta.jpg            # team photo
│       └── projects/
│           ├── coffee-place.jpg      # Instagram screenshot
│           ├── zatyshnyi-dim.jpg     # Instagram screenshot
│           ├── biustie.jpg           # Instagram screenshot
│           └── food-place.jpg        # Instagram screenshot
├── src/
│   ├── layouts/
│   │   └── Layout.astro              # base HTML shell, meta, fonts, global CSS import
│   ├── components/
│   │   ├── Hero.astro                # hero section with particles
│   │   ├── About.astro               # team cards
│   │   ├── Services.astro            # flow + service cards
│   │   ├── Cases.astro               # horizontal carousel
│   │   ├── Results.astro             # animated counters
│   │   ├── Formats.astro             # format cards
│   │   ├── Equipment.astro           # horizontal carousel
│   │   └── CTA.astro                 # final CTA
│   ├── styles/
│   │   └── global.css                # Tailwind import + custom CSS vars + animations
│   └── pages/
│       └── index.astro               # assembles all 8 sections
├── astro.config.mjs
├── tsconfig.json
└── package.json
```

---

### Task 1: Scaffold Astro Project and Install Dependencies

**Files:**
- Create: `package.json`, `astro.config.mjs`, `tsconfig.json`

- [ ] **Step 1: Create Astro project**

```bash
cd /Users/danylo/Arena/portfolio
npm create astro@latest -- . --template minimal --no-install --typescript strict
```

Select defaults if prompted. The `--template minimal` gives a bare project.

- [ ] **Step 2: Install dependencies**

```bash
cd /Users/danylo/Arena/portfolio
npm install
npx astro add tailwind -y
npm install gsap
```

`npx astro add tailwind` installs `@tailwindcss/vite` and configures `astro.config.mjs` automatically.

- [ ] **Step 3: Verify astro.config.mjs has Tailwind plugin**

Read `astro.config.mjs` and verify it contains the `@tailwindcss/vite` plugin. If `astro add tailwind` didn't configure it, set it to:

```javascript
// @ts-check
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  vite: {
    plugins: [tailwindcss()],
  },
});
```

- [ ] **Step 4: Verify project builds**

```bash
cd /Users/danylo/Arena/portfolio
npm run build
```

Expected: Build succeeds with no errors.

- [ ] **Step 5: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add -A
git commit -m "feat: scaffold Astro project with Tailwind 4 and GSAP"
```

---

### Task 2: Copy Assets and Set Up Images

**Files:**
- Create: `public/images/team/yelyzaveta.jpg`, `public/images/team/vlasta.jpg`
- Create: `public/images/projects/coffee-place.jpg`, `public/images/projects/zatyshnyi-dim.jpg`, `public/images/projects/biustie.jpg`, `public/images/projects/food-place.jpg`

- [ ] **Step 1: Create image directories and copy photos**

```bash
cd /Users/danylo/Arena/portfolio
mkdir -p public/images/team public/images/projects

# Team photos
cp /Users/danylo/Downloads/639502413_18559706272003876_4003531176310318002_n.jpg public/images/team/yelyzaveta.jpg
cp /Users/danylo/Downloads/621575140_18501353014076769_8075981825132043635_n.jpg public/images/team/vlasta.jpg

# Project screenshots
cp /Users/danylo/Downloads/photo_5370893871539230833_y.jpg public/images/projects/coffee-place.jpg
cp /Users/danylo/Downloads/photo_5370893871539230832_y.jpg public/images/projects/zatyshnyi-dim.jpg
cp /Users/danylo/Downloads/photo_5370893871539230831_y.jpg public/images/projects/biustie.jpg
cp /Users/danylo/Downloads/photo_5370893871539230830_y.jpg public/images/projects/food-place.jpg
```

- [ ] **Step 2: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add public/images/
git commit -m "feat: add team and project images"
```

---

### Task 3: Global Styles, Tailwind Theme, and Base Layout

**Files:**
- Create: `src/styles/global.css`
- Create: `src/layouts/Layout.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create global.css with Tailwind 4 theme**

Create `src/styles/global.css`:

```css
@import "tailwindcss";

@theme {
  /* Background colors */
  --color-bg-deep: #0a0a14;
  --color-bg-card: #12121f;
  --color-bg-section: #1a1a2e;

  /* Primary violet */
  --color-violet-900: #4C1D95;
  --color-violet-700: #6D28D9;
  --color-violet-600: #7C3AED;
  --color-violet-400: #A78BFA;
  --color-violet-300: #C084FC;
  --color-violet-200: #DDD6FE;
  --color-violet-100: #E9D5FF;
  --color-violet-50: #FAF5FF;

  /* Deep violet for gradients */
  --color-deep-violet: #2D1B69;

  /* Text */
  --color-text-primary: #ffffff;
  --color-text-body: #e0e0e0;
  --color-text-muted: #888888;

  /* Fonts */
  --font-sans: 'Inter', system-ui, -apple-system, sans-serif;

  /* Breakpoints */
  --breakpoint-sm: 640px;
  --breakpoint-md: 768px;
  --breakpoint-lg: 1024px;
  --breakpoint-xl: 1280px;
}

/* Base styles */
html {
  scroll-behavior: smooth;
}

body {
  background-color: var(--color-bg-deep);
  color: var(--color-text-body);
  font-family: var(--font-sans);
  overflow-x: hidden;
}

/* Scrollbar styling */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}
::-webkit-scrollbar-track {
  background: var(--color-bg-deep);
}
::-webkit-scrollbar-thumb {
  background: var(--color-violet-900);
  border-radius: 4px;
}
::-webkit-scrollbar-thumb:hover {
  background: var(--color-violet-600);
}

/* Horizontal carousel scrollbar hide */
.carousel-track::-webkit-scrollbar {
  display: none;
}
.carousel-track {
  -ms-overflow-style: none;
  scrollbar-width: none;
}
```

- [ ] **Step 2: Create Layout.astro**

Create `src/layouts/Layout.astro`:

```astro
---
interface Props {
  title: string;
  description: string;
}

const { title, description } = Astro.props;
---

<!doctype html>
<html lang="uk">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content={description} />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <link rel="preconnect" href="https://fonts.googleapis.com" />
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
    <link
      href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap"
      rel="stylesheet"
    />
    <title>{title}</title>
  </head>
  <body>
    <slot />
  </body>
</html>
```

- [ ] **Step 3: Update index.astro to use Layout**

Replace `src/pages/index.astro` with:

```astro
---
import Layout from '../layouts/Layout.astro';
import '../styles/global.css';
---

<Layout title="Єлизавета & Власта — SMM" description="SMM-спеціалісти, що масштабують ваш бізнес">
  <main>
    <p class="text-text-primary text-center py-20 text-2xl">Portfolio coming soon...</p>
  </main>
</Layout>
```

- [ ] **Step 4: Verify dev server runs and shows styled page**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Opens at `localhost:4321`, dark background, white text visible.

- [ ] **Step 5: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/ 
git commit -m "feat: add Layout, global styles, and Tailwind 4 theme"
```

---

### Task 4: Hero Section

**Files:**
- Create: `src/components/Hero.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Hero.astro**

Create `src/components/Hero.astro`:

```astro
---
---

<section id="hero" class="relative min-h-screen flex items-center justify-center overflow-hidden">
  <!-- Gradient background -->
  <div class="absolute inset-0 bg-gradient-to-br from-bg-deep via-deep-violet to-violet-900"></div>

  <!-- Animated particles canvas -->
  <canvas id="hero-particles" class="absolute inset-0 w-full h-full pointer-events-none"></canvas>

  <!-- Content -->
  <div class="relative z-10 text-center px-4">
    <!-- Team photos -->
    <div class="flex justify-center gap-6 mb-8">
      <div class="hero-avatar w-28 h-28 md:w-36 md:h-36 rounded-full overflow-hidden border-3 border-violet-300 shadow-[0_0_30px_rgba(124,58,237,0.4)]">
        <img src="/images/team/yelyzaveta.jpg" alt="Єлизавета" class="w-full h-full object-cover" />
      </div>
      <div class="hero-avatar w-28 h-28 md:w-36 md:h-36 rounded-full overflow-hidden border-3 border-violet-300 shadow-[0_0_30px_rgba(124,58,237,0.4)]">
        <img src="/images/team/vlasta.jpg" alt="Власта" class="w-full h-full object-cover" />
      </div>
    </div>

    <!-- Heading -->
    <h1 class="hero-title text-4xl md:text-6xl lg:text-7xl font-bold text-text-primary mb-4">
      Єлизавета <span class="text-violet-300">&</span> Власта
    </h1>

    <!-- Subheading -->
    <p class="hero-subtitle text-lg md:text-xl text-violet-300 mb-10 max-w-xl mx-auto">
      SMM, що масштабує ваш бізнес
    </p>

    <!-- CTA button -->
    <a
      href="https://www.instagram.com/"
      target="_blank"
      rel="noopener noreferrer"
      class="hero-cta inline-flex items-center gap-2 bg-violet-600 hover:bg-violet-700 text-white font-semibold py-4 px-10 rounded-full text-lg transition-all duration-300 shadow-[0_0_40px_rgba(124,58,237,0.5)] hover:shadow-[0_0_60px_rgba(124,58,237,0.7)]"
    >
      Написати в Instagram
      <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
    </a>
  </div>

  <!-- Scroll indicator -->
  <div class="absolute bottom-8 left-1/2 -translate-x-1/2 text-violet-400 animate-bounce">
    <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 14l-7 7m0 0l-7-7m7 7V3" />
    </svg>
  </div>
</section>

<script>
  import gsap from 'gsap';

  // Particle animation
  const canvas = document.getElementById('hero-particles') as HTMLCanvasElement;
  if (canvas) {
    const ctx = canvas.getContext('2d')!;
    let particles: { x: number; y: number; vx: number; vy: number; size: number; opacity: number }[] = [];

    function resize() {
      canvas.width = window.innerWidth;
      canvas.height = window.innerHeight;
    }
    resize();
    window.addEventListener('resize', resize);

    for (let i = 0; i < 60; i++) {
      particles.push({
        x: Math.random() * canvas.width,
        y: Math.random() * canvas.height,
        vx: (Math.random() - 0.5) * 0.5,
        vy: (Math.random() - 0.5) * 0.5,
        size: Math.random() * 3 + 1,
        opacity: Math.random() * 0.5 + 0.1,
      });
    }

    function animate() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (const p of particles) {
        p.x += p.vx;
        p.y += p.vy;
        if (p.x < 0 || p.x > canvas.width) p.vx *= -1;
        if (p.y < 0 || p.y > canvas.height) p.vy *= -1;
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
        ctx.fillStyle = `rgba(192, 132, 252, ${p.opacity})`;
        ctx.fill();
      }
      requestAnimationFrame(animate);
    }
    animate();
  }

  // GSAP entrance animations
  gsap.from('.hero-avatar', {
    scale: 0,
    opacity: 0,
    duration: 0.8,
    stagger: 0.2,
    ease: 'back.out(1.7)',
    delay: 0.3,
  });

  gsap.from('.hero-title', {
    y: 40,
    opacity: 0,
    duration: 0.8,
    delay: 0.7,
  });

  gsap.from('.hero-subtitle', {
    y: 30,
    opacity: 0,
    duration: 0.8,
    delay: 0.9,
  });

  gsap.from('.hero-cta', {
    y: 20,
    opacity: 0,
    duration: 0.8,
    delay: 1.1,
  });
</script>
```

- [ ] **Step 2: Add Hero to index.astro**

Replace `src/pages/index.astro`:

```astro
---
import Layout from '../layouts/Layout.astro';
import Hero from '../components/Hero.astro';
import '../styles/global.css';
---

<Layout title="Єлизавета & Власта — SMM" description="SMM-спеціалісти, що масштабують ваш бізнес">
  <main>
    <Hero />
  </main>
</Layout>
```

- [ ] **Step 3: Verify Hero renders in dev server**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Full-screen hero with gradient background, floating particles, two circular photos, heading, subheading, Instagram CTA button, scroll indicator. GSAP animations play on load.

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/Hero.astro src/pages/index.astro
git commit -m "feat: add Hero section with particles and GSAP animations"
```

---

### Task 5: About Section

**Files:**
- Create: `src/components/About.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create About.astro**

Create `src/components/About.astro`:

```astro
---
const team = [
  {
    name: 'Єлизавета',
    role: 'контент-стратег, копірайтер, сценарист',
    image: '/images/team/yelyzaveta.jpg',
  },
  {
    name: 'Власта',
    role: 'дизайнер, сторісмейкер, візуальний стратег',
    image: '/images/team/vlasta.jpg',
  },
];
---

<section id="about" class="py-20 md:py-32 px-4">
  <div class="max-w-5xl mx-auto">
    <h2 class="about-title text-3xl md:text-5xl font-bold text-text-primary text-center mb-6">
      Хто ми та що пропонуємо?
    </h2>
    <p class="about-desc text-text-muted text-center max-w-2xl mx-auto mb-16 text-lg">
      Ми не просто знімаємо відео — ми впроваджуємо чітку систему, яка перетворює глядачів на клієнтів.
    </p>

    <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
      {team.map((person) => (
        <div class="about-card group bg-bg-card border border-deep-violet rounded-2xl p-8 text-center transition-all duration-300 hover:border-violet-600 hover:shadow-[0_0_40px_rgba(124,58,237,0.15)]">
          <div class="w-32 h-32 mx-auto mb-6 rounded-full overflow-hidden border-3 border-violet-900 group-hover:border-violet-300 transition-all duration-300 group-hover:shadow-[0_0_30px_rgba(124,58,237,0.4)]">
            <img src={person.image} alt={person.name} class="w-full h-full object-cover" />
          </div>
          <h3 class="text-2xl font-bold text-violet-100 mb-2">{person.name}</h3>
          <p class="text-violet-300 text-sm tracking-wide">{person.role}</p>
        </div>
      ))}
    </div>
  </div>
</section>

<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.about-title', {
    y: 40,
    opacity: 0,
    duration: 0.8,
    scrollTrigger: { trigger: '#about', start: 'top 80%' },
  });

  gsap.from('.about-desc', {
    y: 30,
    opacity: 0,
    duration: 0.8,
    delay: 0.2,
    scrollTrigger: { trigger: '#about', start: 'top 80%' },
  });

  gsap.from('.about-card', {
    y: 60,
    opacity: 0,
    duration: 0.8,
    stagger: 0.2,
    scrollTrigger: { trigger: '.about-card', start: 'top 85%' },
  });
</script>
```

- [ ] **Step 2: Add About to index.astro**

Add import and component after Hero in `src/pages/index.astro`:

```astro
---
import Layout from '../layouts/Layout.astro';
import Hero from '../components/Hero.astro';
import About from '../components/About.astro';
import '../styles/global.css';
---

<Layout title="Єлизавета & Власта — SMM" description="SMM-спеціалісти, що масштабують ваш бізнес">
  <main>
    <Hero />
    <About />
  </main>
</Layout>
```

- [ ] **Step 3: Verify About section renders and animates on scroll**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: After scrolling past Hero, the About section fades in with two team cards side by side (stacked on mobile). Hover on cards shows violet glow.

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/About.astro src/pages/index.astro
git commit -m "feat: add About section with team cards"
```

---

### Task 6: Services Section

**Files:**
- Create: `src/components/Services.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Services.astro**

Create `src/components/Services.astro`:

```astro
---
const steps = ['Аналіз ЦА', 'Стратегія', 'Контент', 'Воронка продажів'];

const services = [
  { title: 'Контент-стратегія', icon: '📋' },
  { title: 'Фото/Відеозйомка', icon: '📸' },
  { title: 'Дизайн та верстка', icon: '🎨' },
  { title: 'Ведення акаунтів', icon: '📱' },
];
---

<section id="services" class="py-20 md:py-32 px-4 bg-bg-section">
  <div class="max-w-5xl mx-auto">
    <h2 class="services-title text-3xl md:text-5xl font-bold text-text-primary text-center mb-16">
      Повний цикл проекту під ключ
    </h2>

    <!-- Process flow -->
    <div class="flex flex-wrap items-center justify-center gap-3 md:gap-4 mb-16">
      {steps.map((step, i) => (
        <div class="flex items-center gap-3 md:gap-4">
          <span class="flow-step bg-deep-violet text-violet-300 px-5 py-3 rounded-xl text-sm md:text-base font-medium">
            {step}
          </span>
          {i < steps.length - 1 && (
            <span class="flow-arrow text-violet-600 text-xl font-bold">→</span>
          )}
        </div>
      ))}
    </div>

    <!-- Service cards -->
    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
      {services.map((service) => (
        <div class="service-card bg-bg-card border border-deep-violet rounded-2xl p-6 text-center transition-all duration-300 hover:border-violet-600 hover:shadow-[0_0_30px_rgba(124,58,237,0.15)] hover:-translate-y-1">
          <div class="text-4xl mb-4">{service.icon}</div>
          <h3 class="text-violet-100 font-semibold">{service.title}</h3>
        </div>
      ))}
    </div>
  </div>
</section>

<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.services-title', {
    y: 40,
    opacity: 0,
    duration: 0.8,
    scrollTrigger: { trigger: '#services', start: 'top 80%' },
  });

  gsap.from('.flow-step', {
    x: -30,
    opacity: 0,
    duration: 0.5,
    stagger: 0.15,
    scrollTrigger: { trigger: '.flow-step', start: 'top 85%' },
  });

  gsap.from('.flow-arrow', {
    scale: 0,
    opacity: 0,
    duration: 0.3,
    stagger: 0.15,
    delay: 0.1,
    scrollTrigger: { trigger: '.flow-step', start: 'top 85%' },
  });

  gsap.from('.service-card', {
    y: 50,
    opacity: 0,
    duration: 0.6,
    stagger: 0.1,
    scrollTrigger: { trigger: '.service-card', start: 'top 90%' },
  });
</script>
```

- [ ] **Step 2: Add Services to index.astro**

Add import and `<Services />` after `<About />` in `src/pages/index.astro`.

- [ ] **Step 3: Verify flow animation and service cards**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Process flow appears step-by-step with animated arrows. Service cards stagger in from below.

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/Services.astro src/pages/index.astro
git commit -m "feat: add Services section with flow and cards"
```

---

### Task 7: Cases Section (Horizontal Carousel)

**Files:**
- Create: `src/components/Cases.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Cases.astro**

Create `src/components/Cases.astro`:

```astro
---
const cases = [
  {
    name: 'Coffee Place №1',
    image: '/images/projects/coffee-place.jpg',
    followers: '2 873',
    instagram: 'https://www.instagram.com/coffeeplace___1/',
    category: 'Кав\'ярня',
  },
  {
    name: 'Затишний дім',
    image: '/images/projects/zatyshnyi-dim.jpg',
    followers: '2 076',
    instagram: 'https://www.instagram.com/zatyshnyjdim.official/',
    category: 'Ресторан',
  },
  {
    name: 'Бюстьє',
    image: '/images/projects/biustie.jpg',
    followers: '42.1K',
    instagram: 'https://www.instagram.com/biustie/',
    category: 'Магазин білизни',
  },
  {
    name: 'Food Place №1',
    image: '/images/projects/food-place.jpg',
    followers: '1 480',
    instagram: 'https://www.instagram.com/foodplace___1/',
    category: 'Кафе',
  },
];
---

<section id="cases" class="py-20 md:py-32">
  <div class="max-w-7xl mx-auto">
    <h2 class="cases-title text-3xl md:text-5xl font-bold text-text-primary text-center mb-4 px-4">
      Наш досвід, кейси та експертиза
    </h2>
    <p class="text-text-muted text-center mb-12 px-4">Повний цикл проекту під ключ</p>

    <!-- Carousel wrapper -->
    <div class="relative group">
      <!-- Arrow left -->
      <button
        class="carousel-arrow carousel-prev absolute left-2 top-1/2 -translate-y-1/2 z-10 w-12 h-12 bg-bg-card/80 backdrop-blur border border-deep-violet rounded-full flex items-center justify-center text-violet-300 hover:bg-violet-600 hover:text-white transition-all opacity-0 group-hover:opacity-100 hidden md:flex"
        aria-label="Previous"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/></svg>
      </button>

      <!-- Track -->
      <div class="carousel-track flex gap-6 overflow-x-auto scroll-smooth snap-x snap-mandatory px-4 md:px-12 pb-4">
        {cases.map((c) => (
          <a
            href={c.instagram}
            target="_blank"
            rel="noopener noreferrer"
            class="case-card flex-shrink-0 w-[85vw] sm:w-[60vw] md:w-[40vw] lg:w-[30vw] snap-center bg-bg-card border border-deep-violet rounded-2xl overflow-hidden transition-all duration-300 hover:border-violet-600 hover:shadow-[0_0_40px_rgba(124,58,237,0.2)] hover:-translate-y-2 block"
          >
            <div class="aspect-[4/3] overflow-hidden">
              <img
                src={c.image}
                alt={c.name}
                class="w-full h-full object-cover transition-transform duration-500 hover:scale-105"
              />
            </div>
            <div class="p-6">
              <span class="text-xs text-text-muted uppercase tracking-wider">{c.category}</span>
              <h3 class="text-xl font-bold text-violet-100 mt-1 mb-2">{c.name}</h3>
              <div class="flex items-center justify-between">
                <span class="text-violet-300 font-semibold">{c.followers} підписників</span>
                <span class="text-violet-400 flex items-center gap-1 text-sm">
                  Instagram
                  <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14"/></svg>
                </span>
              </div>
            </div>
          </a>
        ))}
      </div>

      <!-- Arrow right -->
      <button
        class="carousel-arrow carousel-next absolute right-2 top-1/2 -translate-y-1/2 z-10 w-12 h-12 bg-bg-card/80 backdrop-blur border border-deep-violet rounded-full flex items-center justify-center text-violet-300 hover:bg-violet-600 hover:text-white transition-all opacity-0 group-hover:opacity-100 hidden md:flex"
        aria-label="Next"
      >
        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
      </button>
    </div>

    <!-- Dots indicator -->
    <div class="flex justify-center gap-2 mt-6">
      {cases.map((_, i) => (
        <button
          class="carousel-dot w-2 h-2 rounded-full bg-violet-900 transition-all duration-300"
          data-index={i}
          aria-label={`Slide ${i + 1}`}
        ></button>
      ))}
    </div>
  </div>
</section>

<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  // Title animation
  gsap.from('.cases-title', {
    y: 40,
    opacity: 0,
    duration: 0.8,
    scrollTrigger: { trigger: '#cases', start: 'top 80%' },
  });

  // Cards stagger reveal
  gsap.from('.case-card', {
    x: 100,
    opacity: 0,
    duration: 0.6,
    stagger: 0.15,
    scrollTrigger: { trigger: '#cases', start: 'top 70%' },
  });

  // Carousel navigation
  const track = document.querySelector('#cases .carousel-track') as HTMLElement;
  const prevBtn = document.querySelector('#cases .carousel-prev');
  const nextBtn = document.querySelector('#cases .carousel-next');
  const dots = document.querySelectorAll('#cases .carousel-dot');

  if (track && prevBtn && nextBtn) {
    const getCardWidth = () => {
      const card = track.querySelector('.case-card') as HTMLElement;
      if (!card) return 300;
      return card.offsetWidth + 24; // card width + gap
    };

    prevBtn.addEventListener('click', () => {
      track.scrollBy({ left: -getCardWidth(), behavior: 'smooth' });
    });

    nextBtn.addEventListener('click', () => {
      track.scrollBy({ left: getCardWidth(), behavior: 'smooth' });
    });

    // Update dots on scroll
    track.addEventListener('scroll', () => {
      const scrollLeft = track.scrollLeft;
      const cardWidth = getCardWidth();
      const activeIndex = Math.round(scrollLeft / cardWidth);
      dots.forEach((dot, i) => {
        dot.classList.toggle('bg-violet-400', i === activeIndex);
        dot.classList.toggle('w-6', i === activeIndex);
        dot.classList.toggle('bg-violet-900', i !== activeIndex);
        dot.classList.toggle('w-2', i !== activeIndex);
      });
    });

    // Dot click navigation
    dots.forEach((dot) => {
      dot.addEventListener('click', () => {
        const index = parseInt((dot as HTMLElement).dataset.index || '0');
        track.scrollTo({ left: index * getCardWidth(), behavior: 'smooth' });
      });
    });

    // Initialize first dot
    dots[0]?.classList.add('bg-violet-400', 'w-6');
    dots[0]?.classList.remove('bg-violet-900', 'w-2');
  }
</script>
```

- [ ] **Step 2: Add Cases to index.astro**

Add import and `<Cases />` after `<Services />` in `src/pages/index.astro`.

- [ ] **Step 3: Verify carousel scrolls horizontally with arrows and swipe**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Horizontal carousel of project cards. Desktop shows arrow buttons on hover. Mobile is swipeable. Dots update on scroll. Cards link to Instagram in new tab.

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/Cases.astro src/pages/index.astro
git commit -m "feat: add Cases section with horizontal carousel"
```

---

### Task 8: Results Section (Animated Counters)

**Files:**
- Create: `src/components/Results.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Results.astro**

Create `src/components/Results.astro`:

```astro
---
const stats = [
  { value: 218000, suffix: '+', label: 'переглядів рілсів', display: '218K' },
  { value: 93400, suffix: '', label: 'охоплення', display: '93.4K' },
  { value: 500, suffix: '+', label: 'нових підписників', display: '500' },
];
---

<section id="results" class="py-20 md:py-32 px-4 bg-bg-section">
  <div class="max-w-5xl mx-auto">
    <h2 class="results-title text-3xl md:text-5xl font-bold text-text-primary text-center mb-4">
      Віральний контент — результати
    </h2>
    <p class="text-violet-300 text-center mb-16">знаємо алгоритми віральності</p>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      {stats.map((stat) => (
        <div class="result-card bg-bg-card border border-deep-violet rounded-2xl p-8 text-center transition-all duration-300 hover:border-violet-600 hover:shadow-[0_0_40px_rgba(124,58,237,0.2)]">
          <div class="text-4xl md:text-5xl font-bold text-violet-400 mb-2">
            <span class="counter" data-value={stat.value} data-display={stat.display}>{stat.display}</span>
            <span class="text-violet-300">{stat.suffix}</span>
          </div>
          <p class="text-text-muted text-sm mt-2">{stat.label}</p>
        </div>
      ))}
    </div>
  </div>
</section>

<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.results-title', {
    y: 40,
    opacity: 0,
    duration: 0.8,
    scrollTrigger: { trigger: '#results', start: 'top 80%' },
  });

  // Counter animation
  const counters = document.querySelectorAll('.counter');
  counters.forEach((counter) => {
    const el = counter as HTMLElement;
    const targetValue = parseInt(el.dataset.value || '0');
    const displayValue = el.dataset.display || '0';

    // Format number like the display value (e.g., 218K, 93.4K)
    const formatNumber = (n: number): string => {
      if (displayValue.includes('K')) {
        const kValue = n / 1000;
        if (displayValue.includes('.')) {
          return kValue.toFixed(1) + 'K';
        }
        return Math.floor(kValue) + 'K';
      }
      return Math.floor(n).toLocaleString();
    };

    const obj = { val: 0 };
    gsap.to(obj, {
      val: targetValue,
      duration: 2,
      ease: 'power2.out',
      scrollTrigger: {
        trigger: el,
        start: 'top 85%',
        once: true,
      },
      onUpdate: () => {
        el.textContent = formatNumber(obj.val);
      },
    });
  });

  gsap.from('.result-card', {
    y: 50,
    opacity: 0,
    duration: 0.6,
    stagger: 0.15,
    scrollTrigger: { trigger: '.result-card', start: 'top 85%' },
  });
</script>
```

- [ ] **Step 2: Add Results to index.astro**

Add import and `<Results />` after `<Cases />` in `src/pages/index.astro`.

- [ ] **Step 3: Verify counters animate from 0**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Three stat cards. When scrolled into view, numbers animate from 0 to target values (218K+, 93.4K, 500+).

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/Results.astro src/pages/index.astro
git commit -m "feat: add Results section with animated counters"
```

---

### Task 9: Formats Section

**Files:**
- Create: `src/components/Formats.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Formats.astro**

Create `src/components/Formats.astro`:

```astro
---
const formats = [
  {
    title: 'Backstage & Product',
    description: 'Живий контент про продукт та команду',
    icon: '🎬',
  },
  {
    title: 'Atmospheric',
    description: 'Естетика, яка продає',
    icon: '✨',
  },
  {
    title: 'Воронки',
    description: 'Створюємо шлях клієнта від перегляду reels до покупки в direct',
    icon: '🎯',
  },
];
---

<section id="formats" class="py-20 md:py-32 px-4">
  <div class="max-w-5xl mx-auto">
    <h2 class="formats-title text-3xl md:text-5xl font-bold text-text-primary text-center mb-16">
      Формати та типи контенту
    </h2>

    <div class="grid grid-cols-1 md:grid-cols-3 gap-8">
      {formats.map((format) => (
        <div class="format-card bg-bg-card border border-deep-violet rounded-2xl p-8 transition-all duration-300 hover:border-violet-600 hover:shadow-[0_0_30px_rgba(124,58,237,0.15)] hover:-translate-y-1">
          <div class="text-4xl mb-4">{format.icon}</div>
          <h3 class="text-xl font-bold text-violet-100 mb-3">{format.title}</h3>
          <p class="text-text-muted leading-relaxed">{format.description}</p>
        </div>
      ))}
    </div>
  </div>
</section>

<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.formats-title', {
    y: 40,
    opacity: 0,
    duration: 0.8,
    scrollTrigger: { trigger: '#formats', start: 'top 80%' },
  });

  gsap.from('.format-card', {
    y: 50,
    opacity: 0,
    duration: 0.6,
    stagger: 0.15,
    scrollTrigger: { trigger: '.format-card', start: 'top 85%' },
  });
</script>
```

- [ ] **Step 2: Add Formats to index.astro**

Add import and `<Formats />` after `<Results />` in `src/pages/index.astro`.

- [ ] **Step 3: Verify**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Three format cards with icons, titles, and descriptions. Fade in on scroll.

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/Formats.astro src/pages/index.astro
git commit -m "feat: add Formats section"
```

---

### Task 10: Equipment Section (Horizontal Carousel)

**Files:**
- Create: `src/components/Equipment.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Equipment.astro**

Create `src/components/Equipment.astro`:

```astro
---
const equipment = [
  { name: 'Камера Canon', type: 'hardware', icon: '📷' },
  { name: 'DJI Mic Mini', type: 'hardware', icon: '🎙️' },
  { name: 'iPhone 15 Pro Max', type: 'hardware', icon: '📱' },
  { name: 'iPhone 14 Pro', type: 'hardware', icon: '📱' },
  { name: 'Figma', type: 'software', icon: '🎨' },
  { name: 'CapCut', type: 'software', icon: '🎬' },
];
---

<section id="equipment" class="py-20 md:py-32 bg-bg-section">
  <div class="max-w-7xl mx-auto">
    <h2 class="equip-title text-3xl md:text-5xl font-bold text-text-primary text-center mb-4 px-4">
      Наша система та обладнання
    </h2>
    <p class="text-text-muted text-center mb-12 px-4 max-w-2xl mx-auto">
      Ми переносимо нашу відпрацьовану систему на вашу нішу, щоб отримати прогнозований результат, а не випадкові перегляди
    </p>

    <!-- Horizontal carousel -->
    <div class="relative group">
      <button
        class="equip-prev absolute left-2 top-1/2 -translate-y-1/2 z-10 w-10 h-10 bg-bg-card/80 backdrop-blur border border-deep-violet rounded-full flex items-center justify-center text-violet-300 hover:bg-violet-600 hover:text-white transition-all opacity-0 group-hover:opacity-100 hidden md:flex"
        aria-label="Previous"
      >
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 19l-7-7 7-7"/></svg>
      </button>

      <div class="equip-track carousel-track flex gap-4 overflow-x-auto scroll-smooth snap-x snap-mandatory px-4 md:px-12 pb-4">
        {equipment.map((item) => (
          <div class="equip-card flex-shrink-0 w-48 md:w-56 snap-center bg-bg-card border border-deep-violet rounded-2xl p-6 text-center transition-all duration-300 hover:border-violet-600 hover:shadow-[0_0_20px_rgba(124,58,237,0.15)]">
            <div class="text-4xl mb-3">{item.icon}</div>
            <h3 class="text-violet-100 font-semibold text-sm">{item.name}</h3>
            <span class="text-xs text-text-muted mt-1 block capitalize">{item.type}</span>
          </div>
        ))}
      </div>

      <button
        class="equip-next absolute right-2 top-1/2 -translate-y-1/2 z-10 w-10 h-10 bg-bg-card/80 backdrop-blur border border-deep-violet rounded-full flex items-center justify-center text-violet-300 hover:bg-violet-600 hover:text-white transition-all opacity-0 group-hover:opacity-100 hidden md:flex"
        aria-label="Next"
      >
        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5l7 7-7 7"/></svg>
      </button>
    </div>
  </div>
</section>

<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.equip-title', {
    y: 40,
    opacity: 0,
    duration: 0.8,
    scrollTrigger: { trigger: '#equipment', start: 'top 80%' },
  });

  gsap.from('.equip-card', {
    x: 80,
    opacity: 0,
    rotation: 5,
    duration: 0.6,
    stagger: 0.1,
    ease: 'back.out(1.4)',
    scrollTrigger: { trigger: '#equipment', start: 'top 75%' },
  });

  // Carousel navigation
  const track = document.querySelector('.equip-track') as HTMLElement;
  const prevBtn = document.querySelector('.equip-prev');
  const nextBtn = document.querySelector('.equip-next');

  if (track && prevBtn && nextBtn) {
    const scrollAmount = 240;
    prevBtn.addEventListener('click', () => {
      track.scrollBy({ left: -scrollAmount, behavior: 'smooth' });
    });
    nextBtn.addEventListener('click', () => {
      track.scrollBy({ left: scrollAmount, behavior: 'smooth' });
    });
  }
</script>
```

- [ ] **Step 2: Add Equipment to index.astro**

Add import and `<Equipment />` after `<Formats />` in `src/pages/index.astro`.

- [ ] **Step 3: Verify horizontal scroll with equipment cards**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Horizontal scrollable row of equipment cards. Float-in animation from right with slight rotation. Arrows on desktop hover. Swipeable on mobile.

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/Equipment.astro src/pages/index.astro
git commit -m "feat: add Equipment section with horizontal carousel"
```

---

### Task 11: CTA Section

**Files:**
- Create: `src/components/CTA.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create CTA.astro**

Create `src/components/CTA.astro`:

```astro
---
---

<section id="cta" class="py-20 md:py-32 px-4">
  <div class="max-w-3xl mx-auto">
    <div class="cta-block relative bg-gradient-to-br from-deep-violet to-violet-900 rounded-3xl p-12 md:p-16 text-center overflow-hidden">
      <!-- Background glow -->
      <div class="absolute inset-0 opacity-30">
        <div class="absolute top-1/2 left-1/2 -translate-x-1/2 -translate-y-1/2 w-96 h-96 bg-violet-600 rounded-full blur-[120px]"></div>
      </div>

      <div class="relative z-10">
        <h2 class="cta-heading text-3xl md:text-5xl font-bold text-text-primary mb-6">
          Готові масштабувати ваш бізнес?
        </h2>
        <p class="text-violet-200 mb-10 text-lg">
          Напишіть нам і ми обговоримо вашу стратегію
        </p>
        <a
          href="https://www.instagram.com/"
          target="_blank"
          rel="noopener noreferrer"
          class="cta-button inline-flex items-center gap-3 bg-white text-violet-900 font-bold py-4 px-12 rounded-full text-lg transition-all duration-300 hover:shadow-[0_0_60px_rgba(255,255,255,0.3)] hover:scale-105"
        >
          Написати в Instagram Direct
          <svg class="w-5 h-5" fill="currentColor" viewBox="0 0 24 24"><path d="M12 2.163c3.204 0 3.584.012 4.85.07 3.252.148 4.771 1.691 4.919 4.919.058 1.265.069 1.645.069 4.849 0 3.205-.012 3.584-.069 4.849-.149 3.225-1.664 4.771-4.919 4.919-1.266.058-1.644.07-4.85.07-3.204 0-3.584-.012-4.849-.07-3.26-.149-4.771-1.699-4.919-4.92-.058-1.265-.07-1.644-.07-4.849 0-3.204.013-3.583.07-4.849.149-3.227 1.664-4.771 4.919-4.919 1.266-.057 1.645-.069 4.849-.069zM12 0C8.741 0 8.333.014 7.053.072 2.695.272.273 2.69.073 7.052.014 8.333 0 8.741 0 12c0 3.259.014 3.668.072 4.948.2 4.358 2.618 6.78 6.98 6.98C8.333 23.986 8.741 24 12 24c3.259 0 3.668-.014 4.948-.072 4.354-.2 6.782-2.618 6.979-6.98.059-1.28.073-1.689.073-4.948 0-3.259-.014-3.667-.072-4.947-.196-4.354-2.617-6.78-6.979-6.98C15.668.014 15.259 0 12 0zm0 5.838a6.162 6.162 0 100 12.324 6.162 6.162 0 000-12.324zM12 16a4 4 0 110-8 4 4 0 010 8zm6.406-11.845a1.44 1.44 0 100 2.881 1.44 1.44 0 000-2.881z"/></svg>
        </a>
      </div>
    </div>

    <!-- Footer -->
    <p class="text-text-muted text-center text-sm mt-12">
      &copy; 2026 Єлизавета & Власта. Всі права захищені.
    </p>
  </div>
</section>

<script>
  import gsap from 'gsap';
  import { ScrollTrigger } from 'gsap/ScrollTrigger';
  gsap.registerPlugin(ScrollTrigger);

  gsap.from('.cta-block', {
    y: 60,
    opacity: 0,
    duration: 0.8,
    scrollTrigger: { trigger: '#cta', start: 'top 80%' },
  });

  gsap.from('.cta-heading', {
    y: 30,
    opacity: 0,
    duration: 0.6,
    delay: 0.2,
    scrollTrigger: { trigger: '#cta', start: 'top 80%' },
  });

  // Pulsing glow on button
  gsap.to('.cta-button', {
    boxShadow: '0 0 40px rgba(124, 58, 237, 0.6)',
    duration: 1.5,
    repeat: -1,
    yoyo: true,
    ease: 'sine.inOut',
    scrollTrigger: { trigger: '.cta-button', start: 'top 90%' },
  });
</script>
```

- [ ] **Step 2: Add CTA to index.astro**

Final `src/pages/index.astro`:

```astro
---
import Layout from '../layouts/Layout.astro';
import Hero from '../components/Hero.astro';
import About from '../components/About.astro';
import Services from '../components/Services.astro';
import Cases from '../components/Cases.astro';
import Results from '../components/Results.astro';
import Formats from '../components/Formats.astro';
import Equipment from '../components/Equipment.astro';
import CTA from '../components/CTA.astro';
import '../styles/global.css';
---

<Layout title="Єлизавета & Власта — SMM" description="SMM-спеціалісти, що масштабують ваш бізнес">
  <main>
    <Hero />
    <About />
    <Services />
    <Cases />
    <Results />
    <Formats />
    <Equipment />
    <CTA />
  </main>
</Layout>
```

- [ ] **Step 3: Verify CTA renders with pulsing glow button**

```bash
cd /Users/danylo/Arena/portfolio
npm run dev
```

Expected: Gradient card with heading, subheading, white Instagram button with pulsing violet glow. Footer below.

- [ ] **Step 4: Commit**

```bash
cd /Users/danylo/Arena/portfolio
git add src/components/CTA.astro src/pages/index.astro
git commit -m "feat: add CTA section and assemble full page"
```

---

### Task 12: Responsive Polish and Final Build

**Files:**
- Modify: `src/styles/global.css` (if needed)
- Modify: various components (if breakpoint issues found)

- [ ] **Step 1: Run production build to catch any errors**

```bash
cd /Users/danylo/Arena/portfolio
npm run build
```

Expected: Build succeeds with no errors. Output in `dist/`.

- [ ] **Step 2: Preview production build**

```bash
cd /Users/danylo/Arena/portfolio
npm run preview
```

Expected: Opens at `localhost:4321`. All 8 sections visible. Scroll through entire page verifying:
- Hero: particles, photos, CTA button
- About: two cards side by side (stacked on mobile)
- Services: flow + 4 service cards (4 cols → 2 → 1)
- Cases: horizontal carousel, swipeable, arrows, dots, Instagram links
- Results: animated counters
- Formats: 3 cards
- Equipment: horizontal carousel
- CTA: pulsing button

- [ ] **Step 3: Fix any responsive or visual issues found**

Check and fix at 3 breakpoints (resize browser window):
- Desktop (1280px): full layouts, all columns visible
- Tablet (768px): 2-column grids, carousels functional
- Mobile (375px): single column, stacked layouts, swipe carousels

- [ ] **Step 4: Add .gitignore entries**

Verify `.gitignore` includes:

```
node_modules/
dist/
.astro/
.superpowers/
```

- [ ] **Step 5: Final commit**

```bash
cd /Users/danylo/Arena/portfolio
git add -A
git commit -m "feat: responsive polish and production build verification"
```

---

## Self-Review Checklist

- **Spec coverage**: All 8 sections implemented (Hero, About, Services, Cases, Results, Formats, Equipment, CTA). Dark Violet palette. Hybrid layout with horizontal carousels for Cases (Task 7) and Equipment (Task 10). GSAP animations in every section. Responsive breakpoints. Instagram links for all 4 projects. Ukrainian language only.
- **Placeholder scan**: No TBD/TODO items. All code blocks complete. All file paths exact.
- **Type consistency**: Component names consistent across imports and files. CSS class names consistent between HTML and GSAP selectors. Data attributes match between HTML and script.
