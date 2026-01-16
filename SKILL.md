---
name: flow-next-ooux
description: "Apply OOUX methodology to a spec file. Runs interview (if sparse) → 7 parallel research agents → ORCA analysis → Wireframes → Writing → Usability. Outputs enriched spec + .flow/ux/ artifacts for flow-next:plan. Use before /flow-next:plan for user-facing features."
---

# Flow-Next OOUX

Apply Object-Oriented UX (OOUX) methodology to any spec file. Transforms rough ideas into structured ORCA models through interview and multi-agent research.

## Users

- Solo developers building their own apps
- UX/Product leads ensuring perfect UX before development
- Anyone wanting ORCA-structured specs for flow-next

## Input

```bash
/flow-next:ooux <spec-file.md>
/flow-next:ooux <spec-file.md> --quick
/flow-next:ooux <spec-file.md> --auto      # No pauses between phases
/flow-next:ooux <spec-file.md> --dry-run   # Preview what will happen
```

Examples:
```bash
/flow-next:ooux SPEC.md                     # Full app
/flow-next:ooux PRD.md                      # Product requirements
/flow-next:ooux .flow/epics/fn-2.md         # Specific epic
/flow-next:ooux features/onboarding.md --quick
```

**Input quality varies** - from rough 1-paragraph ideas to detailed PRDs.

## Workflow

```
[Spec File]
    ↓
[Interview] ← If input is sparse, light interview (5-10 questions on core functionality)
    ↓
[Research Agents] ← 7 agents run in parallel (hybrid execution)
    ↓
[ORCA Analysis] ← Extract Objects, Relationships, CTAs, Attributes from research
    ↓
[Wireframes] ← WireMD layouts based on ORCA objects
    ↓
[UX Writing] ← Real copy for all CTAs, errors, empty states
    ↓
[Usability] ← Full validation, auto-fix minor issues
    ↓
[Enriched Spec] ← ORCA-structured output for flow-next:plan
```

## Phase 0: Interview (Conditional)

**Triggers when**: Input spec is sparse (rough idea, < ~500 words)

Light interview (~5-10 questions) focused on core functionality:
- What does this actually do?
- Who is the primary user?
- What are the key actions?
- What are the main screens/views?

Interview outputs feed into research agents.

## Phase 1: Research Agents (Parallel Swarm)

7 specialized agents run in hybrid parallel/sequential based on dependencies:

| Agent | Focus | Output |
|-------|-------|--------|
| **User Researcher** | Persona, JTBD, user mental models | Nouns for ORCA |
| **Competitor Researcher** | Analyze competitor UX patterns | Best practices |
| **Design Pattern Researcher** | UI patterns for this domain | Pattern library |
| **Futurist Designer** | Emerging UI patterns, aesthetic trends | Innovation ideas |
| **Business Innovation Agent** | Novel monetization, engagement | Business model |
| **Accessibility Expert** | WCAG/a11y considerations | A11y requirements |
| **Tech Feasibility Agent** | What's buildable with current stack | Constraints |

Tech Feasibility agent scans `package.json`, `tsconfig.json`, etc. for stack info.

All outputs are concatenated into a single research document.

## Phase 2: ORCA Analysis

Process research outputs using OOUX methodology:

### What Makes a Good Object
Objects are **nouns from user's mental model** (not database tables, not UI elements):
- Real-world things users think about: Products, Users, Reviews, Listings
- High-value anchors that draw users to the system

**NOT Objects**: UI containers, collection views, abstract concepts, actions

### ORCA Components

| Component | Definition | Example |
|-----------|------------|---------|
| **Objects** | Core entities (nouns from user mental model) | Book, User, Order |
| **Relationships** | How objects connect | User has many Books |
| **CTAs** | Actions per object (verbs) | Upload, Delete, Play |
| **Attributes** | Visible properties per object | Title, Duration, Status |
| **States** | Per-object state inventory | loading, error, empty |

### Output Format

Dual output for LLM + human consumption:
- `02-orca.json` - Structured JSON for LLM processing
- `02-orca.md` - Condensed summary for human review

## Phase 3: Wireframes (WireMD)

Text-based wireframes using WireMD syntax:

```wiremd
[[ Logo | Library | Settings | Sign Out ]]

# My Books

[Search_______] [Filter v]

| Title | Author | Actions |
| Book 1 | Author A | [Play] |

[Upload New Book]
```

Includes:
- Screen layouts based on ORCA objects
- State variations (empty, loading, error, success)
- Responsive breakpoint notes
- Interaction notes

**Skipped in --quick mode**

## Phase 4: UX Writing

Generate **real copy** (not placeholders):
- Headlines and page titles
- CTA button labels
- Error messages (what happened + how to fix)
- Empty state copy
- Microcopy (helper text, tooltips)
- Confirmation messages

## Phase 5: Usability Validation

Full validation suite:
- Nielsen's 10 heuristics
- WCAG 2.1 AA accessibility
- Edge case coverage
- Consistency check across all artifacts

**Auto-fixes minor issues** by iterating on previous phases.
**Flags major issues** for user decision.

## Output

### Artifacts in `.flow/ux/`

```
.flow/
├── epics/           # flow-next epics
├── tasks/           # flow-next tasks
└── ux/              # OOUX artifacts
    └── {spec-name}/
        ├── 00-interview.md     # Interview transcript (if conducted)
        ├── 01-research.md      # Concatenated research outputs
        ├── 02-orca.json        # Structured ORCA (for LLM)
        ├── 02-orca.md          # ORCA summary (for human)
        ├── 03-wireframes.md    # WireMD layouts
        ├── 04-writing.md       # All UX copy
        └── 05-usability.md     # Validation results
```

### Enriched Spec

Final output is an **ORCA-structured spec** that can be passed to `/flow-next:plan`:
- Objects, Relationships, CTAs, Attributes as primary structure
- Supporting notes reference the ORCA model
- Compatible with flow-next:plan input format

### Git Integration

Auto-commits artifacts with message: `ooux: {spec-name}`

## Flow Control

### Default: Pause Between Phases

After each phase:
1. Show output summary
2. User can: **Continue** | **Edit** | **Regenerate** (with free text feedback)

### --auto Flag

Run all phases without pausing.

### --dry-run Flag

Preview what will happen:
- Which agents will run
- Which phases will execute
- Estimated time

## Options

| Flag | Description |
|------|-------------|
| `--quick` | ~15 min: Skip wireframes, abbreviated research |
| `--auto` | No pauses between phases |
| `--dry-run` | Preview execution plan |
| `--phase <n>` | Run single phase (0-5) |
| `--skip-wireframes` | Skip phase 3 |

## Quick Mode

| Phase | Full (~30 min) | Quick (~15 min) |
|-------|----------------|-----------------|
| Interview | If needed | If needed |
| Research | 7 agents | 3 agents (user, competitor, tech) |
| ORCA | Full | Full (always) |
| Wireframes | All screens | **Skipped** |
| Writing | All copy | CTAs + errors only |
| Usability | Full + auto-fix | Checklist only |

## Error Handling

If a research agent fails or returns poor results:
- **Stop and ask user** how to proceed
- Options: Retry, Skip agent, Abort

## Completion

After all phases:
1. Report artifacts created
2. Highlight key ORCA objects and critical issues
3. Ask: "Run /flow-next:plan with enriched spec?"

## When to Use

| Use `/flow-next:ooux` first | Use `/flow-next:plan` directly |
|----------------------------|-------------------------------|
| New features | Bug fixes |
| User-facing changes | Refactors |
| New screens/flows | Backend-only changes |
| Redesigns | Config changes |
| PRD → Implementation | Technical debt |
