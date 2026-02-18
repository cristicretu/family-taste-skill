---
name: design-with-taste
description: Apply the "Family Values" design philosophy to every UI you build. Use this skill whenever creating frontends, components, apps, landing pages, dashboards, or any user-facing interface. Enforces three core principles — Simplicity (gradual revelation), Fluidity (seamless transitions), and Delight (selective emphasis) — so that every output feels crafted, intentional, and alive. Prevents generic, static, lifeless UI. Works alongside other skills like frontend-design, docx, pptx, etc.
---

# Design with Taste

This skill encodes the design philosophy behind [Family](https://family.co) — a product widely praised for feeling *alive*, *welcoming*, and *intentional*. These aren't abstract ideals. They are concrete, actionable principles that separate craft from slop.

**Read this before writing any UI code. Every time.**

The user wants something built. Your job is to make it feel like a human who gives a shit designed it.

---

## The Three Pillars

Every interface you build must be evaluated against these three pillars. They are ordered by priority — you cannot have Delight without Fluidity, and you cannot have Fluidity without Simplicity.

### 1. Simplicity — Gradual Revelation

> "Each action by the user makes the interface unfold and evolve, much like walking through a series of interconnected rooms."

**The problem**: Most UIs dump everything on screen at once. Every feature, every option, every edge case — all visible, all the time. This is lazy. It transfers the cognitive burden from the designer to the user.

**The principle**: Show only what matters *right now*. Reveal complexity progressively as the user goes deeper. The interface should feel like walking through a series of rooms — you see a glimpse of what's next before you get there.

**Concrete rules**:

- **One primary action per view.** If a screen has two equally weighted CTAs, you've failed. Identify the single most important thing and make everything else secondary.
- **Progressive disclosure over feature dumps.** Use expandable sections, layered trays, step-by-step flows. Never show a 12-field form when you can show 3 steps of 4 fields.
- **Context-preserving overlays over full-page navigations.** When possible, use sheets/trays/modals that overlay the current context instead of navigating away. The user should never feel lost.
- **Compact signals invite engagement.** Small, approachable UI elements (a card, a pill, a collapsed section) signal "this is manageable" — far better than a sprawling full-screen layout.
- **Vary heights of stacked layers.** If you stack sheets/modals/trays, each subsequent layer must be a different height so the user can see the progression. Never stack two identical-height sheets.
- **Every tray/sheet/modal needs a clear title and dismiss action.** The user must always know what they're looking at and how to get back.

**Implementation patterns**:

```
// GOOD: Progressive tray system
<Sheet>
  <SheetTrigger>View Details</SheetTrigger>
  <SheetContent className="h-[40vh]"> <!-- First tray: compact -->
    <SheetHeader>Transaction Details</SheetHeader>
    <!-- Core info only -->
    <Button>Confirm Send</Button>
  </SheetContent>
</Sheet>

// BAD: Everything on one screen
<div className="grid grid-cols-3 gap-4">
  {/* 47 fields, 12 buttons, 3 tables, user is overwhelmed */}
</div>
```

**Self-check**: Look at your screen. Can the user tell within 1 second what they should do next? If not, simplify.

---

### 2. Fluidity — Seamless Transitions

> "We fly instead of teleport."

**The problem**: Static transitions make products feel dead. A dead product feels uncared for. When elements appear/disappear instantly, the user loses their sense of spatial orientation within the app — where did that come from? Where did it go?

**The principle**: Every state change should have a visible, logical transition. Elements should move *from* somewhere *to* somewhere. The user should always be able to follow the trail.

**Concrete rules**:

- **No instant show/hide.** Every element that appears or disappears must animate. Fade, slide, scale, morph — pick one that makes spatial sense.
- **Shared element transitions.** If an element exists in State A and State B (e.g., a card that expands into a detail view), it should visually *travel* between those states. Never unmount and remount — morph.
- **Directional consistency.** If a user navigates "right" (next tab, next step), content should slide from right-to-left. If they go "back," it slides left-to-right. Tabs left of current = slide left. This creates spatial memory.
- **Text morphing over instant replacement.** When a button label changes (e.g., "Continue" → "Confirm"), animate the transition. Shared letters can stay fixed while others morph. At minimum, crossfade — never just swap text.
- **Persistent elements stay put.** If a header, card, or element persists across a transition, it should NOT animate out and back in. It stays. Only the changing parts move. This is critical — redundant animations kill fluidity.
- **Loading states inherit position.** A spinner or skeleton should appear exactly where the content will be. A pending transaction spinner should move to where the user can find it (e.g., the activity tab icon).
- **Micro-directional cues.** Small icons like chevrons, arrows, and carets should animate to reflect the action taken (e.g., `→` morphs to `←` when navigating forward, a chevron rotates when an accordion opens).

**Implementation patterns**:

```css
/* Fluid page transitions */
.page-enter { opacity: 0; transform: translateX(20px); }
.page-enter-active { opacity: 1; transform: translateX(0); transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1); }
.page-exit { opacity: 1; transform: translateX(0); }
.page-exit-active { opacity: 0; transform: translateX(-20px); transition: all 0.3s cubic-bezier(0.16, 1, 0.3, 1); }

/* Shared element: card expanding to detail */
.card { transition: all 0.4s cubic-bezier(0.16, 1, 0.3, 1); }
.card.expanded { position: fixed; inset: 0; border-radius: 0; }

/* Text morphing: crossfade at minimum */
.button-label { transition: opacity 0.2s, transform 0.2s; }
.button-label-exit { opacity: 0; transform: translateY(-4px); position: absolute; }
.button-label-enter { opacity: 0; transform: translateY(4px); }
.button-label-enter-active { opacity: 1; transform: translateY(0); }
```

```jsx
// Directional tab transitions
const direction = newIndex > currentIndex ? 1 : -1;
<motion.div
  key={currentTab}
  initial={{ x: direction * 20, opacity: 0 }}
  animate={{ x: 0, opacity: 1 }}
  exit={{ x: -direction * 20, opacity: 0 }}
  transition={{ duration: 0.3, ease: [0.16, 1, 0.3, 1] }}
/>
```

**The golden easing curve**: `cubic-bezier(0.16, 1, 0.3, 1)` — fast start, gentle settle. This is the "feels alive" curve. Use it as your default. Avoid linear. Avoid `ease-in-out` for spatial movement (it feels sluggish). Reserve `ease-in` for exits only.

**Self-check**: Record your screen and play it back at 0.5x speed. Can you follow every element's journey? If anything teleports, add a transition.

---

### 3. Delight — Selective Emphasis

> "Mastering delight is mastering selective emphasis."

**The problem**: Most products either have zero personality (corporate slop) or overdo it (everything bounces and sparkles, which quickly becomes annoying). Both miss the point.

**The principle**: Delight is about making specific moments feel special. The key insight is the **Delight-Impact Curve** — the less frequently a feature is used, the MORE delightful it should be. Daily actions need efficiency with subtle touches. Rare actions (onboarding, achievements, destructive actions) deserve theatrical moments.

**Concrete rules**:

- **Delight-Impact Curve**: Frequently used features → subtle touches (comma animations, micro-transitions). Rarely used features → theatrical moments (confetti, custom animations, sound design, easter eggs).
- **Polish everything equally.** The settings page, the empty state, the error screen — they all get the same level of care as the hero section. An unpolished corner makes the whole product feel unpolished. "Like a fancy restaurant with a dirty bathroom."
- **Easter eggs reward exploration.** Hide interactive moments in unexpected places. A tap on a QR code triggers a ripple. A swipe reveals a hidden animation. These moments create stories users share.
- **Celebrate completions.** When a user completes a significant action (backup, onboarding, first transaction), reward them. Confetti, a custom animation, a satisfying sound. Don't just show a green checkmark.
- **Make destructive actions satisfying.** Deleting items? Show them tumbling into a trash can. Clearing data? A satisfying sweep animation. Even negative actions can feel good.
- **Animate numbers.** When values change (prices, counts, balances), they should count/flip/morph — never just swap. Commas should shift position smoothly when numbers grow.
- **Empty states are opportunities.** Never show a blank page with "No items yet." Add an animated illustration, a playful arrow pointing to the create button, a warm message. Empty states are first impressions.
- **Sound design (when applicable).** Completion sounds, subtle interaction feedback. Sound reinforces the feeling of physicality and reward.

**Implementation patterns**:

```jsx
// Animated number transitions
function AnimatedNumber({ value }) {
  const [display, setDisplay] = useState(value);
  useEffect(() => {
    // Animate from current to new value
    const start = display;
    const end = value;
    const duration = 600;
    const startTime = performance.now();
    function tick(now) {
      const t = Math.min((now - startTime) / duration, 1);
      const eased = 1 - Math.pow(1 - t, 3); // ease-out cubic
      setDisplay(Math.round(start + (end - start) * eased));
      if (t < 1) requestAnimationFrame(tick);
    }
    requestAnimationFrame(tick);
  }, [value]);
  return <span>{display.toLocaleString()}</span>;
}

// Celebration confetti on completion
function CompletionScreen() {
  return (
    <motion.div
      initial={{ scale: 0.8, opacity: 0 }}
      animate={{ scale: 1, opacity: 1 }}
      transition={{ type: "spring", damping: 15, stiffness: 200 }}
    >
      <ConfettiExplosion />
      <h2>You're all set!</h2>
    </motion.div>
  );
}

// Playful empty state
function EmptyState() {
  return (
    <div className="flex flex-col items-center gap-4 py-16">
      <motion.div
        animate={{ y: [0, -8, 0] }}
        transition={{ repeat: Infinity, duration: 2, ease: "easeInOut" }}
      >
        <IllustrationIcon />
      </motion.div>
      <p className="text-muted">Nothing here yet</p>
      <motion.div
        animate={{ x: [0, 4, 0] }}
        transition={{ repeat: Infinity, duration: 1.5 }}
      >
        <ArrowRight /> <span>Create your first item</span>
      </motion.div>
    </div>
  );
}
```

**Self-check**: Show your UI to someone for 30 seconds. Do they smile at any point? If not, you haven't added enough delight. Do they look annoyed? You've added too much to high-frequency interactions.

---

## The Taste Checklist

Run this checklist before considering any UI "done":

### Simplicity
- [ ] Each screen has ONE clear primary action
- [ ] Complex flows are broken into digestible steps
- [ ] Information is revealed progressively, not all at once
- [ ] Context is preserved during transitions (overlays > navigations)
- [ ] The user always knows where they are and how to go back

### Fluidity
- [ ] Zero instant show/hide — everything animates
- [ ] Shared elements morph between states, not unmount/remount
- [ ] Directional transitions match spatial logic (left/right, in/out)
- [ ] Persistent elements don't redundantly animate
- [ ] Text changes are animated (crossfade at minimum)
- [ ] Loading states appear where content will be
- [ ] The golden curve `cubic-bezier(0.16, 1, 0.3, 1)` is the default

### Delight
- [ ] Frequently used features have subtle micro-interactions
- [ ] Infrequently used features have memorable moments
- [ ] Empty states are designed, not afterthoughts
- [ ] Completions are celebrated
- [ ] Numbers animate when they change
- [ ] All corners of the app are equally polished
- [ ] At least one moment would make someone say "oh, that's nice"

### General Taste
- [ ] No generic AI aesthetics (Inter font, purple gradients, cookie-cutter layouts)
- [ ] Typography is intentional — display font + body font pairing
- [ ] Color palette has a dominant color with sharp accents, not evenly distributed pastels
- [ ] Spacing is generous and consistent
- [ ] The interface feels like a physical space you move through, not a slideshow
- [ ] Every pixel looks like a human who cares placed it there

---

## Anti-Patterns — Things That Kill Taste

These are the hallmarks of slop. If you catch yourself doing any of these, stop and fix it:

1. **Static tab switches.** Tabs that instant-swap content with no transition. Add directional sliding.
2. **Modals that pop from nowhere.** Modals should grow from their trigger element or slide from an edge. Never just `opacity: 0 → 1` in the center.
3. **Skeleton screens that don't match layout.** If your skeleton has 3 bars but your content has 5 lines, you've broken the illusion.
4. **Redundant animations.** A header that fades out and back in during a page transition even though it stays the same. Persistent elements must persist.
5. **Linear easing.** Nothing in the physical world moves linearly. Use spring physics or cubic-bezier curves.
6. **"No items" empty text.** An empty state with just text and no visual design. This is a first impression — treat it like one.
7. **Uniform sizing in stacked layers.** Two modals/sheets of the same height stacked on each other. Each layer must be a different height.
8. **Toast notifications for important feedback.** Toasts are for background info. Important outcomes (success, error, completion) deserve inline, contextual, animated feedback.
9. **Forms that are just stacked inputs.** Break long forms into focused steps with transitions between them.
10. **Buttons that don't respond to interaction.** Every clickable element needs hover, active, and focus states. A button that doesn't visually react to being pressed feels broken.

---

## Easing & Timing Reference

| Use Case | Easing | Duration |
|---|---|---|
| Element entering | `cubic-bezier(0.16, 1, 0.3, 1)` | 300-400ms |
| Element exiting | `cubic-bezier(0.4, 0, 1, 1)` | 200-250ms |
| Shared element morph | `cubic-bezier(0.16, 1, 0.3, 1)` | 350-500ms |
| Micro-interaction (hover, press) | `cubic-bezier(0.2, 0, 0, 1)` | 100-150ms |
| Spring (bouncy) | `type: "spring", damping: 20, stiffness: 300` | auto |
| Spring (smooth) | `type: "spring", damping: 30, stiffness: 200` | auto |
| Number counting | `ease-out cubic` | 400-800ms |
| Page transition | `cubic-bezier(0.16, 1, 0.3, 1)` | 300ms |
| Stagger delay between items | - | 30-60ms per item |

---

## How to Use This Skill

1. **Read this before every UI task.** Not after. Before.
2. **Apply all three pillars.** Simplicity, Fluidity, Delight — in that order of priority. You can't polish a turd with animations.
3. **Run the checklist.** Before delivering, go through every checkbox. Fix what's missing.
4. **Pair with frontend-design skill.** This skill handles *feel* and *interaction quality*. Frontend-design handles *visual aesthetics* (typography, color, layout). Use both.
5. **When in doubt, animate.** It's easier to tone down animation than to add life to a dead interface.
6. **Record and review at 0.5x.** This is the ultimate test. Slow motion reveals every teleport, every jarring cut, every missed opportunity.

The goal is not to make something that "works." The goal is to make something that someone uses and thinks: *"Whoever made this actually gives a shit."*

That's taste.