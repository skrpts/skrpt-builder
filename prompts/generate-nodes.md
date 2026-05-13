---
type: prompt
id: generate-nodes
title: Generate Nodes
description: "Generates all file contents from the node graph blueprint"
tags: [Production]
connections:
  - target: node-generation
    type: derived_from
metadata:
  output_format: structured
  prompt_type: generation
---

## Purpose

Generates the complete file contents for every node in the skrpt, using the node graph blueprint and templates reference.

## Prompt

You are a skrpt generator. Using the node templates reference for structure and the authoring reference for rules, generate every file in the skrpt.

### Node Graph Blueprint

{{steps.previous.output}}

### Generation Rules

1. **skrptiq.yaml** — fill in all manifest fields. Use the exact IDs, counts, and requires block from the blueprint.
2. **Skill files** — each skill gets: frontmatter (type, id, title, description, tags, connections, metadata if gate), body with Capability, When to Use, What It Does (numbered steps), Inputs table (if first step), Outputs section.
3. **Prompt files** — each prompt gets: frontmatter (type, id, title, description, tags, inputs if first-step prompt, connections with derived_from, metadata), body with Purpose and Prompt sections. The Prompt section contains the actual instructions.
4. **Workflow file** — frontmatter with all connections, metadata, output_step, composite_steps, loops (if any), execution block. Body with Overview, Pipeline Stages, Inputs table, Outputs table.
5. **Service files** — llm-service copied from template. Any additional services with appropriate metadata.
6. **tags.yaml** — tag definitions matching the manifest tags.

### Prompt Writing Guidelines

- First-step prompts: use `{{input.VAR}}` variables with matching `inputs` declarations
- Later-step prompts: reference upstream output via `{{steps.previous.output}}` or `{{steps.Step Title.output}}`
- Every prompt needs a clear role instruction ("You are a...")
- Include numbered steps for what the LLM should do
- Specify the output format explicitly
- Gate prompts: tell the user what to check and how to respond

### Critical Checks

- Every prompt has `derived_from` connection to its skill
- Every `{{input.VAR}}` in prompt body has matching `inputs` frontmatter entry
- Every `inputs` entry has `label` and `description`
- Workflow `execution` entries all have `skill`, `prompt`, and `step_type`
- `contents` counts match the actual number of generated files
- Loops are in workflow frontmatter, NOT in skrptiq.yaml
- All node IDs are kebab-case: `[a-z0-9][a-z0-9-]*`

### Output Format

Output each file with its path as a header, using fenced code blocks:

````
## skrptiq.yaml

```yaml
(manifest content)
```

## tags.yaml

```yaml
(tags content)
```

## skills/skill-name.md

```markdown
(full file content including frontmatter)
```

## prompts/prompt-name.md

```markdown
(full file content including frontmatter)
```
````

Generate ALL files. Do not skip any node from the blueprint.

If this is a revision (loop iteration 2+), incorporate the validation findings and user feedback from the previous iteration. Fix the specific issues identified.
