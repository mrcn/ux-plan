# Flow-Next OOUX

A flow-next skill that applies ORCA/OOUX methodology to spec files. Generates UX artifacts in `.flow/ux/` for use by `flow-next:plan` and `flow-next:work`.

## Installation

```bash
# Project-level
git clone https://github.com/mrcn/ux-plan .claude/skills/flow-next-ooux

# Global
git clone https://github.com/mrcn/ux-plan ~/.claude/skills/flow-next-ooux
```

## Usage

```bash
/flow-next:ooux <spec-file.md>
/flow-next:ooux <spec-file.md> --quick
```

Examples:
```bash
/flow-next:ooux SPEC.md                    # Full app spec
/flow-next:ooux PRD.md                     # Product requirements
/flow-next:ooux .flow/epics/fn-2.md        # Specific epic
/flow-next:ooux features/onboarding.md --quick
```

## What It Does

Reads spec file → runs 5 UX phases → outputs to `.flow/ux/` → ready for flow-next:plan

```
[Spec File] → Research → ORCA → Wireframes → Writing → Usability
                                    ↓
                            .flow/ux/{name}/
                                    ↓
                          /flow-next:plan (uses artifacts)
                                    ↓
                          /flow-next:work (implements)
```

## Output

```
.flow/
├── epics/           # flow-next epics
├── tasks/           # flow-next tasks
└── ux/              # OOUX artifacts
    └── {spec-name}/
        ├── 01-research.md
        ├── 02-orca.md
        ├── 03-wireframes.md
        ├── 04-writing.md
        └── 05-usability.md
```

## The 5 Phases

### 1. Research
- Target persona (who)
- Jobs-to-be-done (why)
- Happy path flow
- Edge cases, success metrics

### 2. ORCA Analysis

| Component | What | Example |
|-----------|------|---------|
| **Objects** | Nouns | Book, User, Order |
| **Relationships** | Connections | User has many Books |
| **CTAs** | Verbs | Upload, Delete, Play |
| **Attributes** | Properties | Title, Duration |
| **States** | Conditions | loading, error, empty |

### 3. Wireframes (WireMD)
```wiremd
[[ Logo | Library | Settings ]]
# My Books
[Search___] [Filter v]
| Title | Author | [Play] |
[Upload New Book]
```

### 4. UX Writing
- Headlines, CTA labels
- Error messages, empty states
- Microcopy, tooltips

### 5. Usability
- Nielsen's 10 heuristics
- WCAG 2.1 AA
- Critical issues flagged

## Flow-Next Integration

### ORCA → Implementation

`/flow-next:plan` uses ORCA to inform architecture:

| ORCA | Technical Decision |
|------|-------------------|
| Objects | Data models, DB tables, types |
| Relationships | Foreign keys, API nesting |
| CTAs | Endpoints, mutations, actions |
| Attributes | Props, form fields, columns |
| States | State machine, UI states |

### Workflow

```bash
# 1. Run OOUX on your spec
/flow-next:ooux SPEC.md

# 2. Plan implementation (auto-references .flow/ux/)
/flow-next:plan "Build the app"

# 3. Execute
/flow-next:work fn-1
```

## Options

| Flag | Description |
|------|-------------|
| `--quick` | ~15 min vs ~45 min |
| `--phase <n>` | Single phase (1-5) |
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

| `/flow-next:ooux` first | `/flow-next:plan` directly |
|------------------------|---------------------------|
| New features | Bug fixes |
| User-facing changes | Refactors |
| New screens/flows | Backend-only |
| Redesigns | Config changes |
| PRD → Implementation | Technical debt |

## File Structure

```
flow-next-ooux/
├── SKILL.md
├── steps.md
├── agents/
│   ├── ux-research-scout.md
│   ├── ux-orca-scout.md
│   ├── ux-wireframe-scout.md
│   ├── ux-writing-scout.md
│   └── ux-usability-scout.md
└── resources/templates/
```

## Background

ORCA = Object-Oriented UX by Sophia Voychehovski. Defines structural foundation (objects, relationships, actions, attributes) before visual design, bridging UX and information architecture.

## License

MIT
