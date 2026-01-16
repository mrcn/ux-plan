# UX Plan

A Claude Code skill for UX-first feature planning using ORCA methodology. Enforces structured UX thinking before implementation.

## What It Does

Runs 5 sequential phases, creates artifacts, then invokes implementation planning with full UX context:

```
Research → ORCA → Wireframes → Writing → Usability → Implementation
   ↓         ↓         ↓          ↓          ↓
 who/why   O-R-C-A   WireMD     copy      validate
```

## Installation

Copy to your project's Claude skills directory:

```bash
cp -r ux-plan /path/to/project/.claude/skills/
```

Or clone directly:

```bash
git clone https://github.com/your-username/ux-plan .claude/skills/ux-plan
```

## Usage

```bash
# Plan a new feature
/ux-plan Add dark mode toggle to settings

# Add UX to existing Flow epic
/ux-plan fn-1

# Quick mode (abbreviated, ~15 min vs ~45 min)
/ux-plan --quick Add logout button
```

## When to Use

| Use `/ux-plan` for | Use direct planning for |
|-------------------|------------------------|
| New features | Bug fixes |
| User-facing changes | Refactors |
| New screens/flows | Backend changes |
| Redesigns | Config changes |

## The 5 Phases

### Phase 1: Research

**Purpose**: Understand WHO and WHY

The research scout identifies:
- **Target persona** - primary user, context, expertise, constraints
- **Jobs-to-be-done** - main job, related jobs, emotional jobs
- **Happy path** - step-by-step ideal flow
- **Edge cases** - errors, empty states, loading, boundaries
- **Success metrics** - completion rate, time, error rate

Output: `.flow/ux/{feature}/01-research.md`

### Phase 2: ORCA Analysis

**Purpose**: Define structural foundation

ORCA = Object-Oriented UX:

| Component | Definition | Example |
|-----------|------------|---------|
| **Objects** | The "nouns" - things users interact with | Book, Chapter, User |
| **Relationships** | How objects connect | Book has many Chapters |
| **CTAs** | The "verbs" - actions per object | Upload, Delete, Play |
| **Attributes** | Data/properties displayed | Title, Author, Duration |

Plus **State Inventory** - what states each object can be in (loading, error, empty, success).

Output: `.flow/ux/{feature}/02-orca.md`

### Phase 3: Wireframes

**Purpose**: Visualize layouts based on ORCA

Uses WireMD syntax (markdown-based wireframing):

```wiremd
[[ Logo | Library | Settings | Sign Out ]]

# My Books

[Search_______] [Filter v]

---

| Title | Author | Duration | Actions |
| Book 1 | Author A | 2h 30m | [Play] |
| Book 2 | Author B | 1h 45m | [Play] |

[Upload New Book]
```

Creates:
- Key screen layouts
- State variations (default, empty, loading, error, success)
- Responsive breakpoint notes
- Interaction notes

Output: `.flow/ux/{feature}/03-wireframes.md`

*Skipped in `--quick` mode*

### Phase 4: UX Writing

**Purpose**: Define all user-facing copy

Covers:
- **Headlines** - page titles, section headers
- **CTA labels** - button text (verb-first, specific)
- **Error messages** - what happened + how to fix
- **Empty states** - what goes here + how to start
- **Microcopy** - helper text, tooltips, confirmations, loading messages
- **Voice & tone** - brand consistency

Output: `.flow/ux/{feature}/04-writing.md`

### Phase 5: Usability

**Purpose**: Validate before implementation

Checks against:

**Nielsen's 10 Heuristics**:
1. Visibility of system status
2. Match system & real world
3. User control and freedom
4. Consistency and standards
5. Error prevention
6. Recognition over recall
7. Flexibility and efficiency
8. Aesthetic minimalist design
9. Error recovery
10. Help and documentation

**WCAG 2.1 AA Accessibility**:
- Perceivable (alt text, contrast)
- Operable (keyboard, focus)
- Understandable (predictable, errors)
- Robust (valid markup)

**Edge case coverage** from research phase

Output: `.flow/ux/{feature}/05-usability.md`

## Output Structure

All artifacts saved to `.flow/ux/{feature-slug}/`:

```
.flow/
└── ux/
    └── dark-mode-toggle/
        ├── 01-research.md
        ├── 02-orca.md
        ├── 03-wireframes.md
        ├── 04-writing.md
        └── 05-usability.md
```

## Quick Mode

For smaller features, `--quick` runs abbreviated process:

| Phase | Full | Quick |
|-------|------|-------|
| Research | Full persona, flows, metrics | Goals + happy path only |
| ORCA | Full | Full |
| Wireframes | All screens + states | **Skipped** |
| Writing | All copy categories | CTAs + errors only |
| Usability | Full heuristics + a11y | Checklist only |

Time: ~15 min vs ~45 min

## WireMD Syntax Reference

```wiremd
# Navigation
[[ Logo | Nav Item | Nav Item | Sign Out ]]

# Buttons
[Primary Button]
[Secondary Button]

# Inputs
[Placeholder text_______]
[Password__________]

# Checkboxes & Radio
[ ] Unchecked
[x] Checked
( ) Unselected
(x) Selected

# Tables
| Column 1 | Column 2 |
| Data | Data |

# Dividers
---
```

## ORCA Deep Dive

### Objects

Objects are nouns users recognize from real world:

**Good objects**: Book, Message, User, Order, Task
**Bad objects**: DataHandler, Service, Controller (internal, not user-facing)

Rules:
- Must be tangible/concrete
- Users should recognize them
- Clear boundaries

### Relationships

| Type | Example |
|------|---------|
| has many | User has many Books |
| belongs to | Chapter belongs to Book |
| has one | User has one Profile |
| many-to-many | User has many Tags, Tag has many Users |

### CTAs (Calls-to-Action)

Verbs users can do to objects:

| Object | Primary CTAs | Secondary CTAs |
|--------|-------------|----------------|
| Book | Play, Download | Share, Delete |
| Chapter | Play, Bookmark | Skip |
| User | Edit Profile | Logout |

Rules:
- Must be user-initiated (not system actions)
- Verb phrases
- Primary vs Secondary priority

### Attributes

What users see (not database fields):

| Object | Visible Attributes |
|--------|-------------------|
| Book | Title, Author, Cover, Duration, Progress |
| Chapter | Title, Duration, Status (played/unplayed) |
| User | Name, Avatar, Email |

Rules:
- Only user-visible
- Prioritize by importance
- Consider display format

### State Inventory

Every object can be in states:

| Object | States |
|--------|--------|
| Book | uploading, processing, ready, error |
| Chapter | unplayed, playing, paused, completed |
| Upload | selecting, uploading, validating, complete, failed |

## Templates

Blank templates in `resources/templates/`:
- `research.md` - User research template
- `orca.md` - ORCA analysis template
- `wireframes.md` - WireMD wireframe template
- `writing.md` - UX writing template
- `usability.md` - Usability checklist template

## File Structure

```
ux-plan/
├── SKILL.md              # Main skill definition
├── steps.md              # Sequential phase instructions
├── agents/
│   ├── ux-research-scout.md    # Phase 1 agent
│   ├── ux-orca-scout.md        # Phase 2 agent
│   ├── ux-wireframe-scout.md   # Phase 3 agent
│   ├── ux-writing-scout.md     # Phase 4 agent
│   └── ux-usability-scout.md   # Phase 5 agent
└── resources/
    └── templates/
        ├── research.md
        ├── orca.md
        ├── wireframes.md
        ├── writing.md
        └── usability.md
```

## Integration

After UX phases complete, automatically invokes `/flow-next:plan` (or your implementation planner) with UX context.

The implementation plan references UX artifacts and uses ORCA objects to inform technical architecture:
- Objects → Data models
- Relationships → Database schema
- CTAs → API endpoints / UI actions
- Attributes → Component props
- States → UI state management

## Success Criteria

Before moving to implementation:
- [ ] Research identifies clear persona and goals
- [ ] ORCA defines all objects and CTAs
- [ ] Wireframes show key screens (unless --quick)
- [ ] Writing covers all user-facing copy
- [ ] Usability finds no critical issues

## Background

ORCA (Object-Oriented UX) is a methodology developed by Sophia Voychehovski that bridges UX design and information architecture. It ensures designs are grounded in user mental models rather than database schemas.

This skill enforces ORCA thinking + supporting UX processes (research, wireframes, writing, validation) as a prerequisite to implementation planning.

## License

MIT
