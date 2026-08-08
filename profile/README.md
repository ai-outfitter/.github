# AI Outfitter

Open, vendor-neutral tooling for controlling agents: ramp a user, a team, or an
organization from AI-assisted coding to a fully autonomous software
development lifecycle.

AI Outfitter is a complete system for managing agentic development.
Everything here builds on one convention: an agent's context, tools, skills,
and permissions are plain files in a `.agents/` directory — committed,
reviewed, and shared like the rest of your code. The same definition runs
across environments (on a laptop, headless in CI, or as a resident agent in
a cluster) and with any model vendor, including self-hosted models. The
core is open source under MIT — free to adopt, free to fork, free to build
a business on.

As you adopt AI Outfitter, we strongly suggest following these two
guidelines:

1. **Automate nothing you have not first done manually.**
2. **Hand over control one layer at a time.**

Follow these two guidelines and adopting agents stops being a leap of faith. You
never skip a step you don't understand, and nothing you build on one rung is
thrown away on the next.

Key terms to understand:

- **harness** — the CLI tool that runs an agent: Claude Code, Pi, Codex.
- **forge** — where code and issues live: GitHub, GitLab.
- **profile** — one agent's full definition — context, tools, skills,
  permissions — as plain files.
- **catalog** — a shared, version-pinned collection of profiles and skills.
- **the record** — the accumulated, write-once evidence of what agents
  actually did: transcripts, tool calls, diffs, approvals — captured per
  action, stored in [pensieve](https://github.com/ai-outfitter/pensieve).

## Why we built AI Outfitter

Your team already uses coding agents — Claude Code, Cursor, Codex,
[Pi](https://github.com/earendil-works/pi) — locally, ad hoc, and
enthusiastically. Pull requests are up, and so is review load, because
generated changes arrive faster than anyone can check them. One team is
quietly rebuilding the tool another team finished last month. Someone in
management has asked how all of this is audited, and the honest answer today
is: a summary written by the same agent that did the work. Every attempt to
fix one piece — a policy doc here, a prompt library there — lives on one
laptop and drifts.

None of that means your organization is behind. It means you are partway up a
ramp that most software organizations are climbing right now, and the next
rung is hard to see from where you stand.

We built AI Outfitter to help you climb the ramp faster. AI Outfitter is a
clear foundation and open set of primitives for managing agentic
engineering processes and AI SDLC.

## The ramp

Five rungs, from AI-assisted coding to an autonomous lifecycle
([full definition](https://github.com/ai-outfitter/outfitter/blob/main/docs/philosophy.md)).
Each Outfitter component targets a rung, so you climb without rebuilding what
got you here — and without adopting complexity too early.

1. **Assisted** — autocomplete and chat; a human's hands stay on the
   keyboard. *You are here if* you use autocomplete in an IDE.
   There is nothing to govern yet — but the habit that matters starts here:
   document what works and what doesn't in `AGENTS.md`/`CLAUDE.md`, and
   keep it in the repo.

2. **Delegated** — a local agent does the task; you define the idea and
   review the PR. *You are here if* engineers run a coding agent in a
   terminal and push the result. This is where configuration starts to
   matter: [outfitter](https://github.com/ai-outfitter/outfitter) composes
   what an agent knows and may do — its context, tools, skills, and
   permissions — into a **profile**: plain files in your `.agents/` folder,
   reviewed like code and portable across environments and harnesses.
   [deepwork](https://github.com/ai-outfitter/deepwork) adds
   step-by-step quality gates so the agent checks its own work.

3. **Automated** — a workflow runs without your laptop: an issue, a message,
   or a schedule triggers agents in CI or a cluster; adversarial review is
   part of the pipeline; session logs are captured before merge. *You are
   here when* you close your laptop and the work keeps going.
   [actions](https://github.com/ai-outfitter/actions) runs any profile
   headless — no human at the keyboard — in GitHub Actions, on any trigger;
   [channels](https://github.com/ai-outfitter/channels) pushes email, Slack,
   and forge events (GitHub or GitLab activity) into an agent session, so
   one message can start the same workflow.

4. **Governed** — the organization shares one version-pinned catalog of
   agents, skills, and policy; every agent action lands in an auditable
   record; **resident agents** — long-lived agents onboarded like teammates,
   with their own accounts and boundaries — take on standing jobs. *You are
   here when* agents work across many teams, and the organization needs
   shared policy — and proof of what every agent did. The org's own
   [.agents](https://github.com/ai-outfitter/.agents) repository is the
   pattern for that shared catalog;
   [agent-operator](https://github.com/ai-outfitter/agent-operator)
   provisions and supervises resident agents on your own infrastructure; and
   [pensieve](https://github.com/ai-outfitter/pensieve) is the write-once
   evidence store the audit story lands in. The record is an immutable log
   of every action taken — bound to environments, agents, tools, sessions,
   artifacts, costs, and more. That log is what lets you audit your
   processes, prove compliance, and run automated, recursive
   self-improvement on them.

5. **Self-improving** — the audit record feeds evals and improvement; humans
   set goals and acceptance gates, agents own the middle.
   [evals](https://github.com/ai-outfitter/evals) proves that a change to a
   profile, model, or workflow made things better, with reproducible,
   attested benchmarks;
   [autoimprove](https://github.com/ai-outfitter/autoimprove) trains
   portable skills against real outcomes. The youngest components in the
   org, matching the rung they serve.

## Start with one workflow, end to end

The right first goal for most organizations is one workflow automated end to
end: **a feature idea becomes a merged PR**, and every step leaves evidence.
This is what rung 3 looks like up close.

![A feature idea flows through plan, implement, and adversarial review to a merged PR, with every transition writing to the evidence record](./assets/feature-to-pr.svg)

1. **Entry point.** Someone files an issue and assigns it to an agent — from
   the forge, from chat, or from a planning session at a desk. Different
   doors, same workflow.
2. **Plan.** A planner profile with read-only tools turns the issue into a
   spec artifact and posts it back to the issue, where a human can approve
   it (we recommend an enforced spec system, like
   [2119](https://github.com/Unsupervisedcom/2119), for more autonomous
   workflows).
3. **Implement.** An implementer profile picks up the approved spec in a
   fresh container with exactly the tools the work needs — a different
   agent, a clean context window, the same shared catalog.
4. **Review.** An adversarial reviewer profile, which shares none of the
   implementer's context, tries to break the change before any human reads
   it.
5. **Merge.** The PR arrives with its history attached: session transcripts,
   tool calls, and diffs captured as artifacts before the environment that
   produced them is torn down.

The same shape handles other great starting workflows. A vulnerability
report instead of a feature idea turns the pipeline into governed security
remediation: the scanner files the issue (most scanners already can), the
planner scopes the fix, and the same steps carry it to a tested, approved
PR. Bug reports run the same way with a triage step in front: an agent
reproduces and prioritizes each report, and only the ones that clear triage
enter the pipeline.

Every step is an agent profile from a shared catalog — plain files, pinned
by commit, reviewed by pull request, the same files at a desk or in CI.
That composition is also the adoption motion: an engineer refines a skill
in their own `~/.agents` against real work; the team mines
[pensieve](https://github.com/ai-outfitter/pensieve) for the patterns
behind successful and failing runs. When a change earns trust it moves by
pull request into the org catalog, where every agent composes it by name.
One person's improvement becomes everyone's default without anyone else
reconfiguring anything.

## Why plain files

Your agent setup is already configuration: system prompts, skills, MCP
servers, model choices, permissions. Today that configuration lives per tool
and per laptop, gets pasted between repositories, and drifts. Every other
kind of configuration your organization depends on graduated from that stage
years ago — into files, in a repository, behind review.

The [`.agents` convention](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/concepts.md#the-agents-protocol)
is an open standard for doing the same for agents:

```text
.agents/
  agents.md            # shared operating context
  system-prompt.md     # base system prompt
  mcp.json             # MCP servers
  models.json          # model configuration
  agents/<id>/agent.md # agent identities + loadouts
  skills/<id>/...      # capability packages
  knowledge/           # reference documents
  commands/            # slash commands
```

Markdown and JSON. Readable in an afternoon, reviewable in a pull request,
diffable in an audit. Layers merge by name — a project's `.agents/` over an
engineer's `~/.agents/` over the organization's pinned catalog — so
individuals keep their preferences and organizations keep their policy
([conventions](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/conventions.md)).

It is also the exit door. The directory is the source of truth and is useful
without Outfitter. Vendor-neutral cuts in every direction — models,
harnesses, and us. Swap model vendors freely. Run the catalog through any
harness: Pi has the deepest runtime support today, with Claude Code tracked
component by component in the
[support matrix](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/support-matrix.md)
— a Claude Code team starts on Claude Code, and the matrix shows the gaps
before you hit them.
And if you drop Outfitter itself, the catalog you built is still yours —
plain files, still working.

## Start this afternoon

None of these require an org-wide rollout, a purchase order, or new
infrastructure.

**1. See where you are — read-only, one afternoon.** The
[org-onboarding runbook](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/usecases/org-onboarding-sdlc-report.md)
produces a baseline **SDLC report**: where your organization sits on the
ramp, with evidence, gaps, and next-rung recommendations. Concretely: one
engineer runs the `sdlc-report` skill from their own local agent with a
read-only forge token. It reads through the API — never clones, never
writes — and produces two local files you review before anyone else sees
them. We ran it on this organization the day we wrote this page; the output
is committed at
[.agents/reports](https://github.com/ai-outfitter/.agents/tree/main/reports/sdlc),
so you can see exactly what you would get.

**2. Try the toolchain — ten minutes.**

```bash
npx @ai-outfitter/outfitter
```

This launches the Outfitter CLI with the
[Pi](https://github.com/earendil-works/pi) harness bundled and walks you
through composing your first agent profile from a starter catalog. You end
in a working agent session, and everything it created is plain files under
`~/.agents` — which you can read, edit, or delete afterward.

**3. Automate one workflow.** Pick feature-to-PR or bug-to-PR, keep it to
one repository, and promote the profiles you already trust at a desk into
[CI](https://github.com/ai-outfitter/actions). The
[getting started guide](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/getting-started.md)
and the [use cases](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/README.md)
cover the path.

## The repositories

Start with **[outfitter](https://github.com/ai-outfitter/outfitter)** — the
toolchain for `.agents`: compose an agent's context, tools, skills, and
permissions as a reviewable profile, then run the same profile locally, in
GitHub Actions, or in Kubernetes.

- **[actions](https://github.com/ai-outfitter/actions)** *(rung 3)* — run
  any profile headless in CI: scheduled or event-driven reviewers,
  implementers, and auditors.
- **[channels](https://github.com/ai-outfitter/channels)** *(rungs 3–4)* —
  push email, Slack, Signal, and forge events into an agent session; wake
  only on real work.
- **[agent-operator](https://github.com/ai-outfitter/agent-operator)**
  *(rung 4, in active build)* — Kubernetes `Organization` and `Agent`
  resources for resident agents in your cluster; running internally today.
- **[pensieve](https://github.com/ai-outfitter/pensieve)** *(rung 4, in
  active build)* — the write-once evidence store: per-harness collectors, an
  S3 Object Lock backend, and a verifier. The specification leads the code
  by design — capture the bytes first, build views second — and collectors
  are already landing against it.
- **[deepwork](https://github.com/ai-outfitter/deepwork)** *(rungs 2–3)* —
  structured multi-step jobs with typed arguments and quality gates. Runs on
  Pi, Claude Code, and Codex.
- **[evals](https://github.com/ai-outfitter/evals)** *(rung 5, new)* —
  reproducible, attested benchmarks for agent profiles and harnesses.
- **[autoimprove](https://github.com/ai-outfitter/autoimprove)** *(rung 5)*
  — recursive skill improvement: train portable markdown skills against
  real outcomes.
- **[community-profiles](https://github.com/ai-outfitter/community-profiles)**
  / **[default-profiles](https://github.com/ai-outfitter/default-profiles)**
  — shared catalogs of agents and skills, pinned by release, composed by
  name.
- **[.agents](https://github.com/ai-outfitter/.agents)** — this
  organization's own catalog; it holds our baseline SDLC report — the
  runbook's first step, run on ourselves.

Pi extension packages:
[ulta-tasklist](https://github.com/ai-outfitter/ulta-tasklist),
[file-talk](https://github.com/ai-outfitter/file-talk),
[bash-saver](https://github.com/ai-outfitter/bash-saver).

## Where this is today

This stack is built with the leverage it sells, and it shows in the commit
dates: v1.5.0 followed v1.4.0 by three days, and the evidence store went
from an empty repository to a public specification to its first working
collectors in under 48 hours. Actions and the catalogs carry this org's own
real workloads. Read the status tags above against that tempo — the unit is
days to weeks, not quarters.

The model is open core: the convention, the toolchain, and the defaults are
MIT — modules you can rip out and replace — while some advanced capabilities
ship under an enterprise license.
If you are evaluating this for an organization, start
with the
[SDLC report](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/usecases/org-onboarding-sdlc-report.md),
then [open an issue](https://github.com/ai-outfitter/outfitter/issues) with
what you found. The gaps you hit are the roadmap we want.

## Philosophy

The full argument lives in
[docs/philosophy.md](https://github.com/ai-outfitter/outfitter/blob/main/docs/philosophy.md);
the short version:

- **Trust through evidence.** An agent is trusted the way a new teammate is:
  bounded scopes, adversarial review as a pipeline step, every transition on
  the record.
- **Own your session data.** The record that satisfies an auditor also feeds
  your evals, policy tuning, and training — losing it discards the asset
  that makes the system improvable.
- **Expeditious agents.** Tight, purpose-built profiles preserve the context
  headroom that turns into better decisions and faster sessions.
- **Composition over accumulation.** Stack a personal baseline, a team
  convention, and a project role; switch as the work changes.
