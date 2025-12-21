# Tether

Tether is a Claude Code plugin for disciplined, anchored, and tightly scoped development. It seizes the step function jump in Agentic coding capabilities brought on by Opus 4.5. 

## Background 

Before Opus 4.5, most agent and tool orchestrations were implicitly working *around* the model's worst behaviors: scope creep and over-engineering. Coding assistants felt like overeager Junior-savants that needed careful steering whenever projects became more complex than simple, clean linear flows. 

This pattern was cleanly broken by Opus 4.5. If you're reading this you've likely felt the shift. When talking to Opus 4.5, it *gets* it. The ineffable *it*. We can safely say this is the change of LLM Agents from spastic assistants to powerful, intelligent collaborators.

## Core Principle: The Workspace

Tether builds on this agentic paradigm shift with a Workspace to represent knowledge and action in constrained environments. Workspaces are the linked, evolving record of a project's scope, decisions, and implementations. They are built from structured markdown files with a naming convention inspired by Herbert A. Simon's List of Lists. And, most importantly, they persist across sessions.

Each task becomes a file. The naming convention *is* the data structure:

```
workspace/NNN_task-slug_status[_from-NNN].md
```

| Name Part   | Purpose                       | Changes                                        |
| ----------- | ----------------------------- | ---------------------------------------------- |
| `NNN`       | Sequence number (001, 002...) | Grows as work progresses                       |
| `task-slug` | Human-readable identifier     | Set at creation                                |
| `status`    | Current state                 | `active` → `complete`, `blocked`, or `handoff` |
| `_from-NNN` | Lineage suffix                | Links child tasks to parents                   |

The filesystem becomes queryable memory. `ls workspace/` shows the active, ongoing work. `ls workspace/*_from-003*` reveals everything that emerged from task 003. Understanding compounds across sessions. When you return tomorrow, the structure is waiting and ready to go.

Workspace files have three sections that mirror tether's development cycle:

| Section    | Purpose                                                                                                             |
| ---------- | ------------------------------------------------------------------------------------------------------------------- |
| **Anchor** | The fixed point. Scope, exclusions, patterns to follow, path, and delta—everything you're bound to.                 |
| **Trace**  | The reasoning traced during build. Patterns noticed, decisions made, constraints hit—written *before* implementing. |
| **Close**  | The proof. What was delivered, what was deliberately omitted, why.                                                  |

## tether's Development Cycle

Tether follows a four-phase development rhythm:

1. **Assess** — Can this anchor to a single behavior? Does it need a workspace file, or is execution obvious?
2. **Anchor** — Create the workspace file. Define the scope boundary and path. Write what you will *not* do.
3. **Build** — Implement. Trace your reasoning before coding. If scope expands, stop.
4. **Close** — Add the omissions section. Rename the file to its final status.

These phases are how drift becomes visible *before* it leaks and compounds.

## Drift Signals

Certain phrases are reliable indicators of scope creep:

- *"flexible," "extensible," "comprehensive"*
- *"while we're at it," "might as well"*
- *"in case," "future-proof"*

When tether detects these phrases, it stops and anchors around the original request before moving forward.

## Architecture

Tether is a plugin made up of skills, agents, and commands:

| Component               | Purpose                                                                    |
| ----------------------- | -------------------------------------------------------------------------- |
| **Skill**               | Auto-invoked on create/build/implement. Embeds the checkpoint methodology. |
| **code-builder agent**  | Test-first implementation. Edits over creates. No unprompted abstractions. |
| `/tether:workspace`     | Query active tasks and their lineage.                                      |
| `/tether:anchor [task]` | Create a new workspace file with scope boundary.                           |
| `/tether:close [task]`  | Complete a task. Add omissions. Rename to final status.                    |
| `/tether:drift`         | Review current work against its anchor for scope creep.                    |

## Guiding Principles

tether bases all of its work on the following key principles:

| Principle                  | Description                                             |
| -------------------------- | ------------------------------------------------------- |
| **Edit over create**       | Modify what exists before creating something new.       |
| **Concrete over abstract** | Build the specific solution, not an abstract framework. |
| **Present over future**    | Implement current requirements, not anticipated ones.   |
| **Explicit over clever**   | Choose clarity over sophistication.                     |


## When to Use Tether

Tether is the right tool when:

- Precision matters more than speed
- Understanding must persist across sessions
- Reasoning needs to be visible and auditable
- Scope requires explicit negotiation with the model

## When Not to Use Tether

For all of its power, tether does add overhead. While that overhead compounds into value over multi-session, complex work, it is the wrong tool for:

- Exploratory prototyping where you *want* the model to wander
- Autonomous long-running tasks (use Ralph or similar orchestrators)
- Simple one-off queries that need no persistence

## Installation

Add this plugin to your marketplace and install it. 
```bash
claude /plugin install <your-marketplace>/tether
```

Make sure to update the marketplace.json file to include the plugin.

## Command Usage

```bash
/tether:workspace                              # check existing work
/tether:anchor implement password reset flow   # start new task
/tether:close password-reset                   # complete task
```

## Structure

```
tether/
├── skills/tether/
│   ├── SKILL.md
│   └── references/
├── agents/code-builder.md
└── commands/
```