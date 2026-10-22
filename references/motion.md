# Motion

Library: `motion` (`import { motion } from "motion/react"`), not the old `framer-motion` package. Lenis for smooth scroll on landing pages only. GSAP only for one-off scroll-scrubbed or WebGL-heavy scenes.

## `lib/motion.ts`

```ts
/** Every shared-layout indicator: pill tabs, nav rail, sidebar active row. Slides and settles with a hint of overshoot. */
export const TAB_SPRING = { type: "spring", stiffness: 350, damping: 30 } as const;

/** Dialogs that grow out of their trigger. */
export const DIALOG_SPRING = { type: "spring", stiffness: 320, damping: 34, mass: 0.8 } as const;

/** Banners and toasts that slide in. Exit is a plain fade: { opacity: 0, y: 8, duration: 0.16 }. */
export const BANNER_SPRING = { type: "spring", stiffness: 420, damping: 34 } as const;

/** House ease-out (quint-like). Entrances, reveals, popups. CSS twin: --ease-out-quint. */
export const EASE_OUT = [0.22, 1, 0.36, 1] as const;

/** Soft in-out for slow morphs: a theme "dusk" tween, rail tick morphs. */
export const EASE_IN_OUT = [0.45, 0.05, 0.2, 1] as const;

/** Scroll-scrub smoothing for useSpring(scrollYProgress, SCRUB_SPRING). */
export const SCRUB_SPRING = { damping: 40, stiffness: 220, restDelta: 0.0005 } as const;
```

The two source apps use near-identical ease-outs: Motion Garden `[0.22, 1, 0.36, 1]`, Raven CSS `cubic-bezier(0.23, 1, 0.32, 1)`. Pick one per project and use it in both JS and CSS.

## Durations

| What | Duration |
|---|---|
| Hover colour, press | 150-200ms |
| Popups (tooltip, select, popover) | 100-150ms, fade + `scale-95` |
| Lift on hover | 180ms |
| Panel rise-in | 320ms |
| Code block collapse | 240ms ease-out |
| Scroll reveal | 600ms |
| Bloom, draw-on, wipe-in | 700ms |
| Theme reveal | 480-640ms |
| Stagger step | 55-60ms |

## Scroll reveal

```tsx
"use client";

import type { ReactNode } from "react";
import { motion, useReducedMotion } from "motion/react";
import { EASE_OUT } from "@/lib/motion";

// The initial style never branches on the motion preference: SSR can't know it,
// so branching there causes hydration mismatches. Reduced motion lands the same
// animation instantly instead.
export function Reveal({ children, delay = 0, className }: { children: ReactNode; delay?: number; className?: string }) {
	const reduce = useReducedMotion();
	return (
		<motion.div
			className={className}
			initial={{ opacity: 0, y: 16 }}
			whileInView={{ opacity: 1, y: 0 }}
			viewport={{ once: true, margin: "-80px" }}
			transition={reduce ? { duration: 0 } : { duration: 0.6, delay, ease: EASE_OUT }}
		>
			{children}
		</motion.div>
	);
}
```

List rows that reveal: `y: 20`, `delay: i * 0.06`. Hero groups: `staggerChildren: 0.09, delayChildren: 0.15`, 0.7s.

## CSS keyframes and choreography utilities

Put the keyframes inside `@theme` so Tailwind generates `animate-*` utilities.

```css
@theme {
	--ease-out-quint: cubic-bezier(0.23, 1, 0.32, 1);
	--animate-rise-in: rise-in 320ms var(--ease-out-quint) backwards;
	--animate-bloom: bloom 700ms var(--ease-out-quint) both;
	--animate-pulse-once: pulse-once 900ms var(--ease-out-quint) 1 both;
	--animate-glow-sweep: glow-sweep 3.2s linear infinite;

	@keyframes rise-in {
		from { opacity: 0; transform: translateY(8px); }
		to { opacity: 1; transform: none; }
	}
	@keyframes bloom {
		from { opacity: 0; transform: scale(0.6); }
		60% { opacity: 1; transform: scale(1.03); }
		to { opacity: 1; transform: scale(1); }
	}
	@keyframes pulse-once {
		from { opacity: 0.7; transform: scale(1); }
		to { opacity: 0; transform: scale(2.8); }
	}
	@keyframes glow-sweep {
		to { --sweep-angle: 360deg; }
	}
}

/* Staggered entrance: set style={{ "--i": index }}. `backwards`, not `both`:
   an opacity animation left filling forever makes Chrome treat the element as
   a backdrop root, and backdrop-filter blur inside it stops working. */
@utility entrance {
	animation: rise-in 320ms var(--ease-out-quint) backwards;
	animation-delay: calc(var(--i, 0) * 55ms + 60ms);
}

/* SVG draw-on: pair with pathLength="1". */
@utility draw-on {
	stroke-dasharray: 1;
	stroke-dashoffset: 1;
	animation: draw 700ms var(--ease-out-quint) forwards;
	animation-delay: var(--draw-delay, 380ms);
}
@keyframes draw { to { stroke-dashoffset: 0; } }

/* Left-to-right reveal for stretched SVGs (preserveAspectRatio="none"). */
@utility wipe-in {
	animation: wipe-in 700ms var(--ease-out-quint) both;
	animation-delay: var(--draw-delay, 380ms);
}
@keyframes wipe-in {
	from { clip-path: inset(-8px 100% -8px -8px); }
	to { clip-path: inset(-8px); }
}

/* Radial ring from 12 o'clock: circle pathLength="100", stroke-dashoffset = 100 - value. */
@utility ring-sweep {
	stroke-dasharray: 100;
	animation: ring-sweep 900ms var(--ease-out-quint) both;
	animation-delay: var(--draw-delay, 260ms);
}
@keyframes ring-sweep { from { stroke-dashoffset: 100; } }

/* Play intro choreography once per session: set data-intro="off" on <html> after the first run. */
[data-intro="off"] :is(.entrance, .animate-bloom) { animation: none; }
[data-intro="off"] :is(.draw-on, .ring-sweep, .wipe-in) { animation-duration: 1ms; animation-delay: 0ms; }
```

## Pill indicator (shared `layoutId`)

One indicator per group, springing between items. The pressed item itself stays transparent.

```tsx
const layoutId = React.useId(); // per group instance, never a hard-coded string

{active && (
	<motion.span
		layoutId={layoutId}
		// Animate only when the selection changes. Without this, any re-render after
		// a layout shift above (a streaming list, a live panel) springs the pill over
		// from its old spot.
		layoutDependency={value}
		transition={TAB_SPRING}
		aria-hidden
		className="absolute inset-0 -z-10 rounded-md bg-accent"
	/>
)}
```

- For a round pill whose width changes between items, set the radius through `style={{ borderRadius: 9999 }}` so Motion counter-scales it while it stretches.
- If the indicator sits inside an inner scroller, that scroller must be a `motion.div` with `layoutScroll`. If it jumps anyway, use the `motion-pill-jump` skill.
- A sidebar whose active row is server-rendered should arm its `layoutId` only after the first client navigation, or the first paint animates from nowhere.

## Grow-from-trigger dialog

```tsx
<MotionConfig transition={reducedMotion ? { duration: 0 } : DIALOG_SPRING}>
	{/* trigger and panel share a layoutId; the panel's content fades in after the shape lands */}
	<motion.div layoutId={id} style={{ borderRadius: 16 }} className="... bg-panel" />
	<motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={reduce ? undefined : { duration: 0.18, delay: 0.12 }} />
</MotionConfig>
```

Backdrop: `bg-white/40 dark:bg-black/50 backdrop-blur-xs`. Panel: `max-w-sm max-h-[85svh] shadow-2xl`.

## Theme reveal (View Transitions)

The new theme grows out of the toggle as a circle. CSS:

```css
::view-transition-old(root),
::view-transition-new(root) {
	animation: none;
	mix-blend-mode: normal;
}
::view-transition-new(root) {
	animation: skin-reveal 640ms var(--ease-out-quint);
}
@keyframes skin-reveal {
	from { clip-path: circle(0 at var(--reveal-x, 50%) var(--reveal-y, 50%)); }
	to { clip-path: circle(var(--reveal-r, 150vmax) at var(--reveal-x, 50%) var(--reveal-y, 50%)); }
}
```

JS:

```ts
export function revealTheme(next: "light" | "dark", setTheme: (t: string) => void, origin?: { x: number; y: number }) {
	const root = document.documentElement;
	const apply = () => {
		// next-themes applies the class in an effect; the transition needs it now.
		root.classList.toggle("dark", next === "dark");
		root.classList.toggle("light", next === "light");
		root.style.colorScheme = next;
		setTheme(next);
	};
	const reduced = root.dataset.motion === "reduce" || window.matchMedia("(prefers-reduced-motion: reduce)").matches;
	const unchanged = root.classList.contains("dark") === (next === "dark");
	if (reduced || unchanged || typeof document.startViewTransition !== "function") {
		apply();
		return;
	}
	const x = origin?.x ?? innerWidth / 2;
	const y = origin?.y ?? innerHeight / 2;
	const r = Math.hypot(Math.max(x, innerWidth - x), Math.max(y, innerHeight - y));
	root.style.setProperty("--reveal-x", `${x}px`);
	root.style.setProperty("--reveal-y", `${y}px`);
	root.style.setProperty("--reveal-r", `${r}px`);
	document.startViewTransition(apply);
}
```

Bench uses a diamond instead of a circle (a `polygon()` with four vertices at `cx ± R`, `cy ± R`, where `R = maxRadius * Math.SQRT2`), animated from the collapsed polygon. Whatever shape you pick, keep the attribute and variable names the same in CSS and JS. Render the toggle icons with CSS (`hidden dark:block`), not from `resolvedTheme`, or SSR mismatches.

## Reduced motion: all three layers

**1. CSS backstop** (`app/globals.css`):

```css
@media (prefers-reduced-motion: reduce) {
	*, *::before, *::after {
		animation-duration: 1ms !important;
		animation-iteration-count: 1 !important;
		transition-duration: 1ms !important;
		transition-delay: 0ms !important;
		scroll-behavior: auto !important;
	}
}
/* Same clamp, chosen in-app instead of by the OS. */
html[data-motion="reduce"] :is(*, *::before, *::after) {
	animation-duration: 1ms !important;
	animation-iteration-count: 1 !important;
	transition-duration: 1ms !important;
	transition-delay: 0ms !important;
	scroll-behavior: auto !important;
}
```

**2. Motion config + hook:**

```tsx
"use client";

import { MotionConfig } from "motion/react";

export function MotionProvider({ children, appSetting }: { children: React.ReactNode; appSetting?: "reduce" | "system" }) {
	return <MotionConfig reducedMotion={appSetting === "reduce" ? "always" : "user"}>{children}</MotionConfig>;
}
```

```ts
"use client";

import * as React from "react";

const QUERY = "(prefers-reduced-motion: reduce)";

function subscribe(onChange: () => void) {
	const mql = window.matchMedia(QUERY);
	mql.addEventListener("change", onChange);
	return () => mql.removeEventListener("change", onChange);
}

/** The OS asks for reduced motion, or the in-app setting does. */
export function useReducedMotion(appSetting?: "reduce" | "system"): boolean {
	const system = React.useSyncExternalStore(subscribe, () => window.matchMedia(QUERY).matches, () => false);
	return appSetting === "reduce" || system;
}
```

**3. Per-effect opt-outs:**

- Lenis smooth scroll renders `null`.
- Theme reveal swaps instantly.
- Decorative bursts (sparkles, confetti) do not mount.
- Autoplay loops and WebGL scenes start paused.
- Anything that hides content until it animates in (masonry reveals, staggered grids) receives `reducedMotion` and shows content immediately. The CSS backstop shortens durations but does not fix content stuck at `opacity: 0`.

## Animated components (Motion Garden registry conventions)

For canvas, WebGL or rAF-driven components:

- Drive every frame loop through one shared `useAnimationLoop` hook, never raw `requestAnimationFrame` or a bare `ResizeObserver` per component.
- The frame callback halts only on a literal `false`. Never write `draw?.(dt) ?? false`: that returns `false` on success and freezes after one frame.
- Cap device pixel ratio at 2 and clamp `dt` (max `1/15`s) so a background tab doesn't explode the simulation.
- Every animated component accepts `reducedMotion` and `paused` props.
- Drag-driven effects: `touch-none` on the hit target, `pointermove` not `mousemove`, `select-none` if the surface holds text.
- 3D scenes ship drag-orbit camera controls (yaw, clamped pitch, momentum). Orbit state lives in a ref, not React state.
- In SVG, never seed a child's `opacity` through `style` (it silently beats Motion's attribute). Use `initial`. Set a pivot with `transformBox: "view-box"`.
- Never scale a canvas preview in a popover; translate it. Scaling breaks canvas measurement.
