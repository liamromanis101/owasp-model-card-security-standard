# Framework Mapping: OWASP Model Card Security Standard

> **Standard:** OWASP Model Card Security Standard v0.1  
> **Last Updated:** 2025-04-01

This document maps the sections of the OWASP Model Card Security Standard to related requirements and guidance in three major frameworks: the OWASP Top 10 for LLM Applications, the NIST AI Risk Management Framework (AI RMF), and the EU AI Act.

It is intended to help security teams, compliance officers, and auditors understand how completing a model card contributes to broader regulatory and framework obligations — and to avoid duplicating work across frameworks.

---

## 1. OWASP Top 10 for LLM Applications (2025)

| OWASP LLM Risk | Model Card Section(s) | How the model card helps |
|---|---|---|
| LLM01 — Prompt Injection | Section 5.2 Adversarial Testing; Section 5.3 Output Filtering | Documents whether prompt injection was tested, scope of testing, and what controls are in place |
| LLM02 — Sensitive Information Disclosure | Section 3.2 PII Assessment; Section 5.1 Known Vulnerabilities | Documents PII exposure risk in training data and any known memorisation vulnerabilities |
| LLM03 — Supply Chain Vulnerabilities | Section 6 Supply Chain | Documents base model provenance, dependency audit status, and SBOM availability |
| LLM04 — Data and Model Poisoning | Section 3.3 Data Provenance; Section 3.1 Data Sources | Documents data collection methods, licensing, and known quality issues that could indicate poisoning risk |
| LLM05 — Insecure Output Handling | Section 5.3 Output Filtering; Section 7 Deployment Constraints | Documents output filtering controls and recommended deployment security controls |
| LLM06 — Excessive Agency | Section 7 Deployment Constraints | Documents deployment restrictions and recommended human review controls |
| LLM07 — System Prompt Leakage | Section 5.2 Adversarial Testing | Documents whether system prompt extraction was included in adversarial testing scope |
| LLM08 — Vector and Embedding Weaknesses | Section 5.1 Known Vulnerabilities | Provides a structured place to document embedding-specific vulnerabilities |
| LLM09 — Misinformation | Section 10 Evaluation; Section 5.3 Output Filtering | Documents hallucination rates and output controls |
| LLM10 — Unbounded Consumption | Section 7 Deployment Constraints | Documents rate limiting and resource constraint recommendations |

---

## 2. NIST AI Risk Management Framework (AI RMF 1.0)

The NIST AI RMF is organised around four functions: Govern, Map, Measure, and Manage. The model card contributes primarily to the Map and Measure functions, with supporting material for Govern and Manage.

| NIST AI RMF Function | Sub-category | Model Card Section(s) |
|---|---|---|
| **Govern** | GOVERN 1.1 — Policies and processes for AI risk | Section 8 Regulatory & Compliance; Section 7 Deployment Constraints |
| **Govern** | GOVERN 1.7 — Transparency and accountability | Section 1 Model Overview; Section 12 Changelog |
| **Map** | MAP 1.1 — Context is established for AI risk | Section 2 Intended Use |
| **Map** | MAP 1.5 — Organisational risk tolerances are defined | Section 3.2 PII Assessment (residual risk); Section 5.1 Known Vulnerabilities |
| **Map** | MAP 2.2 — Data collection practices are documented | Section 3 Training Data |
| **Map** | MAP 5.1 — Likelihood of AI risks is assessed | Section 5 Security Assessment; Section 9 Incident History |
| **Measure** | MEASURE 2.5 — Privacy risks are assessed | Section 3.2 PII Assessment |
| **Measure** | MEASURE 2.6 — Evaluation of AI system performance | Section 10 Evaluation & Performance |
| **Measure** | MEASURE 2.7 — AI system security is evaluated | Section 5.2 Adversarial Testing |
| **Measure** | MEASURE 4.1 — Measurement approaches are documented | Section 10 Evaluation & Performance |
| **Manage** | MANAGE 1.3 — Response and recovery processes | Section 9 Incident History |
| **Manage** | MANAGE 2.4 — Mechanisms to sustain effectiveness | Section 12 Changelog; Section 5.1 Known Vulnerabilities |
| **Manage** | MANAGE 3.1 — Risk treatment plans | Section 7 Deployment Constraints |

---

## 3. EU AI Act

The EU AI Act (in force from August 2024, with obligations phasing in through 2026–2027) creates documentation requirements that vary by risk tier. The model card directly supports compliance with several of these.

### High-Risk AI Systems (Annex III)

Organisations deploying high-risk AI systems must maintain technical documentation under Article 11 and Annex IV. The following model card sections map to those requirements:

| EU AI Act Requirement | Annex IV Reference | Model Card Section(s) |
|---|---|---|
| General description of the system | Annex IV, 1(a) | Section 1 Model Overview; Section 2 Intended Use |
| Description of elements and development process | Annex IV, 1(b) | Section 4 Architecture & Training; Section 3 Training Data |
| Monitoring, functioning, and control | Annex IV, 1(c) | Section 7 Deployment Constraints |
| Description of changes to the system | Annex IV, 1(d) | Section 12 Changelog |
| Description of hardware requirements | Annex IV, 1(e) | Section 4 Architecture & Training |
| Training data and data governance | Annex IV, 2 | Section 3 Training Data |
| Data validation and examination | Annex IV, 2(d) | Section 3.2 PII Assessment; Section 3.3 Data Provenance |
| Technical measures for robustness | Annex IV, 3 | Section 5 Security Assessment |
| Description of measures for human oversight | Annex IV, 6 | Section 7 Deployment Constraints |
| Description of logging capabilities | Annex IV, 7 | Section 7 Deployment Constraints |

### Limited-Risk AI Systems

Limited-risk systems (e.g. chatbots, deepfakes) face transparency obligations under Article 52. The model card supports this by documenting the intended use and constraints clearly in Sections 2 and 7.

### GPAI Models (General Purpose AI — Title VIII)

For GPAI models, Article 53 requires providers to maintain technical documentation and cooperate with downstream deployers. Section 6 (Supply Chain) is particularly relevant here, as it documents base model provenance in a way that downstream users can reference.

---

## 4. Gaps and Limitations

This mapping is indicative, not exhaustive. A completed model card is not a substitute for formal compliance assessment against any of these frameworks. In particular:

- **NIST AI RMF** requires organisational processes and governance structures that go well beyond model-level documentation
- **EU AI Act** compliance for high-risk systems requires a conformity assessment process, not just documentation
- **OWASP LLM Top 10** risks require active mitigations — a model card documents what has been done, but does not itself constitute a mitigation

The model card is best understood as a structured evidence document that feeds into broader governance and compliance processes.

---

## 5. Contributing to this Mapping

If you identify gaps, errors, or additional framework mappings that should be included here, please raise an issue or submit a pull request. Framework requirements evolve over time and this document will need to be updated as they do.
