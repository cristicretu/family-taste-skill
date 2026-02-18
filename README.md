# family-taste

A skill for AI coding agents encoding the design philosophy behind [Family](https://family.co) — a product widely praised for feeling *alive*, *welcoming*, and *intentional*.

## Available Skills

### design-with-taste

Apply the "Family Values" design philosophy to every UI you build. Enforces three core principles — **Simplicity** (gradual revelation), **Fluidity** (seamless transitions), and **Delight** (selective emphasis) — so that every output feels crafted, intentional, and alive.

**Use when:**
- Building any user-facing interface, component, or page
- Creating frontends, dashboards, landing pages, or apps
- The result should feel alive — not like generic AI output

**Principles covered:**
- **Simplicity** — One primary action per view, progressive disclosure, context-preserving overlays
- **Fluidity** — No instant show/hide, shared element transitions, directional consistency, the golden easing curve
- **Delight** — Delight-Impact Curve, animated numbers, celebration moments, polished empty states

## Installation

```bash
npx skills add cristicretu/family-taste-skill
```

## Usage

Once installed, the agent will apply the Family design philosophy whenever building UI. Pair it with `frontend-design` for maximum effect — this skill handles *feel* and *interaction quality* while `frontend-design` handles visual aesthetics.

**Examples:**
```
Build me a transaction dashboard
```
```
Create an onboarding flow for my app
```
```
Design a settings page
```

## Skill Structure

```
skills/
└── design-with-taste/
    └── SKILL.md
```

## Credits

The design philosophy in this skill is entirely the work of **[Benji Taylor](https://benji.org)**, documented in his essay [Family Values](https://benji.org/family-values). The three pillars — Simplicity, Fluidity, and Delight — were developed while building [Family](https://family.co), a self-custody crypto wallet for iOS.

This skill is a structured reformatting of his ideas to make them actionable for AI coding agents. All credit belongs to Benji and the Family team.

## License

MIT
