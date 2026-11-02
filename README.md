# Novix Flow

**Incident resolution at the speed of your telemetry.**

Novix Flow is an MCP-native incident-response agent for engineering teams, built on Claude.
It connects **ClickHouse**, **Linear** and your **codebase** to your Claude client and turns error
spikes into triaged Linear issues with a root-cause analysis and a proposed patch.

- Website: **https://novixlab.dev**
- Stage: **Private Beta — invite only**
- Request access: https://novixlab.dev/beta

> This repository is the public overview of Novix Flow. The server implementation is distributed to
> beta teams during onboarding and is not open source.

---

## How it works

Novix Flow runs as an **MCP server** on your own machine, connected to your Claude client
(Claude Desktop / Claude Code). **Claude does the reasoning; Novix provides the tools and context.**

```
Claude client  ◄──MCP──►  Novix Flow (local)  ──►  ClickHouse
                                              ──►  Linear
                                              ──►  your repo
```

Nothing leaves your machine: credentials, source and telemetry stay local. Novix stores no code and
no telemetry.

---

## Status

Every capability below is labeled. Present-tense claims apply **only** to `● Available in Beta`.

### ● Available in Beta

| Tool / surface | What it does |
|----------------|--------------|
| `query_clickhouse_errors` | Finds error spikes by service and time window, straight from your telemetry tables. |
| `get_linear_context` | Pulls related Linear issues for the affected service. |
| `read_repo_files` | Gives Claude the exact source files involved in the failure. |
| `triage-incident` (MCP prompt) | Guides Claude from spike → root-cause hypothesis → proposed patch. |
| `update_linear_issue` | Creates or updates the issue with the root cause and the proposed patch. |
| Local-first | Runs on your machine with your own Claude client. Novix stores no code or telemetry. |

### ◐ In Development

- **Autonomous agent mode** — Claude Haiku triages every spike, escalates complex multi-file cases to Claude Sonnet.
- **Prompt-cached repository context** — repo structure and hot files kept warm in Claude's prompt cache.
- **Verified patches** — each proposed patch runs your test suite in an isolated sandbox before a PR is opened.
- **GitHub PR automation** — opens a PR with root-cause summary, linked Linear issue and test results.
- **Sentry connector** — a second telemetry source alongside ClickHouse.

### ○ Planned

Novix Cloud (hosted) · Incident Command Dashboard · ephemeral microVM verification ·
multi-agent incident response · deploy correlation · postmortem generator ·
more connectors (Datadog, Grafana/Prometheus, PagerDuty, Slack, GitLab, Jira) · policy engine · Enterprise (SSO, audit logs, VPC, SOC 2).

Full roadmap: https://novixlab.dev/roadmap

---

## Setup (beta teams)

Beta teams receive the package during onboarding. Add the server to your Claude client configuration
(for Claude Desktop, `claude_desktop_config.json`), restart the client, and ask Claude to triage a service.

```json
{
  "mcpServers": {
    "novix-flow": {
      "command": "npx",
      "args": ["-y", "@novixlab/flow-mcp"],
      "env": {
        "CLICKHOUSE_URL": "https://your-clickhouse:8443",
        "CLICKHOUSE_USER": "novix_readonly",
        "CLICKHOUSE_PASSWORD": "••••••••",
        "LINEAR_API_KEY": "lin_api_••••••••",
        "NOVIX_REPO_PATH": "/path/to/your/repo"
      }
    }
  }
}
```

Use a **read-only** ClickHouse user. Credentials stay on your machine and are only used by the local server.

Who it's for: teams running **ClickHouse + Linear** who want faster incident triage.
Onboarding is in small batches with a short setup call.

---

## Built on Claude

Novix Flow is designed around Claude's model family and the Model Context Protocol:

- **Claude Haiku** for fast, cheap triage of every spike *(in development)*.
- **Claude Sonnet** for root-cause reasoning and patch synthesis on escalated cases *(in development)*.
- **Prompt caching** to keep repository context warm across repeated analyses *(in development)*.

---

## About

Novix Flow is a product of **Novix Lab**, an independent, early-stage company.

Contact: founder@novixlab.dev

> Novix Lab is an independent company and is not affiliated with Anthropic.
> "Built with Claude" refers to use of the Claude API and the Model Context Protocol.
