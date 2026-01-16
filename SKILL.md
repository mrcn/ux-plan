---
name: ux-plan
description: "Apply ORCA UX methodology to any spec file. Takes a markdown file (feature spec, app spec, PRD) and generates research, ORCA analysis, wireframes, writing, and usability artifacts. Use when starting UX work on any spec."
---

# UX Plan

Apply structured UX process to any specification file. Works on features, apps, PRDs, or any markdown describing what to build.

## Input

```
/ux-plan <path-to-spec.md>
/ux-plan <path-to-spec.md> --quick
/ux-plan <path-to-spec.md> --output <output-dir>
```

Examples:
```
/ux-plan specs/dark-mode.md
/ux-plan SPEC.md --output .flow/ux/app
/ux-plan features/onboarding.md --quick
/ux-plan PRD.md
```

If no file provided, ask: "What spec file should I analyze? Provide a path to a markdown file."

## What It Does

Reads the spec file and runs 5 sequential UX phases:

```
[Spec File] → Research → ORCA → Wireframes → Writing → Usability → [UX Artifacts]
                ↓          ↓         ↓          ↓          ↓
              who/why    O-R-C-A   WireMD     copy      validate
```

## Workflow

Follow [steps.md](steps.md) exactly. Each phase is **sequential**.

### Step 0: Read Spec

```bash
# Read the input spec file
Read(<spec-file-path>)

# Determine output directory
# Default: .ux/{spec-filename-without-ext}/
# Or use --output flag
```

### Step 1-5: Run Phases

Each phase agent receives:
1. The full spec file content
2. Previous phase outputs

See [steps.md](steps.md) for detailed phase instructions.

## Output

Default output: `.ux/{spec-name}/`

```
.ux/
└── {spec-name}/
    ├── 01-research.md      # Persona, goals, flows, metrics
    ├── 02-orca.md          # Objects, Relationships, CTAs, Attributes
    ├── 03-wireframes.md    # WireMD syntax wireframes
    ├── 04-writing.md       # Copy, errors, empty states
    └── 05-usability.md     # Heuristics, a11y, edge cases
```

Custom output with `--output`:
```
/ux-plan SPEC.md --output docs/ux
# Creates docs/ux/01-research.md, etc.
```

## Options

| Flag | Description |
|------|-------------|
| `--quick` | Abbreviated process (~15 min vs ~45 min) |
| `--output <dir>` | Custom output directory |
| `--phase <n>` | Run single phase (1-5) |
| `--skip-wireframes` | Skip phase 3 |

## Quick Mode

`--quick` runs abbreviated process:
- Research: Goals + happy path only
- ORCA: Full (always full)
- Wireframes: **SKIPPED**
- Writing: CTAs + errors only
- Usability: Checklist only

## Use Cases

| Input | Output |
|-------|--------|
| Feature spec | UX for single feature |
| App SPEC.md | UX for entire app |
| PRD | UX from product requirements |
| Epic spec | UX for implementation phase |
| User story | UX for story scope |

## Integration (Optional)

After UX artifacts exist, you can:

```bash
# Use with flow-next
/flow-next:plan "Feature X"
# Plan will detect and reference .ux/ artifacts

# Use with any planning tool
# Just reference the .ux/{name}/ directory

# Store patterns for future
npx @claude-flow/cli@latest memory store \
  --namespace ux-patterns \
  --key "{feature}" \
  --value "ORCA: ..."
```

## Agents

| Phase | Agent | Purpose |
|-------|-------|---------|
| 1 | `ux-research-scout` | Who/why analysis |
| 2 | `ux-orca-scout` | Objects, Relationships, CTAs, Attributes |
| 3 | `ux-wireframe-scout` | WireMD layouts |
| 4 | `ux-writing-scout` | All copy |
| 5 | `ux-usability-scout` | Validation |

## Success Criteria

- [ ] Research identifies clear persona and goals
- [ ] ORCA defines all objects and CTAs
- [ ] Wireframes show key screens (unless skipped)
- [ ] Writing covers all user-facing copy
- [ ] Usability finds no critical issues
