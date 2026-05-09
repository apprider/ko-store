# Moona Agent Marketplace

This directory contains the marketplace data for Moona Agent OS.

## Structure

```
marketplace/
├── index.json              # Marketplace index (list of all agents)
├── schemas/
│   └── agent-manifest-v1.json  # JSON Schema for agent manifests
├── agents/
│   ├── coder.json          # Agent manifests
│   ├── researcher.json
│   └── orchestrator.json
└── packages/               # Agent packages (not in git)
    ├── coder-2.1.0.moona.tar.gz
    └── ...
```

## Using the Marketplace

### Browse Available Agents

```bash
moona agent browse
moona agent browse --category development
```

### Search Agents

```bash
moona agent search "code review"
```

### Install an Agent

```bash
moona agent install coder
moona agent install ai.moona.agents.researcher@1.5.0
```

## For Agent Developers

### Agent Manifest Format

All agents must have a valid JSON manifest conforming to `schemas/agent-manifest-v1.json`.

### Required Fields

- `id` - Unique identifier (reverse-domain notation, e.g., `ai.moona.agents.coder`)
- `name` - Short name for CLI commands
- `version` - Semantic version
- `runtime` - Runtime configuration including LLM model

### Example Manifest

```json
{
  "$schema": "https://raw.githubusercontent.com/apprider/moona-store/main/schemas/agent-manifest-v1.json",
  "id": "ai.moona.agents.example",
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

Configure your Moona instance to use a custom marketplace:

```bash
moona config set marketplace.url https://your-marketplace.example.com
```

## Accessing Agent Manifests

Agents can be accessed via direct URL:

```
https://raw.githubusercontent.com/apprider/moona-store/main/agents/coder.json
```

This allows agents to be installed from any URL:

```bash
moona agent install https://raw.githubusercontent.com/apprider/moona-store/main/agents/coder.json
```