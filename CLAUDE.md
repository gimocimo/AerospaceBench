# AerospaceBench — project memory

Read this first in every session. It holds the brief, the working rules, the decision log and an index of outputs. Detailed material lives in `docs/`, `reports/` and `research_notes/`.

## Brief (from Guglielmo, 2026-10-06; amended by later decisions as noted)

- **Goal:** a benchmark measuring how well frontier LLMs and LLM agents perform demanding aerospace engineering work: design, analysis, test, certification, manufacturing, operations and maintenance, across aircraft, rotorcraft, drones, launch vehicles, spacecraft and their propulsion, systems and software.
- **Secondary motivation (not the main goal):** aerospace draws on most engineering disciplines, so the benchmark may serve as a proxy for general engineering ability.
- **Coverage ambition:** broad, across aerospace sub-disciplines and sectors. The pilot covers selected areas in depth; other areas follow later with the same methodology (D10).
- **Source of difficulty:** authentic engineering reasoning (choosing models, diagnosing problems, handling uncertainty, balancing constraints, integrating systems, verifying results), plus using engineering software where the work needs it.
- **Avoid artificial difficulty:** no puzzle constraints, deliberately vague specifications, unrealistic formats, or anything exploiting incidental LLM weaknesses rather than engineering competence.
- **First milestone:** a small, rigorously validated pilot with grading that experts trust. Tasks are authored by Guglielmo, grounded in real engineering practice and public sources, and validated through short, targeted consultations with practising engineers and academics (D9; this replaces the original plan of building tasks from in-depth interviews).
- **No circularity:** tasks must not be proposed by the LLM that is later tested on them. Claude may sketch example tasks or task families only to show the shape the benchmark could take; concrete tasks, inputs, reference solutions and rubrics come from Guglielmo, public sources and experts.
- **Open-source only for the pilot:** anyone must be able to rerun it. No task may require a proprietary tool (MATLAB/Simulink, MSC Nastran, ANSYS, Abaqus, CATIA, STK, …). A licensed track may come later.
- **Purpose (D6):** an independent personal project. Core: scientific quality of the benchmark and a solid GitHub repository. Additions: a peer-reviewed conference paper on the validated pilot, release of the full benchmark, a public leaderboard.
- **Owner:** Guglielmo Cimolai — Italian, based in London. Aerospace Engineering BSc (Politecnico di Milano), Aerospace Structures MSc (Cranfield), AI MSc (Imperial College London); also an ISAE-Supaero alumnus. Sole author, 10–15 h/week, no target dates (D16).

## Working rules

- No implementation (harness, task code, model runs) until the task design and validation approach are agreed with Guglielmo.
- Keep the brief, decisions and outputs in this file and `docs/` as we go; update the decision log whenever Guglielmo decides something.
- Example tasks and task families written by Claude are illustrative only and labelled as such. They never enter the benchmark without Guglielmo authoring the concrete task and expert validation.
- Sequence (D18): agree the pilot scope and task list first, then research contacts, then a simple consultation plan. Do not draft the consultation or interview kit before the scope and task list are agreed.
- Do not assume answers to Q14–Q16 (environment and task modes, models and harness, release, licence and metric). Ask Guglielmo again when they become relevant (D17).

## Decision log

| ID | Date | Decision | Source |
|----|------|----------|--------|
| D1 | 2026-10-06 | Pilot uses open-source software only; licensed track possibly later | Guglielmo |
| D2 | 2026-10-06 | Defer all implementation until task design and validation approach are agreed | Guglielmo |
| D3 | 2026-10-06 | ~~Tasks grounded in interviews with practising engineers~~ (superseded by D9); expert-trusted grading still applies | Guglielmo |
| D4 | 2026-10-06 | First deliverable: deep research of existing benchmarks, then a proposal and questions | Guglielmo |
| D5 | 2026-10-06 | ~~Start expert interviews within days~~ (superseded by D18) | Guglielmo |
| D6 | 2026-10-06 | Purpose: scientific quality and a solid GitHub repo are the core; peer-reviewed conference paper on the validated pilot, full benchmark release and public leaderboard are additions. Personal project | Guglielmo (Q1) |
| D7 | 2026-10-06 | Run independently of any university. Experts are contributors, not research subjects: written consent plus a short contributor agreement. No co-authors expected | Guglielmo (Q2) |
| D8 | 2026-10-06 | Guglielmo is in London (UK); contributors are UK or European. Exclude military systems. Contributors speak in a personal capacity about non-confidential, non-controlled material | Guglielmo (Q3) |
| D9 | 2026-10-06 | Guglielmo authors the tasks. Experts give short, directed input on specific tasks (e.g. choosing between alternatives, targeted domain expertise, validation). No large-scale elicitation. Claude researches suitable contacts once an initial task list exists | Guglielmo (Q4) |
| D10 | 2026-10-06 | Pilot covers selected areas in depth, then expands with the same methodology. Guglielmo suggests airframe structures, aerodynamics and aircraft performance/design, and propulsion. Claude proposes alternatives for the pilot and the full benchmark | Guglielmo (Q5) |
| D11 | 2026-10-06 | Consultations may be recorded with consent and transcribed locally; Claude may process transcripts to draft write-ups; details later | Guglielmo (Q6) |
| D12 | 2026-10-06 | English only | Guglielmo (Q7) |
| D13 | 2026-10-06 | Contributors unpaid, with acknowledgement; compensation for industry professionals may be discussed later | Guglielmo (Q8) |
| D14 | 2026-10-06 | Individuals only for the pilot; institutional partnerships reserved for future versions. A partnership with Imperial's Department of Aeronautics is under consideration | Guglielmo (Q9) |
| D15 | 2026-10-06 | Minimise cost: ideally only LLM usage, preferably through existing Claude and ChatGPT subscription plans rather than API or third parties. Frontier models only. Details belong to Q15, still open | Guglielmo (Q10) |
| D16 | 2026-10-06 | No target dates; 10–15 h/week; Guglielmo is sole author | Guglielmo (Q11) |
| D17 | 2026-10-06 | Q14–Q16 deferred; Claude must ask again later and not assume | Guglielmo |
| D18 | 2026-10-06 | Work through pilot scope and candidate tasks first; then select experts; then a very simple consultation plan | Guglielmo |

## Status

- **Phase 0 — landscape research:** done (2026-10-06).
- **Phase 1 — scope and task selection:** options drafted in `docs/pilot_scope_options.md` (v0.2). Waiting for Guglielmo to choose the pilot option, the full-benchmark reach, the design options and the task families.
- **Next:** contacts research per selected task family; then a simple consultation plan.
- **Open:** Q12 (later roles) depends on contacts; Q13 (Guglielmo will download the two AIAA papers); Q14–Q16 deferred.

## Key findings from Phase 0 (details in the report)

- No existing benchmark measures aerospace *engineering work*. About two dozen aerospace LLM evaluations exist (mostly 2025–2026), dominated by MCQ recall on aviation ops, maintenance and regulations (frontier models 60–83%).
- The closest analogues are small, single-sector, verifier-graded agent benchmarks: AstroAgentBench (satellite planning, physics verifier), the GTOC-12 agent study (no valid submission from any model), AeroCopilotBench (safety-gated cockpit procedures, pass^3), RocketBench (RocketPy; unreleased), APBench (astrodynamics, numeric tolerance).
- Essentially unmeasured: rotorcraft, aerostructures, certification and safety assessment, test and AIT, orbital launch-vehicle design, spacecraft subsystem budgets, UAV sizing, flight-software generation and verification.
- Adjacent lesson: textbook calculation is near saturation, while executable design is about one-third; "it ran" overstates CFD agents by about 50 points; failures are silent (boundary conditions, units, factors, no verification).
- Methodology lesson: answer-key verification and grader validity, not authors' credentials, set quality (HLE, FrontierMath v2, SWE-bench Verified, SciCode-Verified audits). Harness choice moves agent scores by tens of points.
- An open-source-only toolchain covers most disciplines at conceptual and preliminary fidelity. Hard holes: rotorcraft comprehensive analysis, damage tolerance, Nastran-class flutter, paywalled DO-178C and ARP standards. Licence traps: CEASIOMpy (proprietary since Feb 2026), OpenSees (non-commercial), RCAIDE (AGPL); FUN3D, Cart3D and OVERFLOW are US-only.
- Export control: ITAR has no internet-posting route to the public domain; authoring new launch-vehicle or military design content is the main risk.
- Caveat: AIAA ARC, VFS, ERF, ICAS, RAeS, IAC proceedings and Chinese-language venues were not systematically searchable, so "unmeasured" means absent from the indexed open literature.

## Proposed direction (pending Guglielmo's choice — see `docs/pilot_scope_options.md`)

- Recommended pilot: three pillars (structures; aerodynamics, performance and design; propulsion), ~24 task families, ~25% outside fixed-wing (spacecraft, launcher, UAV), 3 cross-pillar integration tasks, built in stages starting with a structures pathfinder.
- Recommended full benchmark: a curated core across 10 domains × 5 sectors, plus contributed modules that follow the published methodology.
- Validation adapted to a sole author: keys from external data or two independent methods; a short expert review per task; grader sanity checks; AI-assisted key audit (flags only); expert spot-regrading; public errata on GitHub.
- Circularity safeguards: provenance tag on every task; Claude never computes reference solutions; results reported by provenance.

## File index

- `CLAUDE.md` — this file.
- `reports/Aerospace engineering LLM benchmarks.md` — Phase 0 landscape report on existing benchmarks, tools and methodology.
- `research_notes/Aerospace engineering LLM benchmarks/` — six raw research-note files behind the report.
- `docs/pilot_scope_options.md` — v0.2: pilot and full-benchmark scope alternatives, design options, task-family menu, recommended shortlist, effort.
- `docs/pilot_proposal.md` — v0.1 proposal; partly superseded (interview-based elicitation, blind re-solves and effort figures) by D9–D16 and the scope options document.
- `docs/open_questions.md` — questions for Guglielmo with his answers.
