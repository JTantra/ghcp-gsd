# GSD (Get Shit Done)

AI-powered project orchestration framework for VS Code Copilot. GSD manages the full lifecycle — research, planning, execution, and verification — through 18 specialized agents with optimized model assignments.

This works best in GHCP for VS Code.

## Quick Start

```
/gsd-new-project          # Initialize a new project (research → requirements → roadmap)
/gsd-progress             # Check where you are and what's next
/gsd-discuss-phase 1      # Discuss design decisions for a phase
/gsd-plan-phase 1         # Research + plan a phase
/gsd-execute-phase 1      # Execute all plans in a phase
/gsd-verify-work 1        # Validate built features via UAT
/gsd-next                 # Advance to the next logical step
/gsd-autonomous           # Run all remaining phases autonomously
/gsd-help                 # Show all available commands
```

## Agent Catalog

### Planning & Research — Claude Opus 4.6

Deep reasoning for architecture, research, and strategic decisions.

| Agent | Role |
|-------|------|
| `gsd-project-researcher` | Researches domain ecosystem before roadmap creation |
| `gsd-phase-researcher` | Researches how to implement a phase before planning |
| `gsd-research-synthesizer` | Synthesizes findings from parallel researcher agents |
| `gsd-advisor-researcher` | Researches gray area decisions with structured comparison |
| `gsd-assumptions-analyzer` | Analyzes codebase for a phase and surfaces assumptions |
| `gsd-planner` | Creates executable phase plans with task breakdown |
| `gsd-roadmapper` | Creates project roadmaps with phase breakdown and coverage validation |
| `gsd-debugger` | Investigates bugs using scientific method with checkpoints |

### Execution & Verification — GPT-5.3-Codex

Fast, instruction-following execution and code-level verification.

| Agent | Role |
|-------|------|
| `gsd-executor` | Executes plans with atomic commits and deviation handling |
| `gsd-plan-checker` | Verifies plans achieve phase goal before execution |
| `gsd-verifier` | Verifies phase goal achievement through goal-backward analysis |
| `gsd-integration-checker` | Verifies cross-phase integration and E2E flows |
| `gsd-nyquist-auditor` | Fills validation gaps by generating tests for phase requirements |
| `gsd-codebase-mapper` | Explores codebase and writes structured analysis documents |
| `gsd-user-profiler` | Analyzes session messages to produce developer behavioral profile |

### UI Design — Gemini 3.1 Pro (Preview)

Visual reasoning for frontend design contracts and audits.

| Agent | Role |
|-------|------|
| `gsd-ui-researcher` | Produces UI-SPEC.md design contracts for frontend phases |
| `gsd-ui-checker` | Validates UI specs against 6 quality dimensions (BLOCK/FLAG/PASS) |
| `gsd-ui-auditor` | Retroactive 6-pillar visual audit of implemented frontend code |

## Model Assignments

| Tier | Model | Agent Count | Use Case |
|------|-------|-------------|----------|
| Planning | Claude Opus 4.6 | 8 | Research, architecture, planning, debugging |
| Execution | GPT-5.3-Codex | 7 | Code execution, verification, analysis |
| UI | Gemini 3.1 Pro (Preview) | 3 | Frontend design, visual audits |

## Tool & MCP Setup

GSD agents use these MCP servers for documentation and research:

| Server | Purpose |
|--------|---------|
| **context7** | Library/framework API docs — version-aware, authoritative |
| **mslearn** | Microsoft & Azure official documentation, code samples, best practices |
| **playwright** | Browser automation for visual testing |

**Infrastructure preference:** Azure is the default cloud platform. Research agents use `mslearn/*` tools for Azure service documentation and best practices.

### MCP Configuration

Add to your VS Code MCP settings (`.vscode/mcp.json` or user settings):

```json
{
  "servers": {
    // Playwright is used for visual testing
    "playwright": {
      "command": "npx",
      "args": [
        "@playwright/mcp@latest",
        "--caps=vision",
        "--browser=msedge"
      ],
      "type": "stdio"
    },
    // Context7 is used for library/framework API docs especially open source ones
    "context7": {
      "type": "http",
      "url": "https://mcp.context7.com/mcp"
    },
    // MSLearn is used for Azure and MS related service documentation and best practices
    "mslearn": {
      "type": "http",
      "url": "https://learn.microsoft.com/api/mcp"
    }
  },
  "inputs": []
}
```

## Default Config (`~/.gsd/defaults.json`)

User-level defaults stored in `~/.gsd/defaults.json` override model assignments and control agent behavior globally across all projects:

```json
{
  "resolve_model_ids": "omit",
  "model_overrides": {
    "gsd-planner": "Claude Opus 4.6 (copilot)",
    "gsd-roadmapper": "Claude Opus 4.6 (copilot)",
    "gsd-debugger": "Claude Opus 4.6 (copilot)",
    "gsd-phase-researcher": "Claude Opus 4.6 (copilot)",
    "gsd-project-researcher": "Claude Opus 4.6 (copilot)",
    "gsd-research-synthesizer": "Claude Opus 4.6 (copilot)",
    "gsd-advisor-researcher": "Claude Opus 4.6 (copilot)",
    "gsd-assumptions-analyzer": "Claude Opus 4.6 (copilot)",
    "gsd-executor": "GPT-5.3-Codex (copilot)",
    "gsd-verifier": "GPT-5.3-Codex (copilot)",
    "gsd-plan-checker": "GPT-5.3-Codex (copilot)",
    "gsd-integration-checker": "GPT-5.3-Codex (copilot)",
    "gsd-nyquist-auditor": "GPT-5.3-Codex (copilot)",
    "gsd-codebase-mapper": "GPT-5.3-Codex (copilot)",
    "gsd-user-profiler": "GPT-5.3-Codex (copilot)",
    "gsd-ui-researcher": "Gemini 3.1 Pro (Preview) (copilot)",
    "gsd-ui-checker": "Gemini 3.1 Pro (Preview) (copilot)",
    "gsd-ui-auditor": "Gemini 3.1 Pro (Preview) (copilot)"
  }
}
```
