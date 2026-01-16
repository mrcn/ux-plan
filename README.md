# UX Plan

A Claude Code skill that applies ORCA UX methodology to any spec file. Takes a markdown file (feature, app, PRD) and generates structured UX artifacts.

## Installation

```bash
# Project-level (recommended)
git clone https://github.com/mrcn/ux-plan .claude/skills/ux-plan

# Global (all projects)
git clone https://github.com/mrcn/ux-plan ~/.claude/skills/ux-plan
```

## Usage

```bash
/ux-plan <spec-file.md>
/ux-plan <spec-file.md> --quick
/ux-plan <spec-file.md> --output <dir>
```

Examples:
```bash
/ux-plan SPEC.md                           # Analyze app spec
/ux-plan features/dark-mode.md             # Analyze feature
/ux-plan PRD.md --output docs/ux           # Custom output dir
/ux-plan specs/onboarding.md --quick       # Quick mode
```

## What It Does

Reads any markdown spec and runs 5 sequential UX phases:

```
[Spec File] → Research → ORCA → Wireframes → Writing → Usability
                ↓          ↓         ↓          ↓          ↓
              who/why    O-R-C-A   WireMD     copy      validate
```

## Output

Default: `.ux/{spec-name}/`

```
.ux/
└── dark-mode/
    ├── 01-research.md      # Persona, goals, flows, metrics
    ├── 02-orca.md          # Objects, Relationships, CTAs, Attributes
    ├── 03-wireframes.md    # WireMD layouts + state variations
    ├── 04-writing.md       # All copy, errors, empty states
    └── 05-usability.md     # Heuristics, accessibility, issues
```

## Options

| Flag | Description |
|------|-------------|
| `--quick` | Abbreviated (~15 min vs ~45 min) |
| `--output <dir>` | Custom output directory |
| `--phase <n>` | Run single phase (1-5) |
| `--skip-wireframes` | Skip phase 3 |

## The 5 Phases

### 1. Research
- Target persona
- Jobs-to-be-done
- Happy path flow
- Edge cases
- Success metrics

### 2. ORCA Analysis

| Component | What | Example |
|-----------|------|---------|
| **Objects** | Nouns users interact with | Book, User, Order |
| **Relationships** | How objects connect | User has many Books |
| **CTAs** | Actions per object | Upload, Delete, Play |
| **Attributes** | Visible properties | Title, Duration, Status |
| **States** | Per-object states | loading, error, empty |

### 3. Wireframes (WireMD)

```wiremd
[[ Logo | Library | Settings | Sign Out ]]

# My Books

[Search_______] [Filter v]

| Title | Author | Actions |
| Book 1 | Author A | [Play] |

[Upload New Book]
```

Includes: state variations, responsive notes, interaction notes

### 4. UX Writing
- Headlines, page titles
- CTA labels (verb-first)
- Error messages
- Empty states
- Microcopy, tooltips

### 5. Usability
- Nielsen's 10 heuristics
- WCAG 2.1 AA accessibility
- Edge case coverage
- Critical/major/minor issues

## Quick Mode

| Phase | Full | Quick |
|-------|------|-------|
| Research | Full | Goals + happy path |
| ORCA | Full | Full (always) |
| Wireframes | All screens | **Skipped** |
| Writing | All copy | CTAs + errors |
| Usability | Full | Checklist only |

## Use Cases

| Input | Use For |
|-------|---------|
| `SPEC.md` | Full app UX |
| `features/X.md` | Single feature |
| `PRD.md` | Product requirements |
| `epic.md` | Implementation phase |
| `story.md` | User story scope |

## ORCA → Implementation

Use ORCA output to inform technical architecture:

| ORCA | Implementation |
|------|----------------|
| Objects | Data models, DB tables |
| Relationships | Foreign keys, associations |
| CTAs | API endpoints, UI actions |
| Attributes | Component props, form fields |
| States | State management |

## File Structure

```
ux-plan/
├── SKILL.md              # Skill definition
├── steps.md              # Phase instructions
├── agents/
│   ├── ux-research-scout.md
│   ├── ux-orca-scout.md
│   ├── ux-wireframe-scout.md
│   ├── ux-writing-scout.md
│   └── ux-usability-scout.md
└── resources/templates/
    ├── research.md
    ├── orca.md
    ├── wireframes.md
    ├── writing.md
    └── usability.md
```

## Integration

Works standalone or with other tools:

```bash
# Standalone
/ux-plan SPEC.md
# Creates .ux/SPEC/

# With flow-next
/ux-plan .flow/epics/fn-1.md --output .flow/ux/fn-1
/flow-next:plan "Feature X"  # References .flow/ux/

# With any planning tool
# Just point to .ux/{name}/ artifacts
```

## WireMD Reference

```wiremd
[[ Nav | Items | Here ]]     # Navigation bar
# Page Title                  # Heading
[Button Label]               # Button
[Input placeholder___]       # Text input
[ ] Unchecked  [x] Checked   # Checkboxes
( ) Unselected (x) Selected  # Radio buttons
| Col | Col |                # Table
| Data | Data |
---                          # Divider
```

## Background

ORCA (Object-Oriented UX) methodology by Sophia Voychehovski. Bridges UX design and information architecture by defining the structural foundation (objects, relationships, actions, attributes) before visual design.

## License

MIT
