# Nebula skin (Raven dark)

Deep-teal lit glass, with gold signal light and a cyan secondary. Cards are semi-opaque fills floating over a fixed radial atmosphere; real blur is kept for overlays. Same round shapes and type as Ember.

Nebula is the `.dark` half of the Ember/Nebula pair. It needs the shared Raven layer from [ember.md](ember.md) (bridge, type scale, base, utilities). For a dark-only app, install the shared layer and put these values on `:root` as well as `.dark`.

## Nebula tokens (`.dark`)

```css
.dark {
	--band: transparent;
	--sheet: rgb(8 18 19 / 0.48);
	--card-nested: rgb(234 242 241 / 0.045);
	--ink-2: #9fb3b1;
	--ink-3: #5f7573;

	--signal: #e6c27a;
	--signal-ink: #e6c27a;
	--signal-soft: rgb(230 194 122 / 0.14);
	--signal-2: #5ee1d9;
	--inverse: #eaf2f1;
	--inverse-foreground: #0b1616;
	--glass: rgb(13 30 31 / 0.8);
	--glass-thin: rgb(13 30 31 / 0.52);
	--glass-border: rgb(160 220 215 / 0.14);
	--glass-frost: rgb(13 30 31 / 0.46);
	--glass-sheen: rgb(160 220 215 / 0.13);
	--glass-edge: rgb(160 220 215 / 0.24);
	--glass-tint: rgb(230 194 122 / 0.16);
	--glow: rgb(94 225 217 / 0.35);
	--critical: #f0604d;
	--orb-1: #e6c27a;
	--orb-2: #a99bff;
	--orb-3: #f08bb0;
	--orb-4: #5fd3a6;
	--warn: #f2b35b;
	--ok: #5fd3a6;
	--em-scroll: #2a3d3d;

	--shadow-card-value: inset 0 1px 0 rgb(255 255 255 / 0.05), 0 24px 60px -32px rgb(0 0 0 / 0.75);
	--shadow-lift-value: inset 0 1px 0 rgb(255 255 255 / 0.07), 0 26px 50px -24px rgb(0 0 0 / 0.85);
	--shadow-glow-value: 0 0 0 1px rgb(230 194 122 / 0.5), 0 0 34px -6px rgb(230 194 122 / 0.45);

	--background: #050a0b;
	--foreground: #eaf2f1;
	/* Semi-opaque fill instead of backdrop-filter: blur over a static gradient is invisible. */
	--card: rgb(20 44 44 / 0.5);
	--card-foreground: #eaf2f1;
	--popover: #0d1d1e;
	--popover-foreground: #eaf2f1;
	--primary: #e6c27a;
	--primary-foreground: #1a1408;
	--secondary: rgb(234 242 241 / 0.07);
	--secondary-foreground: #eaf2f1;
	--muted: rgb(234 242 241 / 0.05);
	--muted-foreground: #9fb3b1;
	--accent: rgb(94 225 217 / 0.09);
	--accent-foreground: #eaf2f1;
	--destructive: #f0604d;
	--border: rgb(160 220 215 / 0.1);
	--input: rgb(160 220 215 / 0.15);
	--ring: #5ee1d9;
	--chart-1: #e6c27a;
	--chart-2: #5ee1d9;
	--chart-3: #a99bff;
	--chart-4: #f08bb0;
	--chart-5: #5fd3a6;
	--sidebar: rgb(6 13 14 / 0.55);
	--sidebar-foreground: #eaf2f1;
	--sidebar-primary: #e6c27a;
	--sidebar-primary-foreground: #1a1408;
	--sidebar-accent: rgb(94 225 217 / 0.1);
	--sidebar-accent-foreground: #eaf2f1;
	--sidebar-border: rgb(160 220 215 / 0.08);
	--sidebar-ring: #5ee1d9;
	color-scheme: dark;
}
```

## Atmosphere

Three fixed radial gradients and an inset vignette. Cards read as glass because they are translucent over this, not because they blur.

```css
@layer base {
	.dark body {
		background:
			radial-gradient(1100px 760px at 72% -12%, #1e423c 0%, transparent 62%),
			radial-gradient(900px 700px at -6% 108%, #10302c 0%, transparent 58%),
			radial-gradient(900px 620px at 52% 42%, rgb(94 225 217 / 0.07), transparent 70%),
			#050a0b;
		background-attachment: fixed;
	}
	.dark body::before {
		content: "";
		position: fixed;
		inset: 0;
		pointer-events: none;
		box-shadow: inset 0 0 220px rgb(0 0 0 / 0.55);
		z-index: -1; /* below content, above the canvas gradient */
	}
}
```

Panes on this ground: `bg-sheet` with `dark:border-t dark:border-glass-border` and the 40px frame corner. Bento cards get a border and a 160deg top-left sheen automatically through the `bento` utility.

## Glass recipes (overlays only)

These read Ember values in light mode, so they serve both skins.

```css
/* Real frosted glass: tray, panels, tooltips, mobile tab bar. */
@utility glass {
	background: var(--glass);
	border: 1px solid var(--glass-border);
	/* Standard property only: a trailing -webkit- line can win and leave Chrome without blur. */
	backdrop-filter: blur(20px) saturate(125%);
}

/* Lighter glass for cards floating over a busy visual. */
@utility glass-thin {
	background: var(--glass-thin);
	border: 1px solid var(--glass-border);
	backdrop-filter: blur(10px) saturate(140%);
}

/* Rich frost for small floating controls: thinner fill, heavier blur,
   a top sheen and a faint signal glow at the leading edge. */
@utility glass-frost {
	background:
		linear-gradient(180deg, var(--glass-sheen), transparent 55%),
		radial-gradient(90% 160% at 0% 50%, var(--glass-tint), transparent 70%),
		var(--glass-frost);
	border: 1px solid var(--glass-border);
	backdrop-filter: blur(24px) saturate(180%);
}
```

Pair `glass-frost` with `inset-shadow-[0_1px_0_var(--glass-edge)]` for a lit top edge.

## Attention: the glow sweep

A rotating conic border in `--critical` for things that need the user: a failed sync, an expired connection.

```css
@property --sweep-angle {
	syntax: "<angle>";
	inherits: false;
	initial-value: 0deg;
}

@utility glow-sweep {
	border: 1px solid transparent;
	background:
		linear-gradient(var(--card-sweep-fill, var(--popover)), var(--card-sweep-fill, var(--popover))) padding-box,
		conic-gradient(from var(--sweep-angle), transparent 0 62%, var(--critical) 80%, transparent 92%) border-box;
	animation: var(--animate-glow-sweep);
}
```

## Reduced transparency (required)

Both the media query (Chromium only) and an in-app switch swap glass for solids.

```css
@media (prefers-reduced-transparency: reduce) {
	.dark {
		--sheet: #081213;
		--card: #102425;
		--glass: #0d1d1e;
		--glass-thin: #0d1d1e;
		--glass-frost: #0d1d1e;
		--glass-sheen: transparent;
		--sidebar: #060d0e;
	}
	:root:not(.dark) {
		--glass: #ffffff;
		--glass-thin: #ffffff;
		--glass-frost: #ffffff;
		--glass-sheen: transparent;
	}
}
html[data-transparency="reduce"].dark {
	--sheet: #081213;
	--card: #102425;
	--glass: #0d1d1e;
	--glass-thin: #0d1d1e;
	--glass-frost: #0d1d1e;
	--glass-sheen: transparent;
	--sidebar: #060d0e;
}
html[data-transparency="reduce"]:not(.dark) {
	--glass: #ffffff;
	--glass-thin: #ffffff;
	--glass-frost: #ffffff;
	--glass-sheen: transparent;
}
```

## Rules specific to Nebula

- **Gold is the signal, cyan is the instrument light.** Gold (`signal`, `primary`) marks active, unread, starred and the primary action. Cyan (`signal-2`, `ring`) is focus rings and secondary data series. Never make a cyan CTA.
- **The active pill is a gold tint, not solid:** `dark:bg-signal-soft dark:text-signal`, and `dark:shadow-glow` on the one nav item that is active.
- **Lit glass, not flat:** every card has the inset top highlight (`inset 0 1px 0 rgb(255 255 255 / 0.05)`) plus a deep soft drop shadow. Do not add extra borders; use `ring-1 ring-glass-border`.
- **No blur on static cards.** Blur over a fixed gradient is invisible and costs GPU. Semi-opaque fills only; blur is for overlays that float over moving content.
- **Hero glow:** gold display type may carry `text-shadow: 0 0 48px rgb(230 194 122 / 0.35)`; the brand dot may carry `shadow-[0_0_10px_var(--signal)]`. Nowhere else.
- **Sender and category tints come from the orbs** (`dark:bg-signal-2/15 dark:text-signal-2`, `dark:bg-orb-2/20 dark:text-orb-2`, `dark:bg-orb-3/15 dark:text-orb-3`, `dark:bg-orb-4/15 dark:text-orb-4`), never from gold.
- **Third-party HTML** (rendered email, embeds) keeps a paper look (`#f4f2ee`) inside Nebula rather than being force-darkened.
