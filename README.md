# UX Plan (Flow-Next Skill)

A Claude Code skill for UX-first feature planning using ORCA methodology. Integrates with flow-next for seamless implementation planning.

## Installation

### As Flow-Next Skill (Recommended)

```bash
# Clone into your project's .claude/skills directory
git clone https://github.com/mrcn/ux-plan .claude/skills/flow-next-ux-plan

# Or global installation (available in all projects)
git clone https://github.com/mrcn/ux-plan ~/.claude/skills/flow-next-ux-plan
```

### Verify Installation

Restart Claude Code, then:
```
/flow-next:ux-plan --help
```

## Usage

```bash
# Plan a new feature with full UX process
/flow-next:ux-plan Add dark mode toggle to settings

# Add UX to existing Flow epic
/flow-next:ux-plan fn-1

# Quick mode (~15 min vs ~45 min)
/flow-next:ux-plan --quick Add logout button
```

## What It Does

Runs 5 sequential phases before invoking `/flow-next:plan`:

```
Research → ORCA → Wireframes → Writing → Usability → /flow-next:plan
   ↓         ↓         ↓          ↓          ↓
 who/why   O-R-C-A   WireMD     copy      validate
```

## When to Use

| Use `/flow-next:ux-plan` | Use `/flow-next:plan` |
|--------------------------|----------------------|
| New features | Bug fixes |
| User-facing changes | Refactors |
| New screens/flows | Backend changes |
| Redesigns | Config changes |

## The 5 Phases

### Phase 1: Research

Identifies:
- **Target persona** - who, context, expertise
- **Jobs-to-be-done** - main, related, emotional
- **Happy path** - step-by-step ideal flow
- **Edge cases** - errors, empty, loading
- **Success metrics** - measurable outcomes

Output: `.flow/ux/{feature}/01-research.md`

### Phase 2: ORCA Analysis

ORCA = Object-Oriented UX:

| Component | Definition | Example |
|-----------|------------|---------|
| **Objects** | Nouns users interact with | Book, Chapter, User |
| **Relationships** | How objects connect | Book has many Chapters |
| **CTAs** | Verbs/actions per object | Upload, Delete, Play |
| **Attributes** | Visible data/properties | Title, Author, Duration |

Plus **State Inventory** (loading, error, empty, success).

Output: `.flow/ux/{feature}/02-orca.md`

### Phase 3: Wireframes

Uses WireMD syntax (markdown wireframing):

```wiremd
[[ Logo | Library | Settings | Sign Out ]]

# My Books

[Search_______] [Filter v]

| Title | Author | Actions |
| Book 1 | Author A | [Play] |

[Upload New Book]
```

Includes state variations + responsive notes.

Output: `.flow/ux/{feature}/03-wireframes.md`

*Skipped in `--quick` mode*

### Phase 4: UX Writing

Defines all copy:
- Headlines, page titles
- CTA labels (verb-first)
- Error messages (what + how to fix)
- Empty states
- Microcopy, tooltips, confirmations

Output: `.flow/ux/{feature}/04-writing.md`

### Phase 5: Usability

Validates against:
- **Nielsen's 10 Heuristics**
- **WCAG 2.1 AA** accessibility
- Edge case coverage from research

Output: `.flow/ux/{feature}/05-usability.md`

## Output Structure

```
.flow/
└── ux/
    └── {feature-slug}/
        ├── 01-research.md
        ├── 02-orca.md
        ├── 03-wireframes.md
        ├── 04-writing.md
        └── 05-usability.md
```

## Flow-Next Integration

### ORCA → Implementation Mapping

| ORCA | Implementation |
|------|----------------|
| Objects | Data models, database tables |
| Relationships | Foreign keys, associations |
| CTAs | API endpoints, UI actions |
| Attributes | Component props, form fields |
| States | State management, UI states |

### Memory Integration

Stores successful UX patterns for future reference:

```bash
# After successful feature, pattern is stored
npx @claude-flow/cli@latest memory store \
  --namespace ux-patterns \
  --key "dark-mode" \
  --value "Objects: Theme, Preference. CTAs: Toggle, Reset."

# Future invocations search for similar patterns
npx @claude-flow/cli@latest memory search \
  --query "theme settings" \
  --namespace ux-patterns
```

### Flowctl Integration

```bash
# Check existing epic context
flowctl show fn-1 --json
flowctl cat fn-1

# Create epic with UX artifacts
flowctl add epic --title "Feature" --spec .flow/ux/{slug}/
```

## Quick Mode

| Phase | Full | Quick |
|-------|------|-------|
| Research | Full persona, flows | Goals + happy path |
| ORCA | Full | Full |
| Wireframes | All screens | **Skipped** |
| Writing | All copy | CTAs + errors |
| Usability | Full heuristics | Checklist only |

## File Structure

```
flow-next-ux-plan/
├── SKILL.md              # Skill definition
├── steps.md              # Sequential phases
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

## WireMD Syntax

```wiremd
# Navigation
[[ Logo | Nav | Nav | Sign Out ]]

# Buttons
[Primary Button]
[Secondary Button]

# Inputs
[Placeholder_______]

# Checkboxes
[ ] Unchecked
[x] Checked

# Tables
| Col 1 | Col 2 |
| Data | Data |
```

## Prerequisites

- Claude Code with skills support
- flow-next installed (`flowctl` available)
- Optional: `@claude-flow/cli` for memory integration

## Related Skills

- `/flow-next:plan` - Implementation planning (auto-invoked after UX)
- `/flow-next:work` - Execute implementation
- `/flow-next:interview` - Deep spec refinement

## Background

ORCA (Object-Oriented UX) methodology by Sophia Voychehovski bridges UX design and information architecture, ensuring designs are grounded in user mental models.

## License

MIT
