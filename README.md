# Awesome Jev Security [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of research on the **security, robustness, and safety** of [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), TypeSafe AI's first *System One Model*, and of RLCD (Reinforcement Learning for Calibrated Decisions) models more broadly.

Jev maps unstructured state to **typed probabilistic decisions** (`Choice`, `Score`, `Noul`) with calibrated confidence, instead of generating free-form text. That changes the attack surface. Schema constraints rule out malformed outputs, but attackers can still push the decision distribution or the confidence scores.

> Jev was announced on 2026-09-15. This list tracks the first wave of papers and will be updated as more appear.

## Contents

- [Attacks & Adversarial Robustness](#attacks--adversarial-robustness)
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
| 2026-09-25 | **JevAdvBench: A Benchmark and Black-Box Attacks for Reinforcement Learning for Calibrated Decisions Models** | Jianyi Hu, Hangtao Zhang, Yi Liu, Yeqi Zeng, Li Zeng, Xianlong Wang, Rui Wang, Leo Yu Zhang | [arXiv](https://arxiv.org/abs/2609.31142) · [Project](https://JevAdvBench.github.io/JevAdvBench/) |
| 2026-09-24 | **JevOut: Natural Context Can Flip Decision Models** | Zixiang Xu | [arXiv](https://arxiv.org/abs/2609.30243) · [Code](https://github.com/xzx34/JevOut) · [Project](https://xzx34.github.io/jevout/) |
| 2026-09-23 | **Decision Hijacking: Prompt Injection Attacks on Jev's Typed Probabilistic Decisions** | Tiantong Wu, Wei Yang Bryan Lim | [arXiv](https://arxiv.org/abs/2609.28613) |
| 2026-09-22 | **Type-Safe Is Not Error-Free: A Constrained Decision Head Follows the Option Name, Not the Rubric Bound to It** | Yu Sun, Junhao Xu, Jiajia Shi, Zijin Yang | [arXiv](https://arxiv.org/abs/2609.26758) |

- **JevAdvBench** is the first adversarial benchmark for RLCD models: 812 typed questions over 66 scenarios and 9,744 single-edit black-box attack variants. A single unverified opinion appended to the state flips 12.1% of decisions, statistically tied with the strongest injected command (10.1%). It also pushes 38% of confident answers below human-review thresholds.
- **JevOut** uses an optimizer that adds natural-looking context, with no injected commands. It flips 61.4% of Jev's initially correct decisions, and 64.9–73.2% on three other systems (OpenSourceJev, Von, plain Qwen).
- **Decision Hijacking** tests Jev on 510 reconstructed InjecAgent cases. Malicious content raises the attacker target's probability, but Jev rarely picks it (1.8%). Adaptive attacks using score feedback raise that to 3.5%. Schema-defined outputs change the prompt-injection risk but do not remove it.
- **Type-Safe Is Not Error-Free** shows that Jev-style decision heads follow an option's *name*, not its rubric. Renaming `0/1` to `no/yes` with identical definitions changes 70.4 more answers per hundred and drops AUC from .94 to .23, all with 0% type errors.

## Jev as a Security / Safety Detector

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-09-28 | **JEV as a Judge for Agent Trace Security: An Empirical Comparison with Generative LLM Judges** | Zhiqiang Wang, Yichao Gao | [arXiv](https://arxiv.org/abs/2609.34862) |
| 2026-09-27 | **Evaluating System One Models for Agent Security Decisions: Reliability, Calibration, and Selective Automation** | Yixuan Liu | [arXiv](https://arxiv.org/abs/2609.33401) |
| 2026-09-24 | **Just Ask Jev: Reinforcement Learning for Calibrated Decisions as a Zero-Shot Detector of AI Alignment Failures** | Ruoqi Guo, Yi Liu, Gelei Deng, Yuekang Li, Lida Zhao, Yutao Wu, Simin Chen, Ying Zhang, Leo Yu Zhang | [arXiv](https://arxiv.org/abs/2609.29429) · [Code](https://github.com/sumleo/RLCDAlignBench) |

- **JEV as a Judge for Agent Trace Security** compares JEV with four generative judges at classifying the risk of tool-using agent traces, across 5,219 trajectories from four benchmarks. JEV reaches a positive-class F1 of 77.8, against 74.1 for the best generative judge (GLM-5.2). It costs about $0.000195 per judgment with a 0.99 s median latency, but which model leads varies by dataset.
- **Evaluating System One Models for Agent Security Decisions** compares Jev, Laya, Decider, and Bespoke Nimble with specialized classifiers and LLM judges on prompt-injection and harmful-request detection. Good average calibration hides systematic failures on particular attack groups, including attacks the models confidently label safe. Under strict missed-attack limits, very little traffic can be auto-allowed.
- **Just Ask Jev (RLCDAlignBench)** benchmarks Jev as a zero-shot detector of ten alignment failures: sycophancy, jailbreaks, deception, prompt injection, hallucination, privacy violation, social bias, reward hacking, concealing uncertainty, and power seeking. Its evaluation keeps question wording separate from input fields, and Jev is far cheaper than LLM judges.

## Jev in Security Applications

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-09-28 | **JevVibe: Efficient Classification-Guided Secure Code Generation** | Arshak Rezvani, Sasha Behrouzi, Ahmad-Reza Sadeghi | [arXiv](https://arxiv.org/abs/2609.34963) |
| 2026-09-24 | **Calibrated Decision Models for Autonomous Penetration-Testing Harnesses: JEV and Laya as System One Decision Layers for LLM-Driven Pentest Agents** | Joas Antonio dos Santos Barbosa | [arXiv](https://arxiv.org/abs/2609.28940) |
| 2026-09-21 | **Open-Jev Judgments on CallScreenBench: Calibrated One-Pass Scam Screening with a Small Language Model** | Simiao Ren, Kidus Zewde, Xingyu Shen, Yuchen Zhou, Dennis Ng, Ankit Raj, Tommy Duong, Yuxin Zhang, Neo Tiangratanakul | [arXiv](https://arxiv.org/abs/2609.23959) |

- **JevVibe** uses Jev for 50-way CWE classification on 1,916 CyberSecEval examples. Jev beats all six open-weight baselines; GPT-5.6-Sol wins on Top-1 and Macro-F1, while Jev wins on Top-3 and Top-5 at 6.27× lower latency and 55.9× lower cost. A repair agent guided by Jev's diagnosis raises the security pass rate of generated code from 63.5% to 70.7%, against 66.1% for LLM-guided repair.
- **Pentest Harnesses** uses System One models as decision layers in LLM-driven pentest agents at four points: finding adjudication, severity recalibration, agent pruning, and confirmation loops. Each call takes about 236–276 ms with Jev and 33–40 ms with Laya. It also proposes *Rave*, a domain-adapted System One model (a proposal, not yet built). The case study is one exploratory run per condition.
- **CallScreenBench** covers real-time phone-scam screening with *JevLite*, an open Jev-style model built from Qwen3-4B (not TypeSafe's Jev). It reaches AUROC .974 with calibration error .052, gives no false alarms on legitimate calls, and takes 64.5 ms per decision.

## Reliability & Safe Deployment

> These papers are not security papers. They are listed because security pipelines gate allow/block/escalate on Jev's probabilities, so these reliability limits carry over directly.

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-09-27 | **Beyond Calibration: Do a Typed-Decision Model's Probabilities Obey the Probability Axioms?** | Keyi Li, Yihao He, Quanyi Li | [arXiv](https://arxiv.org/abs/2609.33209) · [Code](https://github.com/bro789/typed-decision-coherence) |
| 2026-09-27 | **Type-Safe Decision Frameworks for Agentic 5G Control: A Theory-Driven Testbed Characterization of Where They Can Be Applied** | Michail-Alexandros Kourtis, George Xilouris | [arXiv](https://arxiv.org/abs/2609.33689) |
| 2026-09-26 | **Typed Decision Models: An Early Evidence Audit and Evaluation Checklist** | Lijuan Tang, Yuemeng Zheng | [arXiv](https://arxiv.org/abs/2609.32160) |
| 2026-09-22 | **REFLEX with Jev for Efficient Selective Control in LLM Agents** | Tiantong Wu, Wei Yang Bryan Lim | [arXiv](https://arxiv.org/abs/2609.26532) |

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
