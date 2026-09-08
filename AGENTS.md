<!-- Managed by JLS. Do not edit inside this block; reinstall/update replaces it. -->
## Beads Design

Use `$beads-design` when the user asks to convert an authoritative goal, specification, plan, structured export, or other designated source into a complete actionable Beads work graph without silently losing requirements.

Do not use Beads Design for ordinary issue operations such as checking ready work, claiming an issue, adding notes, or closing completed work. Use the official `beads` skill and current `bd` CLI guidance for ordinary tracker operation.

Beads Design never implements the generated work. It owns exhaustive source coverage, top-down decomposition, provenance, dependency preservation, fine-grained issue design, and independent final review.

The official Beads CLI remains authoritative for command mechanics. Start substantive Beads Design work with:

```text
bd --version
bd prime
```

Use `bd <command> --help` whenever exact current flags or semantics are needed.

JLS installs the required `beads-task-designer` and `beads-task-reviewer` specialists into the selected harness. Invoke those exact named specialists at the workflow stages required by the installed skill. Do not substitute a generic child or parent-thread judgment if a required specialist cannot run.
