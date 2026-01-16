# UX Plan Steps

**CRITICAL**: Phases are SEQUENTIAL. Each phase depends on the previous phase's output.

## Step 0: Initialize

1. Parse the feature request from $ARGUMENTS
2. Generate a slug from the feature name (e.g., "dark-mode-toggle")
3. Create the UX artifact directory:

```bash
mkdir -p .flow/ux/{feature-slug}
```

4. If input is a Flow ID (fn-N), fetch existing context:
```bash
$FLOWCTL show <id> --json
$FLOWCTL cat <id>
```

## Step 1: User Research

**Purpose**: Understand WHO the user is and WHY they need this feature.

Run the research agent:
```
Task ux-research-scout(<feature request>)
```

**Wait for completion**, then save output to `.flow/ux/{feature-slug}/01-research.md`

The research agent will:
- Identify target persona (from PRD or inferred)
- Define jobs-to-be-done and user goals
- Map the happy path flow (step-by-step)
- Identify edge cases and error scenarios
- Define success metrics

## Step 2: ORCA Analysis

**Purpose**: Define the OBJECTS users interact with and their structure.

Run the ORCA agent with research context:
```
Task ux-orca-scout(<feature request>, <01-research.md content>)
```

**Wait for completion**, then save output to `.flow/ux/{feature-slug}/02-orca.md`

The ORCA agent will produce:
- **Objects**: Core entities (nouns)
- **Relationships**: How objects connect
- **CTAs**: Actions per object (verbs)
- **Attributes**: Data/properties per object

Output format: ASCII diagram + markdown matrix table

## Step 3: Wireframes

**Purpose**: Visualize screen layouts based on ORCA objects.

Run the wireframe agent with ORCA context:
```
Task ux-wireframe-scout(<feature request>, <02-orca.md content>)
```

**Wait for completion**, then save output to `.flow/ux/{feature-slug}/03-wireframes.md`

The wireframe agent will create:
- WireMD syntax wireframes for key screens
- Layout of objects and CTAs on screen
- State variations (empty, loading, error, success)
- Responsive breakpoint notes

**Skip in --quick mode**

## Step 4: UX Writing

**Purpose**: Define all copy, labels, and messages.

Run the writing agent with wireframe context:
```
Task ux-writing-scout(<feature request>, <03-wireframes.md content OR 02-orca.md if quick mode>)
```

**Wait for completion**, then save output to `.flow/ux/{feature-slug}/04-writing.md`

The writing agent will produce:
- Headlines and page titles
- CTA button labels
- Error messages
- Empty state copy
- Microcopy (helper text, tooltips)
- Onboarding text (if applicable)

## Step 5: Usability Checklist

**Purpose**: Validate the design against heuristics and accessibility.

Run the usability agent with all previous outputs:
```
Task ux-usability-scout(<feature request>, <all previous outputs>)
```

**Wait for completion**, then save output to `.flow/ux/{feature-slug}/05-usability.md`

The usability agent will check:
- Nielsen's 10 heuristics
- WCAG 2.1 AA accessibility
- Edge case coverage
- Open questions for stakeholders

**Abbreviated in --quick mode** (checklist only, no deep review)

## Step 6: Invoke flow-next:plan

Now that UX artifacts exist, invoke the implementation planner:

```
/flow-next:plan "<feature request>"

Include this context in the plan:
- UX artifacts location: .flow/ux/{feature-slug}/
- Link to research, ORCA, wireframes in the epic spec
```

The implementation plan will reference UX artifacts and use ORCA objects to inform technical architecture.

## Step 7: Link Artifacts

After `/flow-next:plan` creates the epic, ensure the epic spec links to UX:

```markdown
## UX Artifacts

See `.flow/ux/{feature-slug}/` for:
- [Research](../ux/{feature-slug}/01-research.md)
- [ORCA](../ux/{feature-slug}/02-orca.md)
- [Wireframes](../ux/{feature-slug}/03-wireframes.md)
- [Writing](../ux/{feature-slug}/04-writing.md)
- [Usability](../ux/{feature-slug}/05-usability.md)
```

## Quick Mode Flow

When `--quick` flag is present:

```
Step 1: Research (abbreviated - goals + happy path only)
Step 2: ORCA (full)
Step 3: SKIP wireframes
Step 4: Writing (CTAs + errors only)
Step 5: Usability (checklist only)
Step 6: /flow-next:plan
```

Total: ~10-15 min vs ~30-45 min for full process.

## Success Criteria

Before moving to `/flow-next:plan`:
- [ ] Research identifies clear persona and goals
- [ ] ORCA defines all objects and CTAs
- [ ] Wireframes show key screens (unless --quick)
- [ ] Writing covers all user-facing copy
- [ ] Usability finds no critical issues
