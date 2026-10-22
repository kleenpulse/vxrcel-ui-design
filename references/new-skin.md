# Making a new skin

A new skin is one `:root` block and one `.dark` block that fill every role. Components don't change. Use the Raven vocabulary (from [skins/ember.md](skins/ember.md)) for new skins: it is a superset that stays compatible with shadcn primitives. Use the Bench vocabulary only when extending a Bench project.

## 1. Choose the four character decisions

| Decision | Options | Bench | Ember | Nebula |
|---|---|---|---|---|
| Native mode | light-first / dark-first | dark | light | dark |
| Shape | squared (`rounded-md`/`lg`) / round (20-40px, pills, circles) | squared | round | round |
| Depth | flat tonal steps / lit glass (inset highlight + deep shadow) | flat | flat | lit |
| Display face | mono uppercase tracked wide / grotesk tight negative | Space Mono | Instrument Sans | Instrument Sans |

Body stays Geist and data stays Geist Mono in every skin. Only the display face changes, and it must come from `next/font/google`.

## 2. Pick the signal

- **One hue.** It marks active, unread, starred, focused and primary. Everything else is greyscale.
- An optional **secondary** is allowed only for focus rings and data series (as Nebula's cyan is), never as a second action colour.
- Derive the set:
  - `--signal`: the fill (dots, bars, progress, big numerals).
  - `--signal-ink`: the text version, darkened until it reaches >= 4.5:1 on `--sheet` and `--card`. On dark skins it can equal `--signal`.
  - `--signal-soft`: the tint for active rows and the Ask button (around 14% alpha on dark, a pale solid on light).
  - `--primary`: the solid button fill; adjust until `--primary-foreground` labels reach >= 4.5:1.
  - `--glow`, `--glass-tint`, `--shadow-glow-value`: the signal at 16-50% alpha.

## 3. Build the grey staircase

Four grounds, each a visible step from the last:

`band/background` (page) -> `sheet` (pane) -> `card` (row, tile) -> `card-nested` (tile in a tile)

- Light skins: solids, about 2-4% lightness apart, warm or cool to match the signal.
- Dark skins: either solids (Bench) or translucent fills over a fixed atmosphere (Nebula). If translucent, also define the reduced-transparency solids.
- Then `border`/`input`/`glass-border` (hairlines), `ink-2`/`ink-3`/`muted-foreground` (text steps), `inverse`/`inverse-foreground`, and `critical`, `warn`, `ok`.

## 4. Contrast targets

| Pair | Minimum |
|---|---|
| `foreground` on every ground | 7:1 |
| `ink-2` / `muted-foreground` on `sheet` and `card` | 4.5:1 |
| `ink-3` on `sheet` | 3:1 (meta and placeholders only) |
| `signal-ink` on `sheet` and `card` | 4.5:1 |
| `primary-foreground` on `primary` | 4.5:1 |
| `inverse-foreground` on `inverse` | 7:1 |
| `ring` against every ground | 3:1 |

Check them with a contrast tool before you ship. Write the ratio for any borderline pair as a CSS comment next to the token (as Raven does: "burnt coral keeps white labels at 4.6:1").

## 5. Write the blocks

Start from the Ember `:root` and the Nebula `.dark` and replace values. Keep every variable name. Delete nothing: an unset role silently falls back to nothing and breaks a component somewhere else.

```css
/* ─── <Skin name> ─── */
:root {
	--radius: 0.875rem;          /* squared skins: 0.5rem */
	--radius-card-size: 28px;    /* squared skins: 0.5rem */
	--radius-frame-size: 40px;   /* squared skins: 0.75rem */
	--band: …; --sheet: …; --card: …; --card-nested: …;
	--foreground: …; --ink-2: …; --ink-3: …;
	--signal: …; --signal-ink: …; --signal-soft: …; --signal-2: …;
	--primary: …; --primary-foreground: …; --ring: …;
	/* …every other role from ember.md… */
	color-scheme: light;
}
.dark { /* same names, dark values */ color-scheme: dark; }
```

Add the skin's header comment at the top of `globals.css` in the same shape as Raven's: name, mode, one line of mood, one line of signal.

## 6. Signature move (optional, one only)

Each skin earns one motif that is not in the others: Bench has the corner-bracket reticle and the measurement grid, Ember has flat paper bento, Nebula has the radial atmosphere and lit glass. Give a new skin at most one, and make it come from the product (as the raven's eye became the empty-state dot), not from decoration.

## Checklist

1. Every variable in the Ember `:root` block has a value in both `:root` and `.dark`.
2. Exactly one signal hue (plus at most one secondary for focus/data).
3. Every pair in the contrast table meets its minimum, with borderline ratios commented.
4. Translucent grounds have reduced-transparency solids.
5. The display face is loaded in `app/layout.tsx`; Geist and Geist Mono are unchanged.
6. The pill tabs, a list row, a status chip and the empty state are screenshotted in both modes before anything else is built.
