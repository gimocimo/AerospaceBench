# Open questions for Guglielmo

*Raised 2026-10-06. Q1–Q13 answered the same day (summaries below; resulting decisions D6–D18 are in `CLAUDE.md`). Q14–Q16 are deferred: Claude must ask again when they become relevant and must not assume answers.*

## A. Needed before the first contact with experts — answered

| ID | Question | Answer (summary) | Decision |
|----|----------|------------------|----------|
| Q1 | Purpose and venue | Independent personal project. Core: scientific quality and a solid GitHub repo. Additions: a peer-reviewed conference paper on the validated pilot, full benchmark release, public leaderboard | D6 |
| Q2 | Ethics route | Independent of any university. Experts are contributors, not research subjects; written consent and a short contributor agreement; no co-authors expected | D7 |
| Q3 | Jurisdiction and exclusions | Italian, based in London. Contributors UK or European. Exclude military systems. Personal capacity; non-confidential, non-controlled material | D8 |
| Q4 | First interviewees | Guglielmo authors the tasks. Short, directed contacts (PoliMi, Cranfield, ISAE-Supaero and Imperial alumni networks, LinkedIn) for feedback on specific tasks, choosing between alternatives, or targeted expertise. Claude to research suitable contacts once an initial task list exists | D9 |
| Q5 | Pilot scope | Depth over breadth. Suggested areas: airframe structures; aerodynamics and aircraft performance/design; propulsion. Claude to propose alternatives for the pilot and the full benchmark → see `pilot_scope_options.md` | D10 |
| Q6 | Recording and AI use | Record with consent; transcribe locally; Claude may process transcripts to draft write-ups; details later | D11 |
| Q7 | Language | English only | D12 |
| Q8 | Credit and pay | Unpaid, with acknowledgement; compensation for industry professionals possibly later | D13 |
| Q9 | Employer involvement | Individuals only for the pilot; partnerships in future versions. Asked whether to approach Imperial's Department of Aeronautics now | D14 |

## B. Before the design is final — answered or in progress

| ID | Question | Answer (summary) | Decision |
|----|----------|------------------|----------|
| Q10 | Budget | Minimise cost: ideally only LLM usage, preferably through existing Claude and ChatGPT subscriptions rather than API or third parties; frontier models only (Claude Opus/Fable; ChatGPT "Astra/Sol"). How models are run belongs to Q15 | D15 |
| Q11 | Time and roles | No target dates; 10–15 h/week; sole author | D16 |
| Q12 | Later roles | Depends on contacts; to revisit after the contacts research | open |
| Q13 | Paywalled literature | Both papers provided and read on 2026-10-07: no overlap (notes in `related_work_notes.md`) | D25 |

*Newer design questions (private task workspace, git workflow, pilot shape, episode length, feasibility mix, human reference, cross-vendor red team) are in `benchmark_design.md` §11.*

## C. Needed before implementation — deferred (ask again later; do not assume)

| ID | Question | Status |
|----|----------|--------|
| Q14 | Environment and task modes (one sandbox with Python and open tools for every task? include Check tasks?) | Deferred (D17). Largely shaped by D23: project environments with sandboxed open tools |
| Q15 | Models and harness (which models; API vs subscriptions; common harness) | Deferred (D17); preference stated in D15. Now relevant: D23's budget surfaces, inference accounting and checkpointed submissions depend on it |
| Q16 | Release, licence and headline metric | Deferred (D17) |
