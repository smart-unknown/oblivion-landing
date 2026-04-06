# OBLIVION — Where Reality Ends [![Live Demo](https://img.shields.io/badge/Live_Demo-0ff-171717?style=for-the-badge&logo=googlechrome&logoColor=0ff&labelColor=171717)](https://smart-unknown.github.io/oblivion-landing/)

> **"We don't push boundaries. We erase them."**

A single-file, aggressively dark landing page for a fictional spatial creation engine. Opens with a boot screen. Closes with a glitch. Everything in between is designed to make you feel like you're staring into something that shouldn't exist.

![OBLIVION — dark cinematic landing page](https://picsum.photos/seed/ob-readme/1400/800)

---

## ✦ The Vibe

This isn't a landing page. It's a **manifesto with interactions**. Every effect was chosen to reinforce one idea: *this thing exists outside normal design conventions*. Lime green on void black. Wireframe geometry breathing in the background. Click anywhere and watch reality ripple.

---

## ✦ Effect Breakdown

### Boot Screen
The page doesn't load — it **boots**. "OBLIVION" flickers three times in lime monospace, then "INITIALIZING SPATIAL ENGINE" fades in below. After 1.8 seconds, the screen dissolves and the page appears. Sets the tone before a single pixel of content renders.

### 3D Wireframe Icosahedron
Not Three.js. Not WebGL. **Canvas 2D math from scratch.**

- 12 vertices, 30 edges of a regular icosahedron
- Dual Y/X rotation with slow autonomous drift
- Mouse position influences rotation target with 0.06 lerp damping
- **Dual-pass rendering:** first pass draws thick semi-transparent edges (glow), second pass draws thin sharp edges on top
- Depth-based color shifting: front faces lean lime (hue 80), back faces shift toward cyan (hue 20)
- Vertex dots scale with depth (1px → 2.5px)
- 70 floating background particles with sine-wave drift + mouse parallax offset

```js
// Edge rendering — glow then sharp
for (const [i, j] of edges) {
  // Pass 1: glow (3px, low alpha)
  ctx.strokeStyle = `rgba(205,255,0,${.04 + depth * .08})`;
  ctx.lineWidth = 3;
  // Pass 2: sharp (0.8px, high alpha, hue-shifted)
  ctx.strokeStyle = `hsla(${80 - depth * 60}, 100%, ${50 + depth * 15}%, ${.08 + depth * .35})`;
  ctx.lineWidth = 0.8;
}
```

### Click Ripple
Every click on the page spawns an expanding circle — 1px lime border, `scale(0)` → `scale(50)`, 600ms ease-out, then self-destructs. Subtle but addictive. Makes the entire page feel tactile.

### 3D Tilt Cards
Showcase cards use `perspective(800px)` with dynamic `rotateY` / `rotateX` based on cursor position (±12°). Three layers move independently:
- **Card body** — tilts and scales to 1.02
- **Shine overlay** — radial gradient follows cursor via CSS custom properties
- **Image** — parallax offset at 8px inverse to cursor direction

Reset uses a spring easing (`cubic-bezier(.34, 1.56, .64, 1)`) for that satisfying snap-back.

### Rotating Conic-Gradient Borders
Feature cards wrapped in a 1px padding container with animated `conic-gradient` border using `@property --border-angle` (Houdini API). Each card has a staggered `animation-delay` so the borders rotate out of phase — four cards, four offsets, creates an organic wave pattern.

### SVG Ring Stats
Instead of plain numbers, stats are wrapped in SVG circular progress rings. `stroke-dashoffset` animates from full (263.9) to 15% remaining on scroll-into-view. Each ring has a different accent color: lime, coral, cyan.

### CSS Glitch Text
The CTA headline uses `::before` and `::after` pseudo-elements with `data-text` attribute duplication. On scroll-into-view, the `.active` class triggers:
- `::before` — coral tint, clipped to top third, `translate(-4px, -2px)` jitter
- `::after` — cyan tint, clipped to bottom third, `translate(4px, 2px)` jitter
- Both run 2 iterations of 300ms, overlapping with text scramble

### Morphing Blobs
Three absolutely-positioned divs with animated `border-radius` and `transform` in the CTA section. Lime, coral, and cyan at different sizes and animation delays. 80px blur makes them feel like light bleeding through the void.

### Dual Marquee
Two rows scrolling in opposite directions at different speeds (30s / 35s). Different font weights and sizes create visual hierarchy. Pause on hover.

---

## ✦ Full Effect Inventory

| # | Effect | Type |
|---|--------|------|
| 1 | Boot screen with flicker | CSS animation |
| 2 | 3D wireframe icosahedron | Canvas 2D |
| 3 | Floating background particles | Canvas 2D |
| 4 | Custom cursor (dot + ring + glow) | DOM + JS |
| 5 | Click ripple on any click | DOM + CSS animation |
| 6 | Scroll progress bar (2px lime) | CSS transform |
| 7 | Header blur on scroll | CSS backdrop-filter |
| 8 | Dual-direction marquee | CSS animation |
| 9 | Manifesto blur-to-clear reveal | IntersectionObserver + CSS |
| 10 | 3D perspective tilt cards | DOM + JS |
| 11 | Card shine overlay | CSS custom properties |
| 12 | Image parallax inside cards | JS transform |
| 13 | Rotating conic-gradient borders | @property + CSS |
| 14 | SVG ring progress stats | SVG + JS |
| 15 | Number counter animation | JS requestAnimationFrame |
| 16 | Testimonial carousel | JS + CSS transition |
| 17 | CSS glitch text (dual clip-path) | CSS pseudo-elements |
| 18 | Text scramble (nav + CTA) | JS setInterval |
| 19 | Magnetic button pull | JS transform |
| 20 | Morphing background blobs | CSS border-radius animation |
| 21 | Noise overlay | Canvas (200×200 tiled) |
| 22 | Scroll reveal (fade up) | IntersectionObserver |
| 23 | Mobile menu | DOM toggle |
| 24 | Smooth anchor scroll | Native scroll-behavior |

**24 distinct interactions. One file.**

---

## ✦ Tech Stack

```
HTML5           → Semantic structure
CSS3            → @property, backdrop-filter, clip-path, conic-gradient
Canvas 2D       → Wireframe icosahedron + particles
Vanilla JS      → No frameworks, no libraries
Tailwind CSS    → CDN utility classes
Iconify         → Phosphor Icons via CDN
Google Fonts    → Syne · DM Sans · JetBrains Mono
```

**Zero npm packages. Zero build tools. Zero config files.**

---

## ✦ Icosahedron — The Math

For anyone who wants to understand the geometry:

```
Vertices: 12 points derived from (0, ±1, ±φ) and permutations
where φ = (1 + √5) / 2 ≈ 1.618

Normalization: each vertex divided by its magnitude → unit sphere

Edges: 30 edges connecting vertex pairs where distance < 1.2
(this catches all edges of the regular icosahedron and none of the diagonals)

Rotation: standard Y-axis and X-axis rotation matrices applied sequentially

Projection: perspective divide → fov / (fov + z), then scale to screen size
```

No libraries. No `gl-matrix`. Just `Math.cos`, `Math.sin`, and linear algebra 101.

---

## ✦ Color System

| Token | Hex | Identity |
|-------|-----|----------|
| `ob-bg` | `#060608` | The void — darker than standard black |
| `ob-elevated` | `#0D0D14` | Card surfaces |
| `ob-border` | `#1A1A26` | Subtle separators |
| `ob-text` | `#F0F0F5` | Primary text |
| `ob-muted` | `#6B6B80` | Secondary text |
| `ob-lime` | `#CDFF00` | Primary — aggression, energy, otherness |
| `ob-coral` | `#FF3355` | Secondary — danger, urgency, heat |
| `ob-cyan` | `#00D4FF` | Tertiary — cold, depth, technology |
| `ob-dim` | `#2A2A3A` | Muted labels |

The lime is the signature. It's not green, not yellow — it's **electric acid**. Deliberately uncomfortable on dark backgrounds. That's the point.

---

## ✦ Font Choice

**Syne** — Geometric, slightly alien, not overused. The `800` weight at large sizes creates that aggressive editorial feel that Inter or Space Grotesk can't quite match. Perfect for a product called OBLIVION.

---

## ✦ Performance

| Concern | Solution |
|---------|----------|
| Canvas redraw | Single `requestAnimationFrame` loop for wireframe + particles |
| DPR handling | Capped at 2x to prevent 3x screens from killing performance |
| Cursor tracking | Ring uses 0.14 lerp — smooth without overhead |
| Click ripples | DOM elements self-remove after 650ms — no accumulation |
| Resize | Debounced at 250ms |
| Observers | All `unobserve` after first trigger |
| Touch devices | Cursor elements, click ripples, mouse parallax — all hidden via `pointer: coarse` |
| Reduced motion | Single media query kills every animation and transition |

---

## ✦ File Structure

```
oblivion-landing/
└── index.html    ← ~520 lines. The entire experience.
```

---

## ✦ Quick Start

```bash
git clone https://github.com/smart-unknown/oblivion-landing.git
cd oblivion-landing
open index.html
```

That's it. No server needed. No `npm install`. No `node_modules` black hole.

If you want a local server (for the fonts to load faster):

```bash
npx serve .
# or
python3 -m http.server 8000
```

---

## ✦ Browser Support

| Browser | Version | Notes |
|---------|---------|-------|
| Chrome | 90+ | Full support |
| Firefox | 95+ | `@property` supported from 128+ — borders degrade gracefully |
| Safari | 15.4+ | `@property` supported from 15.4 |
| Edge | 90+ | Full support |

**Graceful degradation:** If `@property` isn't supported, the rotating borders simply show as static conic gradients. Everything else works identically.

---

## ✦ Credits

- **Fonts** — [Syne](https://fonts.google.com/specimen/Syne) · [DM Sans](https://fonts.google.com/specimen/DM+Sans) · [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono)
- **Icons** — [Phosphor Icons](https://phosphoricons.com/) via Iconify
- **Images** — [Picsum Photos](https://picsum.photos)
- **Icosahedron math** — Standard computational geometry, public domain
- **Attitude** — Every dark landing page that made you whisper "how did they do that"
