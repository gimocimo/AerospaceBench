# Related work notes

*Papers read in full or in part after the Phase 0 report. Each entry covers what it is, what it found, and what it means for AerospaceBench. PDFs are not stored in the repository (copyright); cite by link.*

## Connolly (2025): "Development of an Aerospace Engineering Evaluation Set for LLM Benchmarking"

- **Venue:** AIAA SciTech 2025, paper 2025-0702 (Southwest Research Institute). https://arc.aiaa.org/doi/10.2514/6.2025-0702
- **What it is:**
  - 42 true/false and short-answer items transcribed from MIT OpenCourseWare homework in air and marine propulsion (licensed CC BY-NC-SA).
  - Tested GPT-4 Turbo, GPT-4o, Claude 3 Sonnet and Claude 3.5 Sonnet, plus two simple RAG set-ups over internal SwRI training material.
  - Not released, because of licensing uncertainty.
- **Findings:**
  - Best exact accuracy was ~45%, rising to 67% within 30% of the answer (Claude 3.5 Sonnet). True/false items were much easier than short answers.
  - RAG sometimes hurt performance.
  - o1 and GPT-4o with Python, spot-checked on the hardest multi-step items, still failed, through early logical errors in the solution strategy.
  - The author first tried generating questions from internal training material, by hand and with an LLM. Models answered them easily even without the source, so that approach would have saturated immediately.
- **Overlap with AerospaceBench:** none. It is a small, unreleased question-answering set evaluated on 2024 models.
- **Useful for us:**
  - Evidence that LLM-generated questions saturate.
  - A cautionary example of licence-encumbered source material (we author original content).
  - A reminder that "within X%" tolerances must be engineering-justified.

## Mangortey et al. (2025): "Aerospace Language Understanding Evaluation (ALUE)"

- **Venue:** AIAA AVIATION 2025, paper 2025-3247 (MITRE). https://arc.aiaa.org/doi/10.2514/6.2025-3247; code at https://github.com/mitre/alue (Apache-2.0)
- **What it is:**
  - A framework for aviation natural-language tasks: classification of safety reports (ICAO CICTT categories, runway incursions), extractive QA on ATC transcripts, named-entity extraction from NTSB narratives, sentiment on traffic-management logs, NOTAM classification, RAG over NASA directives, and aviation exam questions.
  - Metrics include LLM-judge binary correctness and a claim-decomposition "composite correctness".
  - Results reported mainly for open models (Llama, Mistral), with few-shot prompting effects.
- **Overlap:** none. It is aviation text understanding, not engineering work.
- **Useful for us:**
  - The claim-decomposition idea is related to our claim-integrity check, but we verify claims by re-execution against independent verifiers, not by an LLM judge.
  - Useful as an external comparison point if a language-understanding baseline is ever wanted.

## Wijk et al. (2024/2025): "RE-Bench: Evaluating frontier AI R&D capabilities of language model agents against human experts"

- **Venue:** METR; arXiv 2411.15114 (v2, May 2025). Environments at https://github.com/METR/ai-rd-tasks
- **What it is:**
  - 7 hand-crafted ML research-engineering environments. Each has a starting solution (normalised score 0), a hidden reference solution (score 1) and a scoring function the agent can call at any time; every call is logged with a timestamp.
  - 71 eight-hour attempts by 61 human experts, from three sources whose average scores differed widely (0.96 for professional-network experts vs about 0.46 for hiring applicants).
- **Findings:**
  - With a 2-hour total budget, agents scored about 4× higher than humans. Humans narrowly beat agents at 8 hours and reached about 2× the best agent at 32 hours (best-of-k across attempts).
  - Agents ran the scorer 25–37 times per hour vs 3.4 for humans. That helped on noisy, fast-feedback tasks and led to overfitting where the test score was visible: one result fell from 0.88 to 0.69 on a re-run.
  - Failure modes: stubborn incorrect assumptions; not noticing contradictory information; poor recovery; misreading instructions in ways that exploit loopholes; low solution diversity.
  - The human–AI gap grew with engineering complexity (lines of code edited by the reference solution, R² ≈ 0.60).
  - Agent runs cost about US$123 per 8 hours vs about US$1,855 paid per expert attempt.
  - Creation was iteration-heavy: more than 12 specifications and more than 5 implementations were discarded. Blocking technical issues affected more than 5% of runs.
  - The authors note their environments are cleaner and better specified than real research, and plan to hide test scores in future.
- **Useful for us:** see `benchmark_design.md` §8. We borrow:
  - the environment structure and normalisation;
  - the timestamped score log, giving anytime curves;
  - best-of-k and time-allocation analysis;
  - the creation discipline;
  - the failure-mode taxonomy.

  We change: acceptance first rather than optimisation only; withheld conditions; claim integrity; feasibility classes; staged changes; costed investigations.
