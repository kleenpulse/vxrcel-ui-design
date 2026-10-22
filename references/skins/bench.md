# Bench skin (Motion Garden)

A dark graphite instrument bench with one amethyst accent. Dark is the native identity; a light "lab bench" theme sits on the same roles. The shell stays chromatically quiet so the content (live previews, charts, media) supplies the colour.

Use it for docs shells, component catalogs, dev tools, control panels and data-dense dashboards.

## Identity in one line

`surface #0a0b0c, panel #101215, ink #e2e6e9, dim #bcc2c8, accent #a855f7. Space Mono display (uppercase, tracked), Geist body.`

## Fonts (`app/layout.tsx`)

```tsx
import { Geist, Geist_Mono, Space_Mono } from "next/font/google";

const geistSans = Geist({ variable: "--font-geist-sans", subsets: ["latin"] });
const geistMono = Geist_Mono({ variable: "--font-geist-mono", subsets: ["latin"] });
// Display + control readouts share one mono voice: the instrument signature.
const spaceMono = Space_Mono({ variable: "--font-space-mono", subsets: ["latin"], weight: ["400", "700"] });

// <html className={`${geistSans.variable} ${geistMono.variable} ${spaceMono.variable} antialiased`}>
// <body className="font-sans">
```

Theme: `next-themes` with `attribute="class"`, `defaultTheme="dark"`, `enableSystem={false}`. Dark is the intended first impression.

## Tokens (`app/globals.css`)

Paste as is. The semantic roles resolve to runtime `--sg-*` variables, re-exposed through `@theme inline` so a theme swap re-tints everything. Rename the `sg` prefix if you like, but keep it consistent.

```css
@import "tailwindcss";
@import "tw-animate-css";

@custom-variant dark (&:where(.dark, .dark *));

@theme {
	/* Graphite ramp: the raw palette, theme-independent */
	--color-bench-950: #0a0b0c;
	--color-bench-900: #0e1012;
	--color-bench-875: #101215;
	--color-bench-850: #131619;
	--color-bench-800: #191d21;
	--color-bench-750: #20242a;
	--color-bench-700: #2a2f36;
	--color-bench-600: #3a4048;
	--color-bench-500: #4e555e;
	--color-bench-400: #6c737c;
	--color-bench-300: #939aa2;
	--color-bench-200: #bcc2c8;
	--color-bench-100: #e2e6e9;
	--color-bench-50: #f3f5f6;

	/* Amethyst fixed to its dark value, for surfaces that stay graphite in both themes. */
	--color-amethyst: #a855f7;

	--font-display: var(--font-space-mono), ui-monospace, "SFMono-Regular", monospace;
	--font-sans: var(--font-geist-sans), ui-sans-serif, system-ui, sans-serif;
	--font-mono: var(--font-geist-mono), ui-monospace, "SFMono-Regular", monospace;

	/* Fluid hero display: a monospace unbreakable word can't be served by fixed steps. */
	--text-hero: clamp(1.75rem, 8.5vw, 6rem);

	/* Landing content column (`max-w-landing`). */
	--container-landing: 1900px;
}

@theme inline {
	--color-surface: var(--sg-surface);
	--color-panel: var(--sg-panel);
	--color-raised: var(--sg-raised);
	--color-hairline: var(--sg-hairline);
	--color-ink: var(--sg-ink);
	--color-ink-dim: var(--sg-ink-dim);
	--color-ink-mute: var(--sg-ink-mute);
	--color-accent: var(--sg-accent);
	--color-accent-hover: var(--sg-accent-hover);
	--color-accent-active: var(--sg-accent-active);
	--color-accent-muted: var(--sg-accent-muted);
	--color-on-accent: var(--sg-on-accent);
	--color-danger: var(--sg-danger);
}

/* Light: the lab bench under bright light. Every role mixes toward its dark
   value by --sg-dusk (0..1), so a page can tween from light into a graphite
   section instead of hitting it on a hard line. At 0 each mix is exactly the
   light literal. Ink flips on a steep curve around the midpoint. */
:root {
	--sg-dusk: 0;
	--sg-dusk-p: calc(var(--sg-dusk) * 100%);
	--sg-dusk-ink: calc(clamp(0, (var(--sg-dusk) - 0.4) * 5, 1) * 100%);
	--sg-surface: color-mix(in oklab, var(--color-bench-950) var(--sg-dusk-p), #f9f9fb);
	--sg-panel: color-mix(in oklab, var(--color-bench-875) var(--sg-dusk-p), #ffffff);
	--sg-raised: color-mix(in oklab, var(--color-bench-800) var(--sg-dusk-p), #eceaef);
	--sg-hairline: color-mix(in oklab, var(--color-bench-700) var(--sg-dusk-p), #dde2e7);
	--sg-ink: color-mix(in oklab, var(--color-bench-100) var(--sg-dusk-ink), #16191d);
	--sg-ink-dim: color-mix(in oklab, var(--color-bench-200) var(--sg-dusk-ink), #545b64);
	--sg-ink-mute: color-mix(in oklab, var(--color-bench-400) var(--sg-dusk-ink), #878e97);
	--sg-accent: color-mix(in oklab, #a855f7 var(--sg-dusk-p), #7e22ce);
	--sg-accent-hover: color-mix(in oklab, #b87df9 var(--sg-dusk-p), #6b21a8);
	--sg-accent-active: color-mix(in oklab, #9333ea var(--sg-dusk-p), #581c87);
	--sg-accent-muted: color-mix(in oklab, #5b3a7a var(--sg-dusk-p), #d8b4fe);
	--sg-on-accent: color-mix(in oklab, var(--color-bench-950) var(--sg-dusk-ink), #ffffff);
	--sg-danger: color-mix(in oklab, #f87171 var(--sg-dusk-p), #dc2626);
	--sg-grid: color-mix(
		in oklab,
		color-mix(in oklab, var(--color-bench-600) 6%, transparent) var(--sg-dusk-p),
		color-mix(in oklab, var(--color-bench-500) 3%, transparent)
	);
	--sg-scroll: color-mix(in oklab, #464d56 var(--sg-dusk-p), #c3c9cf);
	color-scheme: light;
}

/* Dark: the native graphite instrument. */
.dark {
	--sg-surface: var(--color-bench-950);
	--sg-panel: var(--color-bench-875);
	--sg-raised: var(--color-bench-800);
	--sg-hairline: var(--color-bench-700);
	--sg-ink: var(--color-bench-100);
	--sg-ink-dim: var(--color-bench-200);
	--sg-ink-mute: var(--color-bench-400);
	--sg-accent: #a855f7;
	--sg-accent-hover: #b87df9;
	--sg-accent-active: #9333ea;
	--sg-accent-muted: #5b3a7a;
	--sg-on-accent: var(--color-bench-950);
	--sg-danger: #f87171;
	--sg-grid: color-mix(in oklab, var(--color-bench-600) 6%, transparent);
	--sg-scroll: #464d56;
	color-scheme: dark;
}

@layer base {
	body {
		background-color: var(--color-surface);
		color: var(--color-ink);
		font-family: var(--font-sans);
		/* Faint measurement grid, faded out by ~40% of the viewport. */
		background-image:
			linear-gradient(to bottom, transparent 15%, var(--color-surface) 40%),
			linear-gradient(to right, var(--sg-grid) 1px, transparent 1px),
			linear-gradient(to bottom, var(--sg-grid) 1px, transparent 1px);
		background-size: 100% 100%, 50px 50px, 50px 50px;
		@media (min-width: 640px) {
			background-size: 100% 100%, 114px 104px, 114px 104px;
		}
		background-attachment: fixed;
	}

	::selection {
		background-color: color-mix(in oklab, var(--sg-accent) 26%, transparent);
		color: var(--color-ink);
	}

	/* Accent focus ring on every keyboard-focused control */
	:focus-visible {
		outline: 2px solid var(--sg-accent);
		outline-offset: 2px;
		border-radius: 2px;
	}

	html {
		scrollbar-width: thin;
		scrollbar-color: var(--sg-scroll) transparent;
	}
	::-webkit-scrollbar { width: 4px; height: 4px; }
	::-webkit-scrollbar-thumb { background: var(--sg-scroll); border-radius: 999px; }
	::-webkit-scrollbar-track { background: transparent; }
}

@utility scrollbar-thin { scrollbar-width: thin; }
@utility scrollbar-none {
	scrollbar-width: none;
	&::-webkit-scrollbar { display: none; }
}

@media (prefers-reduced-motion: reduce) {
	*, *::before, *::after {
		animation-duration: 0.01ms !important;
		animation-iteration-count: 1 !important;
		transition-duration: 0.01ms !important;
		scroll-behavior: auto !important;
	}
}
```

## Role cheat sheet

| Role | Light | Dark |
|---|---|---|
| `surface` | `#f9f9fb` | `#0a0b0c` |
| `panel` | `#ffffff` | `#101215` |
| `raised` | `#eceaef` | `#191d21` |
| `hairline` | `#dde2e7` | `#2a2f36` |
| `ink` | `#16191d` | `#e2e6e9` |
| `ink-dim` | `#545b64` | `#bcc2c8` |
| `ink-mute` | `#878e97` | `#6c737c` |
| `accent` | `#7e22ce` | `#a855f7` |
| `accent-hover` | `#6b21a8` | `#b87df9` |
| `accent-active` | `#581c87` | `#9333ea` |
| `accent-muted` | `#d8b4fe` | `#5b3a7a` |
| `on-accent` | `#ffffff` | `#0a0b0c` |
| `danger` | `#dc2626` | `#f87171` |

## Shape and depth

- **Radii are Tailwind defaults, squared:** `rounded-md` for controls, buttons and tab pills; `rounded-lg` for panels, cards and tab tracks; `rounded-sm` for sidebar rows; `rounded-full` only for switches and chips. The slider thumb is `3px`, squarish on purpose.
- **Borderless.** Panels separate by tonal steps (`bg-panel` on `bg-surface`, header strips `bg-raised/30`). A 1px `border-hairline` appears only on floating layers (select, tooltip, popover, dialog), decorative rules, and small chips.
- **Shadows only on floating layers:** `shadow-lg shadow-black/25` (select, tooltip, popover), `shadow-xl shadow-black/30` (preview card), `shadow-2xl` (modals).
- **Glass, sparingly:** sticky bars `bg-panel/80 backdrop-blur`; popovers `bg-panel/95 backdrop-blur-2xl`; modal scrim `bg-white/40 dark:bg-black/50 backdrop-blur-xs`.
- **Density is compact:** controls `h-8` (`h-7` on mobile), rows `py-1.5`, microtype 10-11px. Main content `px-3 py-6 lg:px-8 lg:py-10`.

## Typography rules

- `font-display` (Space Mono) is almost always **uppercase with wide tracking**: headings, eyebrows, tab labels, badges, readouts, breadcrumbs.
  - Catalog H1: `font-display text-3xl uppercase tracking-[0.08em]`
  - Detail H1: `font-display text-2xl uppercase tracking-[0.08em]`
  - Landing H2: `font-display text-3xl sm:text-4xl uppercase tracking-[0.04em]`
  - Hero H1: `font-display text-hero uppercase leading-[0.95] tracking-[0.02em]`
  - Eyebrow / section label: `font-display text-[11px] uppercase tracking-[0.2em] text-ink-mute`; hero eyebrow `tracking-[0.34em] text-accent`
- `font-sans` (Geist): body and descriptions, `leading-relaxed`.
- `font-mono` (Geist Mono): code, control labels, inputs, kbd, small text actions (`font-mono text-[11px] text-ink-mute hover:text-accent`).
- Readout numbers: `font-display text-xs tabular-nums text-accent`.
- Greek or other non-Latin glyphs inside uppercase headings get `normal-case` (an uppercase alpha reads as a Latin A).

## Signature moves

**Corner-bracket reticle** on the selected row, the instrument selection motif. It rides a `layoutId` fill:

```tsx
{isActive && (
  <motion.span
    layoutId="sidebar-active"
    className="pointer-events-none absolute inset-0 z-0 rounded-sm bg-accent/10"
    transition={TAB_SPRING}
  >
    <span className="absolute -start-px -top-px size-2.5 border-s border-t border-accent" />
    <span className="absolute -end-px -top-px size-2.5 border-e border-t border-accent" />
    <span className="absolute -bottom-px -start-px size-2.5 border-b border-s border-accent" />
    <span className="absolute -bottom-px -end-px size-2.5 border-b border-e border-accent" />
  </motion.span>
)}
```

Active row text: `font-medium text-accent`. Idle: `text-ink-dim hover:bg-raised/40 hover:text-ink`.

**Specimen plate**, an instrument-log label between hairline rules (`EXHIBIT α-01 · BACKGROUNDS · 7 UNITS`):

```tsx
<div className="flex items-baseline gap-3 whitespace-nowrap border-y border-hairline py-2 font-display text-[10px] uppercase tracking-[0.28em] text-ink-mute">
  <span className="text-accent">Exhibit <span className="normal-case">{greek}</span>-{index}</span>
  <span aria-hidden>·</span>
  <span className="text-ink-dim">{label}</span>
  <span aria-hidden>·</span>
  <span>{meta}</span>
</div>
```

Sections are numbered with Greek letters (α, β, γ ...).

**The detented slider** with a squarish amethyst thumb, paired with a mono LED-style readout:

```css
@layer components {
	.bench-range {
		-webkit-appearance: none;
		appearance: none;
		height: 4px;
		border-radius: 999px;
		background: var(--sg-hairline);
		cursor: pointer;
	}
	.bench-range::-webkit-slider-thumb {
		-webkit-appearance: none;
		appearance: none;
		height: 14px;
		width: 14px;
		border-radius: 3px;
		background: var(--sg-accent);
		border: 1px solid var(--sg-accent-active);
		box-shadow: 0 0 0 3px color-mix(in oklab, var(--sg-accent) 16%, transparent);
		transition: transform 0.08s ease;
	}
	.bench-range::-webkit-slider-thumb:hover { transform: scale(1.12); }
	.bench-range::-moz-range-thumb {
		height: 14px;
		width: 14px;
		border-radius: 3px;
		background: var(--sg-accent);
		border: 1px solid var(--sg-accent-active);
	}
	.bench-range::-moz-range-track {
		height: 4px;
		border-radius: 999px;
		background: var(--sg-hairline);
	}
}
```

**The Grainient**, the one sanctioned multi-colour gradient. Used for the "New" badge, the route progress bar and glow borders. On dark grounds swap the middle stop to `#a78bfa` (the `#6d28d9` stop drops to 2.9:1 on black).

```css
.text-grainient {
	background: linear-gradient(120deg, #f754f1 0%, #6d28d9 50%, #22d3ee 100%);
	-webkit-background-clip: text;
	background-clip: text;
	color: transparent;
}
.text-grainient-bright {
	background: linear-gradient(120deg, #f754f1 0%, #a78bfa 50%, #22d3ee 100%);
	-webkit-background-clip: text;
	background-clip: text;
	color: transparent;
}
```

Pointer-proximity card glow colours: `["#a855f7", "#6d28d9", "#22d3ee"]`, glow colour `"271 91 65"` (HSL triplet of `#a855f7`), kept quiet on chrome (`glowIntensity: 0.6`, `fillOpacity: 0.25`).

**Self-inverting hero headline:** `text-white mix-blend-difference` over a WebGL or media backdrop, with a `from-surface via-surface/85` scrim underneath.

**Living wordmark:** the product name in Space Mono, first word `text-ink`, second word `text-accent`, with an occasional ink-to-accent shimmer sweep.

## Graphite in both themes (the one sanctioned exception)

Some surfaces stay graphite in light mode too, and may use `bench-*` and `--color-amethyst` directly:

- **Code blocks:** Shiki highlights server-side with a dark theme (`vesper`), so the code surface is `background: var(--color-bench-950)` in both themes. `pre.shiki { font-size: 12px; line-height: 1.6; padding: 1rem }`.
- **Exhibit plates and archive plates:** dark-only visuals (WebGL rays, glow) need a dark ground.

Everywhere else, never hardcode `bench-950`, white or black on a themed surface: it breaks light mode.

## Do / don't

- Do use `bg-accent/10`-`/15` for selected tints, a solid `bg-accent` + `text-on-accent` for the active pill, `text-accent` for the active label.
- Do keep hover colour-only in chrome: `text-ink-dim -> text-ink` or `-> text-accent`, backgrounds `-> bg-raised/40`. No lift or scale in shell chrome.
- Do use `opacity-40` for disabled controls.
- Don't run `shadcn init`: it injects a rival `--color-accent` and a duplicate dark variant. Vendor primitives into `components/ui/` and restyle them (`bg-panel`, `border-hairline`, `text-ink`, `focus:bg-accent/15`).
- Don't add a "Free" badge or any second accent hue to chrome. Content (previews, charts) may be colourful; the shell may not.
- Don't use cva in the shell; plain `SIZE_CLASSES` / `VARIANTS` objects merged with `cn` (clsx + tailwind-merge) are enough.
