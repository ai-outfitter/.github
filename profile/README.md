# AI Outfitter

Open, vendor-neutral tooling for controlling agents: ramp a user, a team,
or an organization from AI-assisted coding to an autonomous software
development lifecycle.

Everything here builds on one convention: an agent's context, tools, skills,
and permissions are plain files in a `.agents/` directory — committed,
reviewed, and shared like the rest of your code. The same definition runs
across environments (on a laptop, headless in CI, or as a resident agent in
a cluster) and with any model vendor, including self-hosted models. The core
is open source under MIT.

## Why we built AI Outfitter

We built AI Outfitter to help teams adopt mature agentic engineering
processes faster. It meets you where you are — sane, opinionated defaults
with enough flexibility to customize — because most teams are already
partway up the ramp below, and the next rung is hard to see from where you
stand.

## The ramp (how to use Outfitter at different levels of agentic engineering maturity)

Five rungs, from AI-assisted coding to an autonomous lifecycle
([full definition](https://github.com/ai-outfitter/outfitter/blob/main/docs/philosophy.md)).
Each Outfitter component targets a rung, so you climb without rebuilding what
got you here — and without adopting complexity too early. Rungs 2, 3, and 4
each open with the **runbook** that gets you there: the concrete steps, and a
success check you run rather than judge.

1. **Assisted** — autocomplete and chat; a human's hands stay on the
   keyboard. *You are here if* you use autocomplete in an IDE.
   There is nothing to govern yet — but the habit that matters starts here:
   document what works and what doesn't in `AGENTS.md`/`CLAUDE.md`, and
   keep it in the repo.

2. **Delegated** — a local agent does the task; you define the idea and
   review the PR. *You are here if* engineers run a coding agent in a
   terminal and push the result. This is where configuration starts to
   matter.
   - **Runbook: [Share one catalog](https://github.com/ai-outfitter/outfitter/blob/main/docs/runbooks/share-one-catalog.md)**
     — one pinned catalog the organization shares, instead of per-laptop
     configuration.
   - [outfitter](https://github.com/ai-outfitter/outfitter) — composes what
     an agent knows and may do into a **profile**: plain files in your
     `.agents/` folder, reviewed like code and portable across environments
     and **harnesses** (the CLI that runs an agent: Claude Code, Pi, Codex).
   - [deepwork](https://github.com/ai-outfitter/deepwork) — step-by-step
     quality gates so the agent checks its own work.

3. **Automated** — a workflow runs without your laptop: an issue, a message,
   or a schedule triggers agents in CI, on a cluster, or on a remote server
   you never sit at; adversarial review is part of the pipeline; session
   logs are captured before merge. *You are here when* you close your laptop
   and the work keeps going. What promotes you is the trigger, not the
   hardware: an agent you drive over SSH is rung 2 on a bigger machine.
   - **Runbook: [Run it without your laptop](https://github.com/ai-outfitter/outfitter/blob/main/docs/runbooks/run-without-your-laptop.md)**
     — an event triggers the workflow, its output lands through review, and
     the session is captured.
   - [actions](https://github.com/ai-outfitter/actions) — runs any profile
     headless in GitHub Actions, on any trigger.
   - [channels](https://github.com/ai-outfitter/channels) — pushes email,
     Slack, Signal, and forge events (GitHub or GitLab activity) into an
     agent session, so one message can start the same workflow.
   - [agent-operator](https://github.com/ai-outfitter/agent-operator) —
     hosts the same profiles on your own infrastructure, well before you
     need its resident-agent story on the next rung.

4. **Governed** — the organization shares one version-pinned catalog of
   agents, skills, and policy; every agent action lands in an auditable
   record; **resident agents** — long-lived agents onboarded like teammates,
   with their own accounts and boundaries — take on standing jobs. *You are
   here when* agents work across many teams, and the organization needs
   shared policy — and proof of what every agent did.
   - **Runbook: [Give the agent a residence](https://github.com/ai-outfitter/outfitter/blob/main/docs/runbooks/give-the-agent-a-residence.md)**
     — a named, assignable agent with an account and somewhere to live.
   - **Build your catalog: [Share one catalog](https://github.com/ai-outfitter/outfitter/blob/main/docs/runbooks/share-one-catalog.md)**
     — the rung 2 runbook creates it and links on to the layout, pinning,
     and governance references. Our own
     [.agents](https://github.com/ai-outfitter/.agents) is a worked example
     you can read end to end.
   - [agent-operator](https://github.com/ai-outfitter/agent-operator) —
     provisions and supervises resident agents on your own infrastructure.
   - [pensieve](https://github.com/ai-outfitter/pensieve) — the write-once
     evidence store the audit story lands in: an immutable log of every
     action taken, bound to environments, agents, tools, sessions,
     artifacts, and costs. That log is what lets you audit your processes,
     prove compliance, and run automated, recursive self-improvement on
     them.

5. **Self-improving** — the audit record feeds evals and improvement; humans
   set goals and acceptance gates, agents own the middle. Rung 5 is where
   the stack is thinnest today.
   - [evals](https://github.com/ai-outfitter/evals) — proves that a change
     to a profile, model, or workflow made things better, with reproducible,
     attested benchmarks.
   - [autoimprove](https://github.com/ai-outfitter/autoimprove) — trains
     portable skills against real outcomes.

## Start with one workflow end to end

The right first goal for most organizations: **a feature idea becomes a
merged PR**, automated end to end, and every step leaves evidence. This is
what rung 3 looks like up close.

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

The same shape handles other starting workflows. A vulnerability report
instead of a feature idea turns the pipeline into governed security
remediation: the scanner files the issue (most scanners already can), the
planner scopes the fix, and the same steps carry it to a tested, approved
PR. Bug reports run the same way with a triage step in front: an agent
reproduces and prioritizes each report, and only the ones that clear triage
enter the pipeline.

Composition is also the adoption motion: an engineer refines a skill in
their own `~/.agents` against real work; the team mines
[pensieve](https://github.com/ai-outfitter/pensieve) for the patterns behind
successful and failing runs. When a change earns trust it moves by pull
request into the org catalog, where every agent composes it by name. One
person's improvement becomes everyone's default at the next pin bump — no
one else reconfigures anything.

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
[.agents/reports/sdlc](https://github.com/ai-outfitter/.agents/tree/main/reports/sdlc),
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
[CI](https://github.com/ai-outfitter/actions). Work the runbooks in order —
[share one catalog](https://github.com/ai-outfitter/outfitter/blob/main/docs/runbooks/share-one-catalog.md),
[run it without your laptop](https://github.com/ai-outfitter/outfitter/blob/main/docs/runbooks/run-without-your-laptop.md),
then [give the agent a residence](https://github.com/ai-outfitter/outfitter/blob/main/docs/runbooks/give-the-agent-a-residence.md).
Each ends with the one concrete step that starts the next.

## The repositories

Everything composes with **[outfitter](https://github.com/ai-outfitter/outfitter)**,
the toolchain for `.agents`. The rung column maps each repo to the ramp
above.

| Repo | Rung | What it is |
| --- | --- | --- |
| **[outfitter](https://github.com/ai-outfitter/outfitter)** | 2 | Compose a profile, then run that same profile locally, in GitHub Actions, or in Kubernetes. |
| **[deepwork](https://github.com/ai-outfitter/deepwork)** | 2–3 | Structured multi-step jobs with typed arguments and quality gates. Runs on Pi, Claude Code, and Codex. |
| **[actions](https://github.com/ai-outfitter/actions)** | 3 | Any profile, headless in CI: scheduled or event-driven reviewers, implementers, and auditors. |
| **[channels](https://github.com/ai-outfitter/channels)** | 3–4 | Push email, Slack, Signal, and forge events into an agent session; wake only on real work. |
| **[agent-operator](https://github.com/ai-outfitter/agent-operator)** | 4 | Kubernetes `Organization` and `Agent` resources for resident agents in your cluster. *In active build; running internally today.* |
| **[pensieve](https://github.com/ai-outfitter/pensieve)** | 4 | The write-once evidence store: per-harness collectors, an S3 Object Lock backend, and a verifier. *In active build; the specification leads the code by design.* |
| **[evals](https://github.com/ai-outfitter/evals)** | 5 | Reproducible, attested benchmarks for agent profiles and harnesses. *New.* |
| **[autoimprove](https://github.com/ai-outfitter/autoimprove)** | 5 | Recursive skill improvement: train portable markdown skills against real outcomes. |
| **[default-profiles](https://github.com/ai-outfitter/default-profiles)** / **[community-profiles](https://github.com/ai-outfitter/community-profiles)** | any | Shared catalogs of agents and skills, pinned by release, composed by name. |
| **[.agents](https://github.com/ai-outfitter/.agents)** | any | This organization's own catalog, holding the baseline SDLC report we ran on ourselves. |

Pi extension packages:
[ulta-tasklist](https://github.com/ai-outfitter/ulta-tasklist),
[file-talk](https://github.com/ai-outfitter/file-talk),
[bash-saver](https://github.com/ai-outfitter/bash-saver).

## Agent configuration as code

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

Markdown and JSON. Readable in a sitting, reviewable in a pull request,
diffable in an audit. Layers merge by name — a project's `.agents/` over an
engineer's `~/.agents/` over the organization's **catalog**, a shared
collection pinned by version — so individuals keep their preferences and
organizations keep their policy
([conventions](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/conventions.md)).

Composition beats accumulation. A profile is a selection from those files,
and profiles stack: a personal baseline, a team convention, a project role,
switched as the work changes. That keeps each profile tight, and tight
profiles preserve the context headroom that turns into faster, better
sessions.

The directory is also the exit door. It is the source of truth, and it is
useful without Outfitter. Vendor-neutral cuts in every direction — models,
harnesses, and us. Swap model vendors freely. Run the catalog through any
harness: Pi has the deepest runtime support today, with Claude Code tracked
component by component in the
[support matrix](https://github.com/ai-outfitter/outfitter/blob/main/docs/documentation/support-matrix.md)
— a Claude Code team starts on Claude Code, and the matrix shows the gaps
before you hit them. And if you drop Outfitter itself, the catalog you
built is still yours — plain files, still working.

## Where this is today

This stack is built with the leverage it sells, and it shows in the commit
dates: v1.5.0 followed v1.4.0 by three days, and the evidence store went
from an empty repository to a public specification to its first working
collectors in under 48 hours. Actions and the catalogs carry this org's own
real workloads. Read the rung column and the status notes beside it against
that tempo — the unit is days to weeks, not quarters.

The model is open core: the convention, the toolchain, and the defaults are
MIT — modules you can rip out and replace — while some advanced capabilities
ship under an enterprise license. If you are evaluating this for an
organization, [open an issue](https://github.com/ai-outfitter/outfitter/issues)
with what you found. The gaps you hit are the roadmap we want.

## As you climb

Two suggestions we make strongly, and follow ourselves:

1. **Automate nothing you have not first done manually.**
2. **Hand over control one layer at a time.**

You never skip a step you don't understand, and nothing you build on one
rung is thrown away on the next.
