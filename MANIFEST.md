# Ko app manifest contract (Phase 1)

Authoritative OS plan: [`korun/docs/internal/plans/2026-06-12-phase1-app-manifest-contract.md`](../docs/internal/plans/2026-06-12-phase1-app-manifest-contract.md)

## What you are publishing

A **Ko app** is an installable agent package — like an iOS app. It declares **what it can do** (tools, permissions, integrations), not **how the OS schedules it**.

Ko installs the app globally. The owner enables it per Space. **Ko delegates** work via `delegate_task`; the app runs in a background task using the OS slice driver.

## Required fields

| Field | Purpose |
|---|---|
| `id` | Reverse-domain ID (`ai.ko.agents.coder`) |
| `name` | CLI / delegate name (`coder`) |
| `version` | Semver |
| `runtime.type` | `llm` (most apps), `wasm`, or `python` |
| `runtime.systemPrompt` | File path or inline prompt |

## Declare capabilities (the “permissions screen”)

| Field | Purpose |
|---|---|
| `tools.builtin` | Tools the app needs |
| `permissions` | Network, filesystem, shell, memory grants |
| `dependencies.mcpServers` | MCP integrations |
| `knowledge.packs` | Bundled skills |
| `a2a.skills` | Discoverable skills (helps Ko pick the right app) |

## Optional execution hint

```json
"execution": { "profile": "quick" }
```

Cadence hint only (`quick` | `durable`) when Ko has delegated a background task to this app. Most apps omit `execution` entirely.

Do not declare `execution.engine` or `execution.workflow` — removed from the schema.

## Canonical shapes in this repo

| Agent | Role |
|---|---|
| `agents/coder.json` | Full-featured app — no `execution` block |
| `agents/researcher.json` | Research app — no `execution` block |
| `agents/demo-runner.json` | Durable delegated demo — `execution.profile` only |

## Bundled workflows (optional, Phase 3+)

Apps that need multi-step pipelines may ship:

```
agents/expedia/
  agent.json
  system_prompt.md
  workflows/book-trip.json
```

The app’s LLM uses OS workflow tools when available — the manifest does not set `execution.engine: workflow`.

## Validate locally

```bash
# From korun workspace (after Phase 1 PRs land):
ko agent install ./ko-store/agents/coder.json
```

Schema: `schemas/agent-manifest-v1.json`