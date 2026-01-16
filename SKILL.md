---
name: ux-plan
description: "Plan features with UX-first process using ORCA methodology. Runs research → ORCA (objects/relationships/CTAs/attributes) → wireframes → writing → usability, then calls /flow-next:plan. Use for new features instead of /flow-next:plan directly."
---

# UX Plan

Plan features with a complete UX process before implementation. This skill enforces UX thinking by running 5 sequential phases, creating artifacts, then invoking `/flow-next:plan` with full UX context.

## Input

Full request: $ARGUMENTS

Accepts:
- Feature description in natural language
- Existing Flow ID to add UX artifacts to

Examples:
- `/ux-plan Add dark mode toggle to settings`
- `/ux-plan User onboarding flow with tutorial`
- `/ux-plan fn-1` (add UX to existing epic)

If empty, ask: "What feature should I plan? Describe it in 1-3 sentences."

## Workflow

Follow [steps.md](steps.md) exactly. Each phase is **sequential** - outputs feed into the next phase.

```
Research → ORCA → Wireframes → Writing → Usability → /flow-next:plan
   ↓         ↓         ↓          ↓          ↓
 who/why   O-R-C-A   WireMD     copy      validate
```

## Output

All UX artifacts go into `.flow/ux/{feature-slug}/`:
- `01-research.md` - Persona, goals, flows, metrics
- `02-orca.md` - Objects, Relationships, CTAs, Attributes
- `03-wireframes.md` - WireMD syntax wireframes
- `04-writing.md` - Copy, errors, empty states
- `05-usability.md` - Heuristics, a11y, edge cases

After UX phases complete, `/flow-next:plan` is invoked automatically.

## When to Use

| Use `/ux-plan` for | Use `/flow-next:plan` for |
|-------------------|--------------------------|
| New features | Bug fixes |
| User-facing changes | Refactors |
| New screens/flows | Backend changes |
| Redesigns | Config changes |

## Quick Mode (Optional)

For smaller features, use `--quick` to run abbreviated process:
```
/ux-plan --quick "Add logout button"
```
Quick mode: Research → ORCA → Writing (skips wireframes + full usability)
