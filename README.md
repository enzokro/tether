# Tether

Tether is a Claude Code plugin for disciplined and scoped development. It seizes the step function jump in Agentic coding capabilities unlocked by Opus 4.5.

## Background

Before Opus 4.5, most agent and tool orchestrations were implicitly working *around* the model's worst tendencies: scope creep and over-engineering. Coding assistants felt like overeager Junior-savants that had to be carefully steered whenever projects became more than simple, clean data flows.

Opus 4.5 broke this pattern. If you're reading this then you've likely felt the shift. Talking to Opus 4.5, it *gets* it. The ineffable *it*. We can safely say this is the change in LLM Agents from spastic assistants to powerful, intelligent collaborators.

We can fully leverage the combination of models being much more capable and understanding us better to create a truly excellent development loop. Tether builds on this shift using an orchestrator that coordinates sub-agents with structural constraints: tiered phases, verified callbacks, and externalized thinking traces that gather over sessions.

## Philosophy

Tether is based on the following key principles:

| Principle                  | Description                                             |
| -------------------------- | ------------------------------------------------------- |
| **Edit over create**       | Modify what exists before creating something new.       |
| **Concrete over abstract** | Build the specific solution, not an abstract framework. |
| **Present over future**    | Implement current requirements, not anticipated ones.   |
| **Explicit over clever**   | Choose clarity over sophistication.                     |


## The Development Cycle

Tether follows a four-phase development cycle. Each phase is a separate agent with bounded context. The orchestrator verifies that artifacts exist before spawning the next phase. This is *structural enforcement*, not self-discipline.

```
[Assess] → route → [Anchor] → file+T1 → [Build] → T2,T3+ → [Close]
    ↓                  ↓                    ↓                  ↓
 decide            verify T1           verify T2,T3      verify Omitted
```

### Phase 1: Assess

Can this request anchor to a single, concrete behavior? Does it need a workspace file, or is the path obvious?

| Actionable? | Needs thinking? | Route to           |
| ----------- | --------------- | ------------------ |
| Yes         | Yes             | Anchor (full flow) |
| Yes         | No              | Build (direct)     |
| No          | —               | User (clarify)     |

### Phase 2: Anchor

Create the workspace file. Explore the codebase for patterns. Define the scope boundary, the path, and crucially—what the model will *not* do.

**Contract**: Anchor must fill T1 before Build can proceed. T1 captures what was learned during exploration—patterns found, approach chosen, constraints identified. This is the decision trace that informs implementation.

### Phase 3: Build

Implement exactly what was anchored. No more, no less.

The critical discipline here is the **Connection Requirement**: each Trace entry (T2, T3+) must reference the Anchor explicitly. Not "I did X." Instead: "I did X, advancing Path step 2, avoiding Y which is in Excluded."

| Moment             | Action                                     |
| ------------------ | ------------------------------------------ |
| First step done    | Write T2 → reference Anchor path           |
| Made a decision    | Write T3+ → reference Anchor constraints   |
| Update TodoWrite   | Also write to Trace (pairing rule)         |
| Can't connect      | Stop → you've drifted, reassess            |
| Complexity growing | Run `/tether:creep` → check against Anchor |

**Contract**: Build must fill T2 and T3+ before Close can proceed. Each must reference the Anchor.

### Phase 4: Close

Verify all contracts. Fill the Close section with what was delivered and what was *deliberately omitted*. Rename the file to its final status.

**Contract**: Omitted must be non-empty. If you implemented everything you could think of, scope creep occurred. The measure of discipline is what was deliberately left out.


## The Workspace: Externalized Knowledge Across Sessions

Tether builds on this agentic paradigm shift with a Workspace. Workspaces are the linked, evolving record of a project's scope, decisions, and implementations. They are built from structured markdown files with a naming convention inspired by Herbert A. Simon's List of Lists. We created them in this spirit to represent knowledge and action in constrained environments. And, critically, they persist across sessions.

### Workspace File Structure

Each task creates workspace files. The file naming convention *is* the data structure:

```
workspace/NNN_task-slug_status[_from-NNN].md
```

| Name Part   | Purpose                       | Changes                                        |
| ----------- | ----------------------------- | ---------------------------------------------- |
| `NNN`       | Sequence number (001, 002...) | Grows as work progresses                       |
| `task-slug` | Human-readable identifier     | Set at creation                                |
| `status`    | Current state                 | `active` → `complete`, `blocked`, or `handoff` |
| `_from-NNN` | Lineage suffix                | Links child tasks to parents                   |

Workspace files contain three sections that mirror Tether's development cycle:

| Section    | Purpose                                                                                      |
| ---------- | -------------------------------------------------------------------------------------------- |
| **Anchor** | The fixed point. Scope, exclusions, patterns, path, delta. Everything the model is bound to. |
| **Trace**  | Decision traces (T1, T2, T3...). Each must reference the Anchor. Verified between phases.    |
| **Close**  | What was delivered, what was *deliberately omitted*. Omitted must be non-empty.              |

### Workspaces as Persistent, Queryable Memory

With this structure, the filesystem becomes queryable memory. `ls workspace/` shows the active, ongoing work. `ls workspace/*_from-003*` reveals everything that emerged from task 003. Understanding compounds across sessions. When you come back tomorrow, the structure is waiting for you and ready to go.


## Installation

Add the tether plugin to your marketplace and install it.
```bash
claude /plugin install <your-marketplace>/tether
```

Make sure to update the marketplace.json file to include the plugin.


## Architecture

Tether v2.0 introduces orchestrated phases with contract verification between each.

### Phase Agents

| Agent                        | Purpose                                             | Model   |
| ---------------------------- | --------------------------------------------------- | ------- |
| `tether:tether-orchestrator` | Coordinates phases, verifies contracts              | inherit |
| `tether:assess`              | Routing decision                                    | haiku   |
| `tether:anchor`              | Creates workspace file, explores codebase, fills T1 | inherit |
| `tether:code-builder`        | Implementation with T2/T3+ decision traces          | inherit |
| `tether:close`               | Verifies contracts, fills Close, renames file       | haiku   |

### Commands (Manual Intervention)

| Command                 | Purpose                              |
| ----------------------- | ------------------------------------ |
| `/tether:workspace`     | Query active tasks and their lineage |
| `/tether:anchor [task]` | Manually create a workspace file     |
| `/tether:close [task]`  | Manually complete a task             |
| `/tether:creep`         | Check for scope creep during Build   |


## Decision Traces

The Trace section is where reasoning becomes visible and auditable. Each checkpoint must reference the Anchor—that's what keeps the model connected to scope during long implementation sprints.

- **T1** (Anchor fills): Patterns found, approach chosen, references Path
- **T2** (Build fills): After first implementation step, references Anchor
- **T3+** (Build fills): Significant decisions during implementation, each references Anchor

The **Pairing Rule**: every TodoWrite update pairs with a Trace write. TodoWrite tracks *what* you're doing; Trace captures *why* and connects it to the Anchor.


## When to Use Tether

Tether is the right tool when:

- Precision matters more than speed
- Understanding must persist across sessions
- Reasoning needs to be visible and auditable
- Scope requires explicit negotiation with the model

## When Not to Use Tether

For all of its power, Tether does add overhead. While that overhead compounds into value over multi-session, complex work, it is the wrong tool for:

- Exploratory prototyping where you *want* the model to wander
- Autonomous long-running tasks (use Ralph or similar orchestrators)
- Simple one-off queries that need no persistence
