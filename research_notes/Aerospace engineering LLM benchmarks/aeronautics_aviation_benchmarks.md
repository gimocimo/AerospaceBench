# Aeronautics & Aviation LLM Benchmarks and Evaluations (catalogue + gap analysis, as of 2026-10-06)

Scope: fixed-wing, rotorcraft, aero-engines/gas turbines, avionics, and their design / analysis / test / certification / manufacturing / operations & maintenance. Spacecraft, launch vehicles, drones/UAVs and flight software are covered by another researcher and appear here only as cross-references. Generic engineering benchmarks are cross-referenced briefly.

Method note: verification used arXiv abstract and HTML pages, the arXiv export API (keyword listings for "aviation", "aerospace", "air traffic", "aircraft", airworthiness, DO-178C, ARP4754A/4761, avionics, aero-engine, turbofan, helicopter and rotorcraft, each combined with "language model"), GitHub/Hugging Face/Zenodo pages, and full-text PDFs where reachable. AIAA ARC, MDPI and some Georgia Tech repository pages returned HTTP 403, so details for those items come from abstracts, search snippets or secondary citations and are marked as such. The session's web-search quota ran out near the end. Targeted searches for NOTAM, NDT and assembly process planning could not be run, so the arXiv API listings were used instead (see Gaps).

Model names are given exactly as the cited papers report them, including 2026 models (e.g., "GPT-5.5", "Claude Opus 4.8", "GPT-5.6-sol", "gemini-3.5-flash").

---

## Q1. Which aerospace/aviation-specific LLM benchmarks or QA datasets exist? (verified catalogue)

### Takeaway
About a dozen aviation or aeronautics LLM benchmarks or datasets are verifiable. Almost all date from 2025–2026 and cluster in operations, ATC, maintenance and safety text: Pre-Flight, ALUE, CAMB, PilotBench/FLY-EVAL++, AeroCopilotBench, the ATC Risk-Score benchmark, KEO/OMIn, AeroQA/AviationQA, and the aerospace manufacturing MCQ set. Only one small, human-authored aircraft-*engineering* QA set exists (AeroEngQA, 80 items). No benchmark tests quantitative aeronautical analysis, design, certification or agentic tool use with engineering software. The best recognition-style aviation knowledge scores are 60–83%, still well below the expert reference. Agentic and safety-gated scores are lower still: 72.6% safety-gated success, and an ATC Risk Score of 0.69 that collapses under ASR noise.

### Cited Findings

**A. Pre-Flight (aviation operational knowledge MCQ), 2026**
- Authors: Alex Brooker, Tim Hughes. Submitted 2 Jul 2026; arXiv:2607.01829 — [arXiv](https://arxiv.org/abs/2607.01829)
- Format and size: 300 MCQs. The HF schema has 5 options per item (4 answers + "no suitable option"). Categories: international airport ground operations 152 (50.7%), ICAO rules 85 (28.3%), FAA rules 51 (17.0%), aviation trivia 8, complex ground scenarios 4 — [arXiv HTML](https://arxiv.org/html/2607.01829); [HF dataset](https://huggingface.co/datasets/AirsideLabs/pre-flight-06)
- Sourcing and validation: written by the lead author with "a small group of aviation practitioners" (ATM, ground ops, commercial flying). Validation was "expert based and source grounded, but partial": not every item had a second review and no inter-annotator agreement was computed — [arXiv HTML](https://arxiv.org/html/2607.01829)
- Grading: MCQ accuracy using the UK AISI Inspect framework; included in `inspect_evals` — [arXiv HTML](https://arxiv.org/html/2607.01829)
- Scores (44 models, snapshot 29 Jun 2026): GPT-5.5 82.7%, GPT-5 80.3%, Claude Opus 4.8 79.0%, Gemini 2.5 Pro 79.0%, Qwen3.5 122B-A10B int4 77.3%; lowest ALLaM 2 7B 25.3%. Early-2025 models scored about 75%. The informal expert reference is about 95%, from a small, self-selected sample of conference attendees — [arXiv HTML](https://arxiv.org/html/2607.01829); [arXiv abs](https://arxiv.org/abs/2607.01829)
- Contamination and saturation: the public tier may be contaminated; the authors mitigate this with an unreleased harder tier. Answer-key problems were found in the FAA category and need revalidation. Not saturated (≈83% vs ≈95% expert) — [arXiv HTML](https://arxiv.org/html/2607.01829). The HF dataset card does not mention the held-out tier — [HF](https://huggingface.co/datasets/AirsideLabs/pre-flight-06)
- Licence and availability: HF dataset `AirsideLabs/pre-flight-06` is MIT, with 5,701 downloads in the last month per the card (the paper cites 11,416+ by June 2026). No tools needed — [HF](https://huggingface.co/datasets/AirsideLabs/pre-flight-06); [arXiv HTML](https://arxiv.org/html/2607.01829)
- Limitations (authors' own): MCQ tests recognition, not generation or operational judgment; it does not cover the full breadth of aviation operations; category sizes are uneven. The authors advise restricting deployment to "non safety critical aviation operations" — [arXiv](https://arxiv.org/abs/2607.01829)

**B. ALUE — Aerospace (Aviation) Language Understanding Evaluation (MITRE/FAA), 2025**
- Authors: Eugene Mangortey, Satyen Singh, Shuo Chen, Kunal Sarkhel. Another paper's reference list adds Bulent Ayhan and Shreya Kurdukar. AIAA AVIATION Forum 2025, paper 2025-3247 (a correction notice also exists) — [GitHub](https://github.com/mitre/alue); [AIAA ARC](https://arc.aiaa.org/doi/10.2514/6.2025-3247); [AeroCopilotBench refs](https://arxiv.org/abs/2608.16349)
- Naming is inconsistent: the GitHub citation says "Aviation Language Understanding Evaluation", while AIAA and MITRE say "Aerospace Language Understanding Evaluation" — [GitHub](https://github.com/mitre/alue); [AIAA ARC](https://arc.aiaa.org/doi/10.2514/6.2025-3247)
- What it is: an open framework more than a fixed test set. Tasks: MCQA (accuracy), RAG (recall@k, context relevancy, composite correctness), summarization (claim-decomposition precision/recall/F1 using LLM judges), extractive QA (EM/F1), classification. Backends: OpenAI, vLLM, TGI, Ollama, Transformers — [GitHub](https://github.com/mitre/alue); [docs](https://mitre.github.io/alue/)
- Bundled data visible in the repo: `aviation_knowledge_exam/3_1_aviation_test.json`, `aviation_exam.json` (MCQA), `ntsb_tail_extraction_dummy.json`, plus dummy RAG and summarization sets. Sizes and provenance are not documented on the pages fetched. No leaderboard is published — [GitHub](https://github.com/mitre/alue); [docs](https://mitre.github.io/alue/)
- Positioning: announced by MITRE and FAA in Sept 2025 as a tool "for guiding the assurance of LLMs" in aerospace. It supports custom datasets, user prompts and domain LLMs — [BusinessWire/MITRE release](https://www.businesswire.com/news/home/20250917980616/en/MITRE-and-FAA-Introduce-Novel-Aerospace-Large-Language-Model-Evaluation-Benchmark). Pre-Flight describes it as targeting the NAS/ATM "systemic layer", with a roadmap toward multimodal, retrieval-grounded tasks such as chart extraction and consulting manuals — [Pre-Flight HTML](https://arxiv.org/html/2607.01829)
- Licence: Apache-2.0 — [GitHub](https://github.com/mitre/alue)

**C. CAMB — Civil Aviation Maintenance Benchmark, 2025**
- Authors: Feng Zhang, Chengjie Pang, Yuehan Zhang, Chenyu Luo et al. (Qihoo 360 and affiliates). arXiv:2508.20420, 28 Aug 2025 — [arXiv HTML](https://arxiv.org/html/2508.20420v1)
- 7 tasks: bilingual (zh–en) terminology alignment; aircraft fault system localization; text chapter localization; maintenance MCQ; fault description ↔ FIM manual matching; maintenance QA; fault-tree-structured QA — [arXiv HTML](https://arxiv.org/html/2508.20420v1)
- Sizes: 7,969 MCQs across 12 aircraft types; 202 QA pairs; 50 real fault-tree cases (B737/A320); 1,336 aligned zh–en; 5,984 sentence pairs; 100 categorization; 613 clustering — [arXiv HTML](https://arxiv.org/html/2508.20420v1)
- Sources: maintenance textbooks (ATA, B737NG, FAA), FIM/TSM manuals, real B737 and A320 fault cases, "professional licensing examination questions", and an Aviation Maintenance English corpus — [arXiv HTML](https://arxiv.org/html/2508.20420v1). The repo describes the MCQs as 737NG maintenance certification exam questions — [GitHub](https://github.com/CamBenchmark/cambenchmark)
- Grading: F1, accuracy, NDCG@10. Open QA and fault-tree tasks use 0/1/2 scoring by human and LLM judges. GPT-4o plus humans were used to check annotation consistency — [arXiv HTML](https://arxiv.org/html/2508.20420v1)
- MCQ scores (13 LLMs): Qwen3-235B-A22B-Instruct 68.87%, Claude-opus-4 63.81%, Qwen3-235B-A22B (thinking) 63.44%, DeepSeek-R1-0528 59.27%. The overall MCQ range is 60–70%. The best of 8 embedding models was Qwen3-Embedding-8B at 66.27 — [arXiv HTML](https://arxiv.org/html/2508.20420v1)
- Licence: CC BY-NC-SA 4.0 (non-commercial). Primarily Chinese, with an English README — [GitHub](https://github.com/CamBenchmark/cambenchmark)
- Limitations: the fault-tree set has only 50 cases. Licensing-exam items are public, so contamination is plausible. Not saturated — [arXiv HTML](https://arxiv.org/html/2508.20420v1)

**D. Aerospace Manufacturing expertise MCQ (Chengdu Aircraft Industrial Group + Beihang), 2025**
- Authors: Beiming Liu, Zhizhuo Cui, Siteng Hu, Xiaohua Li, Haifeng Lin, Zhengxin Zhang. arXiv:2501.17183 (Jan 2025) — [arXiv](https://arxiv.org/abs/2501.17183); [HTML](https://arxiv.org/html/2501.17183v1)
- About 7,500 multi-answer MCQs with six options (A–F) in three areas: aerospace assembly (the largest), aerospace materials (coatings, plating, textiles) and structural steel. Each source snippet yields 1 easy, 2 medium and 2 hard items — [HTML](https://arxiv.org/html/2501.17183v1)
- Sourcing: questions were LLM-generated by Gemini Pro Vision from textbooks, standards, technical manuals and white papers. Source titles were withheld "to maintain evaluation fairness". About 300 questions were expert-reviewed — [HTML](https://arxiv.org/html/2501.17183v1)
- Grading: +1 per correct selection, −3 per incorrect selection — [HTML](https://arxiv.org/html/2501.17183v1)
- Scores: Gemini-2.0-flash-exp 50.75%, Claude-3-5-sonnet-20241022 49.37%, DeepSeek-Chat-V3 45.77%, GPT-4o 44.00%, GLM-4-Plus 43.79%. Aerospace materials averaged 35.40% across models — [HTML](https://arxiv.org/html/2501.17183v1)
- Release status was not stated in the pages fetched.

**E. AeroEngQA (aircraft engineering QA, University of Southampton), 2024/2025**
- Authors: Edmar A. Silva, Robert Marsh, Hau Kit Yong, Stuart E. Middleton, András Sóbester. AIAA SciTech 2025, paper 2025-0700 — [preprint PDF](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf); [DOI](https://doi.org/10.2514/6.2025-0700)
- 80 context–question–answer sets drawn from public NASA reports, NTSB reports and patents. Balanced across answerable/unanswerable, single/multi-hop, and short/long context. Written by 4 experienced aeronautical engineers with no LLM or automation involved — [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
- Models: gpt-3.5-turbo-0125, gpt-4-turbo (zero-shot and in-context), and Llama3-70B-Instruct (4-bit, Ollama) with RAG — [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
- Grading: manual expert coding of answer accuracy (correct/partial/incorrect) and simplicity (simple/verbose), with consensus reached in annotation meetings. Results were "preliminary": in-context prompting added about 5–10% accuracy, GPT-4 beat the smaller models, and RAG was comparable (better on multi-hop) — [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
- Contamination evidence: models often produced sensible answers to "unanswerable" questions by drawing on training data rather than the supplied context, and they struggled to abstain — [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
- Available on Zenodo — [doi:10.5281/zenodo.14215677](https://doi.org/10.5281/zenodo.14215677) (licence not verified)

**F. AeroQA (KITLM) and AviationQA (IIT Bombay / IBM), older, 2022–2023**
- AviationQA: 1M synthetic factoid QA pairs, template-generated from about 12k NTSB reports, used with an Aviation Knowledge Graph (ICON 2022; arXiv:2301.04013). Example: the damage level of a given NTSB accident number — [arXiv](https://arxiv.org/abs/2301.04013); [PwC dataset page](https://astro.paperswithcode.com/dataset/aviationqa)
- AeroQA: released with KITLM (arXiv:2308.03638, Aug 2023) together with an NTSB-derived Aviation Corpus — [arXiv](https://arxiv.org/abs/2308.03638). The Southampton authors describe AeroQA as ChatGPT-generated, about 27,000 QA pairs, single and multi-hop. Their manual review found the questions "not authentic" and answer quality variable — [AeroEngQA preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf). Other sources give the size as 34k questions. The figure is unresolved.

**G. PilotBench (general-aviation flight prediction with safety constraints), 2026**
- Authors: Yalun Wu (NUS), Haotian Liu (Xiamen U.), Zhoujun Li and Boyang Wang (Beihang). arXiv:2604.08987, IEEE IJCNN 2026 — [arXiv](https://arxiv.org/abs/2604.08987); [HTML](https://arxiv.org/html/2604.08987)
- Data: 708 real VFR trajectory segments (DA40 and C172N; 38 h 18 min; 34-channel telemetry at 10 Hz; 9 FAA-aligned flight phases) — [HTML](https://arxiv.org/html/2604.08987)
- Task and grading: trajectory and attitude prediction. Pilot-Score = 60% regression accuracy + 40% instruction adherence and safety compliance — [arXiv](https://arxiv.org/abs/2604.08987)
- Scores (41 LLMs): Qwen3-32B MAE 9.54 / Pilot-Score 90.23; Qwen2.5-72B-Instruct 11.91/88.58; DeepSeek-V3 11.94/88.31; GPT-4o-mini 12.11/88.09; o3-mini 10.94/87.76. The non-LLM FlightPatchNet has the best MAE (7.01) but cannot take instructions. Errors spike in Climb and Approach; Physics-CoT cuts safety violations by about 25% — [HTML](https://arxiv.org/html/2604.08987); [search summary of abstract](https://www.emergentmind.com/papers/2604.08987)
- Code: [GitHub](https://github.com/haotian-io/PilotBench-A-Benchmark-for-General-Aviation-Agents-with-Safety-Constraints) (licence not verified)
- Follow-up, FLY-EVAL++ (COLM 2026, arXiv:2609.04021): extends PilotBench with history-conditioned and multi-step rollout tasks and rubric-aggregated scores for protocol compliance, physical feasibility and safety. Across 66 LLMs, safety score spreads exceeded 28 points despite similar predictive accuracy. CC BY 4.0 — [arXiv](https://arxiv.org/abs/2609.04021)

**H. AeroCopilotBench (agentic emergency procedures in an executable cockpit, Beihang), 2026**
- Authors: Yuchen Yuan, Zhenghuang Wu, Yuangan Li, Liang Ma, Ke Li (School of Aeronautic Science and Engineering, Beihang). arXiv:2608.16349 v1 17 Aug 2026; v2 27 Sep 2026 retitled "Safety-Gated Evaluation of LLM Agents on Aircraft Emergency Procedures in an Executable Cockpit" — [arXiv](https://arxiv.org/abs/2608.16349)
- Environment: ACOE, a custom symbolic state-transition cockpit (preconditions/postconditions), not a commercial flight simulator. Agents use function calling with read-only information tools and write tools. No POH text or checklist-retrieval tool is given to the agent — [arXiv PDF](https://arxiv.org/pdf/2608.16349)
- Size: 12 scenario templates and 73 tasks on the Cessna 172S and Piper PA-44-180 (engine failure and restart, engine fire, electrical fire, voltage malfunction, forced landing, etc.). Success criteria come from the POHs — [arXiv PDF](https://arxiv.org/pdf/2608.16349)
- Grading: safety-gated success (all goals met with no hard-constraint violation), pass^3 repeatability over 3 runs per task (219 episodes per model; 2,628 trajectories in total), and ineffective-action rate — [arXiv PDF](https://arxiv.org/pdf/2608.16349)
- Scores (SR): GPT-5.6-sol 0.726 (pass^3 0.534), gemini-3.5-flash 0.594, qwen3.7-max 0.589, GLM-5.1 0.530, deepseek-v4-pro 0.461, GLM-5.2 0.457, deepseek-v4-flash 0.292, Kimi-K2.6 0.283, MiniMax-M2.5 0.224, Nex-N2-Pro 0.210, Qwen3.5-397B-A17B 0.187, Qwen3.5-122B-A10B 0.123. 93.2% of unsafe episodes also failed the task. Models skip steps in some runs that they perform in others — [arXiv PDF](https://arxiv.org/pdf/2608.16349)
- Auxiliary aviation knowledge test: Claude generated it from FAA manuals, the AIM, 14 CFR and POHs. Release of the environment, tasks, code and trajectories is promised, not yet confirmed — [arXiv PDF](https://arxiv.org/pdf/2608.16349)

**I. ATC language understanding with consequence-aware scoring (NTU ATMRI et al.), 2026**
- Chang, Guleria, Pham, …, Duong, Alam (ATMRI NTU Singapore; IIT Mandi; VinUniversity). arXiv:2605.11769 — [arXiv](https://arxiv.org/abs/2605.11769); [HTML](https://arxiv.org/html/2605.11769)
- Data: 1,000 manually verified utterances from Singapore Changi (WSSS) ground frequencies, 17–23 Mar 2025 (560 pilot, 440 controller). Tasks: speaker role, intent, risk-weighted entity slots, and an action-level Risk Score. Five licensed controllers (Beijing Capital Tower) rated entity criticality — [HTML](https://arxiv.org/html/2605.11769)
- Zero-shot Risk Score: Gemini-3-flash-preview 0.6922, Gemini-3-pro-preview 0.6884, GPT-5.1 0.6775, qwen3-max 0.5234, grok-4-1-fast-reasoning 0.5206, gpt-4o-mini 0.4483, deepseek-chat 0.4003. Scores collapse to 0.00–0.07 under ASR noise — [HTML](https://arxiv.org/html/2605.11769)
- Related: "Beyond Semantic Accuracy" (arXiv:2608.24621, Aug 2026) uses a diagnostic ATC benchmark grounded in standards, with input from 40 ATCOs in 3 countries. Across 8 models, standard semantic metrics overstated operational reliability. Errors included misread altitudes, dropped conditions and confused call signs — [news summary](https://aiunderstanding.org/news/preprint-finds-standard-language-model-scores-can-overstate-safety-in-air-traffic); [arXiv listing](http://export.arxiv.org/api/query?search_query=abs:%22air+traffic%22+AND+abs:%22language+model%22&sortBy=submittedDate&sortOrder=descending&max_results=60)

**J. KEO / OMIn QA benchmark (University of Notre Dame), 2025**
- Ai, Karr, Jiang, Chawla, Wang. arXiv:2510.05524 — [HTML](https://arxiv.org/html/2510.05524v1)
- OMIn: 2,748 FAA aviation incident records of 1–3 sentences each. QA set: 133 questions (83 global sensemaking + 50 knowledge-to-action derived from MaintNet procedures) — [HTML](https://arxiv.org/html/2510.05524v1)
- Grading: LLM-as-judge (GPT-4o and Llama-3.3-70B) on 1–5 rubrics: correctness, completeness, practicality, safety, clarity. Generators were local models only (Gemma-3-27B, Phi-4 14B, Mistral-Nemo 12B). The authors state that LLM-as-judge "remains neither fully valid nor reliable" — [HTML](https://arxiv.org/html/2510.05524v1)
- A companion study ran 16 NLP tools and LLMs zero-shot for knowledge-graph construction on FAA failure data and found significant limitations for "trusted" offline systems — [arXiv:2507.22935](https://arxiv.org/abs/2507.22935)

**K. Other datasets that matter for AerospaceBench design (smaller or unverified)**
- **AIAA SciTech 2025 paper 2025-0702, "Development of an Aerospace Engineering Evaluation Set for LLM Benchmarking"** (Brian J. Connolly). Title, author and venue confirmed via Semantic Scholar — [S2 API](https://api.semanticscholar.org/graph/v1/paper/DOI:10.2514/6.2025-0702?fields=title,authors,abstract,year,venue,citationCount); [AIAA](https://arc.aiaa.org/doi/10.2514/6.2025-0702). The search snippet describes "a multimodal benchmark designed to evaluate LLMs' capabilities on a sampling of aerospace engineering test questions and tasks". Size, tasks, scores and release status could not be verified (AIAA returned 403).
- **AviationBench (Georgia Tech PhD, Xiao Jing; advisor D. Mavris)**: described as a benchmark framework for NLP models on aviation-safety narratives, addressing class imbalance and rare high-stakes categories. Thesis defense was announced for April 2026. Known only from a GT event-page snippet; contents, size and results are unverified — [GT news node](https://hg.gatech.edu/node/691026)
- **Aviation Safety QA dataset (Georgia Tech ASDL)**: expert-written questions over NTSB and ASRS narratives. Answers were extracted by few-shot Llama 3.3 70B and then human-verified. Size not verified (repository 403) — [GT repository](https://repository.gatech.edu/entities/publication/b5570ed2-71ce-432e-b564-104c13b54726)
- **aeroBERT corpora (Georgia Tech)**: see Q3.

**L. Unverified or rumoured items**
- One search-engine summary credited a trilingual (English / simplified / traditional Chinese) aviation benchmark with 12,723 / 34,251 / 14,123 questions. No primary source was found and the summary appeared to conflate it with Pre-Flight. Treat as unverified.
- No benchmark named "AeroGPT", "AeroEval", or an LLM-focused "AeroBench" was found. An "AeroBench" exists but is an aerial video-generation benchmark — [awesomepapers](https://awesomepapers.io/generative-models/datasets/aerobench). "CAST-Eval" (civil aviation safety), cited in CAMB's related work, could not be located — [CAMB HTML](https://arxiv.org/html/2508.20420v1)

### Inferences
- The field is very young. Nearly every substantive aviation LLM benchmark appeared between mid-2025 and Sept 2026, and most items are recognition-style MCQ or text understanding over operations, maintenance and safety corpora.
- Every MCQ benchmark is far from saturated: Pre-Flight ≈83% vs ≈95% expert, CAMB 60–70%, aero manufacturing ≈50%. Gains are slow (Pre-Flight: about 75% in early 2025 to 82.7% by mid-2026), which suggests these sets bottleneck on domain fact recall rather than reasoning.
- The most mature evaluation design ideas are in the ops/agentic items (AeroCopilotBench, the ATC Risk Score, PilotBench): hard safety gates, repeatability (pass^k), and consequence-weighted scoring. AerospaceBench should borrow these rather than plain accuracy.
- Chinese groups (Beihang, COMAC/NUAA, Chengdu Aircraft, Qihoo 360) are major contributors. Their data are often Chinese-language, non-commercially licensed (CAMB), or unreleased.

### Gaps
- Could not read the full texts of ALUE (AIAA 2025-3247) or Connolly (AIAA 2025-0702); dataset sizes and model scores for both are unknown.
- AeroQA's exact size (27k vs 34k) is unresolved; the KITLM abstract does not state it.
- No leaderboard results are published for ALUE.

---

## Q2. Evaluations of LLMs on aircraft/rotorcraft design and engineering analysis (aero, flight mechanics, structures, propulsion, avionics/FCS)

### Takeaway
There is no quantitative, verified benchmark of LLMs on aeronautical engineering analysis: no sizing, aerodynamic calculation, performance, stability and control, stress/fatigue/damage-tolerance, composites, gas-turbine cycle or flight-control design benchmark. Existing work consists of:
- small qualitative or human-coded studies: AeroEngQA (80 items), Penn State course MCQs (100 items), ICAS 2024 ChatGPT system-configuration study;
- agent or tool demonstrations with little benchmarking: TurboAgent, AVPD/Wingbuilder, AirfoilAgent, AutoFPDesigner;
- generic engineering benchmarks with small aerospace slices (EngDesign, ERI, IndustryCode).

Rotorcraft has no LLM evaluation at all.

### Cited Findings
- **Penn State course-exam MCQ study (ASEE 2025)**: J. B. Coder, J. G. Coder, M. D. Maughmer. 104 original MCQs from junior/senior aerodynamics and aeronautics exams (Aeronautics, Stability & Control, Numerical Methods). 100 were analysed; 4 image questions were excluded, as were short-answer items needing sketches. Results: ChatGPT-4 69.0% and Gemini 63.0% overall. ChatGPT-4 scored Remember 86.5% (32/37), Understand 66.7% (28/42), Apply 50.0% (9/18); only 3 Analyze items. Chi-square tests show accuracy falls as Bloom's level rises — [ASEE PDF](https://peer.asee.org/57581.pdf)
- **AeroEngQA (Southampton, AIAA SciTech 2025)**: the only human-authored aircraft-engineering QA set. LLMs answered verbosely, hallucinated answers to unanswerable questions, and question "technical ambiguity" drove difficulty more than multi-hop reasoning — [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
- **AVPD, Aerospace Visual Programming Dataset (Southampton, arXiv:2606.16806, Jun 2026)**: 18 expert-designed aerospace wing-geometry tasks with ground-truth solutions. A GPT 5.4 ReAct copilot works in Grasshopper (a Rhino plugin, i.e., proprietary CAD) with the "Wingbuilder" component library. Evaluation was a user trial with 2 experienced engineers from a large aircraft manufacturer; slow ReAct inference limited usefulness — [papers.cool summary](https://papers.cool/arxiv/2606.16806); [arXiv listing](http://export.arxiv.org/api/query?search_query=abs:aerospace+AND+abs:%22language+model%22&sortBy=submittedDate&sortOrder=descending&max_results=100)
- **ICAS 2024, "Large Language Model in Aircraft System Design"** (Petter Krus, Linköping): ChatGPT-4/4o generates configuration rules for hybrid aircraft propulsion systems, PlantUML component diagrams, and Python configuration and simulation code. Correctness is studied statistically over repeated sampling because outputs are non-deterministic. This is a demonstration, not a benchmark — [ICAS PDF](https://www.icas.org/icas_archive/icas2024/data/papers/icas2024_0514_paper.pdf)
- **TurboAgent (arXiv:2604.06747, Apr 2026)**: an LLM multi-agent framework for transonic single-rotor compressor aerodynamic design (generative design, surrogate prediction, multi-objective optimisation, physics-based validation). Surrogates reach R² > 0.91 and NRMSE < 8%; optimisation gave +1.61% isentropic efficiency and +3.02% total pressure ratio in about 30 min. The CFD solver is not named in the abstract and no code release is mentioned — [arXiv](https://arxiv.org/abs/2604.06747)
- **AirfoilAgent (Advanced Engineering Informatics, 2025)**: multi-agent LLM airfoil aerodynamic optimisation (planning, knowledge integration, evaluation/reflection). Known only from a ScienceDirect snippet; metrics not verified — [ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S1474034625011395)
- **AutoFPDesigner (arXiv:2410.14989, Oct 2024)**: a multi-agent LLM for PBN flight-procedure design, reporting about 75% task completion and "nearly 100%" safety of the designed procedures. Validation against ICAO PANS-OPS is not described in the abstract — [arXiv](https://arxiv.org/abs/2410.14989)
- **SwRI internal R&D 18-R6365 (May–Sep 2023, older)**: GPT-4 helped with pair programming, literature review and hypersonic vehicle design, but "struggled with … complex mathematics … and rigorous physical reasoning" — [SwRI](https://www.swri.org/what-we-do/internal-research-development/2023/energy-environment/evaluating-ai-large-language-models-mechanical-aerospace-engineering-applications-18-r6365)
- **Linköping "LLM pilot" in system-of-systems simulation (FT2025)**: Gemma3 local models act as pilots in a firefighting-aircraft SoS simulation. The qualitative findings were inconsistency, format errors and hallucinations, and in one test the agent never flew back. Conclusion: "not ready for actual flight" — [FT2025 slides](https://ftfsweden.se/wp-content/uploads/2025/10/FT2025_Presentation_JorgeLovaco.pdf)
- **Aero-engine**:
  - NUAA and COMAC Shanghai Aircraft Design & Research Institute built an LLM-generated aero-engine QA dataset (J. Data Acquisition & Processing 40(3), 2025). Quality was filtered by source-text faithfulness similarity plus LLM semantic scoring, and fine-tuned models improved on aero-engine QA. No public test set or size found — [NUAA abstract](https://sjcj.nuaa.edu.cn/sjcjyclen/article/abstract/202503004)
  - LLM transfer learning for turbofan RUL prediction (C-MAPSS FD002/FD004) beats SOTA. This is a prognostics regression study, not a knowledge benchmark — [arXiv:2410.03134](https://arxiv.org/abs/2410.03134)
- **Generic engineering cross-references** (covered by another researcher):
  - EngDesign (NeurIPS 2025 D&B; arXiv:2509.16204): 9 engineering domains, simulation-verified grading using SPICE, FEA and MATLAB Control System Toolbox (proprietary). Top overall pass rate 34.38% — [arXiv](https://arxiv.org/abs/2509.16204); [NeurIPS](https://proceedings.neurips.cc/paper_files/paper/2025/hash/664f777548205fb6e0cbb0965e8d2e16-Abstract-Datasets_and_Benchmarks_Track.html). The arXiv abstract mentions aerospace; the size of any aerospace slice is not verified.
  - ERI benchmark: 57,750 records across 9 fields including aerospace. GPT-5, Claude Sonnet 4 and DeepSeek V3.1 score above 4.30/5 (judge-scored, saturating) — [arXiv:2603.02239](https://arxiv.org/abs/2603.02239)
  - IndustryCode: 579 sub-problems across domains including aerospace; Claude 4.5 Opus 68.1% / 42.5% — [arXiv:2604.02729](https://arxiv.org/abs/2604.02729)
- **Space/hypersonic cross-references** (out of scope): TPS-CalcBench (420 items, hypersonic TPS calculations, rubric plus human audit) — [arXiv:2604.17966](https://arxiv.org/abs/2604.17966); APBench (astrodynamics) — [MIT AeroAstro](https://aeroastro.mit.edu/news-impact/apbench-and-benchmarking-large-language-model-performance-in-fundamental-astrodynamics-problems-for-space-engineering)

### Inferences
- The largest white space for AerospaceBench is quantitative aeronautical engineering judgment and calculation: sizing, constraint diagrams, drag build-up, propulsion cycle decks, V-n diagrams, loads, stress/fatigue/DT, composites allowables, stability derivatives, handling qualities, and control-law design with margins. Its neighbours (TPS-CalcBench for hypersonics, EngDesign for generic design) show this kind of item is feasible with numeric-tolerance or simulation grading.
- The Penn State result (Apply 50% vs Remember 86.5% for GPT-4, Jan 2025) and SwRI's 2023 observation both indicate that applied and analytic items discriminate far better than recall items. AerospaceBench should weight Apply/Analyze/Evaluate/Create.
- The agentic design demos (TurboAgent, AVPD, AirfoilAgent) depend on bespoke and sometimes proprietary toolchains (Grasshopper/Rhino; unspecified CFD) and lack reproducible scoring. An open-tool agentic track would be new (e.g., OpenVSP/VSPAERO, XFOIL/AVL, SU2, OpenMDAO/pyCycle, CalculiX/Code_Aster). These are suggestions; their absence from existing benchmarks is an inference from the arXiv listings.

### Gaps
- No LLM evaluation was found for rotorcraft (VFS Forum, ERF, CEAS) despite targeted searches. It may exist in proceedings not indexed by web search.
- No LLM evaluation was found for aerostructures (stress, fatigue, damage tolerance, composites); searches returned only conventional structures literature.
- No avionics/FCS LLM benchmark was found. Adjacent generic controller-design benchmarks (e.g., arXiv:2608.07004, ControlAgent 2410.19811) exist but were not verified in detail.
- The ICAS 2024/2026 and RAeS/Aeronautical Journal proceedings were not systematically searchable here.

---

## Q3. Evaluations on airworthiness/certification, standards, safety assessment, requirements engineering and MBSE

### Takeaway
No benchmark tests LLMs on certification work: CS-25/Part 25 compliance, CS-27/29, DO-178C/DO-254 objectives, or ARP4754A/ARP4761 FHA/PSSA/FTA/FMEA generation and review. The closest verified work is requirements-level NLP:
- aeroBERT corpora built from 14 CFR Parts 23/25;
- ChatGPT/Gemini on aerospace requirements NER (very poor);
- aerospace requirements → LTL;
- assurance-case reviews by LLM judges;
- hazard scenario generation from ASRS.

### Cited Findings
- **aeroBERT-Classifier dataset (Georgia Tech, Aerospace 10(3), 2023, older)**: 325 requirements from 14 CFR Parts 23 and 25. The card's class counts are design 149, functional 99, performance 62 (these sum to 310, not 325). Apache-2.0, gated on HF. The card recommends P/R/F1 because of class imbalance — [HF](https://huggingface.co/datasets/archanatikayatray/aeroBERT-classification); [MDPI](https://www.mdpi.com/2226-4310/10/3/279)
- **aeroBERT-NER corpus (older)**: 1,432 sentences (1,100 general aerospace + 332 requirements) with 5 entity types (SYS, RES, VAL, ORG, DATETIME) — [preprints.org description](https://www.preprints.org/manuscript/202410.0656); [AIAA JAIS via Crossref](https://api.crossref.org/works/10.2514%2F1.I011251)
- **Generative LMs for requirements engineering (Saleem et al., DFKI, arXiv:2412.00959, Dec 2024)**: on the aerospace requirements NER dataset (6,347 words), ChatGPT F1 0.36 and Gemini 0.25 vs SOTA 0.92. Other RE tasks: extraction 0.76/0.77 vs 0.86; classification 0.78/0.78 vs 0.96; QA 0.91/0.88 vs 0.90 — [arXiv](https://arxiv.org/abs/2412.00959)
- **AeroReq2LTL (arXiv:2604.21715, Apr 2026)**: LLM plus data dictionary plus template requirement language turns industrial aerospace requirements into LTL. 85% precision and 88% recall; outputs feed model checkers. The specific system and dataset size are not in the abstract. CC BY 4.0 — [arXiv](https://arxiv.org/abs/2604.21715)
- **AI-assisted RE vs expert judgment (arXiv:2604.15222, Apr 2026)**: compares experienced systems engineers with an AI tool on INCOSE "good requirement" criteria. AI was consistent on syntactic and structural attributes; experts were needed for context, ambiguity and trade-offs. The domain is not stated as aerospace — [arXiv](https://arxiv.org/abs/2604.15222)
- **Hazard scenarios from ASRS (Mascia, Pietrantuono, Rodriguez, Russo; arXiv:2608.04697, Aug 2026)**: LLMs generate candidate hazard scenarios for a target adverse outcome. Evaluated on plausibility (co-occurrence evidence), traceability to held-out ASRS reports, and validity/realism across prompting and fine-tuning — [arXiv](https://arxiv.org/abs/2608.04697)
- **Assurance-case review by LLM judges (arXiv:2511.02203, Nov 2025)**: GPT-4o, GPT-4.1, DeepSeek-R1 and Gemini 2.0 Flash review GSN assurance cases with predicate rules; DeepSeek-R1 and GPT-4.1 did best. Earlier, GPT-4 Turbo was tested on generating assurance-case defeaters (arXiv:2401.17991, 2024). Both are generic safety assurance, not aviation-specific — [arXiv:2511.02203](https://arxiv.org/abs/2511.02203); [arXiv listing](http://export.arxiv.org/api/query?search_query=abs:%22language+model%22+AND+(abs:airworthiness+OR+abs:%22DO-178C%22+OR+abs:ARP4754A+OR+abs:ARP4761+OR+abs:avionics+OR+abs:%22aero-engine%22+OR+abs:turbofan+OR+abs:helicopter+OR+abs:rotorcraft)&sortBy=submittedDate&sortOrder=descending&max_results=80)
- **LLM-ACNC (MDPI Aerospace 12(6):463, 2025)**: LLM-based knowledge-graph construction from aerospace requirement texts. Known from a search snippet only; metrics not verified — [MDPI](https://www.mdpi.com/2226-4310/12/6/463)
- **Drone regulatory-compliance RAG assistant (EASA UAS rules; arXiv:2603.09999)**: out of scope (UAS); noted as the closest regulatory-RAG design — [arXiv](https://arxiv.org/abs/2603.09999)
- An arXiv API keyword sweep ("language model" with airworthiness, DO-178C, ARP4754A, ARP4761, avionics, aero-engine, turbofan, helicopter, rotorcraft) returned no certification-evaluation paper for manned aircraft — [arXiv API listing](http://export.arxiv.org/api/query?search_query=abs:%22language+model%22+AND+(abs:airworthiness+OR+abs:%22DO-178C%22+OR+abs:ARP4754A+OR+abs:ARP4761+OR+abs:avionics+OR+abs:%22aero-engine%22+OR+abs:turbofan+OR+abs:helicopter+OR+abs:rotorcraft)&sortBy=submittedDate&sortOrder=descending&max_results=80)

### Inferences
- Certification and safety-assessment work is a clear, high-value gap. Candidate tasks include:
  - compliance-matrix drafting against CS-25/Part 25 paragraphs with AMC/AC references;
  - FHA failure-condition classification and probability budgets per AMC 25.1309;
  - fault-tree construction and quantification;
  - DO-178C objective and DAL mapping;
  - reviewing a flawed certification artefact.
- These suit expert rubrics plus partially checkable sub-answers (DAL, classification, numeric probability).
- The aeroBERT-NER result (ChatGPT F1 0.36 vs 0.92) is old (2023-era models) and needs re-measurement with 2026 frontier models before it is cited as current capability.

### Gaps
- No verified study evaluates LLMs on DO-178C/DO-254, ARP4754A/ARP4761, CS-27/29 or MBSE/SysML specifically in aerospace. SysML-generation benchmarks may exist generically but were not verified here.
- A search snippet claimed "Claude Sonnet 3.5 85% vs 380 engineer evaluations, GPT-4 45%, Llama 3 47.9%" on INCOSE requirement quality. No primary source was confirmed, so it is unverified.
- EASA's AI Concept Paper and roadmap guidance on LLMs was not retrieved in this pass.

---

## Q4. Evaluations on operations & maintenance (MRO, logbooks, troubleshooting, manuals, safety reports, ATC, flight ops, pilots)

### Takeaway
This is the best-covered area. Maintenance has CAMB, KEO/OMIn, MaintNet action prediction, LogSyn, MRO text-to-SQL and a C172 manual RAG. Safety reports have ASRS/NTSB/HFACS studies. ATC has the Risk Score benchmark, consequence-aware evaluation, non-towered airport hazards, ATC transmissions, conflict-resolution agents and NOTAM interpretation. Flight ops have Pre-Flight, PilotBench and AeroCopilotBench. Common findings:
1. Frontier models reach roughly 60–80% on knowledge items but are fragile under realistic noise and safety-weighted scoring.
2. Lexical metrics (ROUGE/BLEU) dominate older work and are poor proxies for safety.
3. Small fine-tuned models often match or beat GPT-4o on narrow log tasks.

### Cited Findings
**Maintenance / MRO**
- CAMB (see Q1-C): 60–70% MCQ; fault-tree reasoning on 50 real B737/A320 cases — [arXiv](https://arxiv.org/html/2508.20420v1)
- MaintNet maintenance-action prediction (Kumar, Farahat, Gupta; PHM Asia-Pacific 2025): GPT-4o had the strongest semantic alignment zero/few-shot. Fine-tuned Gemma-3-4B surpassed GPT-4o on several metrics; fine-tuning improved LLaMA-3.2-3B BLEU by up to 90% and Phi-4-mini ROUGE-2 by 30%. Metrics: ROUGE, BLEU, cosine, BERTScore — [PHM Society](https://papers.phmsociety.org/index.php/phmap/article/view/4652)
- LogSyn (arXiv:2511.18727, INCOM 2026): few-shot LLM structuring of 6,169 general-aviation maintenance log records into a hierarchical ontology — [arXiv](https://arxiv.org/abs/2511.18727)
- Text-to-SQL for aviation MRO (arXiv:2506.13785, Jun 2025): LLM-synthesised question–SQL pairs plus an F1 "soft metric" on result overlap, instead of binary execution accuracy. CC BY 4.0 — [arXiv](https://arxiv.org/abs/2506.13785)
- Multimodal RAG for the Cessna 172 maintenance manual (arXiv:2608.18465, Aug 2026): recall@5 93.37% and semantic similarity 87.20% on synthetic queries, including diagrams — [arXiv API](http://export.arxiv.org/api/query?id_list=2608.18465)
- KEO/OMIn (Q1-J) and Trusted Knowledge Extraction (16 tools zero-shot on FAA data) — [arXiv:2510.05524](https://arxiv.org/html/2510.05524v1); [arXiv:2507.22935](https://arxiv.org/abs/2507.22935)

**Safety reports (ASRS / NTSB / HFACS)**
- ChatGPT on ASRS (Georgia Tech, Aerospace 10(9):770, 2023, older): generated synopses were compared with ASRS ground-truth synopses, with aeroBERT closest in similarity. Human-factor attribution concurrence was 61%, and ChatGPT was cautious about attributing human factors — [MDPI](https://www.mdpi.com/2226-4310/10/9/770/html); [preprint](https://www.preprints.org/manuscript/202307.0192)
- GA-ACAPS (Liu, Yan, Li, Feng; AHFE): 2,250 NTSB GA accident reports, HFACS prediction with three prompting methods. Best on unsafe acts and some preconditions; witness narratives helped — [AHFE open access](https://openaccess.cms-conferences.org/publications/book/978-1-964867-24-3/article/978-1-964867-24-3_8)
- HFACS multi-label classification by GRPO fine-tuning of Llama-3.1-8B (arXiv:2508.21201, Aug 2025): exact match rose from 0.04 to 0.18 on synthetic accident data, outperforming GPT-4-mini and Gemini-2.5-flash (as reported). The low absolute EM shows how hard the task is — [arXiv API](http://export.arxiv.org/api/query?id_list=2508.21201)
- FlightLLM hard-landing explanation (arXiv:2608.18017, Aug 2026): 704 real A320 flight samples; CatBoost plus semantic discretization plus few-shot LLM explanations — [arXiv API](http://export.arxiv.org/api/query?id_list=2608.18017)
- UMD student study: fine-tuned T5 on 1,107 ASRS narratives (2018–2024); ROUGE-L 0.357 and SBERT cosine 0.610. Low rigour — [UMD PDF](https://www.cs.umd.edu/content/large-language-models-aviation-safety-enhancing-incident-analysis-through-summarization-asrs)
- Hazard scenarios from ASRS (arXiv:2608.04697) — see Q3.

**ATC and airspace operations**
- Risk Score ATC benchmark (NTU, Q1-I): peak 0.69 on clean transcripts; collapses under ASR noise — [HTML](https://arxiv.org/html/2605.11769)
- Consequence-aware ATC evaluation (arXiv:2608.24621): 40 ATCOs, 8 models; finds a semantic–safety gap — [summary](https://aiunderstanding.org/news/preprint-finds-standard-language-model-scores-can-overstate-safety-in-air-traffic)
- Non-towered airport safety assessment (Darrell, Ghazanfari, Kam, Bayen, Tabrizian, Wei; AIAA 2026; arXiv:2605.12332):
  - inputs: CTAF transcripts, METAR, ADS-B and VFR sectionals; 12-category hazard taxonomy; synthetic dataset plus a Half Moon Bay case study;
  - models: Qwen 2.5-7B, Mistral-7B, Gemma-2-9B, GPT-4o, GPT-5.4, Claude Sonnet 4.6, Gemini 2.5 Pro;
  - result: macro-F1 above 0.85 on binary nominal/danger classification — [arXiv](https://arxiv.org/abs/2605.12332)
- ATC transmissions on an SF Bay Tour route (arXiv:2608.19299, Aug 2026): 9 LLMs; lexical, structural and semantic metrics plus a GPT-5.5 judge validated against human annotation. Lighter prompts beat heavily scripted ones — [arXiv](https://arxiv.org/abs/2608.19299)
- LLM ATC conflict-resolution agents (arXiv:2409.09717, 2024): function calling plus an experience library; nearly all of 120 imminent-conflict scenarios (up to 4 aircraft) resolved — [arXiv API](http://export.arxiv.org/api/query?id_list=2409.09717)
- AirTrafficGen (arXiv:2508.02269): LLM ATC scenario generation, benchmarking Gemini 2.5 Pro, o3 and others — [arXiv API](http://export.arxiv.org/api/query?id_list=2508.02269)
- NOTAM-Evolve (arXiv:2511.07982, Nov 2025): 10,000 expert-annotated NOTAMs; +30.4 points absolute accuracy over the base LLM — [arXiv API](http://export.arxiv.org/api/query?id_list=2511.07982)

**Flight operations and pilots**
- Pre-Flight (Q1-A), PilotBench / FLY-EVAL++ (Q1-G) and AeroCopilotBench (Q1-H).
- LeRAAT (arXiv:2503.16477): LLM plus RAG over aircraft manuals and FAA directives in X-Plane (commercial simulator); mean retrieval time 11.93 s for five pages. A system demo, not a benchmark — [arXiv API](http://export.arxiv.org/api/query?id_list=2503.16477)
- LLM trajectory prediction and reconstruction: fine-tuned LLaMA-3.1 on ADS-B (arXiv:2501.17459); LLaMA 2 on trajectory reconstruction (arXiv:2401.06204, older) — [arXiv API](http://export.arxiv.org/api/query?id_list=2501.17459,2401.06204)
- Military air operations (Brazilian Air Force, arXiv:2609.21390): offline multimodal RAG matched analysts on a doctrinal test (8/10), with task time cut from 26.5 to 7.1 min. A pilot study with 4 analysts — [arXiv](https://arxiv.org/abs/2609.21390)
- AviationGPT (arXiv:2311.17686, Nov 2023, older): LLaMA-2/Mistral continued pre-training on aviation text, with a claimed ">40% performance gain in tested cases". The evaluation sets are not described in the abstract — [arXiv](https://arxiv.org/abs/2311.17686)

### Inferences
- Safety-weighted, consequence-aware scoring (Risk Score, safety gates, Pilot-Score) consistently reveals much lower reliability than plain accuracy or F1. This is the key methodological lesson for AerospaceBench's ops and maintenance tracks.
- Troubleshooting with real FIM/TSM logic is covered only thinly (CAMB's 50 fault-tree cases, in Chinese) and no agentic AMM/IPC/FIM navigation benchmark exists. Proprietary manuals block open datasets; the C172 manual and GA POHs are the public workaround used so far.
- Many ops evaluations use LLM-generated or synthetic queries (C172 RAG, text-to-SQL, HFACS GRPO, non-towered airports), which limits authenticity.

### Gaps
- No verified LLM benchmark covers reliability engineering or prognostics at the reasoning level (e.g., MSG-3 task analysis, MTBUR/MTBF interpretation, ETOPS/CDL/MEL decisions).
- MEL/dispatch decision-making and weight & balance and performance calculations (takeoff/landing distance) are not evaluated anywhere found.
- ECCAIRS-based evaluations were not found.

---

## Q5. Evaluations on licence and professional exams (EASA Part-66, FAA A&P, ATPL theory, FAA knowledge tests)

### Takeaway
No peer-reviewed or arXiv study was found that evaluates LLMs on EASA Part-66 modules, FAA A&P, EASA ATPL theory or FAA Airman Knowledge Test question banks, despite targeted searches (medical-exam studies dominate results). Exam-like content appears only inside broader benchmarks:
- CAMB: licensing and 737NG certification exam MCQs, 60–70%;
- Pre-Flight: ICAO/FAA regulations;
- ALUE: an "aviation_knowledge_exam" file;
- AeroCopilotBench: a Claude-generated auxiliary knowledge test from FAA manuals, AIM and 14 CFR;
- Penn State: undergraduate aero exam MCQs, GPT-4 69%.

### Cited Findings
- Penn State aero course MCQs: GPT-4 69% and Gemini 63% (Jan 2025 models); accuracy declines with Bloom's level — [ASEE](https://peer.asee.org/57581.pdf)
- CAMB's MCQ component draws on professional licensing exam questions and 737NG maintenance certification exams; best LLM 68.87% — [arXiv HTML](https://arxiv.org/html/2508.20420v1); [GitHub](https://github.com/CamBenchmark/cambenchmark)
- Pre-Flight's FAA (51) and ICAO (85) regulation items; best 82.7% overall. Answer-key issues were flagged in the FAA category — [arXiv HTML](https://arxiv.org/html/2607.01829)
- ALUE ships an `aviation_knowledge_exam` MCQA file; provenance is undocumented on the pages fetched — [GitHub](https://github.com/mitre/alue)
- AeroCopilotBench auxiliary knowledge test: Claude-generated from FAA flight manuals, the AIM, 14 CFR and POHs. It was used to contrast static QA with interactive execution — [arXiv PDF](https://arxiv.org/pdf/2608.16349)
- Part-66 exams are mostly MCQ with a 75% pass mark (context for any future comparison) — [Part66online](https://www.part66online.com/easa-part-66/)

### Inferences
- The absence of published Part-66/A&P/ATPL LLM studies may reflect question-bank copyright (commercial banks and authority-controlled exams). AerospaceBench should not rely on scraping these banks.
- Public FAA sample questions exist (e.g., the FAA PAR sample question PDF surfaced in results: [FAA](https://www.faa.gov/sites/faa.gov/files/training_testing/testing/test_questions/par_questions.pdf)), but they are public and therefore likely contaminated.
- Given the Pre-Flight and CAMB levels, frontier models would probably pass many recall-style licence exams at ≥75%. That would say little about engineering competence, so exam-style items should be a minor calibration component.

### Gaps
- No direct measurement of frontier-model pass rates on Part-66, A&P, ATPL or FAA knowledge tests was found.
- Chinese CAAC licence exam evaluations may exist in Chinese-language venues; not verified.

---

## Q6. Aerospace manufacturing evaluations (process planning, composites, NDT, quality)

### Takeaway
Only one verified manufacturing-specific aerospace LLM benchmark exists: the Chengdu Aircraft / Beihang set of about 7,500 LLM-generated multi-answer MCQs over assembly, materials and structural steel. Scores are about 44–51% under negative marking, with materials weakest at a 35.4% average. No benchmarks were found for composites manufacturing, NDT/inspection interpretation, process-plan generation or quality/non-conformance disposition. Search budget limits affected this area.

### Cited Findings
- Aerospace manufacturing MCQ: Gemini-2.0-flash-exp 50.75% (top); materials subdomain average 35.40%; LLM-generated items with about 300 expert-reviewed; +1/−3 scoring — [arXiv HTML](https://arxiv.org/html/2501.17183v1)
- The authors conclude that LLM aerospace professional knowledge is "in urgent need of improvement", citing hallucination risk to product quality and flight safety — [arXiv](https://arxiv.org/abs/2501.17183)
- Adjacent, not aerospace-specific: FDM-Bench (additive manufacturing, arXiv:2412.09819) and LLM drawing-annotation-to-CAD mapping (arXiv:2602.18296) appear in the arXiv "aerospace + language model" listing — [arXiv API listing](http://export.arxiv.org/api/query?search_query=abs:aerospace+AND+abs:%22language+model%22&sortBy=submittedDate&sortOrder=descending&max_results=100)

### Inferences
- Because items were generated by an LLM (Gemini) from undisclosed sources and only about 4% were expert-reviewed, the manufacturing set's accuracy figures are noisy and may encode generator bias. AerospaceBench should use practitioner-authored, document-grounded manufacturing tasks, such as:
  - interpreting a drawing callout, GD&T and process spec;
  - writing a cure-cycle deviation disposition;
  - selecting an NDT method and acceptance criteria;
  - fastener and hole-prep selection.

### Gaps
- No NDT, composites (layup, cure, porosity, repair) or MRB/non-conformance LLM evaluation was verified. Web search was exhausted before these targeted queries could run, so a follow-up pass is recommended.

---

## Q7. Relevance to AerospaceBench: what is reusable, pitfalls to avoid, coverage vs. gaps

### Takeaway
Existing aviation benchmarks cover ops knowledge, maintenance text, safety narratives, ATC language and a few agentic flight-procedure tasks. They leave design, analysis, test, certification and most of manufacturing almost untouched. The strongest reusable assets are evaluation methods, not datasets. Datasets are reusable only with care because of licences, language, contamination and synthetic generation.

### Cited Findings (assets and pitfalls, each grounded above)
- **Reusable harnesses**:
  - Pre-Flight is integrated in UK AISI `inspect_evals` (Inspect framework), with an MIT dataset — [arXiv HTML](https://arxiv.org/html/2607.01829); [HF](https://huggingface.co/datasets/AirsideLabs/pre-flight-06)
  - ALUE is Apache-2.0, multi-task, multi-backend, and includes LLM-judge claim-decomposition metrics — [GitHub](https://github.com/mitre/alue)
- **Reusable scoring designs**:
  - safety-gated success plus pass^k repeatability plus ineffective-action rate (AeroCopilotBench) — [arXiv PDF](https://arxiv.org/pdf/2608.16349)
  - consequence-weighted Risk Score with strict variant (ATC) — [HTML](https://arxiv.org/html/2605.11769)
  - composite accuracy plus safety Pilot-Score — [arXiv](https://arxiv.org/abs/2604.08987)
  - negative marking (+1/−3) for multi-answer MCQ — [HTML](https://arxiv.org/html/2501.17183v1)
  - answerable/unanswerable split to measure abstention (AeroEngQA) — [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
  - Bloom's-level coding of items — [ASEE](https://peer.asee.org/57581.pdf)
  - simulation-verified grading (EngDesign) — [arXiv](https://arxiv.org/abs/2509.16204)
- **Pitfalls documented in existing work**:
  - LLM-generated items (aero manufacturing: Gemini-generated; AeroQA: ChatGPT-generated and judged "not authentic") — [HTML](https://arxiv.org/html/2501.17183v1); [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
  - template-synthetic QA (AviationQA, 1M template pairs) — [arXiv](https://arxiv.org/abs/2301.04013)
  - partial expert validation with no IAA, and answer-key errors (Pre-Flight FAA category) — [arXiv HTML](https://arxiv.org/html/2607.01829)
  - LLM-as-judge validity (KEO authors' caveat) — [HTML](https://arxiv.org/html/2510.05524v1)
  - contamination from public NASA/NTSB/FAA documents (AeroEngQA "unanswerable" leakage) — [preprint](https://southampton.ac.uk/~sem03/aiaa-2025-preprint.pdf)
  - lexical metrics overstating safety (ATC consequence-aware studies) — [summary](https://aiunderstanding.org/news/preprint-finds-standard-language-model-scores-can-overstate-safety-in-air-traffic)
  - fragility under realistic input noise (Risk Score 0.69 dropping to ≤0.07 with ASR noise) — [HTML](https://arxiv.org/html/2605.11769)
  - run-to-run inconsistency (AeroCopilotBench step omission) — [arXiv PDF](https://arxiv.org/pdf/2608.16349)
  - image items excluded because models could not process them (Penn State) — [ASEE](https://peer.asee.org/57581.pdf)
- **Licensing and tool constraints**:
  - CAMB is CC BY-NC-SA 4.0 (non-commercial) — [GitHub](https://github.com/CamBenchmark/cambenchmark)
  - aeroBERT data is Apache-2.0 but gated — [HF](https://huggingface.co/datasets/archanatikayatray/aeroBERT-classification)
  - Proprietary tools in existing work: Grasshopper/Rhino (AVPD), X-Plane (LeRAAT), MATLAB Control System Toolbox (EngDesign) — [papers.cool](https://papers.cool/arxiv/2606.16806); [arXiv API](http://export.arxiv.org/api/query?id_list=2503.16477); [EngDesign search summary](https://neurips.cc/virtual/2025/poster/121598)
  - AeroCopilotBench's ACOE and PilotBench need no proprietary tools — [arXiv PDF](https://arxiv.org/pdf/2608.16349); [HTML](https://arxiv.org/html/2604.08987)

### Coverage matrix (inference from the catalogue above)

| Work type → / Sub-discipline ↓ | Design | Analysis | Test | Certification | Manufacturing | Ops & Maintenance |
|---|---|---|---|---|---|---|
| Fixed-wing aircraft (general) | AeroEngQA (80, qualitative); AVPD (18 geometry tasks, user trial) | Penn State aero MCQ (100) | none | aeroBERT reqs (14 CFR 23/25; classification/NER only) | Aero manufacturing MCQ (~7.5k, LLM-generated) | Pre-Flight, ALUE, CAMB, KEO/OMIn, MaintNet, AeroCopilotBench, PilotBench, ATC sets |
| Rotorcraft | none | none | none | none | none | none |
| Aero-engines / gas turbines | TurboAgent (demo) | none (RUL prognostics only) | none | none | none | CAMB partial (engine faults inside 737/A320 cases) |
| Avionics / flight controls | none | none | none | AeroReq2LTL (reqs→LTL), assurance-case judges (generic) | none | AeroCopilotBench (electrical faults) |
| Aerostructures / materials | none | none | none | none | Aero manufacturing materials subset (35% avg) | none |
| Flight mechanics / performance | none | PilotBench (trajectory prediction, not engineering analysis) | none | none | none | AutoFPDesigner (procedure design) |

### Inferences
- **Recommended AerospaceBench emphasis**:
  1. Quantitative analysis items with numeric-tolerance grading: performance, sizing, aero, loads, stress/fatigue/DT, propulsion cycle, stability and control. None exist today.
  2. Certification artefacts graded by expert rubric plus checkable sub-fields (failure classification, DAL, probability targets, compliance references). None exist today.
  3. Agentic tasks with open-source engineering tools and simulation-verified outcomes. Today these exist only as demos.
  4. Test and evaluation (flight-test data reduction, test-plan design). None exist today.
  5. Rotorcraft and aero-engine coverage. Effectively zero today.
  6. A thin ops/maintenance layer, since that area is relatively crowded. Reuse Pre-Flight or ALUE as external calibration instead of duplicating them.
- **Item authoring**: practitioner-authored, document-grounded and held out, with double review and measured IAA. Avoid LLM-generated items, or use them only as distractor scaffolding with full expert review.
- **Scoring**: include an abstention/unanswerable component and consequence weighting so that "confidently wrong" answers on safety-relevant quantities are penalised.
- **Agentic tracks**: run multiple trials and report pass^k.
- **Saturation headroom**: ops MCQs sit at 60–83%; agentic and safety-gated tasks at 12–73%. Expect a design and analysis benchmark with authentic multi-step quantities to be far from saturated. This is inferred from EngDesign's 34% top pass rate and the Penn State Apply-level 50%.

### Gaps
- The ALUE and Connolly (AIAA 2025-0702) full texts are needed to confirm whether either already covers engineering analysis.
- No verified information was found on internal or private evaluations by Airbus, Boeing, Rolls-Royce, GE, Safran or EASA/FAA. Only the MITRE/FAA ALUE release is public. The NASA NTRS search was not completed within the search budget.
- AerospaceBench's novelty claims for rotorcraft, aerostructures, certification and NDT should be rechecked by a follow-up search pass (VFS Forum proceedings, ERF DSpace, AIAA SciTech 2026 sessions, CEAS Aeronautical Journal, RAeS), because those venues are poorly indexed and web search ran out during this pass.
