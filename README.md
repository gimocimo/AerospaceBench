# AerospaceBench

A benchmark for measuring how well frontier LLM agents perform demanding aerospace engineering work: design, analysis, test, certification, manufacturing, operations and maintenance, across aircraft, rotorcraft, drones, launch vehicles and spacecraft.

**Status: design phase.** There are no tasks, harness or results yet. The repository currently holds the landscape research and the evolving design documents.

## Working direction

- **End-to-end engineering projects, not question answering.** Each task is a work package: requirements, project files, data and open-source engineering tools. The agent must deliver an executable result plus verification evidence.
- **Success is established independently.** Submissions are re-run and checked by independent verifiers, including on operating conditions the agent never sees. Hard requirements are pass/fail. Accepted solutions are then scored on quality, robustness and cost.
- **Difficulty from authentic engineering:**
  - coupled physics;
  - choosing and validating models;
  - investigating under a budget;
  - propagating requirement changes;
  - recognising infeasible or underdetermined problems.
- **Open-source only** for the pilot, so anyone can rerun it.

See [`docs/benchmark_design.md`](docs/benchmark_design.md) for the current design framework.

## Repository layout

| Path | Contents |
|---|---|
| `docs/` | Design documents, scope options, open questions |
| `reports/` | Landscape report on existing benchmarks, tools and methodology |
| `research_notes/` | Raw research notes behind the report |
| `CLAUDE.md` | Project brief, decision log and status (also used as working memory by the Claude Code assistant) |

## Licence

Not yet chosen.
