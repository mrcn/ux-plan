---
name: flow-next-ooux
description: "Apply ORCA/OOUX methodology to a spec file before implementation. Takes any markdown (PRD, SPEC, epic) and generates UX artifacts in .flow/ux/ for use by flow-next:plan and flow-next:work. Use before /flow-next:plan for user-facing features."
---

# Flow-Next OOUX

Apply Object-Oriented UX (OOUX/ORCA) methodology to any spec file. Generates structured UX artifacts in `.flow/ux/` that integrate with flow-next planning and implementation.

## Input

```
/flow-next:ooux <spec-file.md>
/flow-next:ooux <spec-file.md> --quick
```

Examples:
```bash
/flow-next:ooux SPEC.md                        # Full app
/flow-next:ooux PRD.md                         # Product requirements
/flow-next:ooux .flow/epics/fn-2.md            # Specific epic
/flow-next:ooux features/onboarding.md --quick # Quick mode
```

If no file provided, ask: "What spec file should I analyze?"

## Output

All artifacts go to `.flow/ux/{spec-name}/`:

```
.flow/
├── epics/           # flow-next epics
├── tasks/           # flow-next tasks
└── ux/              # OOUX artifacts (this skill)
    └── {spec-name}/
        ├── 01-research.md
        ├── 02-orca.md
        ├── 03-wireframes.md
        ├── 04-writing.md
        └── 05-usability.md
```

## Workflow

```
[Spec File] → Research → ORCA → Wireframes → Writing → Usability
                ↓          ↓         ↓          ↓          ↓
              who/why    O-R-C-A   WireMD     copy      validate
                                    ↓
                            .flow/ux/{name}/
                                    ↓
                          /flow-next:plan (uses artifacts)
                                    ↓
                          /flow-next:work (implements)
```

Follow [steps.md](steps.md) for detailed phase instructions.

## Flow-Next Integration

### Before OOUX
```bash
# Check existing flow context
flowctl list epics
flowctl show fn-1 --json
```

### After OOUX Complete
```bash
# Plan implementation using UX artifacts
/flow-next:plan "Feature X"
# → Automatically detects .flow/ux/{name}/ and references it

# Or create epic directly
flowctl add epic --title "Feature" --ux-artifacts .flow/ux/{name}/
```

### ORCA → Implementation Mapping

flow-next:plan uses ORCA to inform architecture:

| ORCA | Technical Decision |
|------|-------------------|
| Objects | Data models, DB tables, TypeScript types |
| Relationships | Foreign keys, associations, API nesting |
| CTAs | API endpoints, UI actions, mutations |
| Attributes | Component props, form fields, columns |
| States | State machine, UI states, loading/error |

## The 5 Phases

### 1. Research
- Target persona (who)
- Jobs-to-be-done (why)
- Happy path flow
- Edge cases
- Success metrics

### 2. ORCA Analysis
- **Objects**: Core entities (nouns)
- **Relationships**: How objects connect
- **CTAs**: Actions per object (verbs)
- **Attributes**: Visible properties
- **States**: Per-object state inventory

### 3. Wireframes (WireMD)
- Screen layouts
- State variations (empty, loading, error, success)
- Responsive notes
- Interaction notes

### 4. UX Writing
- Headlines, titles
- CTA labels
- Error messages
- Empty states
- Microcopy

### 5. Usability
- Nielsen's 10 heuristics
- WCAG 2.1 AA
- Edge case coverage
- Critical issues flagged

## Options

| Flag | Description |
|------|-------------|
| `--quick` | Abbreviated (~15 min vs ~45 min) |
| `--phase <n>` | Run single phase (1-5) |
| `--skip-wireframes` | Skip phase 3 |

## Quick Mode

| Phase | Full | Quick |
|-------|------|-------|
| Research | Full | Goals + happy path |
| ORCA | Full | Full (always) |
| Wireframes | All | **Skipped** |
| Writing | All | CTAs + errors |
| Usability | Full | Checklist only |

## When to Use

| Use `/flow-next:ooux` first | Use `/flow-next:plan` directly |
|----------------------------|-------------------------------|
| New features | Bug fixes |
| User-facing changes | Refactors |
| New screens/flows | Backend-only changes |
| Redesigns | Config changes |
| PRD → Implementation | Technical debt |

## Success Criteria

- [ ] Research identifies clear persona and goals
- [ ] ORCA defines all objects and CTAs
- [ ] Wireframes show key screens (unless skipped)
- [ ] Writing covers all user-facing copy
- [ ] Usability finds no critical issues
- [ ] Artifacts in `.flow/ux/` ready for flow-next:plan
