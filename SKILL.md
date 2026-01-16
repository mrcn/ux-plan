---
name: flow-next-ux-plan
description: "UX-first feature planning with ORCA methodology. Runs research → ORCA (objects/relationships/CTAs/attributes) → wireframes → writing → usability, then auto-invokes /flow-next:plan. Use for NEW FEATURES instead of /flow-next:plan. Triggers on /flow-next:ux-plan."
---

# UX Plan (Flow-Next Integration)

Plan features with complete UX process before implementation. Enforces UX thinking by running 5 sequential phases, creating artifacts in `.flow/ux/`, then invoking `/flow-next:plan` with full UX context.

## Input

Full request: $ARGUMENTS

Accepts:
- Feature description in natural language
- Existing Flow ID to add UX artifacts to

Examples:
```
/flow-next:ux-plan Add dark mode toggle to settings
/flow-next:ux-plan User onboarding flow with tutorial
/flow-next:ux-plan fn-1   # add UX to existing epic
```

If empty, ask: "What feature should I plan? Describe it in 1-3 sentences."

## Workflow

Follow [steps.md](steps.md) exactly. Each phase is **sequential** - outputs feed into next.

```
Research → ORCA → Wireframes → Writing → Usability → /flow-next:plan
   ↓         ↓         ↓          ↓          ↓
 who/why   O-R-C-A   WireMD     copy      validate
```

## Agents

Run each agent sequentially using Task tool:

| Phase | Agent | Input |
|-------|-------|-------|
| 1 | `flow-next:ux-research-scout` | Feature request |
| 2 | `flow-next:ux-orca-scout` | Feature + 01-research.md |
| 3 | `flow-next:ux-wireframe-scout` | Feature + 02-orca.md |
| 4 | `flow-next:ux-writing-scout` | Feature + 03-wireframes.md |
| 5 | `flow-next:ux-usability-scout` | Feature + all artifacts |

## Output

All UX artifacts go into `.flow/ux/{feature-slug}/`:
```
.flow/
└── ux/
    └── {feature-slug}/
        ├── 01-research.md      # Persona, goals, flows, metrics
        ├── 02-orca.md          # Objects, Relationships, CTAs, Attributes
        ├── 03-wireframes.md    # WireMD syntax wireframes
        ├── 04-writing.md       # Copy, errors, empty states
        └── 05-usability.md     # Heuristics, a11y, edge cases
```

After UX phases complete, `/flow-next:plan` is invoked automatically with UX context.

## Flow-Next Integration

### Before UX Plan
```bash
# Check if Flow ID provided
flowctl show fn-1 --json 2>/dev/null
flowctl cat fn-1 2>/dev/null
```

### After UX Complete
```bash
# Create epic with UX artifacts linked
flowctl add epic --title "Feature Name" --spec .flow/ux/{slug}/

# Or invoke /flow-next:plan which will detect UX artifacts
```

### Memory Integration
```bash
# Store successful UX pattern for future reference
npx @claude-flow/cli@latest memory store \
  --namespace ux-patterns \
  --key "{feature-slug}" \
  --value "ORCA objects: X, Y, Z. Key insight: ..."
```

## When to Use

| Use `/flow-next:ux-plan` for | Use `/flow-next:plan` for |
|-----------------------------|--------------------------|
| New features | Bug fixes |
| User-facing changes | Refactors |
| New screens/flows | Backend changes |
| Redesigns | Config changes |
| UI/UX improvements | API changes |

## Quick Mode

For smaller features, use `--quick`:
```
/flow-next:ux-plan --quick "Add logout button"
```

Quick mode runs abbreviated process:
- Research: Goals + happy path only
- ORCA: Full
- Wireframes: **SKIPPED**
- Writing: CTAs + errors only
- Usability: Checklist only

Time: ~15 min vs ~45 min for full process.

## Success Criteria

Before moving to `/flow-next:plan`:
- [ ] Research identifies clear persona and goals
- [ ] ORCA defines all objects and CTAs
- [ ] Wireframes show key screens (unless --quick)
- [ ] Writing covers all user-facing copy
- [ ] Usability finds no critical issues
