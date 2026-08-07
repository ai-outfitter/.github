# AI Outfitter

Open, vendor-neutral tooling for agent control: ramp a user, a team, or an
organization from AI-assisted coding to a fully autonomous software
development lifecycle.

```bash
npx @ai-outfitter/outfitter
```

## The ramp

Most organizations sit somewhere on these five rungs. Each Outfitter
component targets a rung, so you climb without rebuilding what got you here
([full definition](https://github.com/ai-outfitter/outfitter/blob/main/docs/philosophy.md)):

1. **Assisted** — autocomplete and chat; a human's hands stay on the keyboard.
2. **Delegated** — a local agent does the task; you define the idea and
   review the PR.
3. **Automated** — a workflow runs without your laptop: an issue, a message,
   or a schedule triggers agents in CI or a cluster; adversarial review is
   part of the pipeline; session logs are captured before merge.
4. **Governed** — the organization shares one pinned catalog of agents,
   skills, and policy; every agent action lands in an auditable record;
   resident agents work as onboarded teammates.
5. **Self-improving** — the audit record feeds evals and model improvement;
   humans set goals and acceptance gates, agents own the middle.

Two rules keep the climb honest: automate nothing you have not first done
manually, and move the human locus of control outward one layer at a time.

## Philosophy

The full argument lives in
[docs/philosophy.md](https://github.com/ai-outfitter/outfitter/blob/main/docs/philosophy.md);
the short version:

- **Trust through evidence.** An agent is trusted the way a new teammate is:
  bounded scopes, adversarial review as a pipeline step, every transition on
  the record.
- **Own your session data.** The record that satisfies an auditor also feeds
  your evals, policy tuning, and training — losing it discards the asset that
  makes the system improvable.
- **Expeditious agents.** Tight, purpose-built profiles preserve the context
  headroom that turns into better decisions and faster sessions.
- **Composition over accumulation.** Stack a personal baseline, a team
  convention, and a project role; switch as the work changes.

## Projects

Start with **[outfitter](https://github.com/ai-outfitter/outfitter)** — it is
the toolchain for the open [`.agents`](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/concepts.md#the-agents-protocol)
convention: compose an agent's context, tools, skills, and permissions as a
reviewable profile, then run the same profile locally, in GitHub Actions, or
in Kubernetes. If you already have a `.agents/` directory, you already have
Outfitter configuration.

- **[actions](https://github.com/ai-outfitter/actions)** — run any profile
  headless in CI: scheduled or event-driven reviewers, implementers, and
  auditors.
- **[agent-operator](https://github.com/ai-outfitter/agent-operator)** —
  Kubernetes `Organization` and `Agent` resources for resident agents in your
  cluster; channels (email, forge, chat) compose in at the agent layer.
- **[channels](https://github.com/ai-outfitter/channels)** — agent-to-agent
  messaging, relay, and forge/calendar/Slack event sources: the entry points
  that let one issue or message kick off a whole workflow.
- **[deepwork](https://github.com/ai-outfitter/deepwork)** — structured
  multi-step jobs with typed arguments and quality gates, so an agent checks
  its work at each step. Runs on Pi, Claude Code, and Codex.
- **[evals](https://github.com/ai-outfitter/evals)** — declarative,
  container-based agent evaluations: measure whether a profile, model, or
  workflow change actually made things better.
- **[community-profiles](https://github.com/ai-outfitter/community-profiles)**
  / **[default-profiles](https://github.com/ai-outfitter/default-profiles)** —
  shared catalogs of agents and skills, pinned by release, composed by slug.
- **[.agents](https://github.com/ai-outfitter/.agents)** — this organization's
  own catalog: the convention, dogfooded.

Pi extension packages: [ulta-tasklist](https://github.com/ai-outfitter/ulta-tasklist),
[file-talk](https://github.com/ai-outfitter/file-talk),
[bash-saver](https://github.com/ai-outfitter/bash-saver),
[autoimprove](https://github.com/ai-outfitter/autoimprove).

## Getting started

Follow the [getting started guide](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/getting-started.md).
The components are open source and built to be extended: defaults ship as
modules you can rip out and replace.
