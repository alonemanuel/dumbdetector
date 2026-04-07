# DumbDetector — Design System & Guidelines

> This is a living reference for design decisions, not the source of truth (code is).
> When design and code diverge, update whichever is wrong.

---

## Aesthetic Direction: "Industrial Incident Room"

The core tension that makes DumbDetector memorable: **the design treats AI dumbness with the gravity of a nuclear control room**. Serious infrastructure monitoring aesthetic applied to a fundamentally silly concept. The humor is implicit — never winking, never jokey. The UI is dead serious. The content is absurd.

Think: airport departure boards, power grid control rooms, submarine sonar displays. Not: meme sites, toy dashboards, or startup landing pages.

### What makes it unforgettable
- The contrast between gravitas and silliness
- A massive, tactile, almost-dangerous-feeling report button
- Charts that look like they're monitoring something actually important

---

## Typography

### Primary: Geist Mono
- Used for: data, numbers, chart labels, status badges, the report count
- Why: monospaced fonts evoke terminals and monitoring systems. Geist Mono (by Vercel) is crisp and modern, not retro-kitsch
- Weight: 400 for data, 700 for emphasis

### Secondary: Geist Sans
- Used for: headings, body text, navigation, button labels
- Why: pairs naturally with Geist Mono while being highly readable. Has enough personality through its sharp geometry without being distracting
- Weight: 400 body, 600 semibold for subheadings, 800 for page titles

### Scale (fluid, clamp-based)
```
--text-xs:   clamp(0.65rem, 0.6rem + 0.25vw, 0.75rem)
--text-sm:   clamp(0.8rem, 0.75rem + 0.25vw, 0.875rem)
--text-base: clamp(0.9rem, 0.85rem + 0.25vw, 1rem)
--text-lg:   clamp(1.05rem, 0.95rem + 0.5vw, 1.25rem)
--text-xl:   clamp(1.2rem, 1rem + 1vw, 1.75rem)
--text-2xl:  clamp(1.5rem, 1.2rem + 1.5vw, 2.5rem)
--text-3xl:  clamp(2rem, 1.5rem + 2.5vw, 3.5rem)
```

### Rules
- Numbers and data ALWAYS use Geist Mono
- Never fake-bold monospace text (use actual 700 weight)
- Letter-spacing: +0.02em on uppercase labels, -0.01em on large headings
- Line-height: 1.1 for headings, 1.5 for body

---

## Color

Dark theme only. No light mode. Monitoring systems don't have light mode.

### Palette

```css
/* Surfaces — layered depth, not flat */
--surface-0:    #08080C;   /* page background — near-void */
--surface-1:    #111118;   /* card background */
--surface-2:    #1A1A24;   /* elevated elements, hover states */
--surface-3:    #252530;   /* active states, borders */

/* Text */
--text-primary:    #E8E8F0;   /* high contrast, slightly cool */
--text-secondary:  #6E6E82;   /* muted, for labels and metadata */
--text-tertiary:   #3E3E50;   /* very muted, for disabled states */

/* Signal colors — these are the soul of the product */
--signal-green:    #00E599;   /* "Seems fine" — electric, not pastel */
--signal-yellow:   #FFB800;   /* "Acting up" — amber warning */
--signal-red:      #FF3B3B;   /* "It's dumb" — alarm red */

/* Chart */
--chart-bar:       #6E56CF;   /* default bar — muted purple */
--chart-bar-hot:   #FF3B3B;   /* bars above threshold */
--chart-grid:      #1A1A24;   /* grid lines, nearly invisible */

/* Button — the report button is special */
--button-danger:       #CC2222;   /* idle state — dark red, like a physical button */
--button-danger-hover: #FF3B3B;   /* hover — lights up, feels live */
--button-danger-active:#FF5252;   /* pressed — flash */
--button-disabled:     #2A2A35;   /* cooldown state */

/* Accent */
--accent:          #6E56CF;   /* purple — used sparingly for links and focus rings */
```

### Rules
- Never use pure white (#FFF) or pure black (#000)
- Signal colors are ONLY for status. Don't use red for non-status decorative elements
- Cards have a subtle 1px border using `--surface-3` — not border-less, not heavy
- No gradients on surfaces. Flat, layered, industrial
- The report button is the ONLY element with a glow/shadow effect (it demands attention)

---

## Layout

### Breakpoints

| Name | Width | Behavior |
|------|-------|----------|
| Mobile | < 640px | Single column, full-width cards, stacked layout |
| Split / Tablet | 640px – 1024px | Two-column grid, compact cards |
| Desktop | > 1024px | Two-column grid, generous spacing, max-width container |

### Container
- Max-width: 800px (narrow — this is a focused tool, not a dashboard)
- Horizontal padding: 16px mobile, 24px tablet, 32px desktop
- Centered with auto margins

### Spacing scale
```
--space-1: 4px
--space-2: 8px
--space-3: 12px
--space-4: 16px
--space-5: 24px
--space-6: 32px
--space-7: 48px
--space-8: 64px
```

### Grid
- Homepage cards: CSS Grid, `grid-template-columns: repeat(auto-fill, minmax(280px, 1fr))`
- Gap: `--space-4` (16px)
- This naturally gives 1 column on mobile, 2 on tablet/desktop, and works at any split-screen width

### Rules
- No sidebar. No hamburger menu. No navigation beyond the logo linking home
- Generous vertical spacing between sections (`--space-7` or `--space-8`)
- Cards have `--space-4` internal padding on mobile, `--space-5` on desktop
- The chart takes full container width on all breakpoints

---

## Components

### Model Card (homepage)

```
+-----------------------------------------------+
|  [status dot]  Model Name                      |
|               Provider                         |
|                                                |
|  ▃▁▅▂▇▃▁▄▂▆▃▁▅  (mini sparkline, 24 bars)    |
|                                                |
|  127 reports · past 24h            [arrow →]   |
+-----------------------------------------------+
```

- Status dot: 8px circle, signal color, with a subtle pulse animation when red
- Sparkline: 24 thin bars, max height ~20px, uses `--chart-bar` color, tallest bar uses signal color
- Hover: border color transitions to `--surface-3`, slight lift (translate -1px)
- The entire card is a link (no separate click target)

### Report Chart (model page)

- Recharts `BarChart`, 24 bars for 24 hours
- Bar color: `--chart-bar` by default, `--chart-bar-hot` when above red threshold
- Bar border-radius: 2px top corners only
- Grid: horizontal lines only, `--chart-grid` color, dashed
- X-axis: every 3rd hour labeled ("12a", "3a", "6a"...), `--text-secondary` color, Geist Mono
- Y-axis: hidden (the exact number doesn't matter, relative height does)
- Tooltip on hover: shows exact count + hour, dark background, monospace
- No animation on load (data should feel instant, like a live feed)

### Report Button

This is the hero interaction. It needs to feel physical and consequential.

```
+--------------------------------------------------+
|                                                    |
|           IT'S DUMB RIGHT NOW                      |
|                                                    |
+--------------------------------------------------+
```

- Full container width on mobile, max 400px on desktop, centered
- Height: 56px mobile, 64px desktop — tall enough to feel like a physical button
- Background: `--button-danger`
- Border: 1px solid with slightly lighter red
- Box-shadow: `0 0 20px rgba(255, 59, 59, 0.15)` — subtle red glow underneath
- Text: Geist Sans, 600 weight, uppercase, letter-spacing +0.05em
- Hover: background brightens to `--button-danger-hover`, glow intensifies
- Active/pressed: scale(0.98), instant — feels like pressing down
- After click: brief pulse animation, then transitions to disabled state
- Disabled (cooldown): `--button-disabled` background, text says "REPORTED ·  4:32 remaining", no glow

### Status Dot

- 8px diameter circle
- Color: `--signal-green`, `--signal-yellow`, or `--signal-red`
- Red status: CSS animation `pulse` — scales 1.0 → 1.4 → 1.0, opacity 1.0 → 0.6 → 1.0, 2s loop
- Green/yellow: static, no animation

### Header

```
[◆] DumbDetector
Is your AI dumb right now? You're not alone.
```

- Logo mark: a simple geometric shape (diamond/hexagon), `--accent` color
- "DumbDetector": Geist Sans, 800 weight, `--text-2xl`
- Tagline: Geist Sans, 400 weight, `--text-secondary`, `--text-sm`
- Vertically compact — this is a tool, not a marketing page
- On model pages: just the logo mark + "DumbDetector" as a home link, no tagline

---

## Motion & Interaction

### Philosophy
One moment of delight, not scattered fidgeting. The report button press IS the moment.

### Transitions
- All color transitions: 150ms ease
- Card hover lift: 200ms ease-out
- Button press scale: 100ms ease (snappy)
- Status dot pulse: 2s ease-in-out infinite (CSS only)

### Page load
- No staggered card animations (feels slow and gimmicky for a monitoring tool)
- Content appears instantly. Data-forward tools should feel fast

### Scrolling
- No scroll-triggered animations
- Sticky header only if the page is long enough to warrant it (probably not for MVP)

---

## Micro-copy & Voice

The UI speaks like a dry, deadpan systems engineer who finds the situation amusing but won't crack a smile.

| Context | Copy |
|---------|------|
| Green status | "Seems fine" |
| Yellow status | "Acting up" |
| Red status | "It's being dumb" |
| Button label | "IT'S DUMB RIGHT NOW" |
| After reporting | "Noted. 47 others agree." |
| Rate limited | "Already reported. Try again in 4:32." |
| No reports yet | "No reports. Suspicious." |
| Homepage tagline | "Is your AI dumb right now? You're not alone." |
| Footer | "Community-reported. Not affiliated with any AI provider." |

### Rules
- No exclamation marks (deadpan)
- No emoji in the UI
- Short sentences. Fragments are fine
- Numbers always in monospace font

---

## Responsive Behavior Summary

| Element | Mobile (<640px) | Split (640-1024px) | Desktop (>1024px) |
|---------|----------------|--------------------|--------------------|
| Card grid | 1 column | 2 columns | 2 columns |
| Card padding | 16px | 16px | 24px |
| Container padding | 16px | 24px | 32px |
| Chart height | 200px | 240px | 280px |
| Button width | 100% | 100% | max 400px, centered |
| Button height | 56px | 56px | 64px |
| Header tagline | shown | shown | shown |
| Sparkline | shown | shown | shown |

---

## What This Design Is NOT

- Not a startup landing page (no hero sections, no CTAs beyond the button)
- Not a meme site (no joke fonts, no emoji, no winking)
- Not a generic dashboard (no sidebar, no breadcrumbs, no filters)
- Not Downdetector with a different logo (our layout is narrower, simpler, more focused)
- Not using Inter, Roboto, or any system font stack