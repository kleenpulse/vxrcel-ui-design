# Component recipes

The class strings are copied from the source apps. **[B]** = Bench (Motion Garden), **[R]** = Ember/Nebula (Raven). To port a recipe to the other skin, swap utilities using the role table in `SKILL.md`. Keep the structure.

## Buttons

**[B] Primary**

```
inline-flex h-8 items-center gap-1.5 rounded-md bg-accent px-3 font-sans text-sm font-medium text-on-accent transition-opacity hover:opacity-90 focus-visible:ring-2 focus-visible:ring-accent
```

**[B] Landing CTA pair** (display type, uppercase)

```
primary:   inline-flex h-8 items-center rounded-md bg-accent px-3 font-display text-[10px] md:text-[11px] uppercase tracking-[0.15em] text-on-accent hover:bg-accent-hover
secondary: inline-flex h-8 items-center rounded-md border border-hairline px-3 font-display text-[10px] md:text-[11px] uppercase tracking-[0.15em] text-ink-dim hover:border-accent-muted hover:text-ink
```

**[B] Icon button**

```
grid size-7 md:size-8 place-items-center rounded-md bg-surface text-ink-dim transition-colors hover:text-accent focus-visible:text-accent
```

**[B] Chrome text action** (copy, reset, pause)

```
font-mono text-[11px] text-ink-mute transition-colors hover:text-accent
```

**[R] Button (shadcn base-nova + Raven variants)**

```ts
const buttonVariants = cva(
  "group/button inline-flex shrink-0 items-center justify-center rounded-lg border border-transparent bg-clip-padding text-sm font-medium whitespace-nowrap transition-all outline-none select-none focus-visible:border-ring focus-visible:ring-3 focus-visible:ring-ring/50 active:not-aria-[haspopup]:translate-y-px disabled:pointer-events-none disabled:opacity-50 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/80",
        ghost: "hover:bg-muted hover:text-foreground aria-expanded:bg-muted aria-expanded:text-foreground dark:hover:bg-muted/50",
        destructive: "bg-destructive/10 text-destructive hover:bg-destructive/20",
        inverse: "bg-inverse text-inverse-foreground hover:bg-inverse/88",
        surface: "border-border bg-sheet text-foreground hover:bg-card dark:border-glass-border dark:bg-card-nested dark:hover:bg-accent",
        glass: "glass text-foreground hover:bg-accent",
      },
      size: {
        default: "h-8 gap-1.5 px-2.5",
        sm: "h-7 gap-1 px-2.5 text-[0.8rem]",
        icon: "size-8",
        "icon-sm": "size-7",
        pill: "h-10 gap-2 rounded-pill px-5 text-sm",
        "pill-sm": "h-8 gap-1.5 rounded-pill px-3.5 text-[0.8125rem]",
        circle: "size-11 rounded-full [&_svg:not([class*='size-'])]:size-[18px]",
        "circle-sm": "size-9 rounded-full",
      },
    },
    defaultVariants: { variant: "default", size: "default" },
  },
);
```

Primary actions in Raven are pills (`size="pill"`). Toolbar tools are `variant="surface" size="circle-sm"` inside a tooltip with a `Kbd` hint. Destructive tools stay neutral until hovered (`hover:text-destructive`).

## Inputs

**[B] Search**

```
rounded-md border border-transparent bg-surface px-3 py-1.5 font-mono text-xs text-ink placeholder:text-ink-mute outline-none focus-visible:border-accent
```

**[B] Select trigger / item**

```
trigger: rounded-md border border-hairline bg-raised px-2.5 py-1.5 font-mono text-xs text-ink hover:border-ink-mute focus-visible:border-accent
content: rounded-md border border-hairline bg-panel shadow-lg shadow-black/25
item:    focus:bg-accent/15 focus:text-ink data-[state=checked]:text-accent
```

**[R] Input**

```
h-8 w-full min-w-0 rounded-lg border border-input bg-sheet px-2.5 py-1 text-base text-foreground transition-[border-color,box-shadow,background-color] outline-none placeholder:text-muted-foreground hover:border-foreground/20 focus-visible:border-ring focus-visible:ring-2 focus-visible:ring-ring/25 disabled:opacity-50 md:text-sm dark:bg-card-nested dark:hover:border-foreground/25
```

**[R] Pill search** (top bar): `h-11 rounded-pill pl-11` with the search icon absolutely placed and a `/` `Kbd` on the right.

## Switch [B]

```tsx
<div className="flex items-center gap-3">
  <label id={labelId} className="font-mono text-xs text-ink-dim">{label}</label>
  <button type="button" role="switch" aria-checked={checked} aria-labelledby={labelId}
    onClick={() => onChange(!checked)}
    className={cn("relative h-5 w-9 shrink-0 rounded-full border transition-colors",
      checked ? "border-accent-active bg-accent/25" : "border-hairline bg-raised")}>
    <span className={cn("absolute top-0.5 h-3.5 w-3.5 rounded-full transition-all",
      checked ? "start-[18px] bg-accent" : "start-0.5 bg-ink-mute")} />
  </button>
</div>
```

## Checkbox [R]

`size-4 rounded-[5px] border-foreground/50`; checked fills with `bg-inverse` in Ember and `bg-primary` in Nebula.

## Kbd [R]

```
pointer-events-none inline-flex h-5 w-fit min-w-5 items-center justify-center gap-1 rounded-sm bg-sidebar px-1 font-sans text-xs font-medium text-muted-foreground select-none
```

[B]: `rounded bg-panel px-1.5 py-0.5 font-mono text-[10px] leading-none text-ink-mute`.

## Pill tabs

See the worked example in `SKILL.md` for both skins. Size steps [B]:

```ts
const SIZE_CLASSES = {
  xs: { track: "p-0.5", tab: "py-0.5 text-[10px]" },
  sm: { track: "p-1", tab: "py-1 text-[11px]" },
  md: { track: "p-0.5", tab: "py-1 text-xs" },
} as const;
```

Keyboard: `role="tablist"`, `role="tab"` with `aria-selected`, roving `tabIndex`, ArrowLeft/ArrowRight wrap around.

## Panel with header strip [B]

```tsx
<section className="overflow-hidden rounded-lg bg-panel">
  <header className="flex items-center justify-between bg-raised/30 px-4 py-2.5">
    <h2 className="font-display text-[11px] uppercase tracking-[0.2em] text-ink-dim">Controls</h2>
    <button className="font-mono text-[11px] text-ink-mute transition-colors hover:text-accent">Reset</button>
  </header>
  <div className="grid gap-x-8 gap-y-5 p-4 sm:grid-cols-2 lg:grid-cols-3">{/* ... */}</div>
</section>
```

Tables inside: header row `bg-raised/30`, rows `even:bg-surface/40`, key column `font-mono text-xs text-accent`.

## Cards

**[B] Catalog card** with a full-card overlay link:

```tsx
<article className="group relative flex flex-col gap-2 rounded-lg bg-panel p-4 transition-colors focus-within:bg-raised">
  <Link href={href} className="absolute inset-0 z-0" aria-label={name} />
  <div className="pointer-events-none relative z-10">
    <h3 className="font-display text-sm uppercase tracking-[0.08em] text-ink group-hover:text-accent">{name}</h3>
    <p className="text-[13px] leading-relaxed text-ink-dim">{description}</p>
  </div>
</article>
```

**[R] Bento card**

```tsx
<section className="bento entrance relative min-w-0 p-5" style={{ "--i": i } as React.CSSProperties}>
  <h2 className="font-display text-[17px] leading-tight tracking-[-0.01em]">{title}</h2>
  <div className="mt-4 rounded-[22px] bg-card-nested p-4">
    <p className="readout">Unread</p>
    <p className="font-display text-kpi-sm tabular">{count}</p>
  </div>
</section>
```

Overview grids: `grid grid-cols-12 gap-4`, max width `max-w-[calc(1900px-84px)]` (page width minus the nav rail).

## List row card [R]

Rows are separate round cards, not table lines. The whole card opens the item through one stretched button; the checkbox and star sit above it as siblings.

```tsx
<div className={cn(
  "group/row relative flex items-center gap-3 rounded-[20px] px-3 py-(--row-py) transition-[background-color,box-shadow] duration-200",
  "@3xl/list:gap-4 @3xl/list:py-(--row-py-wide)",
  active ? "bg-signal-soft ring-1 ring-signal/30 dark:ring-signal/45"
    : selected ? "bg-card-nested ring-1 ring-foreground/20"
    : "bg-card hover:z-1 hover:bg-card-nested hover:shadow-lift dark:ring-1 dark:ring-glass-border",
)}>
  {/* size-9 mono-initials avatar that cross-fades into a checkbox on hover / focus */}
  <button className="grid min-w-0 flex-1 grid-cols-[minmax(0,1fr)_auto] items-center gap-x-3 text-left outline-none after:absolute after:inset-0 after:rounded-[20px] focus-visible:after:ring-2 focus-visible:after:ring-ring">
    <span className="flex min-w-0 items-center gap-2">
      {unread && <span aria-hidden className="size-2 shrink-0 rounded-full bg-signal" />}
      <span className={cn("truncate text-[14px] leading-5", unread ? "font-semibold text-foreground" : "font-medium text-foreground/80")}>{sender}</span>
    </span>
    <time className={cn("font-mono text-[11px] tracking-wide tabular", unread ? "text-foreground" : "text-ink-2")}>{date}</time>
    <span className="col-span-2 mt-0.5 min-w-0">
      <span className="block truncate text-[13.5px] leading-5">{subject}</span>
      <span className="block truncate text-[12.5px] leading-[18px] text-ink-2">{preview}</span>
    </span>
  </button>
</div>
```

The list: `space-y-(--row-gap)`, inside a `@container/list` pane so `@3xl/list:` collapses each card to one line with a `13rem` sender column.

**[B] Index row:** `rounded-sm px-2.5 py-1.5 text-sm`, idle `text-ink-dim hover:bg-raised/40 hover:text-ink`, active `font-medium text-accent` on the reticle fill (see `skins/bench.md`).

## Badges and chips

**[R] Status chip**

```ts
const TONE = {
  ok:        { pill: "bg-ok/12 text-ok", dot: "bg-ok" },
  warn:      { pill: "bg-warn/12 text-warn", dot: "bg-warn" },
  attention: { pill: "bg-critical/12 text-critical", dot: "bg-critical animate-pulse-once" },
  running:   { pill: "bg-signal-soft text-signal-ink dark:text-signal" },          // spinner instead of dot
  paused:    { pill: "bg-foreground/8 text-ink-2", dot: "bg-ink-3" },
  idle:      { pill: "text-ink-2 ring-1 ring-border dark:ring-glass-border" },
  soon:      { pill: "border border-dashed border-border text-ink-3 dark:border-glass-border" },
};
// base
"inline-flex h-6 shrink-0 items-center gap-1.5 rounded-pill px-2.5 font-mono text-[10.5px] tracking-wide uppercase"
// dot
"size-1.5 rounded-full"
```

**[B] Pro badge:** `rounded bg-accent/15 px-1.5 py-0.5 font-display text-[9px] uppercase tracking-[0.12em] text-accent`.

**[B] Chip:** `rounded-full bg-panel px-2.5 py-1 font-sans text-xs text-ink-dim hover:bg-raised hover:text-accent`.

**[R] Count badge on a nav circle:** `tabular absolute -top-0.5 -right-0.5 flex h-[18px] min-w-[18px] items-center justify-center rounded-full bg-primary px-1 text-[10px] font-semibold text-primary-foreground ring-2 ring-sheet dark:ring-background`, capped at `99+`.

## Empty and error state [R]

Hairline rings around the signal dot.

```tsx
<div role={tone === "critical" ? "alert" : undefined} className="flex flex-1 flex-col items-center justify-center px-8 py-14 text-center">
  <span aria-hidden className="relative grid size-24 place-items-center">
    <span className="absolute inset-0 rounded-full ring-1 ring-border dark:ring-glass-border" />
    <span className="absolute inset-4 rounded-full ring-1 ring-border dark:ring-glass-border" />
    <span className="grid size-8 place-items-center rounded-full bg-card-nested ring-1 ring-border dark:ring-glass-border">
      <span className={cn("size-2 rounded-full", tone === "critical" ? "bg-critical" : "bg-signal")} />
    </span>
  </span>
  <p className="mt-6 font-display text-xl leading-tight tracking-[-0.01em] text-foreground">{title}</p>
  {body && <div className="mt-2 max-w-xs text-[13px] leading-relaxed text-ink-2">{body}</div>}
  {action && <div className="mt-6">{action}</div>}
</div>
```

Copy is calm and short: "Inbox zero" / "Nothing is waiting on you." For Bench, use squared rings (`rounded-md`) or a specimen plate and the accent dot.

## Skeletons [R]

Shaped like the real UI, fading down the list:

```tsx
<div className="flex items-center gap-3 rounded-[20px] bg-card p-3 dark:ring-1 dark:ring-glass-border"
  style={{ opacity: Math.max(0.25, 1 - i * 0.09) }}>
  <Skeleton className="size-9 shrink-0 rounded-full" />
  <div className="min-w-0 flex-1 space-y-2 py-0.5">
    <div className="flex items-center justify-between gap-3">
      <Skeleton className="h-3.5 w-28" />
      <Skeleton className="h-2.5 w-9" />
    </div>
    <Skeleton className="h-3 w-3/4" />
    <Skeleton className="h-2.5 w-1/2" />
  </div>
</div>
```

Vary widths per row from a small fixed table, never `Math.random()` (hydration).

## App shells

**[B] Docs shell**

```tsx
<div className="lg:flex">
  <Sidebar />   {/* fixed w-72 bg-panel on mobile; lg:sticky lg:top-0 lg:h-screen lg:w-(--sidebar-w,18rem), resizable 220-480px */}
  <div className="flex min-h-screen min-w-0 flex-1 flex-col">
    <MobileBar /> {/* h-10 bg-panel/90 backdrop-blur lg:hidden */}
    <TopBar />    {/* sticky top-0 z-20 h-10 md:h-14 bg-panel/80 backdrop-blur */}
    <main className="min-w-0 flex-1 px-3 py-6 lg:px-8 lg:py-10">{children}</main>
    <SiteFooter />
  </div>
</div>
```

Sidebar category headers: sticky `font-display text-[10px] uppercase tracking-[0.18em] text-ink-mute`. Resize handle: a 1px line, `hover:bg-accent/40`, `data-[dragging=true]:bg-accent`. Content width `max-w-7xl`.

**[R] App frame:** a nav rail plus panes on the band.

```tsx
<div data-ui-zoom className="relative flex h-[calc(100svh/var(--ui-zoom,1))] w-full overflow-hidden">
  <NavRail className="hidden md:flex" />
  <main className="relative min-w-0 flex-1 overflow-hidden pb-[88px] md:pb-0">{children}</main>
</div>
```

- **Nav rail:** `relative z-10 flex w-[84px] shrink-0 flex-col items-center gap-4 bg-band py-5 dark:border-r dark:border-glass-border`. The brand mark on top, then the nav items in a capsule `flex flex-col gap-1.5 rounded-full bg-sheet p-1.5 ring-1 ring-border dark:bg-card-nested dark:ring-glass-border`. Each item is a circle: `relative inline-flex size-11 items-center justify-center rounded-full outline-none transition-[background-color,color,box-shadow,transform] duration-200 focus-visible:ring-2 focus-visible:ring-ring active:scale-95 [&_svg]:size-[18px]`. The active item's `layoutId` pill: `absolute inset-0 -z-10 rounded-full bg-inverse dark:bg-signal-soft dark:shadow-glow`. Settings, theme and account sit at the bottom (`mt-auto`).
- **Mobile tab bar:** `glass fixed inset-x-3 bottom-3 z-30 flex h-16 items-center justify-around rounded-full px-2 shadow-lift md:hidden`. The active tab is `bg-inverse text-inverse-foreground dark:bg-signal-soft dark:text-signal`.
- **Folder sidebar:** `w-60` at `lg:`, a left sheet `w-72` below. Rows `h-10 rounded-pill text-[13.5px]`; the active pill `absolute inset-0 -z-10 rounded-pill bg-sidebar-primary dark:shadow-glow`; section labels `readout px-3 pt-6 pb-2`.
- **Panes:** `@container/list rounded-tl-frame bg-sheet` and `@container/reader rounded-t-frame bg-sheet`. Toolbars `h-16`. List width `md:w-[24rem] xl:w-[26rem]` or user-resized. A list top fade: `mask-[linear-gradient(to_bottom,transparent,black_10px)]`.
- **Floating dock** (compose): `fixed rounded-card bg-sheet shadow-lift ring-1 ring-border dark:glass animate-rise-in`, header and footer `h-16`, minimized to an `h-12 w-64 rounded-pill` chip with a signal dot.

## Focus, hover, disabled

| State | [B] | [R] |
|---|---|---|
| Focus | global `outline: 2px solid accent; outline-offset: 2px`, or `focus-visible:border-accent` on inputs | `outline-none focus-visible:ring-2 focus-visible:ring-ring` |
| Hover (chrome) | colour only: `text-ink-dim -> text-ink / text-accent` | surface steps up: `hover:bg-card`, `dark:hover:bg-accent`; icons `text-ink-2 -> text-foreground` |
| Hover (card) | `focus-within:bg-raised`, title `group-hover:text-accent` | `lift` utility or `hover:shadow-lift` |
| Press | (none) | `active:scale-95` circles, `active:scale-[0.98]` CTAs, `translate-y-px` buttons |
| Disabled | `opacity-40` | `opacity-50 pointer-events-none` |

## Z-index ladder

`z-10` card content above its overlay link · `z-20` sticky bars · `z-30` scrims, mobile tab bar · `z-40` drawers · `z-50` dialogs, select content · `z-[200]` tooltip and popover positioners.
