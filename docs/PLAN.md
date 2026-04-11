# Accordia Website Implementation Plan

## Overview

Single-page Astro site for **Accordia AI** — a Cape Town-based company that helps businesses integrate conversational AI (with a WhatsApp focus) into their workflows. The site features a bold hero animation depicting **chat transforming into business** using GSAP, deployed to **Cloudflare Pages**.

---

## Design Direction: "Signal → System"

**Concept:** Chat messages (signals) converge and transform into an organized business ecosystem (system). The animation tells the story visually — raw conversation becoming structured business value.

**Tone:** Dark, premium, editorial. Confident and forward-looking. A fintech/AI aesthetic with warmth — not cold and sterile, but alive with energy.

**Differentiator:** The hero animation itself — chat bubbles containing real business phrases ("Order status?", "Schedule a meeting", "Send invoice") float in, get processed by an AI core, and re-emerge as organized business elements. It's not a static illustration; it's a living transformation.

---

## Color Palette

| Token               | Value      | Usage                                    |
|----------------------|------------|------------------------------------------|
| `--bg-deep`          | `#08080D`  | Page background                          |
| `--bg-surface`       | `#111118`  | Card/section surfaces                    |
| `--bg-elevated`      | `#1A1A24`  | Elevated elements, hover states          |
| `--accent-primary`   | `#BEFF00`  | Primary accent — electric chartreuse     |
| `--accent-secondary` | `#00D4AA`  | Secondary accent — bright teal           |
| `--accent-glow`      | `#BEFF0033`| Glow/halo effects (primary @ 20%)        |
| `--text-primary`     | `#F0ECE2`  | Headings, primary body text (warm cream) |
| `--text-secondary`   | `#8A8A9A`  | Supporting text, captions                |
| `--text-muted`       | `#4A4A5A`  | Tertiary labels, decorative text         |
| `--chat-bubble`      | `#1E3A2F`  | Chat bubble background (dark green)      |
| `--chat-bubble-alt`  | `#1A1A24`  | Alternate chat bubble (received)         |

The electric chartreuse (`#BEFF00`) nods to WhatsApp's green but electrified — modern, striking, impossible to ignore against the deep dark background. The warm cream text (`#F0ECE2`) avoids pure white harshness.

---

## Typography

### Fonts: Clash Display + Satoshi (Fontshare)

| Font           | Role      | Weights Needed     | Why                                          |
|----------------|-----------|--------------------|----------------------------------------------|
| Clash Display  | Headlines | 500 (Medium), 600 (Semibold), 700 (Bold) | Geometric, bold, futuristic-but-approachable  |
| Satoshi        | Body      | 400 (Regular), 500 (Medium), 700 (Bold)  | Clean geometric sans, excellent readability    |

### Font Files Setup

1. Download from [fontshare.com](https://www.fontshare.com/):
   - [Clash Display](https://www.fontshare.com/fonts/clash-display) → Download WOFF2 files
   - [Satoshi](https://www.fontshare.com/fonts/satoshi) → Download WOFF2 files
2. Place in `public/fonts/`:
   ```
   public/fonts/
   ├── ClashDisplay-Medium.woff2
   ├── ClashDisplay-Semibold.woff2
   ├── ClashDisplay-Bold.woff2
   ├── Satoshi-Regular.woff2
   ├── Satoshi-Medium.woff2
   └── Satoshi-Bold.woff2
   ```
3. Register via `@font-face` in global CSS.

### Type Scale

| Element           | Font          | Size        | Weight |
|-------------------|---------------|-------------|--------|
| Hero headline     | Clash Display | clamp(3rem, 8vw, 7rem) | 700  |
| Section headings  | Clash Display | clamp(2rem, 5vw, 4rem) | 600  |
| Sub-headings      | Clash Display | 1.5rem      | 500    |
| Body              | Satoshi       | 1.125rem    | 400    |
| Large body/lead   | Satoshi       | 1.375rem    | 500    |
| Caption/label     | Satoshi       | 0.875rem    | 500    |

---

## GSAP Hero Animation: "Chat Becomes Business"

The centerpiece — a choreographed GSAP timeline playing on load.

### Animation Phases

**Phase 1 — "The Conversation" (0s – 2s)**
- Chat bubbles stagger in from left/right (alternating, like a real chat)
- Each bubble contains a business phrase: "Order #4521 status?", "Meeting at 3pm", "Invoice sent ✓"
- Uses `gsap.from()` with stagger and `ease: "back.out(1.4)"`
- Subtle float/bob animation on each bubble via `yoyo: true, repeat: -1`

**Phase 2 — "The Convergence" (2s – 3.5s)**
- Bubbles begin drifting toward center using `gsap.to()` with x/y
- A central orb/pulse element scales up with `ease: "power3.out"`
- Glow effect intensifies (CSS box-shadow animated via GSAP)
- Bubbles reduce in opacity as they approach the core

**Phase 3 — "The Transformation" (3.5s – 5.5s)**
- Central orb pulses and emits a ring (scale + autoAlpha)
- Business elements emerge from center: a mini dashboard, a workflow arrow, a notification badge, a chart bar
- Each element uses `gsap.from()` with scale: 0, rotation, stagger
- `ease: "elastic.out(1, 0.5)"` for a satisfying spring effect

**Phase 4 — "The Ecosystem" (5.5s – 7s)**
- Business elements settle into their final positions
- Subtle perpetual floating animation (`yoyo: true, repeat: -1, duration: 3`)
- Faint connecting lines draw between elements (SVG stroke-dashoffset animation)
- The tagline text animates in with a clip-path reveal

### GSAP Techniques Used

- `gsap.timeline()` for sequencing all phases
- `gsap.from()` / `gsap.to()` / `gsap.fromTo()` for individual tweens
- `stagger` with `from: "random"` and `from: "center"` for organic feel
- `gsap.matchMedia()` for responsive behavior + `prefers-reduced-motion`
- `autoAlpha` for proper fade in/out
- Function-based values for dynamic positioning: `x: (i) => ...`
- SVG stroke animation for connecting lines

### Reduced Motion

When `prefers-reduced-motion: reduce` is active:
- Skip all movement animations
- Business elements appear immediately in final state
- Only subtle opacity fades remain (duration: 0.3s)

---

## Site Sections

### 1. Navigation (sticky)
- Wordmark "Accordia" (Clash Display, bold) left-aligned
- Minimal nav links: How It Works, Benefits, Contact
- CTA button: "Get Started" with accent glow

### 2. Hero Section
- Left: Oversized headline — **"Chat becomes business."** (Clash Display 700, clamp 3–7rem)
- Left: Subtext — one sentence about 24/7 AI-powered conversations
- Right: The GSAP animation canvas (SVG + positioned divs)
- Full viewport height, slight diagonal gradient overlay

### 3. "The Shift" (transition statement)
- Large centered text: "The world's most popular messaging platform is now your most powerful business tool."
- Animated on scroll — text reveals word-by-word or line-by-line
- Minimal section, maximum impact

### 4. How It Works (3-step flow)
- **Connect** → WhatsApp icon + "Your customers are already there"
- **Automate** → AI brain icon + "AI handles the routine, surfaces what matters"
- **Grow** → Chart icon + "Your team focuses on high-value work"
- Horizontal flow on desktop, vertical on mobile
- Each step animates in on scroll (GSAP ScrollTrigger or Intersection Observer)

### 5. Benefits Grid
- 2×2 or 3-column grid of benefit cards
- Cards: dark surface with subtle border, accent icon, headline, short description
- Benefits: 24/7 Availability, Smart Routing, Workflow Integration, Real-time Insights
- Cards animate in with stagger on scroll

### 6. CTA Section
- Bold statement: "Ready to transform your conversations into business results?"
- Email + phone placeholders
- "Contact Us" button with hover glow effect
- Location: "Cape Town, South Africa" with subtle map pin icon

### 7. Footer
- Minimal: Wordmark, copyright, Cape Town
- Subtle gradient fade to pure black

---

## Project Structure

```
accordia/
├── public/
│   └── fonts/
│       ├── ClashDisplay-Medium.woff2
│       ├── ClashDisplay-Semibold.woff2
│       ├── ClashDisplay-Bold.woff2
│       ├── Satoshi-Regular.woff2
│       ├── Satoshi-Medium.woff2
│       └── Satoshi-Bold.woff2
├── src/
│   ├── components/
│   │   ├── Nav.astro
│   │   ├── Hero.astro
│   │   ├── HeroAnimation.astro      # SVG + GSAP animation container
│   │   ├── TheShift.astro
│   │   ├── HowItWorks.astro
│   │   ├── Benefits.astro
│   │   ├── CTA.astro
│   │   └── Footer.astro
│   ├── layouts/
│   │   └── BaseLayout.astro          # HTML shell, font loading, global CSS
│   ├── pages/
│   │   └── index.astro               # Assembles all sections
│   └── styles/
│       └── global.css                # CSS variables, @font-face, resets, utilities
├── astro.config.ts
├── package.json
└── tsconfig.json
```

---

## Tech Stack

| Layer        | Choice                     | Reason                                |
|--------------|----------------------------|---------------------------------------|
| Framework    | Astro                      | Content-driven, fast static output    |
| Animation    | GSAP (core + ScrollTrigger)| Timeline sequencing, scroll triggers  |
| Styling      | Vanilla CSS (no framework) | Full creative control, no bloat       |
| Deployment   | Cloudflare Pages           | Edge-deployed, fast globally          |
| Fonts        | Clash Display + Satoshi    | Distinctive, modern, free (Fontshare) |

### Dependencies

```json
{
  "dependencies": {
    "astro": "latest",
    "@astrojs/cloudflare": "latest",
    "gsap": "latest"
  }
}
```

GSAP's ScrollTrigger is included in the `gsap` package (free tier).

---

## Deployment: Cloudflare Pages

1. `npx astro add cloudflare --yes` to add the adapter
2. Configure `astro.config.ts`:
   ```ts
   import { defineConfig } from 'astro/config';
   import cloudflare from '@astrojs/cloudflare';

   export default defineConfig({
     output: 'static',
     adapter: cloudflare(),
     site: 'https://accordia.ai',
   });
   ```
3. Build: `npx astro build`
4. Deploy via Cloudflare Pages (connect repo or `wrangler pages deploy dist/`)

---

## Implementation Order

1. **Scaffold** — `npm create astro@latest`, install deps, configure Cloudflare adapter
2. **Foundations** — Global CSS (variables, fonts, reset), BaseLayout, type scale
3. **Structure** — All section components with static content (no animation yet)
4. **Hero Animation** — GSAP timeline, all four phases, responsive behavior
5. **Scroll Animations** — Section reveals, text animations, benefit card staggers
6. **Polish** — Hover effects, glow details, transitions, reduced-motion, responsive QA
7. **Deploy** — Build check, Cloudflare Pages deploy

---

## Visual Inspiration Notes

- The chat bubbles should feel like actual WhatsApp messages — rounded corners, green-tinted backgrounds, left/right alternation, timestamps
- The "AI core" orb in the center should have a soft radial gradient glow that pulses — alive, not static
- Business elements post-transformation should be abstract/iconic (not literal screenshots) — think geometric shapes that suggest dashboards, flows, charts
- The overall feel: like visiting a premium AI startup's landing page at 2am with the lights off — dark, focused, electric
- Grain texture overlay on the entire page (CSS noise pattern) for tactile depth
- Subtle grid/dot pattern in background sections for structure
