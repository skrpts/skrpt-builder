---
type: prompt
id: validate-structure
title: Validate Structure
description: "Validates generated skrpt files against format rules"
tags: [Production]
connections:
  - target: structure-validation
    type: derived_from
metadata:
  output_format: structured
  prompt_type: task
---

## Purpose

Validates the generated skrpt output against the authoring reference rules.

## Prompt

You are a skrpt validator. Check the generated files below against every rule in the authoring reference. Report all issues found.

### Generated Files

{{steps.previous.output}}

### Validation Checklist

Check each rule. For each failure, note the file and specific issue.

**Connections:**
- [ ] Every prompt has exactly one `derived_from` connection to a skill
- [ ] Every skill has `runs_on` connection to a service (usually llm-service)
- [ ] Workflow has `uses` connection to every skill in the execution block
- [ ] Connection types are valid: uses, derived_from, runs_on, references, depends_on, requires, tagged

**Inputs:**
- [ ] Every `{{input.*}}` variable in prompt bodies has a matching `inputs` entry in that prompt's frontmatter
- [ ] Every `inputs` entry has `label` and `description` fields
- [ ] At least one prompt declares inputs (the workflow needs a user entry point)

**Execution:**
- [ ] Every execution entry has `skill`, `prompt`, and `step_type`
- [ ] All referenced skill IDs exist as skill files
- [ ] All referenced prompt IDs exist as prompt files
- [ ] `step_type` is one of: generation, content, review, synthesis, validation
- [ ] Parallel blocks have 2+ steps

**Manifest:**
- [ ] `contents` counts match the actual number of files per type
- [ ] `contents.total` equals the sum of all type counts
- [ ] `requires.services` lists all service node IDs
- [ ] `name` is kebab-case

**IDs and Naming:**
- [ ] All node IDs are kebab-case: `[a-z0-9][a-z0-9-]*`
- [ ] No duplicate IDs across the skrpt
- [ ] Node `type` matches its directory (skill in skills/, prompt in prompts/)

**Loops (if present):**
- [ ] Loops are in workflow frontmatter, NOT in skrptiq.yaml
- [ ] Each loop has: id, mode, steps (non-empty), maxIterations
- [ ] until_pass loops have a `verifier` that is in the `steps` list
- [ ] for_each loops do NOT have a `verifier`

**Data Flow:**
- [ ] Later-stage prompts reference `{{steps.previous.output}}` or `{{steps.Step Title.output}}`
- [ ] `output_step` references a skill that exists
- [ ] `composite_steps` entries all reference existing skills

### Output Format

```json
{
  "status": "pass|continue|fail",
  "reason": "Summary of validation results",
  "issues": [
    { "file": "path", "rule": "rule name", "detail": "specific issue" }
  ],
  "instructions": "Specific fixes needed for the next generation iteration"
}
```

- **pass**: no issues found — the skrpt is structurally valid
- **continue**: issues found but fixable — list them for the next iteration
- **fail**: fundamental problems (e.g. no workflow, no skills) — needs a redesign
