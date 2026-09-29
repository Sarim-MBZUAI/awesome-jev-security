# Awesome Jev Security [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

A curated list of research on the **security, robustness, and safety** of [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), TypeSafe AI's first *System One Model*, and of RLCD (Reinforcement Learning for Calibrated Decisions) models more broadly.

Jev maps unstructured state to **typed probabilistic decisions** (`Bool`, `Score`, `Choice`) with calibrated confidence, instead of generating free-form text. That changes the attack surface. Schema constraints rule out malformed outputs, but attackers can still push the decision distribution or the confidence scores.

> Jev was announced on 2026-09-15. This list tracks the first wave of papers and will be updated as more appear.

## Contents

- [Attacks & Adversarial Robustness](#attacks--adversarial-robustness)
- [Jev as a Security / Safety Detector](#jev-as-a-security--safety-detector)
- [Jev in Security Applications](#jev-in-security-applications)
- [Background](#background)
- [Related Projects](#related-projects)
- [Contributing](#contributing)

---

## Attacks & Adversarial Robustness

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-09-25 | **JevAdvBench: A Benchmark and Black-Box Attacks for Reinforcement Learning for Calibrated Decisions Models** | Jianyi Hu, Hangtao Zhang, Yi Liu, Yeqi Zeng, Li Zeng, Xianlong Wang, Rui Wang, Leo Yu Zhang | [arXiv](https://arxiv.org/abs/2609.31142) · [Project](https://JevAdvBench.github.io/JevAdvBench/) |
| 2026-09-23 | **Decision Hijacking: Prompt Injection Attacks on Jev's Typed Probabilistic Decisions** | Tiantong Wu, Wei Yang Bryan Lim | [arXiv](https://arxiv.org/abs/2609.28613) |

- **JevAdvBench** is the first adversarial benchmark for RLCD models: 812 typed questions over 66 scenarios and 9,744 single-edit black-box attack variants. A single unverified opinion appended to the state flips 12.1% of decisions, statistically tied with the strongest injected command (10.1%). It also pushes 38% of confident answers below human-review thresholds.
- **Decision Hijacking** introduces attacks in which attacker-controlled content shifts Jev's `Choice` distribution toward an attacker-chosen action. Structured outputs give meaningful protection compared with generative models, but adaptive attacks still raise success rates.

## Jev as a Security / Safety Detector

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-09-27 | **Evaluating System One Models for Agent Security Decisions: Reliability, Calibration, and Selective Automation** | Yixuan Liu | [arXiv](https://arxiv.org/abs/2609.33401) |
| 2026-09-24 | **Just Ask Jev: Reinforcement Learning for Calibrated Decisions as a Zero-Shot Detector of AI Alignment Failures** | Ruoqi Guo, Yi Liu, Gelei Deng, Yuekang Li, Lida Zhao, Yutao Wu, Simin Chen, Ying Zhang, Leo Yu Zhang | [arXiv](https://arxiv.org/abs/2609.29429) · [Code](https://github.com/sumleo/RLCDAlignBench) |

- **Evaluating System One Models for Agent Security Decisions** compares Jev, Laya, Decider, and Bespoke Nimble with specialized classifiers and LLM judges on prompt-injection and harmful-request detection. Good average calibration hides systematic failures on particular attack groups, including attacks the models confidently label safe. Under strict missed-attack limits, very little traffic can be auto-allowed.
- **Just Ask Jev (RLCDAlignBench)** benchmarks Jev as a zero-shot detector of ten alignment failures: sycophancy, jailbreaks, deception, prompt injection, hallucination, privacy violation, social bias, reward hacking, concealing uncertainty, and power seeking. Its evaluation keeps question wording separate from input fields, and Jev is far cheaper than LLM judges.

## Jev in Security Applications

| Date | Paper | Authors | Links |
|------|-------|---------|-------|
| 2026-09-24 | **Calibrated Decision Models for Autonomous Penetration-Testing Harnesses: JEV and Laya as System One Decision Layers for LLM-Driven Pentest Agents** | Joas Antonio dos Santos Barbosa | [arXiv](https://arxiv.org/abs/2609.28940) |

- The paper uses System One models as decision layers in LLM-driven pentest agents at four points: finding adjudication, severity recalibration, agent pruning, and confirmation loops. Each call takes about 236–276 ms with Jev and 33–40 ms with Laya. It also proposes *Rave*, a domain-adapted classifier.

## Background

- [Introducing System One Models & Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev), TypeSafe AI, 2026-09-15. The launch post covers RLCD, parallel sampling, typed outputs, and the vendor's workflow evals, including a security incident response workflow.

## Related Projects

- [jeremymungai/jev-security-playground](https://github.com/jeremymungai/jev-security-playground): Jev experiments for SOC triage, phishing, BEC, and prompt-injection defense.
- [YuyaForest/JEV-Dual-Spectrum-Phishing-Guardian](https://github.com/YuyaForest/JEV-Dual-Spectrum-Phishing-Guardian): phishing and fraud detection built on Jev.

## Contributing

PRs welcome. To be included, a paper must be **about Jev (or RLCD / System One models) and about security, robustness, or safety**. Add it to the right section in reverse-chronological order, using the same table format and a 1–2 sentence summary.
