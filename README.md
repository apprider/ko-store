# Ko Agent Store (ko-store)

Installable **apps** for Ko Agent OS. Ko is the supervisor; store agents are
delegated workers enabled per Space.

**Author contract:** [`MANIFEST.md`](MANIFEST.md)  
**OS carve-out plan:** [`../docs/internal/plans/2026-06-12-phase1-app-manifest-contract.md`](../docs/internal/plans/2026-06-12-phase1-app-manifest-contract.md)

## Structure

```
ko-store/
├── index.json                    # Store catalog (installable apps only)
├── MANIFEST.md                   # App manifest contract for publishers
├── schemas/
│   └── agent-manifest-v1.json    # JSON Schema
├── agents/                       # Public app manifests
│   ├── coder.json
│   ├── coder/system_prompt.md
│   ├── researcher.json
│   ├── clip.json
│   ├── demo-runner.json
│   ├── expedia.json
│   └── …
├── workflows/                    # Bundled pipeline examples (not app engines)
│   └── echo-pipeline.json
├── samples/                      # Reserved (fixtures live in korun tests)
└── packages/                     # Release tarballs (not always in git)
```

## Using the Marketplace

### Browse Available Agents

```bash
ko agent browse
ko agent browse --category development
```

### Search Agents

```bash
ko agent search "code review"
```

### Install an Agent

```bash
ko agent install coder
ko agent install ai.ko.agents.researcher@1.5.0
```

## For Agent Developers

### Agent Manifest Format

All agents must have a valid JSON manifest conforming to `schemas/agent-manifest-v1.json`.

### Required Fields

- `id` - Unique identifier (reverse-domain notation, e.g., `ai.ko.agents.coder`)
- `name` - Short name for CLI commands
- `version` - Semantic version
- `runtime` - Runtime configuration (`type`, system prompt)

See [`MANIFEST.md`](MANIFEST.md) for the full contract. Apps do not declare OS engines.

### Example Manifest

```json
{
  "$schema": "https://raw.githubusercontent.com/apprider/ko-store/main/schemas/agent-manifest-v1.json",
  "id": "ai.ko.agents.example",
  "name": "example",
  "version": "1.0.0",
  "description": "An example agent",
  "runtime": {
    "type": "llm",
    "model": {
      "provider": "anthropic",
      "name": "claude-sonnet-4-20250514"
    },
    "systemPrompt": "system_prompt.md"
  }
}
```

## Hosting Your Own Marketplace

The marketplace is served as static JSON files. You can host it on:
- GitHub Pages
- S3 + CloudFront
- Any static file server

Configure your Ko instance to use a custom marketplace:

```bash
ko config set marketplace.url https://your-marketplace.example.com
```

## Accessing Agent Manifests

Agents can be accessed via direct URL:

```
https://raw.githubusercontent.com/apprider/ko-store/main/agents/coder.json
```

This allows agents to be installed from any URL:

```bash
ko agent install https://raw.githubusercontent.com/apprider/ko-store/main/agents/coder.json
```