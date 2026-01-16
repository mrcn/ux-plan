# Flow-Next OOUX Steps

**CRITICAL**: Phases are SEQUENTIAL. Each phase depends on the previous phase's output.

## Step 0: Initialize

1. Parse arguments:
   - `<spec-file>` - Required path to markdown spec
   - `--quick` - Optional abbreviated mode
   - `--phase <n>` - Optional single phase

2. Read the spec file:
```bash
Read(<spec-file-path>)
```

3. Determine output directory:
```bash
# Always output to .flow/ux/{spec-filename-without-ext}/
# Example: /flow-next:ooux SPEC.md → .flow/ux/SPEC/
# Example: /flow-next:ooux features/dark-mode.md → .flow/ux/dark-mode/
# Example: /flow-next:ooux .flow/epics/fn-2.md → .flow/ux/fn-2/

mkdir -p .flow/ux/{spec-name}
```

4. Check existing flow context:
```bash
# See what epics/tasks exist
flowctl list epics 2>/dev/null
flowctl list tasks 2>/dev/null
```

5. Extract key info from spec:
   - Feature/app name
   - Core functionality described
   - Any existing user info

## Step 1: User Research

**Purpose**: Understand WHO the user is and WHY they need this.

**Input**: Spec file content

Run the research agent:
```
Task ux-research-scout(spec content)
```

**Wait for completion**, then save to `.flow/ux/{spec-name}/01-research.md`

The research agent will:
- Identify target persona (from spec or inferred)
- Define jobs-to-be-done and user goals
- Map the happy path flow
- Identify edge cases and error scenarios
- Define success metrics

**Quick mode**: Goals + happy path only

## Step 2: ORCA Analysis

**Purpose**: Define the OBJECTS users interact with.

**Input**: Spec content + 01-research.md

Run the ORCA agent:
```
Task ux-orca-scout(spec content, research output)
```

**Wait for completion**, then save to `.flow/ux/{spec-name}/02-orca.md`

The ORCA agent produces:
- **Objects**: Core entities (nouns)
- **Relationships**: How objects connect
- **CTAs**: Actions per object (verbs)
- **Attributes**: Data/properties per object
- **States**: Per-object state inventory

Output format: ASCII diagram + markdown matrix tables

**Quick mode**: Full (ORCA is always full)

## Step 3: Wireframes

**Purpose**: Visualize screen layouts based on ORCA objects.

**Input**: Spec content + 02-orca.md

Run the wireframe agent:
```
Task ux-wireframe-scout(spec content, orca output)
```

**Wait for completion**, then save to `.flow/ux/{spec-name}/03-wireframes.md`

The wireframe agent creates:
- WireMD syntax wireframes for key screens
- Layout of objects and CTAs on screen
- State variations (empty, loading, error, success)
- Responsive breakpoint notes

**Skip in --quick mode or with --skip-wireframes**

## Step 4: UX Writing

**Purpose**: Define all copy, labels, and messages.

**Input**: Spec content + wireframes (or ORCA if skipped)

Run the writing agent:
```
Task ux-writing-scout(spec content, wireframes OR orca)
```

**Wait for completion**, then save to `.flow/ux/{spec-name}/04-writing.md`

The writing agent produces:
- Headlines and page titles
- CTA button labels
- Error messages
- Empty state copy
- Microcopy (helper text, tooltips)
- Onboarding text (if applicable)

**Quick mode**: CTAs + error messages only

## Step 5: Usability Checklist

**Purpose**: Validate the design against heuristics and accessibility.

**Input**: Spec content + all previous outputs

Run the usability agent:
```
Task ux-usability-scout(spec content, all previous outputs)
```

**Wait for completion**, then save to `.flow/ux/{spec-name}/05-usability.md`

The usability agent checks:
- Nielsen's 10 heuristics
- WCAG 2.1 AA accessibility
- Edge case coverage
- Open questions for stakeholders

**Quick mode**: Checklist only, no deep review

## Step 6: Summary & Flow-Next Handoff

After all phases complete:

1. **Report output location**:
```
UX artifacts created in .flow/ux/{spec-name}/:
- 01-research.md
- 02-orca.md
- 03-wireframes.md (if not skipped)
- 04-writing.md
- 05-usability.md
```

2. **Highlight critical findings**:
   - Any critical usability issues
   - Key ORCA objects identified
   - Main user flows

3. **Flow-Next handoff**:
```
Ready for implementation planning.

Next steps:
  /flow-next:plan "Feature X"
  # Will automatically reference .flow/ux/{spec-name}/

Or review artifacts first:
  Read .flow/ux/{spec-name}/02-orca.md  # Key objects/CTAs
  Read .flow/ux/{spec-name}/05-usability.md  # Issues to address
```

4. **ORCA → Architecture guidance**:
```
Use ORCA output to inform /flow-next:plan:
- Objects → Data models, TypeScript types
- Relationships → DB schema, API structure
- CTAs → Endpoints, mutations, UI actions
- Attributes → Props, form fields
- States → State management
```

## Single Phase Mode

With `--phase <n>`, run only that phase:

```
/ux-plan SPEC.md --phase 2
# Runs only ORCA analysis
# Still needs previous phases to exist
```

Phase dependencies:
- Phase 1: No dependencies
- Phase 2: Needs 01-research.md
- Phase 3: Needs 02-orca.md
- Phase 4: Needs 03-wireframes.md (or 02-orca.md)
- Phase 5: Needs all previous

## Quick Mode Summary

| Phase | Full | Quick |
|-------|------|-------|
| 1. Research | Full persona, flows, metrics | Goals + happy path |
| 2. ORCA | Full | Full (always) |
| 3. Wireframes | All screens + states | **SKIPPED** |
| 4. Writing | All copy categories | CTAs + errors only |
| 5. Usability | Full heuristics + a11y | Checklist only |

Time: ~15 min (quick) vs ~45 min (full)

## Success Criteria

Before completing:
- [ ] Research identifies clear persona and goals
- [ ] ORCA defines all objects and CTAs
- [ ] Wireframes show key screens (unless skipped)
- [ ] Writing covers all user-facing copy
- [ ] Usability finds no critical issues

## Output Structure

```
.flow/
├── epics/                    # flow-next epics (existing)
├── tasks/                    # flow-next tasks (existing)
└── ux/                       # OOUX artifacts (this skill)
    └── {spec-name}/
        ├── 01-research.md    # Persona, JTBD, flows, edge cases, metrics
        ├── 02-orca.md        # Objects, Relationships, CTAs, Attributes, States
        ├── 03-wireframes.md  # WireMD layouts, state variations, responsive
        ├── 04-writing.md     # Headlines, CTAs, errors, empty states, microcopy
        └── 05-usability.md   # Heuristics, accessibility, issues, recommendations
```

This structure integrates with flow-next:
- `/flow-next:plan` reads from `.flow/ux/` for UX context
- `/flow-next:work` can reference UX artifacts during implementation
- Epics can link to UX artifacts in their specs
