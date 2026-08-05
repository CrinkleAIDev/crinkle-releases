# Crinkle

Crinkle is a chat-first AI software studio. You describe what you want in plain language,
connect the AI subscriptions or API keys you already have, and a team of specialized
agents plans, builds, tests, and reviews real software for you in an isolated workspace.

It's designed so that non-technical users can drive it like a team chat app, while the
technical details — file changes, commands, test results, logs — stay available without
getting in the way.

## What it does

- **Bring your own AI.** Connect OpenAI, Claude, Gemini, xAI/Grok, OpenRouter, Groq,
  DeepSeek, Together, Mistral, Perplexity, local models (Ollama / LM Studio / Open WebUI),
  or the Claude Code / Codex CLIs. Manual ChatGPT handoff is available with no API key.
  Keys are encrypted at rest.
- **A team of agents.** The Manager, Builder, and Reviewer are the core team; a Designer,
  Advisor, Security reviewer, and Web operator can be added. Each agent's
  role is independent of which AI powers it — route each agent to whatever model you like,
  or turn on **smart auto-routing** and let the Manager pick the best model per task.
- **Autonomous build loop.** The Manager turns your idea into acceptance criteria, assigns work,
  reads each agent's report, and keeps going until the goal is met. The Builder writes real
  multi-file changes; the Reviewer runs the project's *actual* toolchain (`tsc`, lint, build,
  tests) as ground truth so it never declares broken code "done."
- **You stay in control.** Reviewable file-change cards with diffs, approval-gated command
  running (allowlisted), spend caps, Pause (graceful) and Stop (instant), and Git-independent
  restore points (undo).
- **See it run.** A live in-app preview launches the project's dev server (Vite/HMR) or
  serves static builds, updating as the team edits. Download the whole workspace as a ZIP.
- **Built for overnight runs.** Queue several goals and they start automatically, one
  project after another, when the current run finishes. Desktop notifications when a
  run completes or needs you; a Run history panel records each run's duration, steps,
  criteria met, and cost. Lean prompting resends only changed files to the model, and
  the loop skips redundant manager calls on the standard build→test path.
- **Extras.** Image input (Pixel vision), project knowledge docs, cross-project agent
  memory, a credential vault for the browser agent, a first-run setup wizard, and a
  guided welcome tour. AI connections and settings are global — they survive project
  switches and deletions.

## Run locally

```powershell
npm install
npm run dev
```

This starts both the Crinkle API (port 4317) and the Vite UI concurrently. Open the local
URL Vite prints (typically http://localhost:5173).

### Optional capabilities

- **Browser agent (Scout):** `npm i playwright && npx playwright install chromium`.
  Without it, everything else works and Scout is cleanly disabled.
- **Git version history / auto-commit:** install Git and have it on your PATH. Without it,
  Crinkle falls back to its own restore points (`data/checkpoints/`).

## How it works

```text
UI  ->  Orchestrator / agent loop  ->  Provider adapters  ->  Isolated workspace
                  |                                                   |
            Task + state store                              File changes, commands,
            (data/relay-state.json)                         tests, preview, ZIP
```

A role such as "coder" is decoupled from its provider, so the Builder can run on Claude Code,
Codex, or any API model without changing the user-facing workflow.

### Project layout

| Path              | What's there                                                            |
| ----------------- | ----------------------------------------------------------------------- |
| `server/`         | Node HTTP API, agent loop, provider adapters, workspace + safety tools  |
| `src/`            | React + TypeScript (Vite) frontend                                      |
| `tests/`          | `node --test` suite (`npm test`) + live-loop harness                    |
| `data/`           | Local state, encrypted secrets, checkpoints, knowledge (gitignored)     |
| `workspaces/`     | Isolated per-project build folders                                      |

Key server modules: `agentLoop.mjs` (autonomous orchestration), `providers.mjs` (all AI
adapters, routing, streaming, context budgeting), `workspace.mjs` (file ops + approval),
`toolchain.mjs` (real quality gate), `browser.mjs` (Scout), `security.mjs`, `state.mjs`.

## Testing

```powershell
npm test          # unit/logic tests (node --test)
```

The autonomous loop is unit-tested at the helper level. To exercise the full connected
loop end to end, point a provider at a local model (e.g. Ollama) and run
`node tests/live-loop.mjs`.

## Status & roadmap

Crinkle is a working local product, not a hosted service. Current focus areas:

- Splitting the large frontend (`App.tsx` / `styles.css`) into smaller components
- Migrating state persistence from a JSON flat file to SQLite
- Streaming live tokens into the UI (the backend already streams; the UI updates per turn)
- Azure OpenAI / Amazon Bedrock adapters if enterprise users need them

## License

Crinkle is **free to use** for personal and commercial work, and **everything you build with it is yours**. The software itself may not be redistributed or resold — see [LICENSE](LICENSE) for the full terms.

