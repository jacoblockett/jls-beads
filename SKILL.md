---
name: beads-design
description: Exhaustively compile a user-designated authoritative goal, specification, plan, export, or source corpus into a faithful, bite-sized Beads work graph. Use when explicitly invoked as $beads-design or when the user asks to turn source material into durable actionable Beads without losing requirements.
---

# Beads Design

Beads Design is a lossless compiler from authoritative source material to durable Beads work. It does not implement the resulting work.
Use this workflow only to derive, write, and verify Beads issues from the exact source scope the user designated.

The official `beads` skill and live `bd` CLI documentation remain authoritative for Beads mechanics. Beads Design owns source fidelity, decomposition, provenance, coverage, and review.

## Invariants

1. The user-designated source scope is authoritative for this transaction. Do not silently add requirements from chat memory, unrelated repository files, existing Beads, or implementation preferences.
2. Preserve meaning losslessly. Every materially relevant source element must be represented by actionable work, constrain actionable work, block actionable work, be explicitly deferred/out of scope, or be explicitly classified non-actionable with a reason. Nothing may silently disappear.
3. Source units are not task units. Do not create one issue per paragraph, node, bullet, or record unless that is naturally the correct executable boundary.
4. Decompose top-down: objective -> major capability or component -> narrow implementation responsibility -> leaf work.
5. Prefer small leaves. A leaf is acceptable only when one implementation agent can pick it up without first decomposing it into independently assignable changes.
6. Split independently assignable changes. If two changes can be implemented, reviewed, or reverted independently without violating one atomic invariant, they normally belong in separate leaf issues.
7. Parent issues provide structure and shared context. Leaf issues own executable responsibilities. Do not use a parent epic as a substitute for missing leaves.
8. Each executable issue must be self-contained enough to resume without the original conversation. Preserve necessary context, source anchors, constraints, dependencies, and objective acceptance conditions when they exist.
9. Preserve provenance. Each issue must identify the source anchors that justify it. Structured sources should retain native IDs when available. Unstructured sources should use stable path/section/line or equivalent anchors when practical.
10. Preserve dependencies that materially affect execution order. Add inferred technical dependencies only when they are genuinely required, and distinguish them from source-stated dependencies in issue context when useful.
11. Existing Beads are context, not authority over the supplied source. Reuse or update a clearly equivalent existing issue instead of duplicating it, but do not let stale tracker state erase or rewrite authoritative source requirements.
12. Do not invent product decisions to make the task graph look complete. Material ambiguity that prevents faithful decomposition becomes a blocker, not a guessed requirement.
13. Children run serially. Consume and close each child before spawning another. Children never spawn children.
14. Spawn prompts contain only dynamic source/project arguments, packets, and exact reviewer deficiencies. The installed specialist definition owns its semantic contract.
15. One repair attempt is allowed after a failed reviewed design and one repair attempt after a failed final durable-state review. Do not enter reviewer/worker ping-pong.
16. Do not implement, edit product code, or perform the work represented by the created Beads.

## Required specialists

JLS installs two native specialists:

- `beads-task-designer`
- `beads-task-reviewer`

Use the exact registered name. Do not replace a required specialist with a generic child or parent-thread semantic judgment. If a required specialist cannot run, fail the stage closed.

## Live Beads guidance

Do not depend on memorized `bd` flags when the installed version can answer directly.
At the start of a substantive transaction:

```text
bd --version
bd prime
```

Treat `bd prime` as the live AI-oriented source of truth for the installed runtime. Before an unfamiliar or version-sensitive operation, use:

```text
bd <command> --help
```

The common command families this skill may need are:

```text
bd list
bd show <id>
bd create
bd update <id>
bd dep
bd ready
```

Use the current help output for exact flags, fields, dependency syntax, hierarchy support, and JSON output. Prefer structured `--json` reads where supported.
Do not run `bd init` automatically. If no Beads database exists, report that prerequisite instead of silently initializing tracker state.

## Start

1. Resolve the project root containing the target Beads database.
2. Resolve the exact authoritative source scope from the user's request. This may be inline text, files, directories, structured exports such as Map JSON, or another explicitly designated corpus.
3. Record the user's requested end result as `GOAL`. Do not expand it beyond the supplied evidence.
4. Run `bd --version` and `bd prime`. Confirm the target Beads database can be read.
5. Inspect existing Beads only enough to identify overlaps, hierarchy, and dependencies relevant to the requested compilation.
6. If the source scope is ambiguous in a way that materially changes what is authoritative, stop and ask. Do not broaden scope by convenience.

A Map export is ordinary structured input here. Do not invoke Map or reconstruct Map workflow semantics merely because the input came from Map.

## Design transaction

Spawn `beads-task-designer` with:

```text
MODE: DESIGN
PROJECT_ROOT: <path>
GOAL: <exact requested end result>
SOURCE_SCOPE: <exact paths and/or exact inline source packet>
EXISTING_BEADS_SCOPE: <relevant existing ids or AUTO>
REVIEW_DEFICIENCIES: NONE
```

The Designer must independently inventory the authoritative source, understand the whole objective before leaf decomposition, inspect relevant existing Beads, and return a `DESIGN_PACKET` containing:

- a complete source coverage ledger
- the proposed issue hierarchy
- proposed issue contents and source anchors
- proposed dependencies
- mappings to reused existing issues where applicable
- explicit blocked/deferred/out-of-scope/non-actionable dispositions

Each coverage-ledger item must have exactly one primary disposition and may reference one or more proposed issues. The packet must be detailed enough for deterministic application without reinterpreting the source.

For very large packets, the parent may allocate temporary storage outside the project repository and pass its path between specialists. Temporary packets are transaction state only and must be deleted after completion or abort. They are never authoritative over the original source.

## Design review

Close Designer, then spawn `beads-task-reviewer` with:

```text
MODE: DESIGN
PROJECT_ROOT: <path>
GOAL: <exact requested end result>
SOURCE_SCOPE: <same authoritative source>
DESIGN_PACKET: <designer output or temporary packet path>
APPLIED_MAPPING: NONE
```

The Reviewer must re-read the authoritative source independently. It must not trust the Designer's coverage ledger as proof of coverage.

PASS requires all of the following:

- every material source requirement, decision, constraint, dependency, deferred item, blocker, and relevant fact is accounted for
- no source meaning is strengthened, weakened, generalized, or invented
- the proposed hierarchy reflects the objective before details
- executable leaves are narrowly scoped and independently assignable where possible
- no leaf hides multiple separable responsibilities
- cross-cutting constraints reach every issue they constrain
- source-stated and necessary inferred dependencies are represented coherently
- existing equivalent Beads are reused or deliberately superseded without accidental duplication
- each executable issue carries enough context and provenance to be picked up cold
- acceptance conditions are objective where the source supports them, without inventing arbitrary tests
- explicit non-actionable/deferred/out-of-scope dispositions are justified and are not being used to hide implementation work

On FAIL, allow one fresh Designer `MODE: DESIGN` attempt using the exact reviewer deficiencies, followed by one fresh design review. If it fails again, stop without applying the design.

## Apply transaction

After design review PASS, spawn a fresh `beads-task-designer` with:

```text
MODE: APPLY
PROJECT_ROOT: <path>
GOAL: <same goal>
SOURCE_SCOPE: <same source scope>
DESIGN_PACKET: <reviewed packet>
REVIEW_DEFICIENCIES: NONE
```

The Designer must use current `bd prime` and command help, then create or update the approved Beads graph without re-planning it.
It may reuse clearly equivalent existing issues identified in the reviewed packet. It must preserve unrelated existing tracker state.
It must read back every created/updated issue and relevant dependency before returning.

The result must include an `APPLIED_MAPPING` from every proposed issue key to its durable Beads ID, plus any blocked operation.

If application cannot faithfully realize the reviewed design because the tracker changed or current Beads semantics differ from the reviewed assumptions, return BLOCKED rather than improvising a new design.

## Final durable-state review

Close Designer, then spawn `beads-task-reviewer` with:

```text
MODE: FINAL
PROJECT_ROOT: <path>
GOAL: <same goal>
SOURCE_SCOPE: <same source scope>
DESIGN_PACKET: <reviewed packet>
APPLIED_MAPPING: <durable ids from Designer>
```

The Reviewer must inspect the actual Beads database using current `bd prime`/help and independently compare durable tracker state against both the authoritative source and reviewed design.

PASS only if:

- every planned issue exists or maps to a valid reused issue
- descriptions, design constraints, acceptance conditions, provenance, hierarchy, and dependencies materially match the reviewed design
- the original source still has complete coverage in the durable graph
- there are no accidental omissions, duplicates, contradictory issues, oversized leaves, orphaned executable work, or invented requirements
- a fresh implementation agent can pick any ready leaf and understand exactly what responsibility it owns without reconstructing the planning conversation

On FAIL, allow one `beads-task-designer` `MODE: REPAIR` transaction using only the exact final-review deficiencies and the original reviewed packet. The Designer may modify only issues in the current transaction or explicitly mapped pre-existing equivalents. It may delete only an erroneous issue created by this transaction when deletion is clearly required by the reviewer; never delete unrelated or pre-existing tracker state.
Then run one fresh FINAL review. If it fails again, stop and report that safe compilation did not complete.

## Completion boundary

Beads Design is complete only when the final Reviewer returns PASS.

Completion means the authoritative source has a reviewed, durable Beads representation in which every material source element has an explicit disposition and every executable responsibility is represented by sufficiently small, self-contained work.

Report the created/updated issue count, reused issue count, blocked/deferred/non-actionable counts, and final review result. Do not implement the issues.
