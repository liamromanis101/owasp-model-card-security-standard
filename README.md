# OWASP Model Card Security Standard

> **Status: Draft — Incubating OWASP Project**  
> This standard is under active development. Contributions and feedback are very welcome.

---

## What is this?

When you download a pre-trained model from HuggingFace or any other repository, you're essentially trusting a black box. You don't know what data it was trained on, whether that data contained personal information, how it's been tested for adversarial attacks, or what its known failure modes are. For most hobby projects that's fine. For anything touching real users, regulated industries, or sensitive data — it really isn't.

Model cards have existed for a while, and they're a good idea. But the existing formats focus almost entirely on performance metrics and fairness considerations. Security is largely an afterthought, if it's there at all.

This project is trying to fix that. We're building an open standard that defines what security-relevant information a model card should contain — things like training data provenance, PII exposure risk, known vulnerabilities, supply chain details, and regulatory compliance status. Something that a security team can actually use when assessing whether a model is safe to deploy.

---

## Why does this matter now?

A few things have converged at once:

- The **EU AI Act** is creating real compliance obligations around AI transparency and documentation
- **GDPR and similar laws** raise legitimate questions about whether personal data ended up in training sets, and what was done about it
- The **security community** is increasingly aware that models themselves are an attack surface — prompt injection, data poisoning, model theft — but documentation standards haven't kept up
- Organisations are deploying third-party models at scale with very little visibility into what they're actually running

A security-focused model card standard gives developers, auditors, and procurement teams a common language and a checklist they can actually act on.

---

## A quick example

Here's a flavour of what a security-focused model card looks like under this standard:

```yaml
model_card_version: "0.1"
model_name: "example-text-classifier"
model_version: "1.2.0"

training_data:
  sources:
    - name: "Common Crawl (filtered)"
      pii_assessment: "Presidio scan applied; residual risk: low"
      license: "CC0"
  pii_removal_method: "Microsoft Presidio + manual review on 5% sample"
  data_cutoff_date: "2024-01-01"

security:
  known_vulnerabilities: []
  adversarial_testing: "Red-teamed against prompt injection; results in /security/redteam-report.md"
  supply_chain:
    base_model: "bert-base-uncased"
    base_model_source: "https://huggingface.co/bert-base-uncased"

compliance:
  eu_ai_act_risk_tier: "Limited"
  gdpr_considerations: "No personal data retained post-training"
```

This is a simplified excerpt. The full schema covers additional fields for incident history, output filtering, deployment constraints, and more.

---

## What's in this repository

```
/
├── README.md                          ← you are here
├── standard/
│   └── OWASP-Modelcard-Standard-Proposal.md   ← the specification
├── templates/
│   └── model-card-template.md            ← blank template to copy and fill in
├── examples/
│   └── llm-example.md                    ← worked example for a large language model
├── schema/
│   └── model-card-schema.json            ← machine-readable JSON schema
├── mappings/
│   └── owasp-nist-euaiact-mapping.md     ← how this standard relates to other frameworks
├── CONTRIBUTING.md                    ← how to get involved
├── CHANGELOG.md
└── LICENSE                            ← CC BY 4.0
```

---

## How this relates to other standards and frameworks

This standard is designed to complement, not replace, existing work:

| Framework | Relationship |
|---|---|
| OWASP Top 10 for LLM Applications | This standard helps document mitigations for several LLM Top 10 risks |
| OWASP AI Exchange | Aligned with AI security guidance; cross-referenced where relevant |
| NIST AI Risk Management Framework | Standard fields map to AI RMF govern, map, measure, and manage functions |
| EU AI Act | Covers transparency and documentation obligations for limited and high-risk AI systems |
| HuggingFace Model Cards | Extends the HuggingFace format with security-specific fields |
| Google Model Cards | Compatible with Google's approach; adds security layer on top |

---

## Getting involved

This is an early-stage project and there's plenty of room to shape where it goes. A few ways to contribute:

- **Review the draft standard** and raise issues if fields are missing, wrong, or unclear
- **Share examples** of model cards from your own work (anonymised is fine) so we can stress-test the schema against reality
- **Join the discussion** on OWASP Slack in `#ai-ml-security`
- **Submit a pull request** — see [CONTRIBUTING.md](CONTRIBUTING.md) for guidance

If you're not sure where to start, the open issues labelled `good first issue` are a reasonable place to look.

---

## Project status

This project is currently going through the OWASP new project process. Once accepted it will move to the OWASP GitHub organisation at `github.com/OWASP/owasp-model-card-security-standard`.

---

## Licence

This standard is published under [Creative Commons Attribution 4.0 International (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/). You are free to use, adapt, and redistribute it — including commercially — as long as you give appropriate credit.

Any tooling in this repository is licensed under [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0).
