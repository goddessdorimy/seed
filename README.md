<div align="center">
  <img src="terminal.svg" alt="SEED terminal" width="740"/>
</div>

<div align="center">

[![Claude Code](https://img.shields.io/badge/Claude%20Code-compatible-8b5cf6?style=flat-square)](https://claude.ai/code)
[![License](https://img.shields.io/badge/license-MIT-blue?style=flat-square)](LICENSE)

**Your AI project incubator.**<br/>
Take raw ideas through type-aware guided exploration and produce structured plans ready for building.

</div>

---

## Contents

- [What SEED Does](#what-seed-does)
- [Commands](#commands)
- [Project Types](#project-types)
- [How It Works](#how-it-works)
- [Architecture](#architecture)
- [The PAUL Connection](#the-paul-connection--ideation-to-managed-build)
- [Ecosystem](#ecosystem)
- [Install](#install)

---

## Ecosystem

SEED is part of a broader Claude Code extension ecosystem:

| System | What It Does | Link |
|--------|-------------|------|
| **AEGIS** | Multi-agent codebase auditing — diagnosis + controlled evolution | [GitHub](https://github.com/ChristopherKahler/aegis) |
| **BASE** | Builder's Automated State Engine — workspace lifecycle, health tracking, drift prevention | [GitHub](https://github.com/ChristopherKahler/base) |
| **CARL** | Context Augmentation & Reinforcement Layer — dynamic rules loaded JIT by intent | [GitHub](https://github.com/ChristopherKahler/carl) |
| **PAUL** | Project orchestration — Plan, Apply, Unify Loop | [GitHub](https://github.com/ChristopherKahler/paul) |
| **SEED** | Typed project incubator — guided ideation through graduation | You are here |
| **CC Strategic AI** | Skool community — courses, community, live support | [Skool](https://skool.com/cc-strategic-ai) |

---

## What SEED Does

You have an idea. Maybe it's an app, a workflow, a client site, a utility, or a content campaign. Before you start building, you need to answer the right questions — and the right questions depend on what type of project it is.

SEED is a typed project incubator for Claude Code. It guides you through collaborative exploration shaped by your project type, produces a structured PLANNING.md, and graduates mature plans into buildable project directories. If you use PAUL, it can initialize a managed build from your plan without re-asking questions you already answered.

**The coach, not the interrogator.** SEED brainstorms alongside you, offers suggestions when you're stuck, and pushes toward decisions when it's time. It doesn't fire questions at you — it thinks with you.

---

## Commands

| Command | What It Does |
|---------|-------------|
| `/seed` | Start a guided ideation session (default) |
| `/seed graduate` | Move completed ideation to `apps/` with git repo and synthesized README |
| `/seed launch` | Graduate + initialize PAUL for managed build (one command) |
| `/seed status` | Show all projects in the ideation pipeline |
| `/seed add-type` | Create a custom project type for SEED's data layer |

---

## Project Types

SEED ships with 5 default types. Each type shapes the conversation differently:

| Type | Rigor | Sections | What It's For |
|------|-------|----------|---------------|
| **Application** | Deep | 10 | Software with UI, data model, API, deployment |
| **Workflow** | Standard | 8 | Claude Code commands, hooks, CARL domains, skills |
| **Client** | Standard | 7 | Client websites — business context, conversion, content |
| **Utility** | Tight | 6 | Small tools, scripts, single-purpose — resists expansion |
| **Campaign** | Creative | 7 | Content, marketing, launches — timeline-driven |

**Rigor adapts behavior:**
- **Tight** — Move fast, resist scope creep, done in one session
- **Standard** — Balanced exploration with clear boundaries
- **Deep** — Thorough, architecture matters, don't rush
- **Creative** — Loose and generative, but always anchored to a measurable goal

Types are composable. Run `/seed add-type` to create your own — just drop files in `data/{type}/` and it works immediately. No code changes needed.

---

## How It Works

```
  /seed                          /seed graduate              /seed launch
    │                                  │                          │
    ▼                                  ▼                          ▼
 Select type ──▶ Guided ──▶ PLANNING.md ──▶ apps/{name}/ ──▶ .paul/ init
 (or discover)   ideation    (populated)     git + README      (headless)
    │                │
    ▼                ▼
 Load data/     Coach persona
 {type}/        adapts rigor
```

1. **Type first.** SEED determines your project type before anything else. Type shapes everything downstream.
2. **Guided exploration.** Conversation sections load from `data/{type}/guide.md`. Each section has prompts (what to explore) and suggestions (what to offer when you're stuck).
3. **PLANNING.md output.** When you're ready, SEED populates a type-specific template and writes it to `projects/{name}/`.
4. **Graduate.** Moves the project to `apps/{name}/` with git init, a synthesized README, and workspace tracking updates.
5. **Launch (optional).** Wraps graduation + headless PAUL init. Your PLANNING.md is rich enough that PAUL derives its structure without re-asking questions.

---

## Architecture

```
seed/
├── seed.md                         Entry point (Skillsmith-compliant)
├── tasks/
│   ├── ideate.md                   Type-first guided ideation
│   ├── graduate.md                 Project graduation to apps/
│   ├── launch.md                   Graduate + PAUL init wrapper
│   ├── status.md                   Pipeline visibility
│   └── add-type.md                 User-extensible type system
├── data/                           Composable per-type data
│   ├── application/
│   │   ├── guide.md                Conversation sections (Explore + Suggest)
│   │   ├── config.md               Rigor, demeanor, required sections
│   │   └── skill-loadout.md        Ecosystem tool recommendations
│   ├── workflow/                   Same structure
│   ├── client/                     Same structure
│   ├── utility/                    Same structure
│   └── campaign/                   Same structure
├── templates/
│   ├── planning-application.md     PLANNING.md output template
│   ├── planning-workflow.md
│   ├── planning-client.md
│   ├── planning-utility.md
│   └── planning-campaign.md
├── checklists/
│   └── planning-quality.md         Quality gate for graduation
├── terminal.svg                    Hero image
└── README.md                       You are here
```

**The key design decision:** Type-specific content lives in `data/{type}/` files, not hardcoded in task files. This means:
- Adding a new type = dropping 3 files in a new `data/` directory
- No task file modifications needed
- The ideate task loads type content dynamically after selection

---

## The PAUL Connection — Ideation to Managed Build

This is where SEED pays off. The entire ideation flow is designed so that the PLANNING.md it produces contains everything [PAUL](https://github.com/ChristopherKahler/paul) needs to set up a structured build — milestones, phases, tech stack, architecture decisions, phase breakdowns — without asking you anything twice.

### The Problem

Starting a PAUL project from scratch means answering a series of setup questions: What's the project? What's the stack? What are the milestones? What are the phases? If you've already thought through all of this during ideation, repeating it during project setup is friction that kills momentum.

### How SEED Solves It

`/seed launch` runs the full graduation flow and then initializes PAUL with **headless context**:

1. SEED produces a rich PLANNING.md during ideation (type-aware, quality-gated)
2. `/seed launch` graduates the project to `apps/{name}/` with git
3. PAUL reads the PLANNING.md and derives its entire project structure from it
4. PAUL proposes milestones, phases, and tech stack — you review and approve
5. You're immediately ready to run `/paul:plan` for Phase 1

**No re-asking.** PAUL doesn't ask "What's this project?" or "What's the tech stack?" — those answers are already in the PLANNING.md that SEED produced. The only interaction is approving the proposed structure.

### What Makes the PLANNING.md PAUL-Ready

SEED's quality gate (`checklists/planning-quality.md`) specifically checks for PAUL-readiness:

- Phase breakdown with independently shippable milestones
- Each phase has build/test/outcome defined
- Tech stack decisions resolved (not "TBD")
- Design decisions documented with rationale
- Enough detail that PAUL can derive structure without questions

### Without PAUL

Don't use PAUL? `/seed graduate` works standalone — you get a clean `apps/{name}/` directory with git and a synthesized README. Build however you want. PAUL is additive, not required.

---

## Ecosystem

SEED works standalone but integrates with the broader toolkit:

| Tool | How SEED Uses It |
|------|-----------------|
| **PAUL** | `/seed launch` initializes PAUL with headless context from PLANNING.md — zero-friction handoff from ideation to managed build |
| **BASE** | Graduate updates ACTIVE.md for workspace-level project tracking |
| **Skillsmith** | SEED was built with Skillsmith — compliant entry point + tasks |
| **AEGIS** | Recommended post-build audit for application-type projects |

All integrations are additive. SEED works without any of them installed.

---

## Install

```bash
npm i -g @chrisai/seed
```

One command. Installs the skill to `~/.claude/commands/seed/` — available in every workspace.

```bash
# Global install (default) — available everywhere
npm i -g @chrisai/seed

# Install to current project only
npm i -g @chrisai/seed -- --local

# Custom Claude config directory
npm i -g @chrisai/seed -- --config-dir /path/to/.claude
```

Then open Claude Code and type `/seed` to start.

### What Gets Installed

```
~/.claude/commands/seed/
├── seed.md              Entry point (routing + persona)
├── tasks/               5 task files
├── data/                15 type-specific data files (5 types × 3 files)
├── templates/           5 PLANNING.md output templates
└── checklists/          Planning quality gate
```

No hooks, no MCP servers, no workspace data. SEED is pure markdown — zero runtime dependencies.

### Requirements

- [Claude Code](https://claude.ai/code)
- Node.js >= 16 (for install script only — SEED itself has no runtime deps)

---

## License

MIT — [Chris Kahler](https://github.com/ChristopherKahler)
