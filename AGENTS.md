<!-- Managed by JLS. Do not edit inside this block; reinstall/update replaces it. -->
## Tasks

Use `$tasks` when the user asks to convert an authoritative goal, specification, plan, structured export, or other designated source into a complete actionable task graph without silently losing requirements.

Tasks currently writes to Beads. Do not use Tasks for ordinary issue operations such as checking ready work, claiming an issue, adding notes, or closing completed work. Use the official `beads` skill and current `bd` CLI guidance for ordinary tracker operation.

Tasks never implements the generated work. It owns exhaustive source coverage, top-down decomposition, provenance, dependency preservation, fine-grained task design, and independent final review.

The Beads CLI remains authoritative for current backend command mechanics. Start substantive Tasks work with:

```text
bd --version
bd prime
```

Use `bd <command> --help` whenever exact current flags or semantics are needed.

JLS installs the required `tasks-designer` and `tasks-reviewer` specialists into the selected harness. Invoke those exact named specialists at the workflow stages required by the installed skill. Do not substitute a generic child or parent-thread judgment if a required specialist cannot run.
