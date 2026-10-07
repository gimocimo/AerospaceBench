# General (non-aerospace) Engineering Benchmarks for LLMs — catalogue, grading, failure modes, lessons for AerospaceBench

Research date: 2026-10-06. Scope: engineering design/analysis/reasoning benchmarks outside aerospace. Agents driving CFD/FEA/CAD tools, aerospace-specific benchmarks (e.g. TPS-CalcBench) and general benchmark methodology (GDPval, HealthBench, BetterBench) are covered by other researchers and only cross-referenced here. "Verified" means the paper or abstract page was retrieved in this session. Items marked [older] date from before 2024. Scores are as reported by the original authors on the model versions named. Few of these benchmarks have been re-run on 2026 frontier models.

---

## 1. Engineering design and analysis benchmarks (catalogue)

### Takeaway
About 25 engineering-domain LLM benchmarks now exist. Most are textbook/exam-style QA with numeric answers. A small but growing set grades **functional correctness of a design** using simulators, analytical oracles, hidden validators or testbenches: EngDesign, ControlEval, VEHBench, ORAgentBench, CVDP, StructureClaw-Bench and FEM-Bench. On those design-type tasks frontier models score low: about 34% on EngDesign (o3, 2025), 35.5% on ORAgentBench (2026) and ≤34% pass@1 on CVDP code generation. On textbook-style analysis they score high: about 94% composite on ThermoQA (Claude Opus 4.6, 2026). The difficulty therefore comes from design synthesis, constraint satisfaction, diagram/drawing reading and open-ended modelling, not from closed-form calculation.

### Cited Findings

#### A. Cross-domain engineering DESIGN benchmarks

**EngDesign ("Toward Engineering AGI: Benchmarking the Engineering Design Capabilities of LLMs")**
- Authors/org: led by University of Illinois Urbana-Champaign, with UPenn, UC San Diego, U. Michigan and Amazon AGI. This is the same "agi4engineering" group that built ControlBench/ControlAgent. Year: 2025. Venue: NeurIPS 2025 Datasets & Benchmarks Track. Link: arXiv 2509.16204. — [arXiv HTML](https://arxiv.org/html/2509.16204v2); [NeurIPS proceedings](https://proceedings.neurips.cc/paper_files/paper/2025/hash/664f777548205fb6e0cbb0965e8d2e16-Abstract-Datasets_and_Benchmarks_Track.html)
- Domain/size: 101 design tasks across 9 domains: OS design, computer architecture, control, mechanical systems, structural design, digital hardware, analog IC, robotics and signal processing. Average prompt length is about 779 tokens. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- Task format: each task has a description with objectives and constraints, an evaluation rubric with partial credit (0–100), an executable evaluation pipeline and a reference solution. LLM outputs are structured (instructor library) so they can be parsed and simulated. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- Tools: **34 tasks need closed-source/licensed tools: MATLAB Control System Toolbox, Cadence, SPICE and structural FEA.** The other 67 tasks use open scripts and are released as the **EngDesign-Open** subset. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- Grading: simulation-based functional verification that outputs a binary pass/fail, a 0–100 score and evaluation logs. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- Item sourcing/validation: tasks were contributed by graduate students and researchers using a standardised template. Validation steps were an o4-mini check that the prompt is sufficient, a first-round clarity/feasibility review, a second-round domain-expert review, and final standardisation. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- Scores (overall pass rate, 2025): o3 34.38% (best), o4-mini-high 34.04%, o3-high 33.57%, o4-mini 31.60%, Gemini-2.5-Pro 29.54%, o1 29.17%, DeepSeek-R1 25.53%, Claude-3.7-Sonnet 22.61%, Claude-3.7-Thinking 20.07%, DeepSeek-v3 17.92%, GPT-4o 15.68%, Gemini-2.0-Flash 14.16%. **Analog IC design scored 0% for every model.** — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- With 10 rounds of simulator-feedback refinement, o3 reached about 60% pass rate. This was run on a 71-task subset with 4 models, and iteration did not help on analog IC tasks. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- Error analysis: domain knowledge error 30–33%, constraint violation 25–36%, prior-knowledge over-reliance 11–19%, hallucination 12–13%, computation error 6–9%. Knowledge and constraint errors together account for about 55–67% of failures. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- Licence/data: dataset on Hugging Face (opt1zer/EngDesign). The project website is CC BY-SA 4.0. The dataset licence was not confirmed. The leaderboard was last updated 2025-06-18 and shows no GPT-5 / Claude 4.x / Gemini 3 results. — [project page](https://agi4engineering.github.io/Eng-Design/)
- Stated limitations: uneven task distribution across domains (follows contributor expertise); the iteration study covered only a subset. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)

**EngiBench (Zhou et al.) — "A Benchmark for Evaluating LLMs on Engineering Problem Solving"**
- Authors/org: Nanyang Technological University, U. Sydney, CUHK-Shenzhen, HK PolyU and INSAIT. Year: 2025 (arXiv 2509.17677). Venue: ACL 2026 (Findings, per ACL Anthology ingest preview). — [arXiv HTML](https://arxiv.org/html/2509.17677v1); [ACL Anthology preview](https://preview.aclanthology.org/ingest-acl/2026.findings-acl.1810/)
- Structure: three levels:
  - L1: foundational knowledge retrieval / single-step formula application.
  - L2: contextual multi-step reasoning in explicitly defined scenarios.
  - L3: open-ended modelling covering information extraction, domain reasoning, trade-offs and uncertainty.
  - Subfields are grouped as Systems & Control, Physical & Structural, and Chemical & Biological. Reported total is 1,717 problems; the extracted sub-counts were 916/334/467. L3 has 43 open-ended tasks. The exact level breakdown should be re-checked in the paper. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- Notable design: L1/L2 items are rewritten into three controlled variants:
  - **perturbed**: numeric and semantic changes to reduce overlap with pretraining data;
  - **knowledge-enhanced**: formulas and constants are supplied, which isolates knowledge gaps;
  - **math abstraction**: engineering context is stripped out.
  - Together these separate robustness, domain knowledge and pure mathematics. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- Sourcing: L1/L2 come from existing benchmarks (MMLU, MATH, GSM8k, Orca-Math) plus university materials, with LLM relevance filtering and human verification. L3 comes from mathematical-modelling competitions (CUMCM, MCM/ICM, APMCM, 2010–2024) with their official rubrics, reformatted by PhD-level professionals. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- Grading: L1/L2 are binary with a ±2% numeric tolerance and LLM-assisted comparison to the reference answer. L3 uses a rubric built from official and expert criteria, scored by PhD professionals on 4 capability dimensions (0–10 each). — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- Scores: most models reach 70–90% on L1. Leading models reach about 80% on L2, and smaller models fall below 40%. On L3 the best models score about 7.0/10 against **human experts at 8.58/10**. Perturbed variants cut L2 accuracy by 8–11%. Models were strongest at information extraction and weakest at domain reasoning and uncertainty handling. Example: Llama 4 scored 0 on a multi-objective decision task because it gave no trade-off analysis, while GPT-4.1 scored 7.5. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- Licence: CC-BY-NC-4.0 (HF: EngiBench/EngiBench; GitHub: EngiBench/EngiBench). Limitations: text-only (no diagrams), no long-context tasks, and L3 is capped at 43 tasks because few rubrics are available. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- Follow-on: EngiAgent (same group, ICML 2026, arXiv 2605.02289) is a multi-agent system for open-ended engineering problems that reports large gains in **feasibility**. It targets data-extraction errors, constraint inconsistencies and solver failures. — [arXiv](https://arxiv.org/abs/2605.02289)
- **Name collision:** a different "EngiBench" (Felten et al., ETH Zürich and U. Maryland, NeurIPS 2025 D&B, arXiv 2508.00831) is an open-source library and set of datasets for *data-driven engineering design optimisation* (aeronautics, heat conduction, photonics, etc.). It has simulator-backed problems and feasibility checks, and is aimed at optimisation and generative/surrogate ML, not LLM QA. — [NeurIPS](https://papers.nips.cc/paper_files/paper/2025/hash/0013efa1327c079e73154d4061c3a396-Abstract-Datasets_and_Benchmarks_Track.html); [arXiv](https://arxiv.org/pdf/2508.00831)

**VEHBench (vibration energy harvester design), 2026**
- Su, Luo, Hu; arXiv 2607.18181, July 2026. 763 literature-grounded tasks across four design roles: **specification triage, verifier-guided search, corrupted-state recovery and policy-conditioned selection**. Tasks are scored by an analytical physical oracle. Finding: "LLM capability is strongly stage-dependent: no single model consistently dominates" the workflow. Dataset is on HF (AnonymousVehbench/vehbench). Named-model scores were not in the abstract. — [arXiv](https://arxiv.org/abs/2607.18181)

**ORAgentBench (operations research / industrial engineering), 2026**
- Li et al.; arXiv 2606.19787, June 2026. 107 human-reviewed end-to-end OR tasks, each with a natural-language brief, multi-file data, configuration artefacts and a required submission schema. Agents write and execute their own solution code. Grading uses hidden validators for schema validity, **hard-constraint feasibility** and normalised objective quality. Best of 14 frontier agent-model configurations: **35.51% overall, 20.59% on hard tasks**. Main weaknesses: missed operational rules, brittle formulations, weak construction of feasible solutions, and insufficient solution improvement. — [arXiv](https://arxiv.org/abs/2606.19787)

**MolDesignBench (adjacent: molecular design), 2026**
- arXiv 2609.27349. Search snippet only; not fetched. It identifies **implicit-constraint interpretation and infeasibility detection** as the main bottlenecks for LLM agents. — [arXiv](https://arxiv.org/abs/2609.27349v1)

#### B. Engineering documentation, drawings and requirements

**DesignQA (MIT + Autodesk Research)**
- Doris, Grandi, Tomich, Alam, Ataei, Cheong, Ahmed; arXiv 2404.07917 (2024). Presented at IDETC-CIE 2024. MIT DSpace lists a published version; the journal venue was not verified. — [arXiv HTML](https://arxiv.org/html/2404.07917v2); [MIT DSpace](https://dspace.mit.edu/handle/1721.1/169000)
- Content: a multimodal benchmark built on the Formula SAE rules document (about 70k tokens), CAD images and engineering drawings. Six subsets: Rule Retrieval (1,192), Compilation (30), Definition (31), Presence (62), Dimension (120), Functional Performance (16). Total 1,451. — [arXiv HTML](https://arxiv.org/html/2404.07917v2)
- Grading: automatic metrics. Retrieval uses F1 bag-of-words; compilation uses F1 over rule numbers; definition uses F1 bag-of-characters; presence uses accuracy; dimension and functional performance use accuracy plus BLEU-2/ROUGE-L/similarity on explanations. — [arXiv HTML](https://arxiv.org/html/2404.07917v2)
- Scores: GPT-4o with all rules in context scored retrieval F1 0.885, compilation 0.424, definition 0.540, presence 0.726, dimension 0.825 and functional performance 0.938. RAG variants were much worse on retrieval (GPT-4o-RAG 0.186). Other models tested: GPT-4, Claude-Opus, Gemini-1.0 and LLaVA-1.5. — [arXiv HTML](https://arxiv.org/html/2404.07917v2)
- Failure modes:
  - cannot reliably extract rules verbatim, even with the full document in context;
  - misidentifies CAD components (e.g. steering parts taken for an impact attenuator);
  - much weaker on **scale-bar-dimensioned drawings** than on directly dimensioned ones;
  - cannot follow chains of rule references across sections;
  - OCR and dimension-combination errors. — [arXiv HTML](https://arxiv.org/html/2404.07917v2)
- Code: [GitHub anniedoris/design_qa](https://github.com/anniedoris/design_qa/). Licence: arXiv non-exclusive (dataset licence not confirmed).

**MechVQA (mechanical drawing understanding), ICML 2026**
- Kou et al.; arXiv 2605.30794. 3.3k mechanical drawings (orthographic, isometric, part and assembly) taken from textbooks, handbooks and design platforms, with 21K QA pairs. Three capability levels (Recognition, Reasoning, Judging) covering 10 tasks. Built semi-automatically with QC. Their fine-tuned MechVL beats the strongest closed-source model by 7.57 pp. Models remain weak at spatial-relation reasoning under strict projection rules. — [arXiv](https://arxiv.org/abs/2605.30794)

**MechReason (multi-image, multi-hop mechanical engineering reasoning), 2026**
- Wang et al.; arXiv 2609.16012 (Aug 2026). 12,000 QA pairs with reasoning-chain annotations over 21,000 visual items: charts, tables, drawings, micrographs, simulation images, CAD images, flowcharts. All derived from real ME papers. Four dimensions: explanation, prediction, **design and diagnosis**. The pipeline masks shortcuts. Best model: 62.89%. — [arXiv](https://arxiv.org/abs/2609.16012)

**AECBench (architecture/engineering/construction, incl. building codes), 2025–26**
- Liang et al.; arXiv 2509.18776 (v2 Jan 2026). 4,800 questions, including open-ended ones, over 5 cognitive levels: memorisation, understanding, reasoning, calculation, application. 23 tasks "derived from authentic AEC practice", from code retrieval up to generating specialised documents. Written mainly by engineers and validated by a two-round expert review. Long-form answers are graded by an LLM judge against expert-derived rubrics. Nine LLMs tested; performance falls steadily across the levels. Specific deficits: **reading tables in building codes**, complex calculation and document generation. Licence CC BY-NC-ND 4.0. Judge-vs-expert agreement statistics were not visible in the abstract. — [arXiv](https://arxiv.org/abs/2509.18776v2)

**SysMBench (MBSE / SysML model generation), 2025**
- arXiv 2508.03215 (Wuhan University per alphaXiv). 151 human-curated scenarios, each with natural-language requirements, a reference SysML model and a diagram. Best scores: **BLEU 4%, SysMEval-F1 62%**. — [arXiv HTML](https://arxiv.org/html/2508.03215v1); [alphaXiv](https://alphaxiv.org/benchmarks/wuhan-university/sysmbench)

**SysEngBench (Naval Postgraduate School), 2024**
- 1,144 multiple-choice questions over 10 SE topic areas, drawn from lecture slides with human review. Evaluated only older open models (Llama 2, Mistral, Orca 2). Presented at the Acquisition Research Symposium, May 2024. Weak as a modern measure. — [NPS DAIR](https://dair.nps.edu/handle/123456789/5135); [NPS PDF](https://dair.nps.edu/bitstream/123456789/5135/1/SYM-AM-24-072.pdf)

#### C. Control engineering

**ControlBench (2024)**
- Kevian, Syed, Guo, Havens, Dullerud, Seiler, Qin, Hu (UIUC, U. Michigan, AI2, UCSD); arXiv 2404.03647. 147 undergraduate classical-control problems from a standard textbook plus UMich EECS 460 and UIUC ECE 486, covering 12 topics. 26 problems include plots or diagrams. — [arXiv HTML](https://arxiv.org/html/2404.03647v1)
- Grading: **a panel of human experts**. ACC is accuracy; ACC-s is accuracy after a self-check prompt. Claude 3 Opus 58.5% / 68.7%; GPT-4 45.6% / 47.6%; Gemini 1.0 Ultra 34.0% / 38.8%. Bode-plot problems reached only about 7–13%. Errors included symbol manipulation, misreading graphs, and GPT-4 reasoning gaps versus Claude calculation precision. Responses were sensitive to prompt wording. **ControlBench-C** is a 100-item multiple-choice version for automatic grading; the authors say it gives "less insight". — [arXiv HTML](https://arxiv.org/html/2404.03647v1); data: [agi4engineering.github.io/LLM4Control](https://agi4engineering.github.io/LLM4Control/)

**ControlAgent / ControlEval (2024)**
- arXiv 2410.19811. ControlEval has 500 control-design tasks: 10 task types × 50 systems, ranging from first/second-order stable and unstable systems to time-delay and higher-order systems. Specs combine closed-loop stability, settling time and phase margin, all automatically checkable. The ControlAgent pipeline (LLM + domain expertise + tools, iterative) reaches 82–100% average success rate by category and beats LLM-only and toolbox baselines. — [arXiv](https://arxiv.org/pdf/2410.19811); [arXiv HTML](https://arxiv.org/html/2410.19811v1)

#### D. Structural / mechanical / computational mechanics

**FEM-Bench (2025–26)**
- Mohammadzadeh, Hamdi, Shor, Lejeune; arXiv 2512.20732 (v2 May 2026); CC BY 4.0. Code-generation tasks aligned to a first graduate course in computational mechanics: writing FEM functions and writing unit tests, with objective verification. Over five attempts, Gemini 3 Pro solved 30/33 function tasks at least once and 26/33 in all five runs. GPT-5 led unit-test writing at 73.8% average joint success rate. Summary: "state-of-the-art LLMs do not reliably solve all of them". Search snippets also report struggles with geometric nonlinearity and eigenvalue buckling. — [arXiv](https://arxiv.org/abs/2512.20732)

**FEABench (Google Research + Harvard), 2024–25** — cross-reference (software-operation agents are covered elsewhere)
- arXiv 2504.06260; NeurIPS 2024 workshop. **Requires COMSOL Multiphysics (proprietary).** FEABench Gold has 15 manually verified problems from COMSOL tutorials; FEABench Large has 200 algorithmically parsed problems. The best strategy produced executable API calls 88% of the time, but no LLM or agent fully and correctly solved any problem. — [Google Research](https://research.google/pubs/feabench-evaluating-language-models-on-real-world-physics-reasoning-ability/); [arXiv](https://arxiv.org/abs/2504.06260v1)

**StructureClaw-Bench (2026)** — cross-reference
- arXiv 2607.14896. 150 executable structural-engineering scenarios. A scenario passes only if **all artefact- and execution-level assertions pass in a single run**: requirements → model → validation → solver → code-check → report. Ten agent-model configurations on 50 standard cases averaged 56.8% success with a generic-skill baseline and 88.6% with the full workflow. Open problems: safe handling of invalid numerical inputs, and reconstructing structural models from multimodal input. The authors argue QA-centred evaluations "may therefore reward fluent outputs even when the underlying engineering workflow is incomplete, internally inconsistent, or non-executable". — [arXiv](https://arxiv.org/abs/2607.14896); [HF papers](https://huggingface.co/papers/2607.14896)
- OpenSeesAgentBench (agentic structural analysis in OpenSeesPy, ICML 2026): existence verified only. — [ICML 2026](https://icml.cc/virtual/2026/73566)

#### E. Thermal / fluids

**ThermoQA (2026)**
- Düzkar (Olivenet, Cyprus; single author, preprint); arXiv 2604.19758. 293 open-ended engineering-thermodynamics questions:
  - Tier 1: property lookups (110);
  - Tier 2: component analysis of turbines, compressors, pumps and heat exchangers at three depths — energy, +entropy, +exergy (101);
  - Tier 3: cycle analysis of Rankine, Brayton, refrigeration and combined-cycle systems (82).
  - Fluids are water, R-134a and variable-cp air. — [arXiv HTML](https://arxiv.org/html/2604.19758v1)
- **Ground truth is computed programmatically**: CoolProp 7.2.0 / IAPWS-IF97 (<0.037% vs NIST), a Helmholtz EOS for R-134a with an explicit IIR reference state, and NASA 7-coefficient polynomials for air. Tier 1 tolerance is ±2% relative or ±0.5 absolute. Tiers 2–3 use **step-level weighted scoring** (weights 1–6, with final answers at 4–6). Answers are extracted with regex plus a GPT-4.1-mini pass, keeping the higher of the two. — [arXiv HTML](https://arxiv.org/html/2604.19758v1)
- Composite scores: Claude Opus 4.6 94.1%, GPT-5.4 93.1%, Gemini 3.1 Pro 92.5%, DeepSeek-R1 87.4%, Grok 4 87.3%, MiniMax M2.5 73.0%. Already **near-saturated for frontier models**, but supercritical water (44.5 pp spread), R-134a (44–63% vs 75–98% for water) and combined cycles (31–90%) still separate models. Run-to-run standard deviation varies 25× across models. CC-BY-4.0 ([GitHub](https://github.com/olivenet-iot/ThermoQA)). — [arXiv HTML](https://arxiv.org/html/2604.19758v1)

**UTQA ("From Canonical to Complex: Benchmarking LLM Capabilities in Undergraduate Thermodynamics"), 2025**
- Geißler, Bien, Schöppler, Hertel; arXiv 2508.21452. 50 items covering ideal-gas processes, reversibility and diagram interpretation. The authors set a 95% competence threshold. The best LLMs reached 82%, and visual items often fell to chance. Weak on finite-rate/irreversible processes. Conclusion: "not yet suitable for unsupervised tutoring". — [arXiv](https://arxiv.org/abs/2508.21452)

- No general-purpose fluid-mechanics or heat-transfer LLM QA benchmark was found. The hypersonic TPS-CalcBench (arXiv 2604.17966) is aerospace and covered elsewhere. — [arXiv](https://arxiv.org/abs/2604.17966)

#### F. Materials

**MatSciBench (2025)**
- arXiv 2510.12171 (UCLA per alphaXiv). 1,340 college-level problems across 6 fields and 31 sub-fields, with 3 difficulty tiers set by reasoning length. Includes reference solutions and multimodal items. The best model, Gemini-2.5-Pro, scored under 80%. No single strategy (CoT, tools, self-correction) won everywhere. — [arXiv HTML](https://arxiv.org/html/2510.12171v1)

**MaScQA [2023/24]**
- 650 GATE materials/metallurgical-engineering exam questions (Digital Discovery 2024). GPT-4 scored about 62% with CoT. A later 15-LLM study (arXiv 2501.04277) found Claude-3.5-Sonnet and GPT-4o at about 84%, Llama3-70B at about 56% and Phi3-14B at about 43%. Exam-based and likely approaching saturation. — [RSC Digital Discovery](https://pubs.rsc.org/en/content/articlehtml/2024/dd/d3dd00188a); [arXiv 2501.04277](https://arxiv.org/html/2501.04277v1)
- MATRIX (stress-testing LLM reasoning in materials science, ICLR 2026): existence only. — [ICLR](https://iclr.cc/virtual/2026/10019956)
- LLaMat (materials foundation-model evaluation): not verified in this session (see Gaps).

#### G. Electrical / electronics / hardware design analogues

**EEE-Bench (CVPR 2025)**
- Li, Zhong, Chen, Lai, Psounis; arXiv 2411.01492. 2,860 hand-curated multiple-choice and free-form problems across 10 subdomains (analog circuits, control systems, etc.). 17 LLMs/LMMs scored 19.48–46.78% on average. Identified "**laziness**": models rely on the text and overlook the visual context in technical images. — [arXiv](https://arxiv.org/abs/2411.01492); [CVF](https://openaccess.thecvf.com/content/CVPR2025/html/Li_EEE-Bench_A_Comprehensive_Multimodal_Electrical_And_Electronics_Engineering_Benchmark_CVPR_2025_paper.html)

**ElecBench (power-system dispatch), 2024**
- arXiv 2407.05365. Six metrics (factuality, logicality, stability, security, fairness, expressiveness) with 24 sub-metrics. Subsets: general, monitoring, dispatch and black-start. Eight LLMs evaluated; test set public. A search result reports a Best Paper award at IEEE PES GM 2025, which was not verified at the primary source. — [arXiv](https://arxiv.org/abs/2407.05365)

**AnalogCoder (AAAI 2025)**
- Introduced a 24-circuit analog design benchmark with task descriptions, sample designs and testbenches. The AnalogCoder agent solved 20/24; GPT-4o solved 15/24. — [AAAI](https://ojs.aaai.org/index.php/AAAI/article/view/32016/34171)

**VerilogEval [2023; revisited 2025]**
- 156 HDLBits problems graded by functional simulation. In "Revisiting VerilogEval" (ACM TODAES 2025), GPT-4o scored 63% on spec-to-RTL, Llama 3.1 405B 58% and RTL-Coder 6.7B 34%. Problems are short and self-contained, and probably contaminated because HDLBits is public. — [arXiv 2408.11053](https://arxiv.org/html/2408.11053v2); [Cornell PDF](https://www.csl.cornell.edu/~cbatten/pdfs/pinckney-revisiting-verilogeval-todaes2025.pdf)

**CVDP — Comprehensive Verilog Design Problems (NVIDIA), 2025**
- arXiv 2506.14074. 783 problems in 13 categories (RTL generation, verification, debugging, spec alignment, technical Q&A), **authored by experienced hardware engineers**, in non-agentic and agentic (multi-turn, tool-using) formats. State-of-the-art models score ≤34% pass@1 on code generation. Agentic RTL-reuse and verification tasks are hardest. — [arXiv](https://arxiv.org/abs/2506.14074)

#### H. Chemical / process engineering
- **No dedicated, validated chemical-engineering LLM benchmark was found.** One paper states that ChemLLMBench, SciBench and ChemEval cover chemical *sciences* but not industrial-scale chemical-engineering challenges. Recent P&ID and flowsheet work (P&ID→process graph, ChatP&ID, Sketch2Simulation, DEXPI greenfield design) consists of system papers with small evaluations, not benchmarks. — [arXiv 2509.07034](https://arxiv.org/html/2509.07034v1); [arXiv 2607.19568](https://arxiv.org/pdf/2607.19568); [arXiv 2603.24629](https://arxiv.org/pdf/2603.24629); [arXiv 2609.12656](https://arxiv.org/pdf/2609.12656)

### Inferences
- The benchmarks split into two groups:
  - **Knowledge/analysis QA** (EEE-Bench, MatSciBench, MaScQA, AECBench, ThermoQA, ControlBench). Numeric or multiple-choice answers are easy to grade, but the newer ones already saturate (ThermoQA about 94%, MaScQA about 84%).
  - **Design/workflow benchmarks with executable verification** (EngDesign, ControlEval, VEHBench, ORAgentBench, CVDP, StructureClaw, FEM-Bench). These stay hard (20–35% for open design) and are closest to what AerospaceBench wants.
- One group (UIUC agi4engineering: ControlBench → ControlAgent → EngDesign) produced a large share of the design-benchmark lineage. Their path ran from expert-graded QA to auto-checkable design specs, which suggests scalable grading needs specs that can be checked by a simulator or analytic oracle.
- Dependence on licensed tools (MATLAB, Cadence, COMSOL) is a recurring pain point. EngDesign had to split off an "Open" subset and FEABench is COMSOL-only. Choosing open solvers (CoolProp, SPICE/ngspice, OpenSees, Python control) makes a benchmark reproducible.
- Drawings and diagrams are a consistent weak spot (DesignQA scale bars, ControlBench Bode plots, EEE-Bench laziness, UTQA visuals, MechVQA projections). Aerospace work is heavy on drawings, plots and charts, so this is a fertile area for difficulty.

### Gaps
- No re-runs of EngDesign, ControlBench, DesignQA or EEE-Bench on 2026 frontier models (GPT-5.x, Claude Opus 4.x, Gemini 3.x) were found. The best known scores are mostly 2024–2025 vintage.
- Named-model scores for VEHBench, MechVQA, AECBench and SysMBench were not extracted from the abstracts.
- Dataset licences were not confirmed for EngDesign, DesignQA, VEHBench, MechReason and ControlBench.
- LLaMat (materials LLM) evaluations were not verified in this session.
- No dedicated chemical-engineering, general fluid-mechanics/heat-transfer or industrial-engineering (other than OR) benchmark of quality was found.
- EngiBench level sub-counts were extracted ambiguously (916/334/467 vs 43 L3); check the paper.

---

## 2. Science/engineering reasoning sets used as proxies

### Takeaway
Exam and olympiad sets are frequently used as proxies: SciBench, TheoremQA, JEEBench, OlympiadBench, UGPhysics, PHYBench, MMMU (Tech & Engineering), GPQA, HLE, and FE-exam studies. They measure closed-form, well-posed reasoning with a single answer. Older sets are saturated or close to it (GPQA Diamond about 95% per aggregators; FE exam about 75% for GPT-4 already in 2023). None tests model selection, under-specification, design trade-offs or verification. SciCode shows that defects in a benchmark can understate capability by large margins.

### Cited Findings
- **SciCode** (scientific coding across physics/chemistry/biology/math subproblems): on the original benchmark, 2026 frontier models scored 9–27% on main problems and 45–60% on subproblems, an apparent plateau. **SciCode-Verified** (Hu, Huang, Deng, Chen; arXiv 2608.04975, Aug 2026) found 263 defects across the 65 test problems. 192 of them, affecting 91% of main problems, wrongly reject correct solutions: non-reproducible gold answers, **overly tight numerical tolerances** and self-contradictory specifications. 78% of these defects needed specialist physics or maths knowledge to detect. After correction, main-problem accuracy rose to 69–92% and subproblem accuracy to 84–98%; "GPT-5.6 Sol" reached 92.2% main and 98.3% sub. Quote: "the bottleneck was not model capability, but the quality of the evaluation instrument". — [arXiv](https://arxiv.org/abs/2608.04975)
- **SciBench** [2023; ICML 2024]: college-level maths, chemistry and physics textbook problems; best overall score 43.22% at release. User-study error taxonomy of 10 problem-solving skills. Likely far exceeded by reasoning models (no current figure verified). — [arXiv](https://arxiv.org/abs/2307.10635)
- **TheoremQA** [2023]: 800 questions covering 350 theorems from maths, physics, EE&CS and finance; GPT-4 scored 51% with Program-of-Thoughts. — [arXiv](https://arxiv.org/abs/2305.12524)
- **JEEBench** [2023]: 515 IIT JEE-Advanced pre-engineering maths, physics and chemistry problems; best GPT-4 below 40%. Errors: algebraic manipulation, turning concepts into formulations, retrieving domain knowledge. — [arXiv](https://arxiv.org/abs/2305.15074)
- **OlympiadBench** [2024, ACL]: 8,476 olympiad maths and physics problems; GPT-4V averaged 17.97% at release. — [arXiv PDF](https://web3.arxiv.org/pdf/2402.14008)
- **UGPhysics** (ICML 2025): 5,520 bilingual undergraduate physics problems (mechanics & thermodynamics, E&M, modern physics), with a Model-Assistant Rule-based Judgment (MARJ) grader. Best at release: o1-mini 49.8%. — [arXiv](https://arxiv.org/pdf/2502.00334); [PMLR](https://proceedings.mlr.press/v267/xu25ai.html)
- **PHYBench** (NeurIPS 2025 D&B): 500 real-scenario physics problems; Gemini 2.5 Pro 36.9% vs human experts 61.9%. — [NeurIPS](https://proceedings.neurips.cc/paper_files/paper/2025/hash/01e793f8cf5689aa0ea46a1b01071bea-Abstract-Datasets_and_Benchmarks_Track.html)
- **MMMU** [2023; CVPR 2024]: 11.5K college-level multimodal questions in 6 disciplines including **Tech & Engineering**, 30 subjects and 183 subfields. GPT-4V 56%, Gemini Ultra 59% at release. — [arXiv](https://arxiv.org/abs/2311.16502)
- **GPQA Diamond** [2023]: 198 graduate biology/physics/chemistry multiple-choice questions. Aggregators report it is effectively saturated in 2026 (top scores about 94.6–97%). These figures come from secondary aggregator sites, not Epoch AI directly. — [capitalandcompute](https://capitalandcompute.net/ai-benchmarks/gpqa-diamond/); [benchlm](https://benchlm.ai/benchmarks/gpqaDiamond)
- **Humanity's Last Exam**: engineering is only about 4% of questions; no per-category engineering accuracy was found. — [Wikipedia](https://en.wikipedia.org/wiki/Humanity%27s_Last_Exam)
- **FE exam studies:** on the US FE Environmental exam, GPT-4 scored 66.42%, rising to 75.37% with refined prompts ("reasonably considered a passing grade"), with weaker groundwater and hydrology sections. — [arXiv 2304.12198](https://arxiv.org/abs/2304.12198v1). On FE Mechanical-style questions, GPT-4 scored 76% vs GPT-3.5 at 51%, but text-only input made passing unlikely. — [arXiv 2309.15866](https://arxiv.org/abs/2309.15866v1)
- **Saturation context:** a 2025 study found nearly all reasoning benchmarks released before 2025 have been surpassed by at least one model family; 60% of unsolved benchmarks date from 2025. — [arXiv 2511.01365](https://arxiv.org/pdf/2511.01365)

### Inferences
- The proxies measure a necessary but far from sufficient skill: well-posed calculation and knowledge recall. A model can pass the FE exam (about 75% in 2023) and still fail about two-thirds of EngDesign design tasks (2025), and fail every FEABench problem. Exam competence does not imply design competence.
- Multiple-choice and exam-sourced sets (JEEBench, MaScQA, FE, GPQA) carry contamination risk and saturate within 1–2 years. They are fine as a calibration floor but should not be the core of AerospaceBench.
- SciCode-Verified is a warning in the opposite direction: hard-looking scores may come from defects (tight tolerances, wrong gold answers), not model weakness.

### Gaps
- No verified 2026 scores for SciBench, TheoremQA, JEEBench, OlympiadBench or MMMU Tech & Engineering were found. They are believed saturated or near-saturated for reasoning models, but this is unconfirmed.
- No verified per-category (engineering) breakdown for HLE.
- No verified PE-exam (Professional Engineer) LLM study was found in this pass.

---

## 3. Grading approaches for open-ended engineering outputs

### Takeaway
The most credible design benchmarks grade by **executing the proposed design**: simulation (SPICE, FEA, MATLAB control), analytical physics oracles, hidden feasibility validators, testbenches or unit tests. They report pass/fail plus partial credit. Numeric answers use relative-tolerance bands, preferably checked against programmatically computed ground truth. Expert rubric grading is used for open-ended modelling but does not scale. EngiBench L3 is limited to 43 tasks and ControlBench needed a multiple-choice spin-off for automation. LLM-as-judge with expert rubrics is used (AECBench), but engineering-specific judge-validation statistics are scarce. Text-similarity metrics (BLEU/ROUGE) are poor proxies.

### Cited Findings
- **Simulation-based functional verification**: EngDesign attaches an executable pipeline (SPICE, structural FEA, MATLAB Control System Toolbox, Cadence) to every task and outputs pass/fail, a 0–100 score and logs. This moves evaluation "from linguistic pattern matching to functional verification". — [arXiv HTML](https://arxiv.org/html/2509.16204v2); [NeurIPS](https://proceedings.neurips.cc/paper_files/paper/2025/hash/664f777548205fb6e0cbb0965e8d2e16-Abstract-Datasets_and_Benchmarks_Track.html)
- **Spec-based design checks**: ControlEval checks closed-loop stability, settling time and phase margin automatically. — [arXiv](https://arxiv.org/pdf/2410.19811)
- **Analytical physics oracle**: VEHBench scores harvester-design decisions with an analytical oracle, including "corrupted-state recovery" tasks. — [arXiv](https://arxiv.org/abs/2607.18181)
- **Hidden validators for feasibility + objective quality**: ORAgentBench checks schema validity, hard-constraint feasibility and normalised objective. — [arXiv](https://arxiv.org/abs/2606.19787)
- **All-assertions-in-one-run artefact chains**: StructureClaw-Bench passes a scenario only if every artefact-level and execution-level assertion holds. — [arXiv](https://arxiv.org/abs/2607.14896)
- **Unit tests / testbenches**: FEM-Bench (function correctness plus model-written unit tests), VerilogEval/CVDP (simulation testbenches), AnalogCoder (testbenches). — [FEM-Bench](https://arxiv.org/abs/2512.20732); [CVDP](https://arxiv.org/abs/2506.14074); [AnalogCoder](https://ojs.aaai.org/index.php/AAAI/article/view/32016/34171)
- **Programmatic ground truth + tolerance bands + step weighting**: ThermoQA computes answers with CoolProp/IAPWS and NASA polynomials, uses ±2% relative or ±0.5 absolute tolerance, and weights intermediate steps by engineering importance (final answers weighted 4–6). Answers are extracted by regex plus LLM, keeping the higher score. — [arXiv HTML](https://arxiv.org/html/2604.19758v1). EngiBench uses ±2% tolerance with LLM-assisted answer matching. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- **Tolerances that are too tight, or gold answers that are wrong, distort results**: SciCode-Verified found 192 score-suppressing defects in 91% of main problems; correcting them raised main-problem accuracy from 9–27% to 69–92%. — [arXiv](https://arxiv.org/abs/2608.04975)
- **Expert human grading**: ControlBench uses a panel of experts and also offers ControlBench-C (100 multiple-choice items) because full grading needs control expertise. — [arXiv HTML](https://arxiv.org/html/2404.03647v1). EngiBench L3 uses PhD graders with official competition rubrics on 4 dimensions (0–10). — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- **LLM-as-judge with expert rubrics**: AECBench grades long-form AEC answers this way. Agreement statistics were not visible in the abstract. — [arXiv](https://arxiv.org/abs/2509.18776v2). Outside engineering, PRBench (professional reasoning) reported judge–expert Cohen's κ of 0.535–0.605, on par with expert–expert agreement. Cross-reference only; this is methodology literature covered elsewhere. — [arXiv 2511.11562](https://arxiv.org/pdf/2511.11562)
- **Text-similarity metrics**: DesignQA grades explanations with BLEU-2/ROUGE-L/similarity and rule retrieval with bag-of-words F1. — [arXiv HTML](https://arxiv.org/html/2404.07917v2). SysMBench shows how far surface metrics can diverge from a structural one: best BLEU 4% vs SysMEval-F1 62%. — [arXiv HTML](https://arxiv.org/html/2508.03215v1)
- **Robustness via controlled variants**: EngiBench's perturbed, knowledge-enhanced and math-abstraction rewrites separate memorisation, knowledge gaps and maths skill. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- **Repeated sampling / consistency**: ThermoQA found run-to-run σ varies 25× across models (GPT-5.4 ±0.1% vs DeepSeek-R1 ±2.5%), and says single-run evaluations hide this. — [arXiv HTML](https://arxiv.org/html/2604.19758v1). A 100-trial study of GPT-5.1 at T=1.0 found some questions answered wrongly in 100/100 trials (deterministic confident failure) and others split across 2–3 options. — [ASEE 2026 (Deutsch & Oca, Duke)](https://peer.asee.org/evaluating-engineering-students-detection-of-hallucinations-in-large-language-model-solutions.pdf)

### Inferences
- What worked:
  - executable verification against requirements, with partial credit;
  - programmatic ground truth from trusted property libraries;
  - relative-tolerance bands that match engineering precision;
  - step-weighted scoring, so intermediate states can be diagnosed;
  - controlled perturbation variants;
  - multi-run reporting.
- What did not work well:
  - BLEU/ROUGE for engineering explanations;
  - multiple-choice simplifications (they lose insight);
  - single-run point estimates;
  - tolerances and gold answers that experts never audited (SciCode).
- A hybrid seems most defensible for AerospaceBench:
  - simulator/oracle checks on the deliverable's numbers and feasibility;
  - expert-written rubrics for reasoning quality (assumption statements, model-selection justification, verification steps);
  - an LLM judge only after measured agreement with engineers.

### Gaps
- No engineering-specific study reporting LLM-judge vs engineer agreement (κ) was found.
- No benchmark was found that systematically grades **unit/dimension consistency** as a separate check, or rewards **explicit uncertainty quantification**.

---

## 4. Documented LLM failure modes on engineering tasks

### Takeaway
The best-documented failure modes are:
- insufficient domain knowledge and **constraint violations** (together about 55–67% of design failures);
- over-reliance on memorised "textbook" methods even when told otherwise;
- hallucinated equations, parameters and data;
- **sign-convention and reference-state errors**;
- error cascades from one bad intermediate value;
- failure to read plots, drawings and code tables;
- brittleness to small perturbations;
- missing trade-off and uncertainty analysis in open-ended problems;
- confident, deterministic wrong answers.

Failure to self-verify is partly offset by tool or simulator feedback loops.

### Cited Findings
- **Domain knowledge errors and constraint violations dominate.** EngDesign: domain knowledge error 30–33%, constraint violation 25–36%, prior-knowledge over-reliance 11–19%, hallucination 12–13%, computation 6–9%. Analog IC design scored 0% for all models. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
- **Method rigidity / prior-knowledge over-reliance.** ThermoQA: models apply constant-cp isentropic relations despite an explicit "variable specific heats" instruction. — [arXiv HTML](https://arxiv.org/html/2604.19758v1)
- **Sign conventions and reference states.** ThermoQA: the compressor isentropic-efficiency formula is inverted (multiply vs divide), making compressors the hardest component at 55–75%. Models confuse the IIR and ASHRAE R-134a reference states. — [arXiv HTML](https://arxiv.org/html/2604.19758v1)
- **Error cascading.** ThermoQA: the correlation between an isentropic exit-state error and downstream errors is φ=0.94, so one property error propagates through a whole cycle. Naive interpolation near the critical point gives about 27% enthalpy errors. Models also skip steps entirely. — [arXiv HTML](https://arxiv.org/html/2604.19758v1)
- **Robustness / pattern-matching.** EngiBench perturbations cut L2 accuracy by 8–11%. — [arXiv HTML](https://arxiv.org/html/2509.17677v1). In ControlBench, small wording changes gave drastically different answers. — [arXiv HTML](https://arxiv.org/html/2404.03647v1)
- **No trade-off or uncertainty reasoning in open-ended problems.** EngiBench L3: models were weakest on domain reasoning and uncertainty. Llama 4 scored 0 on multi-objective decision-making because it gave no trade-off analysis. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- **Implicit constraints and infeasibility detection.** ORAgentBench: missed operational rules, brittle formulations, weak feasible-solution construction. — [arXiv](https://arxiv.org/abs/2606.19787). MolDesignBench (snippet only): implicit-constraint interpretation and infeasibility detection are the main bottlenecks. — [arXiv](https://arxiv.org/abs/2609.27349v1). StructureClaw: unsafe handling of invalid numerical inputs. — [arXiv](https://arxiv.org/abs/2607.14896)
- **Diagrams, plots and drawings:**
  - ControlBench Bode problems about 7–13%. — [arXiv HTML](https://arxiv.org/html/2404.03647v1)
  - EEE-Bench "laziness" (ignoring the image). — [arXiv](https://arxiv.org/abs/2411.01492)
  - DesignQA: scale-bar drawings, CAD component misidentification, OCR and dimension-combination errors. — [arXiv HTML](https://arxiv.org/html/2404.07917v2)
  - UTQA: visual items near chance. — [arXiv](https://arxiv.org/abs/2508.21452)
  - MechVQA: spatial reasoning under projection rules. — [arXiv](https://arxiv.org/abs/2605.30794)
- **Standards and code documents:** DesignQA models cannot reliably retrieve rules or follow cross-references even with the full document in context. — [arXiv HTML](https://arxiv.org/html/2404.07917v2). AECBench models are weak at interpreting **tables in building codes** and at generating domain documents. — [arXiv](https://arxiv.org/abs/2509.18776v2)
- **Hallucinated data/calculations in professional deliverables:** GDPval graders cited data hallucination and calculation mistakes across all models, along with instruction-following and formatting failures and misuse of reference files. — [arXiv HTML](https://arxiv.org/html/2510.04374v1)
- **Statics/mechanics/circuits (secondary, via a literature review):**
  - GPT-4 resolves angled forces incorrectly, misclassifies truss members as tension or compression, and adds nonexistent forces to free-body diagrams.
  - Repeated basic-mechanics trials show average errors above 10% in some cases.
  - In a control course, GPT-4 passed structured assessments but did much worse on open-ended design projects and invented overly specific numbers.
  - Circuit analysis is weak on multi-loop and diagram-dependent problems.
  - Primary sources were not individually verified. — [ASEE 2026, Deutsch & Oca](https://peer.asee.org/evaluating-engineering-students-detection-of-hallucinations-in-large-language-model-solutions.pdf)
- **Confident deterministic errors:**
  - Force Concept Inventory subset: GPT-5.1 scored 76.25% per-trial accuracy. Three questions were never answered correctly, and two were answered with the same wrong option in 100/100 trials.
  - Conceptual Survey of Electricity and Magnetism subset: 57.12%.
  - Students (n=38) rejected the two hallucinated solutions at 94.7% and 89.5%. Higher AI-use frequency was significantly correlated with *lower* hallucination-detection accuracy. — [ASEE 2026](https://peer.asee.org/evaluating-engineering-students-detection-of-hallucinations-in-large-language-model-solutions.pdf)
- **Computation precision:** ControlBench: GPT-4's main weakness was reasoning, Claude 3 Opus's was calculation precision. Self-checking raised Claude from 58.5% to 68.7%. — [arXiv HTML](https://arxiv.org/html/2404.03647v1)
- **Simulation tools:** FEABench: 88% executable API calls, but no problem fully solved. — [Google Research](https://research.google/pubs/feabench-evaluating-language-models-on-real-world-physics-reasoning-ability/). FEM-Bench: even simple graduate-level FEM tasks are not solved reliably. — [arXiv](https://arxiv.org/abs/2512.20732)
- **Iteration helps where verifiers exist:**
  - EngDesign o3: about 34% → about 60% with 10 rounds of simulator feedback, but no gain on analog IC. — [arXiv HTML](https://arxiv.org/html/2509.16204v2)
  - StructureClaw: 56.8% → 88.6% with a governed workflow. — [arXiv](https://arxiv.org/abs/2607.14896)
  - ControlAgent: 82–100% with tool-in-the-loop design. — [arXiv](https://arxiv.org/pdf/2410.19811)

### Inferences
- Many failures are "silent" (reference-state mix-ups, inverted efficiency, a wrong exit state that cascades). Final-answer-only grading would miss *why* a model failed. Step-level or intermediate-state checks, as in ThermoQA, make the benchmark diagnostic.
- "Failure to verify" shows up indirectly: large gains from external verifiers (EngDesign, StructureClaw, ControlAgent) imply models do not verify enough on their own. AerospaceBench could measure **self-verification** directly by giving credit for sanity checks, limit cases and dimensional checks, or by including tasks with a planted inconsistency or infeasible requirement.

### Gaps
- No engineering benchmark was found that isolates **unit/dimension errors** as a measured category. They are discussed qualitatively (e.g. a [tianpan.co blog](https://tianpan.co/blog/2026-05-06-llm-confabulation-technical-scientific-domains), low-authority source) but not quantified.
- No study was found that quantifies **hallucinated standards or clause numbers** (e.g. invented ASTM/ISO/Eurocode references), although DesignQA and AECBench show weak retrieval and table reading.
- Overconfidence and calibration have not been measured directly in engineering design (only via MC repeat-sampling in the ASEE study).
- No benchmark tests **under-specified real problems** where the model must ask for or state missing information. EngiBench L3 and VEHBench "specification triage" come closest.

---

## 5. Do engineering benchmark scores correlate with real engineering usefulness?

### Takeaway
Direct evidence is thin. No practitioner study was found that correlates engineering-benchmark scores with on-the-job usefulness. Indirect evidence points one way:
- exam-style scores (FE about 75%, ThermoQA about 94%) far exceed design-task scores (EngDesign about 34%, ORAgentBench about 36%, CVDP ≤34%);
- human experts still beat the best models on open-ended modelling (EngiBench L3: 8.58 vs about 7.0/10);
- GDPval includes mechanical and industrial engineers, but engineering-specific results were not found.

### Cited Findings
- GDPval covers 44 occupations, including **Mechanical Engineers** and **Industrial Engineers** in the Manufacturing sector. Tasks include CAD files and detailed drawings with precise measurements. Overall, the best model in the v1 paper (Claude Opus 4.1) had 47.6% of deliverables rated as good as or better than the expert's. Loss reasons were instruction-following, formatting, data hallucination and calculation errors. **No per-occupation engineering result was found.** Cross-reference: GDPval is covered by another researcher. — [arXiv HTML](https://arxiv.org/html/2510.04374v1); [Epoch AI](https://epoch.ai/benchmarks/gdpval)
- EngiBench L3 human experts 8.58/10 vs best models about 7.0/10. — [arXiv HTML](https://arxiv.org/html/2509.17677v1)
- UTQA: best 82% vs the authors' 95% competence threshold; "not yet suitable for unsupervised tutoring". — [arXiv](https://arxiv.org/abs/2508.21452)
- In a control course, GPT-4 passed structured assessments but did much worse on open-ended design projects (secondary citation). — [ASEE 2026](https://peer.asee.org/evaluating-engineering-students-detection-of-hallucinations-in-large-language-model-solutions.pdf)
- Engineering students' detection of LLM errors falls as their AI use rises, which shows the human-oversight assumption behind "usefulness" is fragile. — [ASEE 2026](https://peer.asee.org/evaluating-engineering-students-detection-of-hallucinations-in-large-language-model-solutions.pdf)
- Benchmark validity can fail in either direction. SciCode-Verified shows defects *understated* capability by about 40–65 pp on main problems. — [arXiv](https://arxiv.org/abs/2608.04975). StructureClaw argues QA evaluations can *overstate* capability by rewarding fluent but non-executable workflows. — [arXiv](https://arxiv.org/abs/2607.14896)

### Inferences
- The size of the gap between exam-style and design-style scores is the best available signal. Benchmarks with closed-form answers mostly measure the "calculator/textbook" layer of engineering. Usefulness for real design work tracks better with executable-design benchmarks, but this has not been validated against practitioner outcomes.

### Gaps
- No practitioner field study, A/B productivity trial or engineer-rated deployment study correlated with benchmark scores was found for any engineering discipline.
- GDPval per-occupation results for Mechanical and Industrial Engineers were not located in the sources retrieved.

---

## 6. Lessons for AerospaceBench

### Takeaway
Copy the *executable-verification* pattern (EngDesign/ControlEval/ORAgentBench/StructureClaw), *programmatic ground truth with tolerance bands and step weights* (ThermoQA), and *controlled variants* (EngiBench). Use expert-sourced, expert-reviewed tasks (EngDesign, CVDP, AECBench). Avoid proprietary-tool lock-in, BLEU-style grading, unaudited gold answers and tolerances, exam-sourced items, and single-run reporting. Still unmeasured anywhere: model selection and justification of fidelity, under-specified requirements, uncertainty/margin reasoning, verification behaviour, multi-discipline system integration, and standards/regulation-aware design using real documents.

### Cited Findings (design patterns grounded in the catalogue)
- **Executable verification with partial credit** (pass/fail + 0–100 + logs). — [EngDesign](https://arxiv.org/html/2509.16204v2)
- **Open-tool subset** to avoid licence barriers (EngDesign-Open: 67 of 101 tasks). FEABench is COMSOL-only. — [EngDesign](https://arxiv.org/html/2509.16204v2); [FEABench](https://arxiv.org/abs/2504.06260v1)
- **Programmatic ground truth** from validated property libraries, explicit reference states, tolerance bands and step weights. — [ThermoQA](https://arxiv.org/html/2604.19758v1)
- **Controlled variants** (perturbed / knowledge-supplied / math-only) to separate failure sources and blunt contamination. — [EngiBench](https://arxiv.org/html/2509.17677v1)
- **Workflow-stage-local tasks** (specification triage, verifier-guided search, corrupted-state recovery, policy-conditioned selection) to show where in the design loop models fail. — [VEHBench](https://arxiv.org/abs/2607.18181)
- **Hidden validators for feasibility + objective**. — [ORAgentBench](https://arxiv.org/abs/2606.19787)
- **All-artefacts-must-pass evidence chains** (requirements → model → checks → report). — [StructureClaw](https://arxiv.org/abs/2607.14896)
- **Expert-authored tasks plus multi-stage expert review**. — [EngDesign](https://arxiv.org/html/2509.16204v2); [CVDP](https://arxiv.org/abs/2506.14074); [AECBench](https://arxiv.org/abs/2509.18776v2)
- **Real engineering documents and drawings** (rules, CAD, scale-bar drawings) to test standards-driven design. — [DesignQA](https://arxiv.org/html/2404.07917v2)
- **Repeat runs and variance reporting**. — [ThermoQA](https://arxiv.org/html/2604.19758v1)
- **Expert audit of tolerances and gold answers before release**. — [SciCode-Verified](https://arxiv.org/abs/2608.04975)

### Inferences (pitfalls to avoid)
- Don't source items from textbooks or exams (FE, JEE, GATE, HDLBits) as the core. They saturate quickly (MaScQA about 84%, ThermoQA about 94%, GPQA about 95%) and are likely contaminated.
- Don't grade engineering reasoning with BLEU/ROUGE/F1-bag-of-words (DesignQA; SysMBench BLEU 4% vs F1 62%).
- Don't depend on licensed tools for core grading (MATLAB, Cadence, COMSOL). Ship open evaluators (e.g. CoolProp, OpenSees, SU2/OpenFOAM-class solvers, Python control/aero libraries) or precomputed oracle tables.
- Don't rely on one simplified MC proxy for a hard expert-graded set (ControlBench-C "less insight").
- Don't report single runs. Don't use tolerances tighter than engineering practice. Don't release without an independent expert audit (SciCode lost about 40–65 pp to defects).
- Uneven domain coverage driven by whoever contributed tasks (EngDesign) leads to lopsided domain scores. Plan coverage deliberately.
- If tasks always give enough information, the benchmark rewards calculation rather than engineering. Include deliberately under-specified or infeasible requirements.

### Inferences (work types still unmeasured by any general engineering benchmark found)
1. **Model / fidelity selection with justification** (e.g. choosing between panel method, RANS, or empirical correlation and stating validity limits). No benchmark grades this explicitly.
2. **Under-specified or conflicting requirements**: asking clarifying questions, stating assumptions, detecting infeasibility. Only partially covered (VEHBench triage, ORAgentBench feasibility, MolDesignBench).
3. **Uncertainty, margins and safety factors**: propagating input uncertainty, choosing margins, reliability reasoning. Not graded anywhere found.
4. **Verification and validation behaviour**: sanity checks, limit cases, dimensional consistency, cross-checking against independent methods or test data. Measured only indirectly through gains from external verifiers.
5. **Diagnosis / anomaly investigation from test or flight-like data**: MechReason "diagnosis" and VEHBench "corrupted-state recovery" are the only near-analogues.
6. **Multi-disciplinary system integration and trade studies** (mass/power/thermal budgets, interface control). EngiBench L3 shows trade-off omission but has only 43 tasks, and SysMBench covers only model syntax.
7. **Standards- and regulation-driven design** using real documents and tables (code tables are a known weak spot in AECBench; rule retrieval in DesignQA).
8. **Calibration/overconfidence** on engineering judgements, and **hallucinated standard or clause citations**. Neither is quantified in any engineering benchmark found.

### Gaps
- No verified evidence on how these lessons transfer to aerospace specifically. Cross-check with the aerospace-specific and tool-operating-agent research notes. Examples: TPS-CalcBench already uses graded difficulty levels L1–L4 for hypersonics ([arXiv](https://arxiv.org/abs/2604.17966)); FEABench and StructureClaw are covered in the tools notes.
