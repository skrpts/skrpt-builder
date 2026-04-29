---
type: skill
id: node-generation
title: Node Generation
description: "Generates the complete file contents for every node in the skrpt"
tags: [Production]
connections:
  - target: llm-service
    type: runs_on
  - target: skrpt-authoring-reference
    type: references
  - target: node-templates-reference
    type: references
---

## Capability

Takes the node graph blueprint and generates the full markdown + YAML content for every file in the skrpt: skills, prompts, workflow, manifest, services, sources, documents, and assets.

## When to Use

After the node graph design is complete. This is the main generation step inside the build-review loop.

## What It Does

1. **Generate the manifest** — skrptiq.yaml with all fields
2. **Generate each skill** — frontmatter + body markdown following the template structure
3. **Generate each prompt** — frontmatter with inputs declarations + prompt body with variable references
4. **Generate the workflow** — complete frontmatter with execution block, loops, connections + body describing the pipeline
5. **Generate service nodes** — copy llm-service, create any additional services
6. **Generate source/document/asset nodes** — if the design calls for them
7. **Generate tags.yaml** — tag definitions

## Outputs

Complete file contents for every node, each delimited with its file path as a header.
