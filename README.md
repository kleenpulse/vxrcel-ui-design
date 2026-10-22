# vxrcel-ui-design

The Vxrcel house design system as an agent skill: quiet greyscale chrome, one signal hue, mono readouts, one spring for everything that slides. It ships with three ready-made skins, Bench, Ember and Nebula.

## Install

```bash
npx skills add kleenpulse/vxrcel-ui-design
```

Works with Claude Code, Codex, Cursor, Copilot, and anything else that reads `SKILL.md`.

## Why this one

Generic "make it look good" prompts produce generic UI. This skill was distilled from two shipped apps: Motion Garden, a component catalog, and Raven, a mail client. It gives the agent exact tokens, type roles, motion constants and component class strings. It also gives the rules that keep them coherent, like one signal hue and roles instead of hex. The skins look nothing alike, but they share one vocabulary, so a component built for one ports to another by swapping a utility name.

| Skin | Mood | Signal |
|---|---|---|
| **Bench** | Graphite instrument bench, squared, Space Mono display | Amethyst |
| **Ember** | Warm paper bento, round, flat | Coral |
| **Nebula** | Deep-teal lit glass, round | Gold + cyan |

## What it does

- Installs a skin as paste-ready Tailwind v4 CSS (`@theme inline` bridge, light and dark blocks)
- Wires fonts (Geist, Geist Mono, plus the skin's display face) and `next-themes`
- Adds the motion constants (`TAB_SPRING` and the others), entrances, the View Transition theme reveal, and all three reduced-motion layers
- Builds with recipes taken from the source apps: buttons, inputs, pill tabs, panels, list rows, status chips, empty states, skeletons, nav rail, docs shell
- Guides you through making a fourth skin with the same vocabulary and contrast targets
- Ends with a grep-able self-check

## What it refuses to do

- Hard-code hex, `bg-white` or `bg-black` in components
- Add a second accent colour to the chrome
- Ship one theme only, or skip reduced motion or reduced transparency
- Use the signal colour for decoration
- Use a `src/` directory or the Pages Router

## Usage

- "Build the settings page in our design system, Ember skin"
- "Set up a new Next.js app with the bench skin"
- "Make this dashboard look like Motion Garden"
- "Add a Nebula dark mode to this app"
- "Make a new skin for a calendar app, same system, green signal"
- "Check this page for design-system drift"

## Layout

```
SKILL.md                    core rules, vocabulary, method, self-check
references/skins/bench.md   Motion Garden tokens and signature moves
references/skins/ember.md   Raven light tokens + shared Raven layer
references/skins/nebula.md  Raven dark tokens, atmosphere, glass
references/motion.md        springs, keyframes, reveal, theme transition, reduced motion
references/components.md    class-string recipes for both skin families
references/new-skin.md      building a fourth skin
```

## License

MIT
