# OpenDataHub AI Labs

This repository contains custom skills, agents, and prompts for AI-assisted development workflows in OpenDataHub and Red Hat AI Labs projects.

## Guidelines

- Do not use emojis in skill, agent, or prompt files unless the content being written explicitly requires them (e.g., user-facing output that must include an emoji).

## Project Overview

- `skills/` — Installable skill packages
- `agents/` — Specialized agent configurations
- `prompts/` — Reusable prompt templates
- `*.zip` — Packaged skills for distribution (at repo root, not under `skills/`)

---

## Skills

### Structure
```
skills/{skill-name}/
  SKILL.md       # Required: frontmatter + skill instructions
  config.json    # Optional: triggers, permissions, env vars
{skill-name}.zip # Required for distribution (at repo root)
```

### Frontmatter
```markdown
---
name: {skill-name}
description: {One sentence with trigger phrases. E.g. "Triage CVE", "Deploy ODH"}
---
```

### Conventions
- Directory: kebab-case (e.g., `cve-triage`)
- Definition file: `SKILL.md` (uppercase)
- Zip file: `{skill-name}.zip` — name must match directory exactly

### Adding a Skill
1. Create `skills/{skill-name}/SKILL.md` with frontmatter and instructions
2. Optionally add `skills/{skill-name}/config.json`
3. Package and place `{skill-name}.zip` at the repo root
4. List it in **Available Skills** below

### Available Skills

- **[commit](skills/commit/)** — Generate and apply a Conventional Commit message based on staged or all changes
- **[cve-triage](skills/cve-triage/)** — Triage CVE Jira tickets: research the fix across upstream sources and Red Hat container images, then post a solution comment
- **[grill-me](skills/grill-me/)** — Ask probing questions until 95% confident about what needs to be done
- **[package-skills](skills/package-skills/)** — Package each skill directory into a zip file at the repo root for distribution
- **[push](skills/push/)** — Rebase, squash all branch commits into one Conventional Commit, and push to the remote
- **[setup-mcp](skills/setup-mcp/)** — Configure .mcp.json from mcp.sample.json by asking the user for required values and verifying each server

---

## Agents

### Structure
```
agents/{agent-name}/
  agent.md       # Required: frontmatter with type, name, description, model
  tools.json     # Optional
  knowledge/     # Optional
  README.md      # Recommended
```

### Conventions
- Directory: kebab-case (e.g., `odh-expert`)

### Adding an Agent
1. Create `agents/{agent-name}/agent.md` with frontmatter (`type: agent`, `name`, `description`, `model`)
2. Optionally add `tools.json` and a `knowledge/` directory
3. List it in **Available Agents** below

### Available Agents

(none yet — add to `agents/`)

---

## Prompts

Reusable prompt live in `prompts/`.

```
prompts/{prompt-name}.md
```
