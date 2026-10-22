# Ember skin (Raven light) + the shared Raven layer

Warm paper bento with a coral signal. Flat: depth comes from stepped greys, not shadows. Everything is round: pills, circles, 20-28px cards, 40px frame corners.

Ember is the light half of a pair; [Nebula](nebula.md) is the dark half. Both run on the shared layer in this file: the `@theme inline` bridge, the type and motion scale, and the utilities. Install this file first, then add the Nebula `.dark` block.

Use it for consumer and productivity apps: mail, calendars, notes, CRMs, finance dashboards.

## Fonts (`app/layout.tsx`)

```tsx
import { Geist, Geist_Mono, Instrument_Sans } from "next/font/google";

const geistSans = Geist({ variable: "--font-geist-sans", subsets: ["latin"] });
const geistMono = Geist_Mono({ variable: "--font-geist-mono", subsets: ["latin"] });
const instrumentSans = Instrument_Sans({ variable: "--font-instrument-sans", subsets: ["latin"], display: "swap" });

// <html className={`${geistSans.variable} ${geistMono.variable} ${instrumentSans.variable} h-full antialiased`}>
// <body className="min-h-full flex flex-col font-sans">
```

`next-themes` with `attribute="class"`. System preference is allowed. A marketing route may force Nebula.

shadcn primitives: style `base-nova` on `@base-ui/react`, restyled with the extra variants in [components.md](../components.md). If you merge classes with a `cn` that understands Tailwind scales, register the custom ones (`radius: card, frame, pill`, `shadow: card, glow, lift`, `text: hero, kpi, kpi-sm, micro`) or `text-kpi` is mistaken for a colour and dropped.

## Shared Raven layer (`app/globals.css`, top)

```css
@import "tailwindcss";
@import "tw-animate-css";
@import "shadcn/tailwind.css";

@custom-variant dark (&:where(.dark, .dark *));

/*
 * Two skins, one token vocabulary.
 *   Ember  (:root)  - warm paper bento, coral signal.
 *   Nebula (.dark)  - deep-teal glass, gold + cyan light.
 * Components only ever reference tokens; a new skin is one more block.
 */

@theme inline {
	--color-background: var(--background);
	--color-foreground: var(--foreground);
	--font-sans: var(--font-geist-sans), ui-sans-serif, system-ui, sans-serif;
	--font-mono: var(--font-geist-mono), ui-monospace, monospace;
	--font-display: var(--font-instrument-sans), var(--font-geist-sans), ui-sans-serif, sans-serif;
	--font-heading: var(--font-display);
	--color-sidebar-ring: var(--sidebar-ring);
	--color-sidebar-border: var(--sidebar-border);
	--color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
	--color-sidebar-accent: var(--sidebar-accent);
	--color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
	--color-sidebar-primary: var(--sidebar-primary);
	--color-sidebar-foreground: var(--sidebar-foreground);
	--color-sidebar: var(--sidebar);
	--color-chart-5: var(--chart-5);
	--color-chart-4: var(--chart-4);
	--color-chart-3: var(--chart-3);
	--color-chart-2: var(--chart-2);
	--color-chart-1: var(--chart-1);
	--color-ring: var(--ring);
	--color-input: var(--input);
	--color-border: var(--border);
	--color-destructive: var(--destructive);
	--color-accent-foreground: var(--accent-foreground);
	--color-accent: var(--accent);
	--color-muted-foreground: var(--muted-foreground);
	--color-muted: var(--muted);
	--color-secondary-foreground: var(--secondary-foreground);
	--color-secondary: var(--secondary);
	--color-primary-foreground: var(--primary-foreground);
	--color-primary: var(--primary);
	--color-popover-foreground: var(--popover-foreground);
	--color-popover: var(--popover);
	--color-card-foreground: var(--card-foreground);
	--color-card: var(--card);

	/* skin surfaces + signal colors */
	--color-band: var(--band);
	--color-sheet: var(--sheet);
	--color-card-nested: var(--card-nested);
	--color-ink: var(--foreground);
	--color-ink-2: var(--ink-2);
	--color-ink-3: var(--ink-3);
	--color-signal: var(--signal);
	--color-signal-ink: var(--signal-ink);
	--color-signal-soft: var(--signal-soft);
	--color-signal-2: var(--signal-2);
	--color-inverse: var(--inverse);
	--color-inverse-foreground: var(--inverse-foreground);
	--color-glass: var(--glass);
	--color-glass-border: var(--glass-border);
	--color-glow: var(--glow);
	--color-critical: var(--critical);
	--color-orb-1: var(--orb-1);
	--color-orb-2: var(--orb-2);
	--color-orb-3: var(--orb-3);
	--color-orb-4: var(--orb-4);
	--color-warn: var(--warn);
	--color-ok: var(--ok);
	--color-em-scroll: var(--em-scroll);

	--radius-sm: calc(var(--radius) * 0.6);
	--radius-md: calc(var(--radius) * 0.8);
	--radius-lg: var(--radius);
	--radius-xl: calc(var(--radius) * 1.4);
	--radius-2xl: calc(var(--radius) * 1.8);
	--radius-3xl: calc(var(--radius) * 2.2);
	--radius-4xl: calc(var(--radius) * 2.6);
	--radius-card: var(--radius-card-size);
	--radius-frame: var(--radius-frame-size);
	--radius-pill: 9999px;

	--shadow-card: var(--shadow-card-value);
	--shadow-lift: var(--shadow-lift-value);
	--shadow-glow: var(--shadow-glow-value);
}

@theme {
	--text-hero: 2.75rem;
	--text-hero--line-height: 1.04;
	--text-hero--letter-spacing: -0.025em;
	--text-kpi: 2.5rem;
	--text-kpi--line-height: 1;
	--text-kpi--letter-spacing: -0.03em;
	--text-kpi-sm: 1.625rem;
	--text-kpi-sm--line-height: 1.05;
	--text-kpi-sm--letter-spacing: -0.02em;
	--text-micro: 0.6875rem;
	--text-micro--line-height: 1.2;

	--ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);

	--animate-rise-in: rise-in 320ms var(--ease-out-quint) backwards;
	--animate-bloom: bloom 700ms var(--ease-out-quint) both;
	--animate-pulse-once: pulse-once 900ms var(--ease-out-quint) 1 both;
	--animate-glow-sweep: glow-sweep 3.2s linear infinite;
	/* keyframes: see motion.md */
}
```

## Ember tokens (`:root`)

```css
:root {
	--radius: 0.875rem;
	--radius-card-size: 28px;
	--radius-frame-size: 40px;

	--band: #f3f3f1;
	--sheet: #ffffff;
	--card-nested: #ffffff;
	--ink-2: #6b6b6b;
	--ink-3: #a3a3a3;

	/* coral: fills, marks, big numerals. Text uses --signal-ink (>=5:1). */
	--signal: #e8603c;
	--signal-ink: #b4461f;
	--signal-soft: #fbe3da;
	--signal-2: #111111;
	--inverse: #111111;
	--inverse-foreground: #ffffff;
	--glass: rgb(255 255 255 / 0.86);
	--glass-thin: rgb(255 255 255 / 0.55);
	--glass-border: rgb(17 17 17 / 0.07);
	--glass-frost: rgb(255 255 255 / 0.48);
	--glass-sheen: rgb(255 255 255 / 0.75);
	--glass-edge: rgb(255 255 255 / 0.95);
	--glass-tint: rgb(232 96 60 / 0.16);
	--glow: rgb(232 96 60 / 0.32);
	--critical: #d93b2b;
	--orb-1: #e8603c;
	--orb-2: #111111;
	--orb-3: #f4b59f;
	--orb-4: #9a9a95;
	--warn: #b76a00;
	--ok: #2f8f5b;
	--em-scroll: #cfcfca;

	--shadow-card-value: none;
	--shadow-lift-value: 0 14px 34px -16px rgb(17 17 17 / 0.22);
	--shadow-glow-value: 0 0 0 1px rgb(232 96 60 / 0.28), 0 10px 30px -10px rgb(232 96 60 / 0.5);

	--background: #f3f3f1;
	--foreground: #111111;
	--card: #f6f6f4;
	--card-foreground: #111111;
	--popover: #ffffff;
	--popover-foreground: #111111;
	/* burnt coral keeps white labels at 4.6:1 */
	--primary: #c84e2a;
	--primary-foreground: #ffffff;
	--secondary: #ecece8;
	--secondary-foreground: #111111;
	--muted: #efefec;
	--muted-foreground: #6b6b6b;
	--accent: #ecece8;
	--accent-foreground: #111111;
	--destructive: #d93b2b;
	--border: #e6e6e1;
	--input: #e2e2dd;
	--ring: #c84e2a;
	--chart-1: #e8603c;
	--chart-2: #111111;
	--chart-3: #f4b59f;
	--chart-4: #9a9a95;
	--chart-5: #f9d9ce;
	--sidebar: #f3f3f1;
	--sidebar-foreground: #111111;
	--sidebar-primary: #111111;
	--sidebar-primary-foreground: #ffffff;
	--sidebar-accent: #ffffff;
	--sidebar-accent-foreground: #111111;
	--sidebar-border: #e6e6e1;
	--sidebar-ring: #c84e2a;
	color-scheme: light;
}
```

Note the shadcn `--accent` here is a neutral hover grey, not the signal. The signal is `--signal`; the primary button colour is the darker `--primary` so white labels pass contrast.

## Shared base and utilities

```css
@layer base {
	* {
		@apply border-border outline-ring/50;
		scrollbar-width: thin;
		scrollbar-color: var(--em-scroll) transparent;
	}
	::-webkit-scrollbar { width: 6px; height: 6px; }
	::-webkit-scrollbar-track { background: transparent; }
	::-webkit-scrollbar-thumb {
		background: color-mix(in oklab, var(--em-scroll) 70%, transparent);
		border-radius: 9999px;
	}
	html {
		@apply font-sans;
		height: 100%;
		font-feature-settings: "cv11", "ss01";
	}
	body {
		@apply bg-background text-foreground;
		height: 100%;
	}
	::selection { background: color-mix(in oklab, var(--signal) 30%, transparent); }
	:focus-visible { outline-offset: 2px; }
}

@utility font-display { font-family: var(--font-display); }

/* Mono uppercase microtype: section labels, metadata keys, provider lines. */
@utility readout {
	font-family: var(--font-mono);
	font-size: 0.6875rem;
	letter-spacing: 0.06em;
	text-transform: uppercase;
	color: var(--muted-foreground);
}

@utility tabular {
	font-variant-numeric: tabular-nums;
	font-feature-settings: "tnum";
}

/* Bento card: flat paper in Ember, lit glass in Nebula. */
@utility bento {
	border-radius: var(--radius-card);
	background: var(--card);
	color: var(--card-foreground);
	box-shadow: var(--shadow-card);
	border: 1px solid transparent;
	.dark & {
		border-color: var(--border);
		background-image: linear-gradient(160deg, rgb(255 255 255 / 0.035), transparent 42%);
	}
}

/* Hover lift only for real pointers; press settles. */
@utility lift {
	transition:
		transform 180ms var(--ease-out-quint),
		box-shadow 180ms var(--ease-out-quint);
	@media (hover: hover) and (pointer: fine) {
		&:hover {
			transform: translateY(-2px);
			box-shadow: var(--shadow-lift);
		}
	}
	&:active { transform: scale(0.985); }
}
```

The glass utilities (`glass`, `glass-thin`, `glass-frost`) and `glow-sweep` are in [nebula.md](nebula.md). They read Ember values in light mode, so install them for both skins.

### Density and interface size

```css
/* Row rhythm for lists (Mail Density setting). */
html { --row-py: 0.75rem; --row-py-wide: 0.625rem; --row-gap: 0.375rem; }
html[data-density="compact"] { --row-py: 0.4375rem; --row-py-wide: 0.3125rem; --row-gap: 0.125rem; }

/* Interface size: the app frame zooms, portaled popups stay outside the zoom. */
html { --ui-zoom: 1; }
html[data-ui-size="large"] { --ui-zoom: 1.08; }
html[data-ui-size="larger"] { --ui-zoom: 1.16; }
html[data-ui-size] [data-ui-zoom] { zoom: var(--ui-zoom); }
```

The app frame then uses `h-[calc(100svh/var(--ui-zoom,1))]`.

## How Ember gets depth

Flat paper, no card shadows (`--shadow-card-value: none`). Depth is a staircase of greys:

`band #f3f3f1` (page, rail) -> `sheet #ffffff` (pane, 40px top corner) -> `card #f6f6f4` (row, bento) -> `card-nested #ffffff` (tile inside a card)

- Panes: `rounded-t-frame bg-sheet` or `rounded-tl-frame bg-sheet` sitting on the `bg-band` canvas.
- List rows: `rounded-[20px] bg-card`, hover `bg-card-nested shadow-lift`.
- KPI tiles: `rounded-[22px] bg-card-nested p-4` inside a `bento p-5`.
- `shadow-lift` appears only on hover and on floating docks (compose window, mobile tab bar).
- `shadow-glow` (coral ring + bloom) marks the one thing that is "on": the Ask button on hover, the active rail item in Nebula.

## Typography

- Display (`font-display`, Instrument Sans), tight and negative:
  - Pane title: `font-display text-[22px] leading-none tracking-[-0.01em]`
  - Reader subject: `font-display text-[22px] leading-[1.15] tracking-[-0.02em] text-balance @xl/reader:text-[28px]`
  - Card title: `font-display text-[17px] leading-tight tracking-[-0.01em]`
  - Empty state title: `font-display text-xl leading-tight tracking-[-0.01em]`
  - KPI numeral: `font-display text-kpi-sm tabular` (or `text-kpi`)
  - Landing hero: `text-[clamp(3.25rem,10vw,7.75rem)] leading-[0.94] tracking-[-0.045em]`
- Body is dense: `text-[14px] leading-5` (row sender), `text-[13.5px]` (subject, sidebar rows), `text-[12.5px]` (preview), `text-[13px]` (secondary copy). Lede: `text-[17px] leading-relaxed text-pretty text-ink-2`.
- Mono for data: dates `font-mono text-[11px] tracking-wide tabular`, chips `font-mono text-[10.5px] tracking-[0.08em] uppercase`.
- Labels: the `readout` utility ("Folders", "Labels", metadata `dt`s, "Autosaves on this device").

## Rules specific to Ember

- **Coral is for fills, marks and big numerals.** Coral text uses `text-signal-ink`. White on coral uses `bg-primary` (`#c84e2a`), never `bg-signal`.
- **The active pill is ink, not coral:** `bg-inverse text-inverse-foreground`. Coral tint (`bg-signal-soft ring-1 ring-signal/30`) marks the active list row.
- **Sender/category tints never use the signal.** Use `bg-foreground/[0.06]`, `bg-orb-4/25`, `bg-orb-3/30`, `bg-sheet ring-1 ring-border`.
- **Round, always.** No square corners and no corner brackets: those belong to square skins.
- **Borders are rings:** prefer `ring-1 ring-border` (and `dark:ring-glass-border`) over `border` on cards and pills.
