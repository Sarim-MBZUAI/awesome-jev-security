# Awesome Jev Security [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of research on the **security, robustness, and safety** of [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), TypeSafe AI's first *System One Model*, and of RLCD (Reinforcement Learning for Calibrated Decisions) models more broadly.

Jev maps unstructured state to **typed probabilistic decisions** (`Choice`, `Score`, `Noul`) with calibrated confidence, instead of generating free-form text. That changes the attack surface. Schema constraints rule out malformed outputs, but attackers can still push the decision distribution or the confidence scores.

> Jev was announced on 2026-09-15. This list tracks the first wave of papers and will be updated as more appear.

## Contents

- [Attacks & Adversarial Robustness](#attacks--adversarial-robustness)
- [Privacy](#privacy)
- [Jev as a Security / Safety Detector](#jev-as-a-security--safety-detector)
- [Jev in Security Applications](#jev-in-security-applications)
- [Reliability & Safe Deployment](#reliability--safe-deployment)
- [Background](#background)
- [Related Projects](#related-projects)
- [Contributing](#contributing)

---

## Attacks & Adversarial Robustness

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-10-13 | **Nudge the Judge: Uncovering Program Decision Hijacking in Real-World Jev Applications** | Yang Luo, Tiantian Ji, Zifeng Kang, Yucheng Wang, Yong Liu, Zhiyi Yin, Xinran Liu | [arXiv](https://arxiv.org/abs/2610.18612) · [Code](https://github.com/xiaoxiaohg8d/JevProbe-) |
| 2026-10-13 | **Same Attack, Different Judge: A First Controlled Adversarial Comparison of Jev, LLM Judges, and Purpose-Built Safety Classifiers** | Xiang Li, Aichun Huang, Mingming Zhang, Zheli Liu | [arXiv](https://arxiv.org/abs/2610.17820) |
| 2026-10-13 | **DecisionRobust: Benchmarking Robustness for JEV Style Decision Models Across Text and Vision** | Yifan Lan, Zhongye Liu, Yurui Chang, Hanyu Wang, Yongkang Du, Lu Lin, Jinghui Chen | [arXiv](https://arxiv.org/abs/2610.19118) · [Project](https://yifan-lan.github.io/DecisionRobust/) |
| 2026-10-11 | **Poisoned into Action: Context Attacks on Typed Decision Workflows** | Chenkai Wu, Xing Chang, Jun Yan | [arXiv](https://arxiv.org/abs/2610.15537) · [Code](https://github.com/WuXixiong/jev-poisoning-bench) |
| 2026-10-09 | **DICE: Tilting the Odds through Interface Attacks on System One Model** | Ben Dong, Tianyi Lu, Zhonghao Guo, Qian Wang | [arXiv](https://arxiv.org/abs/2610.14404) |
| 2026-10-08 | **One Word Opens the Gate: The Option-Channel Attack on Typed Decision Models as Agent Guardrails** | Seyedarmin Azizi, Erfan Baghaei Potraghloo, Massoud Pedram | [arXiv](https://arxiv.org/abs/2610.12292) · [Code](https://github.com/ArminAzizi98/option-channel-attack) |
| 2026-10-08 | **Adversarial Cues in Decision Models Used as Judges: The Role of Request Presentation** | Hongliang Liu | [arXiv](https://arxiv.org/abs/2610.11436) |
| 2026-10-04 | **Hidden Risks of Jev: An Empirical Study of Security, Privacy, and Dual Use** | Shang Wang, Tianqing Zhu, Huajie Chen, Jiayang Li, Meng Yang, Bo Liu | [arXiv](https://arxiv.org/abs/2610.04985) |
| 2026-09-25 | **JevAdvBench: A Benchmark and Black-Box Attacks for Reinforcement Learning for Calibrated Decisions Models** | Jianyi Hu, Hangtao Zhang, Yi Liu, Yeqi Zeng, Li Zeng, Xianlong Wang, Rui Wang, Leo Yu Zhang | [arXiv](https://arxiv.org/abs/2609.31142) · [Project](https://JevAdvBench.github.io/JevAdvBench/) |
| 2026-09-24 | **JevOut: Natural Context Can Flip Decision Models** | Zixiang Xu, Zirui Song, Chiyu Zhang, Xiuying Chen, Xi Liu, Xiyang Hu, Yue Zhao | [arXiv](https://arxiv.org/abs/2609.30243) · [Code](https://github.com/xzx34/JevOut) · [Project](https://xzx34.github.io/jevout/) |
| 2026-09-23 | **Decision Hijacking: Prompt Injection Attacks on Jev's Typed Probabilistic Decisions** | Tiantong Wu, Wei Yang Bryan Lim | [arXiv](https://arxiv.org/abs/2609.28613) |
| 2026-09-22 | **Type-Safe Is Not Error-Free: Typed Decision Models Follow the Option Name, Not the Definition Bound to It** | Yu Sun, Junhao Xu, Jiajia Shi, Zijin Yang | [arXiv](https://arxiv.org/abs/2609.26758) |

- **Nudge the Judge** names a new risk, *Jev Program Decision Hijacking*: attacker-controlled context steers program behavior through Jev answers that are still schema-valid. Its JevProbe framework tested 228 popular real-world Jev apps with 2,273 test cases. Decisions flipped in 81.5% of them, and 98.6% of those flips could cause downstream security impact. Existing defenses offer limited protection. The authors release JevProbeBench and have disclosed the findings to app maintainers.
- **Same Attack, Different Judge** is the first controlled adversarial comparison of Jev with five GPT/GLM judges, Llama Guard 3 and Prompt Guard 2. It covers four tasks and 50 attack variants from 13 families. Jev is the most robust on input moderation: a sentence asking why someone might make a harmful request bypasses 9% of its blocks, against 56% for GPT-6 Astra. On phishing, forged inputs bypass 19–29% of the emails Jev flags, and even without attack Jev catches only 62% of phishing (GLM judges: 77–81%). It also flags the most benign requests (27%).
- **DecisionRobust** is a robustness benchmark for Jev-style decision models on text and vision. It covers semantics-preserving perturbations and prompt injection in both modalities, across 30 text and 17 vision model configurations. Resisting perturbations does not imply resisting prompt injection. Label-consistency checks substantially lower measured attack success.
- **Poisoned into Action** traces poisoned evidence from retrieval through Jev's choice to the automated action and saved state, using 1,659 calls to Jev-1.13.0 in two synthetic workflows. A standalone payload reached all 60 trials and caused five security-violating executions. A source-exclusion defense blocked the violation but also withheld correct decisions. The authors call the results exploratory, since their main contrasts are not significant after correction.
- **DICE** attacks the components *around* Jev (state extraction, question generation, output interpretation) while Jev and its API responses stay untouched. Each of these adapters can change the final decision. Checks that bind the adapters to the task stopped the tested attacks, but source verification also rejected some valid decisions. It is a small synthetic pilot.
- **One Word Opens the Gate** tests seven open-weight typed decision models (not hosted Jev) as agent guardrails. Allow/block accuracy on injection, jailbreak and toxic-content screening is 36–72%, against 50% chance. Six irrelevant server-log lines raise the fail-open rate from 0% to 63%. Giving the permissive option a misleading name raises it to 93–100% on four models. Every defense tested is defeated, and escalating low-confidence cases does not help.
- **Adversarial Cues in Judges** shows that adding a single colon to a candidate answer raises Jev's false-acceptance rate as an answer judge from 1.0% to 26.0% (three labels) and from 3.0% to 26.5% (the four-label grading instruction). The effect depends on how the request is presented. GPT-6 Sol showed no such false acceptances.
- **Hidden Risks of Jev** is the first broad study of Jev's security and privacy, run on the official Jev API and on NanoJev, a local model whose training data the authors control. It covers prompt injection and adversarial suffixes; backdoors planted through training-data poisoning (supply chain); membership, private-attribute and internal-knowledge inference attacks that work despite typed outputs; and dual use (detecting injection, jailbreaks, harmful content and AI-generated text, plus misuse for jailbreaks and model extraction). The authors conclude that Jev is vulnerable to all the threats examined.
- **JevAdvBench** is the first adversarial benchmark for RLCD models: 812 typed questions over 66 scenarios and 9,744 single-edit black-box attack variants. A single unverified opinion appended to the state flips 12.1% of decisions, statistically tied with the strongest injected command (10.1%). It also pushes 38% of confident answers below human-review thresholds.
- **JevOut** uses probability-guided optimization to write short, ordinary-looking background additions with no answer-selection instructions. These redirect 61.4–73.2% of the initially correct decisions of four systems, Jev included, to a wrong option fixed in advance. The attacks transfer across models, and in blinded human evaluation 91.6% of sampled contexts were judged natural and answer-preserving.
- **Decision Hijacking** tests Jev on 510 reconstructed InjecAgent cases. Malicious content raises the attacker target's probability, but Jev rarely picks it (1.8%). Adaptive attacks using score feedback raise that to 3.5%. Schema-defined outputs change the prompt-injection risk but do not remove it.
- **Type-Safe Is Not Error-Free** shows that Jev and two open-weight models follow an option's *name*, not its definition. Reassigning names to definitions, with everything else unchanged, gives decision-flip rates up to 70.4 pp higher with `yes/no` names than with `0/1`, and drops mean AUC from 93.8% to 23.2% (below chance). The type-error rate stays at 0% throughout.

## Privacy

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-10-16 | **TUPLE-Bench: Evaluating and Mitigating Privacy-Utility Trade-offs in Typed Decision Models** | Yiqu Yang, Qiaoyuan Zheng, Yang Tian, Yu Fan, Elliott Ash | [arXiv](https://arxiv.org/abs/2610.23668) |

- **TUPLE-Bench** shows that typed outputs leak more than the chosen label. Across nine models, telling the model to withhold a fine-grained cause fails: 91.88–99.93% of responses reveal it, either through the selection or through residual scores. Even with the causes removed from the candidate set, the full outputs allow 62.85–75.35% recovery (50% chance). Confidence fields increase leakage. The paper proposes WCPQ (workflow-constrained privacy quantization) as a mitigation. *Hidden Risks of Jev* (above) also covers membership and attribute inference.

## Jev as a Security / Safety Detector

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-10-14 | **From Detection to Decision: Calibration and Deferral for Prompt-Injection Detection** | Qianlong Lan, Anuj Kaul, Vinothini Pandurangan, Indranil Sanyal | [arXiv](https://arxiv.org/abs/2610.20907) |
| 2026-10-13 | **Jev-Guard: Fast Routing and Auditing Agent Actions over Editable Policy Trees** | Wenbin Hu, Huihao Jing, Haochen Shi, Yuxuan Liu, Changxuan Fan, Sirui Zhang, Kawai Chung, Hanyu Yang, Zhenhuan Wang, Ziyi Chen, Haoran Li, Yangqiu Song | [arXiv](https://arxiv.org/abs/2610.18326) |
| 2026-10-06 | **Benchmarking System One Models in Online Moderation** | Federico Mazzoni, Andrea Failla | [arXiv](https://arxiv.org/abs/2610.07953) |
| 2026-10-02 | **To Jev or Not? Evaluating the Accuracy and Efficiency of Structured Decision Models for Hate-Speech Moderation** | Demetris Paschalides, George Pallis, Marios D. Dikaiakos | [arXiv](https://arxiv.org/abs/2610.03324) |
| 2026-09-28 | **JEV as a Judge for Agent Trace Security: An Empirical Comparison with Generative LLM Judges** | Zhiqiang Wang, Yichao Gao | [arXiv](https://arxiv.org/abs/2609.34862) |
| 2026-09-27 | **Evaluating System One Models for Agent Security Decisions: Reliability, Calibration, and Selective Automation** | Yixuan Liu | [arXiv](https://arxiv.org/abs/2609.33401) |
| 2026-09-24 | **Just Ask Jev: Reinforcement Learning for Calibrated Decisions as a Zero-Shot Detector of AI Alignment Failures** | Ruoqi Guo, Yi Liu, Gelei Deng, Yuekang Li, Lida Zhao, Yutao Wu, Simin Chen, Ying Zhang, Leo Yu Zhang | [arXiv](https://arxiv.org/abs/2609.29429) · [Code](https://github.com/sumleo/RLCDAlignBench) |

- **From Detection to Decision** compares Jev with Prompt Guard 2 (22M and 86M) and an LLM judge on 47,611 held-out prompt-injection examples, including direct and indirect injection, hard-benign inputs, obfuscation and distribution shift. AUROC: LLM judge 0.897, Jev 0.871, Prompt Guard 2 86M 0.757, 22M 0.714. Jev's native probabilities do not consistently beat post-hoc calibrated scores. At a 1% false-negative target, every gate-plus-reviewer system needs more than 91.8% escalation.
- **Jev-Guard** is a guardrail model that first routes a request to the right policy in an editable policy tree (content moderation, cybersecurity, privacy, legal), then classifies it. On 7,033 cases it reaches 93.72% safety accuracy and 93.10% routing accuracy, with 58 ms mean latency. Inside the Hermes agent harness it cuts mean review time per task from 4.80 s (an SFT-LLM judge) to 1.33 s.
- **Online Moderation** tests Jev and Laya on five moderation benchmarks. Jev is competitive with specialized systems and improves with retrieved precedents; Laya is less consistent. Jev's confidence supports selective human review in several settings, though it is not consistently calibrated.
- **To Jev or Not? (HATEDECIDE)** evaluates six decision-model configurations on four hate-speech datasets. Commercial LLMs significantly beat every decision model on only one dataset. Supplying a dataset's definition changes up to 28% of predictions without consistently helping. On a diagnostic set, the best hosted decision model comes within 1.6 macro-F1 of the best commercial LLM at about 97% lower cost.
- **JEV as a Judge for Agent Trace Security** compares JEV with four generative judges at classifying the risk of tool-using agent traces, across 5,219 trajectories from four benchmarks. JEV reaches a positive-class F1 of 77.8, against 74.1 for the best generative judge (GLM-5.2). It costs about $0.000195 per judgment with a 0.99 s median latency, but which model leads varies by dataset.
- **Evaluating System One Models for Agent Security Decisions** compares Jev, Laya, Decider, and Bespoke Nimble with specialized classifiers and LLM judges on prompt-injection and harmful-request detection. Good average calibration hides systematic failures on particular attack groups, including attacks the models confidently label safe. Under strict missed-attack limits, very little traffic can be auto-allowed.
- **Just Ask Jev (RLCDAlignBench)** benchmarks Jev as a zero-shot detector of ten alignment failures: sycophancy, jailbreaks, deception, prompt injection, hallucination, privacy violation, social bias, reward hacking, concealing uncertainty, and power seeking. Its evaluation keeps question wording separate from input fields, and Jev is far cheaper than LLM judges.

## Jev in Security Applications

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-10-21 | **VX-One: Typed, Calibrated Decision Heads for Real-Time Perimeter Triage - An empirical study of unknown-attack honesty, cascade gray zones, and calibration under drift** | Juan S. Santillana | [arXiv](https://arxiv.org/abs/2610.30090) |
| 2026-10-14 | **Diagnose Before You Refine: Multi-Dimensional Judge Feedback for Query-Efficient Jailbreak Attacks** | Yixin Tan, Chia-Yi Hsu, Jiayang Liu, Chia-Mu Yu, Rui Wen | [arXiv](https://arxiv.org/abs/2610.19971) |
| 2026-10-02 | **SecJev: Bringing Security Expertise to System One Decision Models** | Zheng Chen, Fei Yu, Haohao Huang, Yang Li, Anlong Chen, Lei Chen | [arXiv](https://arxiv.org/abs/2610.03073) |
| 2026-10-01 | **Jev-IDS: System One Models for Network Intrusion Detection** | Paulo Severo, Silvio E. Quincozes, Amanda Dias | [arXiv](https://arxiv.org/abs/2610.01079) |
| 2026-10-01 | **HydroJEV: A one-second, training-free screen for cyber-attack and fault attribution in water distribution networks** | Tianwei Mu, Shengyan Jiang, Mingzhe Yuan, Qing Luo, Min Xiao, Wenhong Wang, Jun Li, Manhong Huang | [arXiv](https://arxiv.org/abs/2610.02048) |
| 2026-09-28 | **JevVibe: Efficient Classification-Guided Secure Code Generation** | Arshak Rezvani, Sasha Behrouzi, Ahmad-Reza Sadeghi | [arXiv](https://arxiv.org/abs/2609.34963) |
| 2026-09-24 | **Calibrated Decision Models for Autonomous Penetration-Testing Harnesses: JEV and Laya as System One Decision Layers for LLM-Driven Pentest Agents** | Joas Antonio dos Santos Barbosa | [arXiv](https://arxiv.org/abs/2609.28940) |
| 2026-09-21 | **Open-Jev Judgments on CallScreenBench: Calibrated One-Pass Scam Screening with a Small Language Model** | Simiao Ren, Kidus Zewde, Xingyu Shen, Yuchen Zhou, Dennis Ng, Ankit Raj, Tommy Duong, Yuxin Zhang, Neo Tiangratanakul | [arXiv](https://arxiv.org/abs/2609.23959) |

- **VX-One** is a small network (100K–10M parameters) for network perimeter triage. It exposes System One-style outputs: a calibrated gate, a choice that always includes an explicit "unknown", and an ordinal score. It is not Jev itself. It wins detection on UNSW-NB15 and under drift on CIC-IDS2017 (AUROC 0.711 vs 0.637), but XGBoost wins multi-class attribution on all three benchmarks. Calibration collapses under temporal drift for every method (ECE 0.04 → 0.40).
- **Jev-Guided Strategy Bandit (JSB)** is an *offensive* use: Jev's probability scores guide a query-based jailbreak attack on other LLMs. With only 10 target queries it reaches an average HarmBench attack success rate of 89.8% across ten open-source settings. The paper also suggests Jev-based output filtering as a defense.
- **SecJev** is a family of Jev-like decision models (0.8B–9B) specialized for security. They are trained on SecJev-Corpus, which covers 14 tasks such as tool outputs, network traffic, authentication and vehicle messages. SecJev-0.8B beats general Kev-9B by 20.51 points in task-macro accuracy, and SecJev-4B improves on Kev-4B and on security-trained generative models in prompt-injection and traffic transfer tests. Adapters, corpus and code are released.
- **Jev-IDS** detects network intrusions by asking Jev two questions per flow: an attack probability and a traffic category. On a 300-flow NSL-KDD pilot it reaches F1 0.859 and novel-attack recall 0.838. It is 4.8× faster and 3.8× cheaper than GPT-5.6 Luna, and gives 15× fewer false alarms than a low-data Random Forest.
- **HydroJEV** uses Jev as a training-free first-pass screen for SCADA alarms in water networks, sorting cyberattacks from physical faults, normal transients and faulty sensors. It matches a hand-written rule tree (macro-F1 0.62–0.64) and beats a supervised classifier by 0.36–0.42 on unseen event subtypes. It is 20–40× faster than frontier LLMs and takes 35–38% of the review load off an LLM reviewer.
- **JevVibe** uses Jev for 50-way CWE classification on 1,916 CyberSecEval examples. Jev beats all six open-weight baselines; GPT-5.6-Sol wins on Top-1 and Macro-F1, while Jev wins on Top-3 and Top-5 at 6.27× lower latency and 55.9× lower cost. A repair agent guided by Jev's diagnosis raises the security pass rate of generated code from 63.5% to 70.7%, against 66.1% for LLM-guided repair.
- **Pentest Harnesses** uses System One models as decision layers in LLM-driven pentest agents at four points: finding adjudication, severity recalibration, agent pruning, and confirmation loops. Each call takes about 236–276 ms with Jev and 33–40 ms with Laya. It also proposes *Rave*, a domain-adapted System One model (a proposal, not yet built). The case study is one exploratory run per condition.
- **CallScreenBench** covers real-time phone-scam screening with *JevLite*, an open Jev-style model built from Qwen3-4B (not TypeSafe's Jev). It reaches AUROC .974 with calibration error .052, gives no false alarms on legitimate calls, and takes 64.5 ms per decision.

## Reliability & Safe Deployment

> These papers are not security papers. They are listed because security pipelines gate allow/block/escalate on Jev's probabilities, so these reliability limits carry over directly.

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-10-14 | **Calibration and Risk-Budgeted Decisions with System One Models** | Soundouss Messoudi, Youness Mansar | [arXiv](https://arxiv.org/abs/2610.19811) |
| 2026-10-01 | **Fast Models, Slow Evidence: A Paired and Self-Audited Evaluation of System-1 Decision Models for LLM Agent Harnesses** | Jiawei Li | [arXiv](https://arxiv.org/abs/2610.02267) · [Code](https://github.com/David-DL-Space/sys1-eval) |
| 2026-09-27 | **Beyond Calibration: Do a Typed-Decision Model's Probabilities Obey the Probability Axioms?** | Keyi Li, Yihao He, Quanyi Li | [arXiv](https://arxiv.org/abs/2609.33209) · [Code](https://github.com/bro789/typed-decision-coherence) |
| 2026-09-27 | **Type-Safe Decision Frameworks for Agentic 5G Control: A Theory-Driven Testbed Characterization of Where They Can Be Applied** | Michail-Alexandros Kourtis, George Xilouris | [arXiv](https://arxiv.org/abs/2609.33689) |
| 2026-09-26 | **Typed Decision Models: An Early Evidence Audit and Evaluation Checklist** | Lijuan Tang, Yuemeng Zheng | [arXiv](https://arxiv.org/abs/2609.32160) |
| 2026-09-22 | **REFLEX with Jev for Efficient Selective Control in LLM Agents** | Tiantong Wu, Wei Yang Bryan Lim | [arXiv](https://arxiv.org/abs/2609.26532) |

- **Risk-Budgeted Decisions** studies allow/block and retain/discard decisions when no human is available to defer to, across six moderation, safety and reranking datasets. Naive threshold selection exceeds the target risk in about half the settings, against 7.8% for Learn Then Test (LTT), which has finite-sample guarantees.
- **Fast Models, Slow Evidence** is a paired evaluation of Laya and Jev on 11 agent decision points, including injection gating: 7,283 base cases plus 6,640 robustness variants. Jev is significantly more accurate on 9 of 11. Laya changes 30% of its answers when the option order is reversed. The authors' audit of their own pipeline found that in-sample thresholds aimed at 5% misses gave up to 17% misses on held-out data, and that an apparent effect on injection false positives disappeared with channel-native content.
- **Beyond Calibration** tests whether Jev's probabilities are logically consistent. On negation pairs, P("X") + P("not X") misses 1 by 0.064 on average (Qwen3.8-27B first-token: 0.293). Good calibration does not guarantee coherent probabilities.
- **Agentic 5G Control** tests Jev, Laya, and AnyJev as act/escalate/abstain gates in a closed 5G core-policy loop. Type safety removes format failures but not wrong actions. Fine-tuned Laya returned its training answer for 98–99.5% of changed questions and then acted wrongly on up to 80% of them. Jev acted wrongly on at most 0.143 of any changed question, but it is hosted and 11–29× slower.
- **Early Evidence Audit** reviews 28 papers from the first nine days after Jev's launch. The typed readout has not yet shown an independent accuracy advantage over label-probability baselines, and Jev's clearest gains are in latency and cost. It proposes a 14-item evaluation checklist.
- **REFLEX** uses Jev as a cheap decision layer in an agent that defers to a strong LLM when confidence drops. It reaches 95% success with 72.7% fewer strong-model calls, but the gains shrink when routing is already accurate.

## Background

- [Introducing System One Models & Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), TypeSafe AI, 2026-09-15. The launch post covers RLCD, parallel sampling, typed outputs, and the vendor's workflow evals, including a security incident response workflow.

## Related Projects

- [jeremymungai/jev-security-playground](https://github.com/jeremymungai/jev-security-playground): Jev experiments for SOC triage, phishing, BEC, and prompt-injection defense.
- [YuyaForest/JEV-Dual-Spectrum-Phishing-Guardian](https://github.com/YuyaForest/JEV-Dual-Spectrum-Phishing-Guardian): phishing and fraud detection built on Jev.
- [OmniJev/awesome-jev-papers](https://github.com/OmniJev/awesome-jev-papers): general index of Jev papers.

## Contributing

PRs welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) first.
