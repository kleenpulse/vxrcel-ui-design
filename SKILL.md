---
name: vxrcel-ui-design
description: Build or restyle web UI in the Vxrcel house design system, distilled from Motion Garden (shadow-garden) and Raven (email-manager). Quiet greyscale chrome, one signal hue, uppercase mono readouts, springy layoutId pills, and three ready-made skins - Bench (graphite + amethyst), Ember (warm paper + coral) and Nebula (deep-teal glass + gold). Use when the user says "build this in the Vxrcel style", "use our design system", "make it look like Motion Garden / shadow-garden", "make it look like Raven", "use the bench skin", "Ember theme", "Nebula theme", "set up design tokens for a new Next.js app", "add a light/dark skin", "make a new skin", or asks for a dashboard, docs shell, inbox or app shell that should match their other projects. Covers Tailwind v4 tokens, typography, motion, reduced motion and component recipes.
license: MIT
---

# Vxrcel UI Design: Quiet Chrome, One Signal

This skill makes an agent build interfaces the way Motion Garden and Raven are built. The skins look different, but the system underneath is the same: semantic tokens bridged through Tailwind v4, a greyscale shell, one signal hue that marks only what is live, mono microtype for data, and one spring for everything that slides. Pick a skin, paste its tokens, and build with the recipes. Or make a new skin with the same vocabulary.

## When to Use

- Starting a new Next.js (App Router) app that should feel like the user's other projects
- Restyling a page, dashboard, inbox, docs shell, settings screen or landing page into this system
- Adding a light or dark skin, or a whole new skin, to an app that already uses these tokens
- Reviewing UI for drift from the system: stray hex values, a second accent, missing reduced motion

**Do not use for:** projects that already have their own established, different design system, marketing pages for third-party brands, or native mobile apps. Do not apply it to a codebase partially: half-migrated tokens are worse than either system.

## Core Principle

**The chrome is quiet; one signal hue speaks.** Grounds, panels, text and borders are greyscale steps. A single signal colour (amethyst, coral or gold) marks only what is live: the active tab, the unread dot, the focus ring, the primary action, a big numeral. Components name roles (`bg-panel`, `text-ink-dim`, `bg-signal`), never values. A skin is one CSS block, so swapping it re-tints the whole app without touching a component.

## The Three Skins

| Skin | Source | Mood | Ground | Signal | Display face | Shape |
|---|---|---|---|---|---|---|
| **Bench** | Motion Garden | Graphite instrument bench, dark-native, light "lab bench" | `#0a0b0c` / `#f9f9fb` | Amethyst `#a855f7` (light `#7e22ce`) | Space Mono, uppercase, tracked wide | Squared: `rounded-md`/`rounded-lg`, borderless tonal panels |
| **Ember** | Raven light | Warm paper bento, flat | `#f3f3f1` | Coral `#e8603c`, text `#b4461f` | Instrument Sans, tight negative tracking | Round: 20-28px cards, 40px frames, pills, circles |
| **Nebula** | Raven dark | Deep-teal lit glass | `#050a0b` + radial atmosphere | Gold `#e6c27a` + cyan `#5ee1d9` | Instrument Sans | Round, same as Ember |

Ember and Nebula are the light and dark of one app. Bench carries its own light and dark. Full paste-ready tokens: [Bench](references/skins/bench.md), [Ember](references/skins/ember.md), [Nebula](references/skins/nebula.md).

## Shared Token Vocabulary

Every skin fills the same roles. Bench uses its own short names; Ember/Nebula use the shadcn-compatible names. Code ports between them by swapping the utility name.

| Role | Bench utility | Ember / Nebula utility | Used for |
|---|---|---|---|
| Page ground | `bg-surface` | `bg-background`, `bg-band` | body, rail, canvas behind panes |
| Pane | `bg-panel` | `bg-sheet` | sidebars, list/reader panes, cards in Bench |
| Tile | `bg-panel` | `bg-card` | bento cards, list rows |
| Step up | `bg-raised`, `bg-raised/30` | `bg-card-nested` | header strips, inputs, hover, nested tiles |
| Hairline | `border-hairline` | `border-border`, `ring-border`, `dark:ring-glass-border` | rules, floating layers only |
| Ink | `text-ink` | `text-foreground` (`text-ink`) | primary text |
| Ink dim | `text-ink-dim` | `text-ink-2` | secondary text, idle icons |
| Ink mute | `text-ink-mute` | `text-ink-3` | placeholders, disabled meta |
| Signal fill | `bg-accent` | `bg-signal` | dots, bars, progress, big numerals |
| Signal text | `text-accent` | `text-signal-ink` | text that must be the signal colour |
| Signal tint | `bg-accent/10`-`/15` | `bg-signal-soft` | active row, Ask button, running chip |
| Solid active | `bg-accent` + `text-on-accent` | `bg-inverse` + `text-inverse-foreground` (Ember), `bg-signal-soft` + `text-signal` (Nebula) | active pill, selected nav item |
| Danger | `text-danger` | `text-critical` / `destructive` | errors, attention |
| Status | (none) | `ok`, `warn`, `critical` | status chips |
| Focus | 2px `outline` in `--sg-accent` | `ring-2 ring-ring` | every focusable control |

## Typography

- **Three faces, three jobs.** Body: Geist (`font-sans`). Data, code and readouts: Geist Mono (`font-mono`). Display: the skin's face (`font-display`): Space Mono for Bench, Instrument Sans for Ember/Nebula. No fourth font.
- **Microtype is mono and uppercase.** Section labels, metadata keys, counts, dates, units and status chips are mono, uppercase, 10-11px, tracked out. Raven ships this as the `readout` utility; Bench writes it inline (`font-display text-[11px] uppercase tracking-[0.2em] text-ink-mute`).
- **Tracking ladder: the smaller the text, the wider the tracking.** Bench headlines `0.02-0.08em`, tabs `0.15em`, eyebrows `0.2-0.34em`. Raven display type goes the other way, tight and negative: pane titles `-0.01em`, subjects `-0.02em`, hero `-0.045em`.
- **Weights: 400, 500, 600 only.** Unread or emphasized = `font-semibold`; read = `font-medium text-foreground/80`. No bold except a one-off wordmark.
- **Every number is tabular.** `tabular-nums` (Bench) or the `tabular` utility (Raven) on counts, times, sizes, KPIs and readouts.
- **Dense body sizes are deliberate.** `text-[13px]`, `text-[13.5px]`, `text-[12.5px]`, `text-sm`. Descriptions use `leading-relaxed`; long copy is capped at `max-w-xl`/`max-w-2xl` and uses `text-pretty` / `text-balance`.
- **Copy:** short words, sentence case outside of microtype, no em dashes in UI strings.

## Motion

One spring, one ease, one entrance. Constants live in `lib/motion.ts`. Full code: [references/motion.md](references/motion.md).

| Constant | Value | Used for |
|---|---|---|
| `TAB_SPRING` | `{ type: "spring", stiffness: 350, damping: 30 }` | every `layoutId` indicator: pill tabs, nav rail, sidebar active row |
| `DIALOG_SPRING` | `{ type: "spring", stiffness: 320, damping: 34, mass: 0.8 }` | grow-from-trigger dialogs |
| `BANNER_SPRING` | `{ type: "spring", stiffness: 420, damping: 34 }` | banners and toasts sliding in |
| `EASE_OUT` | `[0.22, 1, 0.36, 1]` (CSS `--ease-out-quint`) | entrances, reveals, popups |
| `EASE_IN_OUT` | `[0.45, 0.05, 0.2, 1]` | slow morphs: dusk tween, rail ticks |

- **Entrance:** fade + rise. Scroll reveal `y: 16`, `0.6s`, `viewport={{ once: true, margin: "-80px" }}`. In-app panels use CSS `rise-in` (8px, 320ms) staggered `calc(var(--i) * 55ms + 60ms)`.
- **Popups:** 100-150ms fade + `scale-95`. Never scale a canvas preview; translate it.
- **Hover in chrome is colour only** (`ink-dim -> ink` or `-> accent`). Lift (`translateY(-2px)` + `shadow-lift`) is reserved for cards, and only under `(hover: hover) and (pointer: fine)`. Press: `active:scale-95` on circles, `active:scale-[0.98]` on CTAs, `translate-y-px` on buttons.
- **Theme switch:** a View Transition clip-path reveal grown from the toggle (circle in Raven, diamond in Bench). Skipped under reduced motion.

### Reduced motion: three layers, all required

1. **CSS backstop** in `globals.css`: under `prefers-reduced-motion: reduce` (and an in-app `html[data-motion="reduce"]`), clamp every animation and transition to ~1ms.
2. **Motion config:** `<MotionConfig reducedMotion="user">` (or `"always"` when the in-app setting is on), plus `useReducedMotion()` in components that animate in JS.
3. **Per-effect opt-outs:** smooth scroll (Lenis) renders nothing, theme reveal swaps instantly, decorative bursts don't mount, autoplay loops pause, and any component that hides content until it animates receives `reducedMotion` and shows it immediately.

**Hydration rule:** never branch the *initial* state on the preference (SSR can't know it). Keep `initial={{ opacity: 0, y: 16 }}` and set `transition={{ duration: 0 }}` when reduced.

## Shared Component Patterns

Exact class strings for each: [references/components.md](references/components.md).

- **Controls are `h-8`.** Text buttons `h-8 px-2.5`/`px-3`; icon buttons `size-7`/`size-8` (Bench) or circles `size-9`/`size-11` (Raven); pills `h-10 rounded-pill px-5`.
- **Pill tabs** with one shared `motion.span layoutId` indicator per group (`useId()`), `layoutDependency={value}`, `transition={TAB_SPRING}`. The pressed item itself stays transparent.
- **Panels are tonal, not bordered.** A pane is one step off the ground; a header strip is one more step (`bg-raised/30 px-4 py-2.5` with a mono uppercase title). Borders belong to floating layers (popover, select, tooltip, dialog) and decorative rules.
- **Status chip:** `h-6 rounded-pill px-2.5 font-mono text-[10.5px] uppercase tracking-wide` + a `size-1.5` dot, tinted `bg-ok/12 text-ok`, etc.
- **Empty state:** two concentric hairline rings around a `size-2` signal dot, a display-face title, one short line of `text-[13px] text-ink-2`.
- **Skeletons are shaped like the real UI** (row cards, pills, text lines of varied widths), fading down the list: `opacity: Math.max(0.25, 1 - i * 0.09)`.
- **Whole-card click targets** use one stretched link or button (`after:absolute after:inset-0`) with the focus ring on the `after`; secondary controls sit above it as siblings, never nested.
- **Icons:** `lucide-react` only. `size-4` default, `size-3.5` inline meta, `size-[18px]` in circles. Custom glyphs go in `components/icons/` on lucide's 24 grid, `stroke="currentColor" strokeWidth={2}`.
- **Panes use container queries** (`@container/list`, `@3xl/list:`) so a pane adapts to its own width, not the viewport.

## Rules

1. **No raw colours in components.** No hex, `rgb()`, `bg-white`, `bg-black`, `text-white` or `bench-950` on a themed surface. The only exceptions: surfaces that stay dark in both themes on purpose (code blocks, Bench exhibit plates), and the sanctioned gradients listed in the skin file.
2. **One signal hue per skin.** Nebula's cyan (`signal-2`) is the only sanctioned secondary, and it is for focus rings and data, never a second CTA colour.
3. **Signal text uses the ink variant.** Coral `#e8603c` fails as body text on paper; text uses `text-signal-ink` (>= 4.5:1). Labels on solid primary fills must be >= 4.5:1.
4. **Signal is reserved.** It marks active, unread, starred, focused, primary, and the brand dot. Never use it for decoration, avatars or category colours.
5. **Glass is for overlays only** (tray, popovers, mobile tab bar, sticky bars), always with a `prefers-reduced-transparency` / `data-transparency="reduce"` solid fallback.
6. **Both themes, every time.** Every UI change is checked in light and dark, and at phone width.
7. **Reduced motion has all three layers** (CSS backstop, MotionConfig/hook, per-effect opt-outs).
8. **Next.js: App Router, no `src/`.** `app/`, `components/`, `lib/`, `hooks/` at the repo root; `@/*` maps to `./*`.
9. **Never run `shadcn init` on a Bench project.** It injects a rival `--color-accent`. Vendor the primitive into `components/ui/` and restyle it to tokens.
10. **One icon package.** `lucide-react`. Brand marks and animated glyphs are authored in `components/icons/`.

## Method

1. **Pick the skin.** Docs, dev tools, catalogs, instrument-like dashboards: Bench. Consumer apps, inboxes, productivity, anything warm: Ember + Nebula. Neither fits: build a fourth with [references/new-skin.md](references/new-skin.md).
2. **Install the tokens.** Paste the skin's CSS blocks into `app/globals.css` (Tailwind v4, no `tailwind.config`). Ember/Nebula need the shared bridge and utilities from the Ember file plus the `.dark` block from the Nebula file.
3. **Wire fonts** in `app/layout.tsx` with `next/font/google` (`Geist`, `Geist_Mono`, plus `Space_Mono` or `Instrument_Sans`), as CSS variables on `<html>` with `antialiased`.
4. **Wire theming:** `next-themes` with `attribute="class"`. Bench: `defaultTheme="dark"`, `enableSystem={false}`. Raven skins: system allowed.
5. **Add `lib/motion.ts`** and the reduced-motion layers from [references/motion.md](references/motion.md).
6. **Build with the recipes** in [references/components.md](references/components.md). Reach for an existing recipe before inventing a pattern.
7. **Run the self-check below**, then screenshot both themes at desktop and phone width.
8. **Deliver** the changed files, the skin used, and anything you had to invent that is not in this skill (so the user can decide whether it becomes part of the system).

## Worked Example

A two-option view switcher ("Grid / List") in a toolbar.

**Bench (Motion Garden):** squared, uppercase Space Mono, solid amethyst pill.

```tsx
<div role="tablist" className="relative inline-flex items-center gap-1 rounded-lg bg-panel p-1">
  {items.map((item) => {
    const active = item.value === value;
    return (
      <button key={item.value} role="tab" aria-selected={active} tabIndex={active ? 0 : -1}
        onClick={() => onValueChange(item.value)}
        className={cn(
          "relative inline-flex items-center justify-center rounded-md px-3 py-1 font-display text-[11px] uppercase tracking-[0.15em] whitespace-nowrap outline-none transition-colors",
          active ? "text-on-accent" : "text-ink-dim hover:text-ink",
        )}>
        {active && (
          <motion.span layoutId={layoutId} layoutDependency={value} transition={TAB_SPRING}
            className="absolute inset-0 z-0 rounded-md bg-accent dark:border dark:border-accent/40" />
        )}
        <span className="relative z-10">{item.label}</span>
      </button>
    );
  })}
</div>
```

**Ember / Nebula (Raven):** the same structure, round. The track is a hairline pill, the indicator is ink-black on paper and a gold tint on glass.

```tsx
<div role="tablist" className="flex items-center rounded-pill bg-sheet p-1 ring-1 ring-border dark:bg-background/40 dark:ring-glass-border">
  {/* ...same map... */}
  <button className={cn(
      "relative isolate inline-flex h-8 items-center rounded-pill px-3 text-[12px] font-medium outline-none focus-visible:ring-2 focus-visible:ring-ring",
      active ? "text-inverse-foreground dark:text-signal" : "text-ink-2 hover:text-foreground",
    )}>
    {active && (
      <motion.span layoutId={layoutId} layoutDependency={value} transition={TAB_SPRING} aria-hidden
        style={{ borderRadius: 9999 }}
        className="absolute inset-0 -z-10 bg-inverse dark:bg-signal-soft" />
    )}
    {item.label}
  </button>
</div>
```

The roles map one to one: track `bg-panel` -> hairline ring, indicator `bg-accent` -> `bg-inverse`/`bg-signal-soft`, idle text `text-ink-dim` -> `text-ink-2`. The spring, the `layoutDependency` and the per-group `layoutId` do not change.

## Self-Check Before You Deliver

Run these from the repo root. Each is pass or fail.

1. `grep -rnE "#[0-9a-fA-F]{3,8}\b|rgba?\(" app components --include=*.tsx` returns nothing outside the skin's sanctioned exceptions.
2. `grep -rnE "\b(bg|text|border)-(white|black)\b" app components --include=*.tsx` returns nothing on themed surfaces.
3. `grep -n "prefers-reduced-motion" app/globals.css` finds the backstop clamp.
4. Every `layoutId=` in `components/` has `transition={TAB_SPRING}` (or the dialog spring) and a `layoutDependency`, and its id comes from `useId()` unless it is the one global nav indicator.
5. Every number rendered (counts, dates, sizes, KPIs) carries `tabular`/`tabular-nums`.
6. `app/layout.tsx` loads exactly Geist, Geist Mono and the skin's display face.
7. No `src/` and no `pages/` directory exists.
8. Only one signal hue appears in new component classes (`accent`/`signal`), plus status colours on status chips.
9. Light and dark screenshots exist for every changed screen, at desktop and at phone width.
