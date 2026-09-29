# Contributing to Awesome Jev Security

This list tracks research on the **security, robustness, and safety of Jev**, TypeSafe AI's System One model, and of RLCD / System One models in general. Jev takes unstructured state plus a typed question and returns a `Choice`, `Score`, or `Noul` together with a calibrated probability.

## What belongs here

- Papers (arXiv, conference, journal, workshop) that **use or evaluate Jev, or another RLCD / System One model**, **and** study security, robustness, or safety. Examples:
  - attacks on Jev: prompt injection, decision hijacking, confidence manipulation, evasion
  - benchmarks and evaluations of Jev's robustness or calibration under adversarial conditions
  - Jev used as a security or safety component: injection detection, jailbreak detection, SOC triage, phishing, pentest harnesses, agent tool-call gating
  - defenses for Jev-based decision pipelines
- Public, runnable code, datasets, or benchmarks that come with such a paper. These go in **Related Projects**.

## What does not belong here

- General LLM security or guardrail papers that do not evaluate Jev or a System One model
- Vendor marketing, press coverage, or blog posts without a method and results
- Papers that are paywalled or not publicly accessible
- Repositories that only describe an integration without implementing it

## Entry format

Add a row to the table of the correct section:

```markdown
| YYYY-MM-DD | **Paper Title** | Author One, Author Two, ... | [arXiv](https://arxiv.org/abs/XXXX.XXXXX) · [Code](https://github.com/...) |
```

Then add a single summary bullet under the table:

```markdown
- **Short Name**: one or two sentences covering the threat or task, which Jev decision type (`Choice` / `Score` / `Noul`) it targets or uses, and the headline number.
```

## Rules

1. **One section per paper.** Put each paper in the section that best fits its main contribution, and don't list it twice.
2. **Reverse-chronological order.** Newest first, using the first public version date.
3. **Classify by contribution.** Attack, detector or evaluation, or application. Don't classify by venue or toolchain.
4. **Use TypeSafe's terms.** Write `Choice`, `Score`, and `Noul`, not "boolean" or "classifier head."
5. **Traceable numbers.** Every metric in a summary must appear in the paper.
6. **Full author list.** Copy it from the paper, in the paper's order.
7. **Clarity over hype.** Summaries should be neutral and understandable on one read.

## Pull requests

- One paper (or a small set of closely related papers) per PR
- Use the title `Add <Short Name>`
- If you used AI to write or find the entry, say so in the PR description

## Checklist

- [ ] The paper is publicly accessible
- [ ] It involves Jev or an RLCD / System One model **and** security, robustness, or safety
- [ ] It is in the right section and in date order
- [ ] The authors, date, and links are correct
- [ ] The summary is 1–2 sentences, and its numbers can be checked against the paper
- [ ] It is not already listed
