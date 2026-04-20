# Proposed OWASP Model Card Security Standard
### Draft v0.1 — April 2026

**Project:** OWASP GenAI Security Project
**Relates to:** OWASP Top 10 for LLM Applications 2025 (LLM03, LLM04)
**Regulatory mapping:** EU AI Act (Regulation EU 2024/1689), Articles 53 & 55; GPAI Code of Practice (July 2025)
**Status:** Community Proposal — submitted for consideration


## Abstract

This document proposes a formal OWASP Model Card Security Standard (MCSS) for machine learning models published on public repositories such as HuggingFace. It defines a minimum set of required documentation fields, maps each field to specific obligations under the EU AI Act and related regulatory frameworks, aligns with the OWASP Top 10 for LLM Applications 2025, and provides a verification methodology that can be implemented by automated tooling, security teams, and procurement processes.

The standard is motivated by empirical evidence that widely-deployed open-source models (including models downloaded hundreds of millions of times) which systematically fail basic documentation requirements that are now legally mandated for EU market deployment. The absence of a formal, unified standard has left model authors, deployers, and regulators without a common baseline against which compliance, and risk, can be assessed.


## Reference Notation

This standard uses a composite reference notation to link every field to existing OWASP guidance:

```
[LLMxx-Yn]
```

Where:
- **`LLMxx`** — OWASP Top 10 for LLM Applications 2025 risk number (LLM01–LLM10)
- **`Y`** — MCSS section letter (A through H)
- **`n`** — MCSS field number within that section

### Examples

| Reference | Meaning |
|---|---|
| `[LLM03-B1]` | Supply chain risk / no licence declared (MCSS field B1) |
| `[LLM03-G3]` | Supply chain risk / trust_remote_code undisclosed (MCSS field G3) |
| `[LLM04-C5]` | Data poisoning risk / web scraping undisclosed (MCSS field C5) |
| `[LLM02-C6]` | Sensitive info disclosure / PII risk in training data (MCSS field C6) |
| `[LLM02-C7]` | Sensitive info disclosure / no PII mitigation (MCSS field C7) |
| `[LLM09-F1]` | Misinformation risk / no limitations section (MCSS field F1) |
| `[LLM03-A8]` | Supply chain risk / no responsible disclosure (MCSS field A8) |
| `[LLM04-F3] [LLM06-D6]` | Multiple risks: alignment degradation (MCSS fields F3 and D6) |

Where a finding maps to both an MCSS field and an EU AI Act article, both are included:

```
[LLM04-C1] [Art.53(1)(d)] Training dataset not disclosed
[LLM03-G3] [LLM03-G4] Model requires trust_remote_code=True
[LLM04-F3] [LLM06-F3] [Art.55(1)(a)] Model is explicitly uncensored/unaligned
```

This notation is implemented in the reference scanner (HuggingFace Model Card Security Scanner - available on request) so that every issue in the CSV output carries its MCSS and OWASP reference, enabling direct mapping to this standard and to organisational OWASP compliance programmes.


## 1. Background and Motivation

### 1.1 The model card gap

Model cards were introduced by Mitchell et al. (2019) as short documents accompanying trained ML models, disclosing evaluation conditions, intended use, and known limitations. HuggingFace adopted a loosely structured version of this concept as its README format. However, no formal security-oriented standard has been established, and compliance with even the original academic framework is patchy.

Empirical scanning of the ten most-downloaded text-generation models on HuggingFace (April 2026) found:

- **100%** had no structured YAML metadata block
- **100%** had no declared licence in their model card
- **80%** used confirmed web-scraped training data with no compliance language
- **80%** contained PII-risk training data with no mitigation documentation
- **0%** documented a responsible disclosure mechanism

These models have been downloaded a combined total of over **500 million times**.

### 1.2 The OWASP gap

The OWASP Top 10 for LLM Applications 2025 explicitly identifies model card verification as a mitigation for **LLM03:2025 (Supply Chain)**, stating: *"currently there are no strong provenance assurances in published models. Model Cards and associated documentation provide model information relied upon by users, but they offer no guarantees on the origin of the model."*

OWASP further recommends maintaining attestations via "ML-BOM (Machine Learning Bill of Materials) methodology as well as verifying model cards" as a mitigation for training data poisoning (LLM04). However, OWASP has not defined what a verified model card should contain, or what a pass/fail assessment looks like. This standard proposes to fill that gap.

### 1.3 The regulatory gap

The EU AI Act's GPAI obligations (effective August 2025) mandate specific documentation for general-purpose AI models but apply only to providers placing models on the EU market. The vast majority of models on HuggingFace fall outside this mandatory scope, yet are deployed into EU-market products by downstream integrators who inherit undisclosed risks. A community-driven standard accessible to all model authors, regardless of regulatory jurisdiction, is needed to raise the baseline across the ecosystem.


## 2. Scope

This standard applies to any machine learning model published to a public model repository with a downloadable model card or README. It is specifically designed for:

- Text generation models (LLMs, instruction-tuned models, chat models)
- Embedding and retrieval models
- Image classification and generation models
- Multi-modal models
- Fine-tuned variants of any of the above

It may be applied to any model type where a model card exists or is expected.


## 3. Compliance Tiers

The standard defines three compliance tiers, reflecting proportional obligations based on model capability and deployment context.

| Tier | Criteria | Applies to |
|---|---|---|
| **MCSS-1 (Baseline)** | Any publicly published model | All HuggingFace models, open-source releases |
| **MCSS-2 (Standard)** | Models intended for commercial deployment or integration into products | Models used in production applications |
| **MCSS-3 (Systemic)** | Models trained with ≥10²³ FLOPs; GPAI models under EU AI Act definition | Foundation models, LLMs, multimodal models |

MCSS-3 maps directly to the mandatory EU AI Act Article 53 obligations. MCSS-1 and MCSS-2 establish a community standard for the broader ecosystem where mandatory regulation does not yet apply.


## 4. Required Fields by Tier

Each field is assigned:
- A **tier** at which it becomes required (1, 2, or 3)
- A **check type**: MANDATORY (must be present), CONDITIONAL (required if applicable), or RECOMMENDED
- A **regulatory mapping** to EU AI Act articles, OWASP Top 10 entries, and other frameworks
- A **verification method** for automated tooling

### Section A: Identity and Provenance

| Field | Tier | Type | Description |
|---|---|---|---|
| A1. Model identifier | 1 | MANDATORY | Unique model ID including organisation namespace (e.g. `org/model-name`) |
| A2. Version | 1 | MANDATORY | Version number and date of last update |
| A3. Author / organisation | 1 | MANDATORY | Legal name of the responsible party publishing the model |
| A4. Contact information | 1 | MANDATORY | Email address or URL for security and legal enquiries |
| A5. Base model | 1 | CONDITIONAL | If fine-tuned: name and version of the parent model |
| A6. Model lineage | 2 | MANDATORY | Full lineage chain for fine-tunes (base → intermediate → this model) |
| A7. Changelog | 2 | MANDATORY | Version history documenting material changes |
| A8. Responsible disclosure | 2 | MANDATORY | Mechanism for reporting security vulnerabilities in the model |

**Regulatory mapping:**
- EU AI Act Art. 53(1)(a), Annex XI: technical documentation including model identification
- EU AI Act Art. 53(1)(b): information for downstream providers
- GPAI Code of Practice, Transparency Chapter: Model Documentation Form
- OWASP LLM03:2025: supply chain provenance

### Section B: Licence and Legal Status

| Field | Tier | Type | Description |
|---|---|---|---|
| B1. Licence identifier | 1 | MANDATORY | SPDX licence identifier (e.g. `apache-2.0`, `mit`) or full licence name |
| B2. Commercial use permitted | 1 | MANDATORY | Explicit statement of whether commercial use is permitted |
| B3. Redistribution permitted | 1 | MANDATORY | Explicit statement of redistribution rights |
| B4. Modification permitted | 1 | MANDATORY | Explicit statement on derivative work rights |
| B5. Licence stacking disclosure | 2 | CONDITIONAL | If fine-tuned: confirmation that this model's licence is compatible with base model licence |
| B6. Dataset licence compatibility | 2 | MANDATORY | Confirmation that training dataset licences are compatible with the model licence |
| B7. Copyright policy | 3 | MANDATORY | Policy for respecting copyright in training data including TDM opt-out compliance |

**Regulatory mapping:**
- EU AI Act Art. 53(1)(c): copyright compliance policy
- EU AI Act Art. 53(1)(d): training data summary
- GPAI Code of Practice, Copyright Chapter
- EU Copyright Directive (Directive (EU) 2019/790), Art. 4(3): TDM opt-out mechanism
- California AB 2013 (signed 2024, effective 1 January 2025): disclosure of copyrighted training materials
- OWASP LLM03:2025: licence risk in supply chain

### Section C: Training Data

| Field | Tier | Type | Description |
|---|---|---|---|
| C1. Dataset names | 1 | MANDATORY | Names of all datasets used in pre-training and fine-tuning |
| C2. Data modalities | 1 | MANDATORY | Types of data used (text, image, audio, video, code, etc.) |
| C3. Approximate data size | 2 | MANDATORY | Size of training corpus by modality (token count, file size, or range) |
| C4. Data sources | 2 | MANDATORY | Categories of sources used: publicly available, licensed, scraped, user data, synthetic |
| C5. Web-scraped data disclosure | 2 | CONDITIONAL | If scraped: domains/sources scraped, compliance with robots.txt, TDM opt-outs respected |
| C6. PII risk assessment | 2 | MANDATORY | Assessment of whether training data contains personal data; mitigation steps taken |
| C7. Anonymisation / de-identification | 2 | CONDITIONAL | If personal data present: measures taken (anonymisation, pseudonymisation, filtering) |
| C8. Synthetic data disclosure | 2 | CONDITIONAL | If AI-generated data used: source model, volume, and whether provider ToS permits use |
| C9. Data governance | 3 | MANDATORY | Documented data collection and curation process including quality controls |
| C10. Consent and lawful basis | 3 | MANDATORY | For personal data: lawful basis under GDPR or applicable privacy law |
| C11. GDPR / privacy law compliance | 3 | MANDATORY | Statement of compliance with applicable data protection law; jurisdiction scope |
| C12. Training data summary (GPAI template) | 3 | MANDATORY | EU AI Office template for public summary of training content (Art. 53(1)(d)) |

**Regulatory mapping:**
- EU AI Act Art. 53(1)(d), Annex XI: training data documentation
- EU AI Office Training Data Summary Template (July 2025)
- GDPR Art. 6: lawful basis for processing personal data
- CNIL guidance on legitimate interest for AI training (June 2025)
- ICO web scraping guidance
- OWASP LLM03:2025, LLM04:2025: training data integrity and supply chain

### Section D: Training Procedure

| Field | Tier | Type | Description |
|---|---|---|---|
| D1. Training methodology | 2 | MANDATORY | Description of training approach (pre-training, instruction tuning, RLHF, DPO, etc.) |
| D2. Compute resources | 2 | RECOMMENDED | Hardware used, training duration, energy consumption |
| D3. Training compute (FLOPs) | 3 | MANDATORY | Estimated training compute in FLOPs (required for EU AI Act GPAI threshold assessment) |
| D4. Fine-tuning methodology | 2 | CONDITIONAL | If fine-tuned: hyperparameters, training steps, learning rate, batch size |
| D5. Fine-tuning dataset | 2 | CONDITIONAL | If fine-tuned: dataset used for fine-tuning, separately from pre-training data |
| D6. Alignment methodology | 2 | CONDITIONAL | If RLHF/DPO/RLAIF applied: description of process and reward model |
| D7. Safety training | 2 | MANDATORY | Description of safety training applied (or explicit statement that none was applied) |

**Regulatory mapping:**
- EU AI Act Annex XI: technical documentation including training methodology
- GPAI Code of Practice, Transparency Chapter: Model Documentation Form
- OWASP LLM04:2025: data and model poisoning
- Mitchell et al. (2019): training procedure documentation

### Section E: Capabilities and Evaluation

| Field | Tier | Type | Description |
|---|---|---|---|
| E1. Intended use | 1 | MANDATORY | Primary use cases the model is designed for |
| E2. Out-of-scope use | 1 | MANDATORY | Uses the model is not designed for or that are prohibited |
| E3. Evaluation results | 2 | MANDATORY | Benchmark results on standard evaluations, with methodology described |
| E4. Post-fine-tune evaluation | 2 | CONDITIONAL | If fine-tuned: evaluation results specific to the fine-tuned model (not just base model) |
| E5. Safety evaluation | 2 | MANDATORY | Results of safety evaluations (e.g. TruthfulQA, BBQ, red-teaming outcomes) |
| E6. Bias evaluation | 2 | MANDATORY | Results of bias and fairness evaluations across demographic groups |
| E7. Performance across groups | 3 | MANDATORY | Disaggregated evaluation results across cultural, demographic, and linguistic groups |
| E8. Adversarial robustness | 3 | RECOMMENDED | Results of adversarial testing including prompt injection resistance |
| E9. Model evaluation methodology | 3 | MANDATORY | Description of evaluation process including third-party audits if conducted |

**Regulatory mapping:**
- EU AI Act Art. 53(1)(a), Annex XI: evaluation results
- EU AI Act Art. 55: systemic risk assessment for high-capability models
- Mitchell et al. (2019): benchmarked evaluation across demographic groups
- OWASP LLM01:2025: prompt injection; LLM09:2025: misinformation

### Section F: Risks, Limitations and Safety

| Field | Tier | Type | Description |
|---|---|---|---|
| F1. Known limitations | 1 | MANDATORY | Technical and capability limitations of the model |
| F2. Bias acknowledgement | 1 | MANDATORY | Known or potential biases in model outputs |
| F3. Safety risks | 2 | MANDATORY | Known safety risks and misuse vectors |
| F4. Misuse potential | 2 | MANDATORY | Foreseeable harmful misuse scenarios |
| F5. Risk mitigation measures | 2 | MANDATORY | Steps taken or recommended to mitigate identified risks |
| F6. Content safety | 2 | CONDITIONAL | For generative models: content filtering, output safety measures |
| F7. Dual-use risk | 3 | MANDATORY | Assessment of potential for harmful dual-use applications |
| F8. Systemic risk assessment | 3 | CONDITIONAL | For GPAI with systemic risk: comprehensive risk assessment per Art. 55 |
| F9. Incident reporting | 3 | MANDATORY | Process for reporting serious incidents to the AI Office (Art. 55(1)(b)) |

**Regulatory mapping:**
- EU AI Act Art. 55: systemic risk obligations
- EU AI Act Art. 53(1)(b): downstream provider information
- GPAI Code of Practice, Safety and Security Chapter
- OWASP LLM06:2025: excessive agency; LLM09:2025: misinformation


### Section G: Deployment and Technical Safety

| Field | Tier | Type | Description |
|---|---|---|---|
| G1. Serialisation format | 1 | MANDATORY | Format of model weights (safetensors, gguf, onnx, pytorch_bin, etc.) |
| G2. Unsafe serialisation warning | 1 | CONDITIONAL | If pickle/binary format used: explicit warning and recommendation to use safe alternative |
| G3. trust_remote_code disclosure | 1 | MANDATORY | Explicit statement if model requires `trust_remote_code=True`; description of custom code |
| G4. Custom code audit guidance | 2 | CONDITIONAL | If custom code shipped: guidance on auditing and pinning to specific commit hash |
| G5. Minimum compute requirements | 2 | RECOMMENDED | Minimum hardware requirements for safe inference |
| G6. Recommended deployment configuration | 2 | RECOMMENDED | Guidance on safe deployment including content filtering recommendations |
| G7. Cybersecurity measures | 3 | MANDATORY | Security measures applied to the model and its deployment infrastructure (Art. 55(1)(c)) |

**Regulatory mapping:**
- EU AI Act Art. 55(1)(c): cybersecurity measures for systemic risk models
- OWASP LLM03:2025: supply chain (unsafe serialisation, code execution risk)
- NIST AI RMF: deployment safety


### Section H: Governance and Maintenance

| Field | Tier | Type | Description |
|---|---|---|---|
| H1. Maintenance status | 1 | MANDATORY | Whether the model is actively maintained; expected support period |
| H2. Deprecation policy | 2 | RECOMMENDED | When and how the model will be deprecated; migration guidance |
| H3. Model card update frequency | 2 | MANDATORY | Commitment to keep model card current; date of last update |
| H4. Downstream notification | 3 | MANDATORY | Process for notifying downstream providers of material changes (Art. 53(1)(b)) |
| H5. Documentation retention | 3 | MANDATORY | Commitment to retain documentation for 10 years post-market (Art. 53(1)(a)) |
| H6. Quality control | 3 | MANDATORY | Process for controlling accuracy and integrity of documented information |

**Regulatory mapping:**
- EU AI Act Art. 53(1)(a): documentation retention (10 years)
- EU AI Act Art. 53(1)(b): downstream provider information
- EU AI Office Training Data Summary Template: update requirements (every 6 months)
- GPAI Code of Practice, Transparency Chapter


## 5. Verification Methodology

### 5.1 Automated checks (MCSS-Verify)

The following checks can be performed by automated tooling against publicly available model card content and HuggingFace Hub API metadata:

| Check ID | Field(s) | Method |
|---|---|---|
| V-A1 | A1, A3 | Model ID namespace and author present in metadata |
| V-A2 | A2, H3 | Version string present; last_modified date within 12 months |
| V-A4 | A4 | Email address or URL present in card text |
| V-A5 | A5 | base_model field present in YAML metadata (if fine-tune detected) |
| V-B1 | B1 | SPDX licence identifier present in metadata |
| V-C1 | C1 | datasets field present in YAML metadata or named in card text |
| V-C5 | C5 | Web scraping language detected; compliance signals present/absent |
| V-C6 | C6 | PII-risk dataset names or signals detected |
| V-C8 | C8 | Synthetic data signals detected; commercial provider named |
| V-D4 | D4 | Fine-tuning hyperparameter language present |
| V-E1 | E1 | Intended use section present |
| V-E2 | E2 | Out-of-scope use section present |
| V-E3 | E3 | Evaluation results section present |
| V-F1 | F1 | Limitations section present |
| V-F2 | F2 | Bias acknowledgement language present |
| V-G1 | G1 | File format detectable from repository siblings |
| V-G2 | G2 | Unsafe serialisation files present without safe alternative |
| V-G3 | G3 | trust_remote_code language or custom .py files present |

### 5.2 Manual review triggers

Certain fields cannot be reliably verified by automated tooling and require human review. These should be flagged for manual review when:

- The model scores HIGH or CRITICAL on automated checks
- The model has >100,000 downloads and is MCSS-1 non-compliant on more than 5 fields
- The model is being evaluated for inclusion in a commercial product
- The model operates in a regulated sector (healthcare, finance, legal, HR)

### 5.3 Scoring

Automated compliance scoring maps to four levels:

| Level | Criteria |
|---|---|
| **COMPLIANT** | All MANDATORY fields for the applicable tier are present and populated |
| **PARTIALLY COMPLIANT** | ≥70% of MANDATORY fields present; no CRITICAL gaps (G2, G3, B1, C6) |
| **NON-COMPLIANT** | <70% of MANDATORY fields present, or any CRITICAL gap present |
| **CRITICAL NON-COMPLIANT** | Missing licence + missing training data + trust_remote_code undisclosed, or explicit red-flag language |


## 6. Mapping to OWASP LLM Top 10 (2025)

| OWASP Risk | MCSS Fields | Mitigation |
|---|---|---|
| LLM01 Prompt Injection | E8, F3, F4 | Adversarial robustness evaluation; misuse documentation |
| LLM02 Sensitive Information Disclosure | C6, C7, C10, F3 | PII risk assessment; anonymisation disclosure |
| LLM03 Supply Chain | A1–A8, B1–B7, G1–G4 | Full provenance, licence, serialisation format, trust_remote_code |
| LLM04 Data & Model Poisoning | C1–C12, D1–D7, E3–E5 | Training data transparency; evaluation methodology |
| LLM05 Improper Output Handling | E1, E2, F6 | Intended/out-of-scope use; content safety measures |
| LLM06 Excessive Agency | E1, E2, F4, F7 | Use case constraints; dual-use risk assessment |
| LLM07 System Prompt Leakage | G5, G6 | Deployment configuration guidance |
| LLM09 Misinformation | E3, E4, E5, F1 | Evaluation results; limitations; safety evaluation |
| LLM10 Unbounded Consumption | G5 | Compute requirements disclosed |


## 7. Mapping to EU AI Act

| EU AI Act Requirement | Article | MCSS Fields | Tier |
|---|---|---|---|
| Technical documentation | Art. 53(1)(a), Annex XI | A1–A8, D1–D7, E1–E9 | 3 |
| Downstream provider information | Art. 53(1)(b), Annex XII | A3–A6, B1–B4, E1–E2, G1–G6 | 2–3 |
| Copyright compliance policy | Art. 53(1)(c) | B7, C5 | 3 |
| Public training data summary | Art. 53(1)(d) | C1–C12 | 3 |
| Systemic risk assessment | Art. 55(1)(a) | F7, F8 | 3 |
| Serious incident reporting | Art. 55(1)(b) | F9, H4 | 3 |
| Cybersecurity measures | Art. 55(1)(c) | G7 | 3 |
| Documentation retention (10 years) | Art. 53(1)(a) | H5 | 3 |
| Six-monthly summary updates | AI Office Template | H3, H6 | 3 |


## 8. Mapping to Other Frameworks

| Framework | Relevant MCSS Fields |
|---|---|
| **GDPR** Art. 6 lawful basis | C10, C11 |
| **GDPR** Art. 13/14 transparency | C6, C7, A4 |
| **GDPR** Art. 17 right to erasure | C10, C11 |
| **UK GDPR / DPA 2018** | C10, C11 |
| **CNIL guidance (June 2025)** legitimate interest for AI training | C10, C11 |
| **EU Copyright Directive** (Directive (EU) 2019/790) Art. 4(3) TDM opt-out | B7, C5 |
| **California AB 2013** (signed 2024, effective 2025) copyrighted training data | B7, C1 |
| **Tennessee ELVIS Act** (effective 1 July 2024) voice/likeness rights | C6, C7 |
| **Denmark Digital Identity Law (2025/26)** | C6, C7 |
| **ISO/IEC 42001** AI management system | A7, A8, D7, F8, H5, H6 |
| **NIST AI RMF** | E7, F3–F8, G6 |
| **Mitchell et al. (2019)** original model card framework | E1, E2, E7, F1, F2 |


## 9. Model Card Template

The following is the minimum recommended model card structure for MCSS-2 compliance. Fields marked `[REQUIRED]` are mandatory; `[IF APPLICABLE]` are conditional; `[RECOMMENDED]` are optional but encouraged.

```markdown
---
# YAML metadata block — REQUIRED for all fields
language: [list of languages]
license: [SPDX identifier]        # REQUIRED
base_model: [model ID]            # IF APPLICABLE (fine-tunes)
datasets:
  - [dataset name or HF ID]       # REQUIRED
pipeline_tag: [task type]         # REQUIRED
tags:
  - [relevant tags]
model_version: [version string]   # REQUIRED
last_updated: [YYYY-MM-DD]        # REQUIRED
---

# [Model Name] — Model Card

## Model Identity
- **Model ID:** [org/model-name]
- **Version:** [version]
- **Author / Organisation:** [legal name]          # REQUIRED
- **Contact:** [email or URL]                      # REQUIRED
- **Base model:** [parent model ID and version]    # IF APPLICABLE
- **Last updated:** [date]

## Licence
- **Licence:** [SPDX ID or full name]              # REQUIRED
- **Commercial use:** [Yes / No / Restricted — details]
- **Redistribution:** [Yes / No / Restricted]
- **Modifications:** [Yes / No / Restricted]
- **Dataset licence compatibility:** [statement]   # REQUIRED

## Intended Use
### Intended use cases
[Description of what this model is designed to do]  # REQUIRED

### Out-of-scope use
[Explicit description of uses this model should NOT be used for]  # REQUIRED

## Training Data
- **Datasets used:** [list with links]             # REQUIRED
- **Data modalities:** [text / image / audio / etc.]
- **Approximate corpus size:** [tokens / GB / range]
- **Data sources:** [publicly available / licensed / scraped / synthetic / user data]
- **Web-scraped data:** [Yes / No]                 # REQUIRED
  - If yes: domains, robots.txt compliance, TDM opt-out compliance
- **PII / personal data present:** [Yes / No / Unknown]  # REQUIRED
  - If yes: mitigation measures taken
- **Synthetic data:** [Yes / No]                   # IF APPLICABLE
  - If yes: source model, volume, ToS compliance

## Training Procedure
- **Training methodology:** [pre-training / instruction tuning / RLHF / DPO / etc.]
- **Safety training applied:** [Yes / No / description]  # REQUIRED
- **Fine-tuning details:** [hyperparameters, steps, dataset]  # IF APPLICABLE

## Evaluation
- **Benchmark results:** [table of results with methodology]  # REQUIRED
- **Post-fine-tune evaluation:** [results specific to this model]  # IF APPLICABLE
- **Safety evaluation:** [results of safety benchmarks]        # REQUIRED
- **Bias evaluation:** [fairness evaluation results]           # REQUIRED

## Limitations and Risks
- **Known limitations:** [description]             # REQUIRED
- **Known biases:** [description]                  # REQUIRED
- **Safety risks and misuse potential:** [description]
- **Risk mitigations:** [steps taken or recommended]

## Deployment and Technical Safety
- **Model weight format:** [safetensors / gguf / pytorch_bin / etc.]  # REQUIRED
- **trust_remote_code required:** [Yes / No]       # REQUIRED
  - If yes: description of custom code; guidance on auditing
- **Recommended deployment configuration:** [guidance]

## Governance
- **Maintenance status:** [Active / Passive / Deprecated]
- **Responsible disclosure:** [URL or email for security reports]  # REQUIRED
- **Changelog:** [version history]
```


## 9a. Complete OWASP-MCSS Reference Table

The following table lists every issue the reference scanner can raise, its composite reference tag, and the applicable EU AI Act article where relevant. This table is the normative cross-reference between automated scanner output, MCSS fields, and regulatory obligations.

| Reference Tag | Issue Description | EU AI Act |
|---|---|---|
| `[LLM03-A1]` | No model card found | Art.53(1)(a) |
| `[LLM03-A1]` | No structured YAML metadata block | Art.53(1)(a) |
| `[LLM03-A1]` | Model not found or private (404) | — |
| `[LLM03-A3]` | Unverified author/organisation | Art.53(1)(b) |
| `[LLM03-A3]` | Very low usage metrics / minimal scrutiny | — |
| `[LLM03-A5]` | Base model not disclosed | Annex XI |
| `[LLM03-A5]` | Fine-tune does not disclose base model | Annex XI |
| `[LLM03-A7]` | No versioning or changelog | Art.53(1)(a) |
| `[LLM03-A8]` | No responsible disclosure mechanism | — |
| `[LLM03-B1]` | No licence specified | — |
| `[LLM03-B1]` | Restrictive licence: compliance check required | — |
| `[LLM03-B1]` | Custom/gated licence: review terms | — |
| `[LLM03-B1]` | Unrecognised licence: verify manually | — |
| `[LLM03-B6]` | Dataset licence incompatible with model licence | Art.53(1)(c) |
| `[LLM03-B6] [LLM04-C8]` | Synthetic data from commercial provider (ToS violation risk) | Art.53(1)(c) |
| `[LLM03-C5]` | No scraping compliance language found | Art.53(1)(c) |
| `[LLM03-G1]` | Architecture with known vulnerabilities | — |
| `[LLM03-G2]` | Unsafe serialisation format only (pickle/.bin/.pt) | — |
| `[LLM03-G2]` | Mixed safe/unsafe serialisation formats | — |
| `[LLM03-G3] [LLM03-G4]` | Requires trust_remote_code / custom Python files | — |
| `[LLM03-H1]` | Model not updated in over 2 years | Art.53(1)(a) |
| `[LLM04-C1]` | Training dataset not disclosed | Art.53(1)(d) |
| `[LLM04-C5]` | Training data confirmed web-scraped | Art.53(1)(d) |
| `[LLM04-C5]` | Fine-tune dataset not disclosed | Art.53(1)(d) |
| `[LLM04-C5]` | Indirect internet-scale collection signals | Art.53(1)(d) |
| `[LLM04-C8]` | AI-generated / synthetic training data detected | Art.53(1)(d) |
| `[LLM04-D1]` | Training procedure not described | Annex XI |
| `[LLM04-D4]` | Fine-tuning methodology poorly documented | Annex XI |
| `[LLM04-F3] [LLM02-C6]` | Overfitting / memorisation risk detected | — |
| `[LLM04-F3] [LLM06-D6]` | Fine-tuning has degraded safety alignment | Art.55(1)(a) |
| `[LLM04-F3] [LLM06-F3]` | Model explicitly uncensored or unaligned | Art.55(1)(a) |
| `[LLM02-C6]` | Training data likely contains PII | Art.53(1)(d) |
| `[LLM02-C7]` | No PII mitigation documented | GDPR Art.6 |
| `[LLM05-E1]` | Intended use not described | Art.53(1)(b) |
| `[LLM05-E2]` | Out-of-scope uses not documented | Art.53(1)(b) |
| `[LLM06-F4]` | Red-flag language detected | Art.55(1)(a) |
| `[LLM09-E3]` | No evaluation results or benchmarks | Annex XI |
| `[LLM09-E4]` | No post-fine-tune evaluation results | Annex XI |
| `[LLM09-F1]` | No limitations section | Art.53(1)(b) |
| `[LLM09-F2]` | No bias or fairness discussion | Annex XI |


## 9b. Companion Standard: Dataset Card Security Standard (DCSS)

### Overview

The OWASP Model Card Security Standard addresses documentation at the model level. However, the datasets used to train those models carry their own distinct risks and regulatory obligations. The EU AI Act Art. 53(1)(d) GPAI training data summary template explicitly requires dataset-level disclosure, and HuggingFace explicitly recommends dataset cards as a compliance tool for open-source GPAI providers.

This section defines the companion **Dataset Card Security Standard (DCSS)**, which applies to any dataset published on a public repository that is referenced in a model card as training data. The DCSS uses the same tier and notation conventions as the MCSS, with field identifiers prefixed `DC` and OWASP-MCSS references of the form `[LLMxx-DCn]`.


### DCSS Fields

| Field | Tier | Type | Description |
|---|---|---|---|
| DC1. Dataset card exists | 1 | MANDATORY | A README / dataset card is present for the dataset |
| DC2. Licence declared | 1 | MANDATORY | A licence is explicitly stated (SPDX identifier or full name) |
| DC3. Collection method documented | 1 | MANDATORY | How the data was collected: scraped, curated, licensed, synthesised, crowdsourced |
| DC4. Data source documented | 1 | MANDATORY | Origin of the data: named websites, APIs, platforms, or upstream datasets |
| DC5. Data modalities stated | 1 | MANDATORY | Types of content: text, image, audio, video, code, tabular, etc. |
| DC6. Approximate size stated | 2 | MANDATORY | Size of the dataset: record count, token count, file size, or range |
| DC7. PII / personal data documented | 2 | MANDATORY | Whether personal data is present; what measures were taken if so |
| DC8. Known limitations / biases | 2 | MANDATORY | Known representational gaps, demographic imbalances, or quality issues |
| DC9. Intended use | 2 | MANDATORY | Tasks and domains the dataset is suitable for |
| DC10. Out-of-scope use | 2 | MANDATORY | Uses the dataset is not suitable or should not be used for |
| DC11. Opt-out compliance | 2 | CONDITIONAL | If scraped: robots.txt, TDM opt-outs, and domain exclusions respected |
| DC12. Copyright policy | 2 | CONDITIONAL | If copyrighted material present: lawful basis, licence, or TDM exception documented |
| DC13. Consent / lawful basis | 3 | MANDATORY | For personal data: GDPR lawful basis or equivalent |
| DC14. Illegal content removal | 3 | MANDATORY | Measures taken to detect and remove illegal content before training use |
| DC15. EU AI Act Art. 53(1)(d) fields | 3 | MANDATORY | All categories required by the EU AI Office training data summary template are present |
| DC16. Version history | 2 | RECOMMENDED | Dataset version and changelog documenting material updates |
| DC17. Responsible contact | 2 | RECOMMENDED | Contact for data-related enquiries, takedown requests, and rights-holder queries |


### DCSS Regulatory Mapping

| DCSS Field | EU AI Act | OWASP | Other |
|---|---|---|---|
| DC1. Card exists | Art. 53(1)(d) | LLM04 | — |
| DC2. Licence | Art. 53(1)(c) | LLM03 | California AB 2013 |
| DC3. Collection method | Art. 53(1)(d) | LLM04 | — |
| DC4. Data source | Art. 53(1)(d) | LLM04 | — |
| DC5. Modalities | Art. 53(1)(d) | LLM04 | — |
| DC6. Size | Art. 53(1)(d) | LLM04 | — |
| DC7. PII documentation | Art. 53(1)(d) | LLM02 | GDPR Art. 5, 6, 13/14 |
| DC8. Limitations / biases | Art. 53(1)(b) | LLM09 | Mitchell et al. (2019) |
| DC9. Intended use | Art. 53(1)(b) | LLM05 | — |
| DC10. Out-of-scope use | Art. 53(1)(b) | LLM05 | — |
| DC11. Opt-out compliance | Art. 53(1)(c) | LLM03 | EU Copyright Directive (Directive (EU) 2019/790) Art. 4(3) |
| DC12. Copyright policy | Art. 53(1)(c) | LLM03 | EU Copyright Directive Art. 4(3) |
| DC13. Consent / lawful basis | Art. 53(1)(d) | LLM02 | GDPR Art. 6; CNIL guidance June 2025 |
| DC14. Illegal content removal | Art. 53(1)(d) | LLM04 | DSA obligations |
| DC15. EU Art. 53(1)(d) fields | Art. 53(1)(d) | LLM04 | AI Office Template July 2025 |
| DC16. Version history | Art. 53(1)(a) | LLM03 | — |
| DC17. Responsible contact | — | LLM03 | GDPR Art. 17 (erasure requests) |


### DCSS OWASP-MCSS Reference Tags

| Reference Tag | Issue Description | EU AI Act |
|---|---|---|
| `[LLM04-DC1]` | No dataset card found for referenced dataset | Art. 53(1)(d) |
| `[LLM03-DC2]` | Dataset card has no licence declared | Art. 53(1)(c) |
| `[LLM04-DC3]` | Collection method not documented | Art. 53(1)(d) |
| `[LLM04-DC4]` | Data source not documented | Art. 53(1)(d) |
| `[LLM02-DC7]` | No PII or privacy documentation | GDPR Art. 6 |
| `[LLM09-DC8]` | No limitations or bias documentation | Art. 53(1)(b) |
| `[LLM05-DC9]` | No intended use documentation | Art. 53(1)(b) |
| `[LLM03-DC11]` | No scraping opt-out compliance documentation | Art. 53(1)(c) |
| `[LLM04-DC15]` | Insufficient EU AI Act Art. 53(1)(d) fields | Art. 53(1)(d) |


### DCSS Dataset Card Template

The following is the minimum recommended dataset card structure for DCSS-2 compliance:

```markdown
---
# YAML metadata block
license: [SPDX identifier]          # REQUIRED
task_categories:
  - [task type]                     # REQUIRED
language:
  - [language code]
size_categories:
  - [size range]                    # REQUIRED
---

# [Dataset Name]

## Dataset Description
[Brief description of the dataset and its contents]

## Data Source and Collection
- **Source:** [named websites, APIs, platforms, upstream datasets]   # REQUIRED
- **Collection method:** [scraped / curated / licensed / crowdsourced / synthesised]  # REQUIRED
- **Date collected:** [date or range]
- **robots.txt / TDM opt-outs respected:** [Yes / No / N/A]          # IF SCRAPED

## Licence
- **Licence:** [SPDX ID or full name]                                # REQUIRED
- **Commercial use:** [Yes / No / Restricted]
- **Copyright policy:** [statement of compliance]                    # IF COPYRIGHTED MATERIAL

## Data Modalities and Size
- **Modalities:** [text / image / audio / video / code / tabular]    # REQUIRED
- **Size:** [record count / token count / file size]                 # REQUIRED

## Personal Data and Privacy
- **Personal data present:** [Yes / No / Unknown]                    # REQUIRED
  - If yes: measures taken (anonymisation, de-identification, consent)
- **GDPR lawful basis:** [consent / legitimate interest / etc.]      # IF PERSONAL DATA
- **Illegal content removal:** [measures taken]                      # REQUIRED

## Intended Use
### Suitable for
[Tasks and domains this dataset is appropriate for]                  # REQUIRED

### Not suitable for
[Uses this dataset should not be used for]                           # REQUIRED

## Known Limitations and Biases
[Known representational gaps, quality issues, demographic imbalances]  # REQUIRED

## EU AI Act Training Data Summary
[Narrative description covering the categories required by the EU AI Office
Art. 53(1)(d) template: publicly available datasets, private/licensed datasets,
scraped content (top domains), user data, synthetic data, data processing measures]

## Contact
- **Maintainer:** [name or organisation]
- **Contact:** [email or URL for enquiries, takedown requests]
```


## 10. Relationship to Existing Work

This standard does not replace existing frameworks but synthesises and operationalises them:

- **Mitchell et al. (2019)** — The foundational academic framework. MCSS extends it with security, legal, and supply chain fields.
- **EU AI Act Article 53 / GPAI Code of Practice** — MCSS-3 is designed to be a superset of these mandatory requirements, such that an MCSS-3 compliant card satisfies EU AI Act GPAI obligations.
- **OWASP LLM Top 10 (2025)** — MCSS provides the model-card verification methodology that LLM03 and LLM04 recommend but do not define.
- **ISO/IEC 42001** — MCSS provides the model-level documentation artefact that feeds into the organisational AI management system.
- **HuggingFace Model Card specification** — MCSS is designed to be compatible with HuggingFace's existing YAML metadata format, requiring no changes to repository infrastructure.
- **HuggingFace Dataset Card specification** — The companion DCSS (Section 9b) is similarly designed to be compatible with HuggingFace's existing dataset card format. HuggingFace has explicitly positioned dataset cards as a compliance tool for EU AI Act GPAI obligations.


## 11. Implementation Guidance

### For model authors
Complete at minimum the MCSS-1 required fields for any publicly published model. Use the template in Section 9 as your starting point. The YAML metadata block is the highest-priority element: it enables automated compliance checking.

### For organisations deploying models
Before deploying any model into a production application, verify the model card against the MCSS-2 fields relevant to your deployment context. Models with CRITICAL NON-COMPLIANT status should be treated as unvetted and subjected to additional scrutiny.

### For security teams and auditors
Use the automated verification checks in Section 5.1 to triage large model inventories. Apply manual review to models flagged for HIGH or CRITICAL risk, models in regulated sectors, and models with high download counts.

### For HuggingFace and model repositories
Consider surfacing MCSS compliance as a repository quality signal, in the same way that npm surfaces licence information. Requiring the YAML metadata block (and particularly the `license`, `datasets`, and `base_model` fields) for models to appear in default search results would rapidly improve ecosystem-wide compliance.

## 12. Governance and Contribution

This document is submitted as a community proposal to the OWASP GenAI Security Project. It is intended to evolve through the standard OWASP community process:

1. **Draft (current)** — open for community comment
2. **Review** — structured feedback period with OWASP GenAI working group
3. **Pilot** — validation against real model inventories
4. **Release candidate** — incorporation of pilot feedback
5. **Standard** — formal OWASP publication

Contributions, counterexamples, and implementation experience are welcomed via the OWASP GenAI Security Project GitHub and Slack (`#project-top10-for-llm`).


## 13. Authors and Acknowledgements

**Proposed by:** Liam Romanis
**Contact:** [linkedin.com/in/checkteamleader](https://www.linkedin.com/in/checkteamleader/)
**Based on:** Empirical analysis using the HuggingFace Model Card Security Scanner (April 2026)

**Referenced frameworks and standards:**
- Mitchell, M. et al. (2019). "Model Cards for Model Reporting." Proceedings of the ACM Conference on Fairness, Accountability, and Transparency (FAccT). doi:10.1145/3287560.3287596
- OWASP Top 10 for LLM Applications 2025. OWASP GenAI Security Project.
- OWASP LLM Security Verification Standard (LLMSVS).
- EU AI Act, Regulation (EU) 2024/1689, Articles 53 & 55, Annexes XI & XII.
- GPAI Code of Practice, European AI Office (published 10 July 2025; formally approved 1 August 2025).
- EU AI Office Template for Public Summary of Training Content (published 24 July 2025, pursuant to Art. 53(1)(d) of Regulation (EU) 2024/1689).
- CNIL (Commission nationale de l'informatique et des libertés). Guidance on Legitimate Interest as Legal Basis for AI Model Training, 17 June 2025.
- ISO/IEC 42001:2023. "Information technology — Artificial intelligence — Management system." International Organization for Standardization, December 2023.
- NIST AI Risk Management Framework (AI RMF 1.0), National Institute of Standards and Technology, January 2023.


*This document is released under Creative Commons Attribution-ShareAlike 4.0 (CC BY-SA 4.0), consistent with OWASP licensing.*
