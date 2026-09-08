# JLS Beads

`beads-design` is a lossless source-to-Beads compiler for JLS.

It takes an authoritative goal, specification, plan, structured export, or other user-designated source and derives a complete Beads work graph without silently dropping requirements. It is intentionally separate from the official `beads` skill: the official skill teaches ordinary Beads operation; `beads-design` owns exhaustive decomposition, provenance, coverage, and final fidelity review.

## Architecture

- `beads-task-designer`: inventories the source, decomposes top-down, and creates or repairs the Beads graph.
- `beads-task-reviewer`: independently audits source coverage, granularity, dependencies, fidelity, and the final durable Beads state.

The skill contains no runtime executable. JLS installs `SKILL.md`, the harness-native specialists, and the optional instruction fragment.
