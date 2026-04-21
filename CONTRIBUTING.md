# Contributing to the OWASP Model Card Security Standard

First off — thank you for taking the time to contribute. This is an early-stage project and input from people with real-world experience of AI security, compliance, and ML engineering is exactly what will make it useful.

---

## Ways to contribute

You don't need to be a security expert or write code to contribute. Here are some of the most valuable things you can do:

**Review and critique the standard**  
Read through `standard/model-card-security-standard.md` and tell us what's wrong, missing, or unclear. Raise a GitHub issue with the label `feedback`. Blunt is fine — we'd rather know now than after this gets adopted somewhere.

**Share real-world examples**  
If you've filled in a model card (or tried to and found the process awkward), we'd love to hear about it. Anonymised examples are absolutely fine. Open an issue or drop into the OWASP Slack channel.

**Improve the template or worked example**  
If fields are ambiguous, guidance is missing, or the example doesn't reflect realistic scenarios, submit a pull request against `templates/model-card-template.md` or `examples/llm-example.md`.

**Extend the framework mappings**  
If you work with a framework we haven't mapped (ISO 42001, SOC 2, HIPAA, FCA guidance, sector-specific regulation) and can see how the model card relates to it, we'd very much like to add it. See `mappings/owasp-nist-euaiact-mapping.md` as a reference for the format.

**Improve the JSON schema**  
If you're implementing tooling against the schema and find it lacking, raise an issue or PR against `schema/model-card-schema.json`.

---

## Ground rules

This project follows the [OWASP Code of Conduct](https://owasp.org/www-policy/operational/code-of-conduct). In short: be respectful, assume good faith, and keep discussion focused on the work.

We also ask that contributions are your own original work, or that you have the right to contribute them under the project licence (CC BY 4.0 for documentation, Apache 2.0 for any tooling).

---

## How to raise an issue

1. Check whether the issue already exists before opening a new one
2. Use a clear, descriptive title
3. Describe the problem or suggestion with enough context for someone unfamiliar with your specific situation to understand it
4. Use labels where appropriate: `feedback`, `bug`, `enhancement`, `question`, `good first issue`

---

## How to submit a pull request

1. Fork the repository
2. Create a branch from `main` with a descriptive name, e.g. `add-iso42001-mapping` or `fix-pii-field-guidance`
3. Make your changes
4. Write a clear PR description explaining what you've changed and why
5. Submit the PR — a project maintainer will review it and get back to you

For significant changes (new sections, structural changes to the schema, new framework mappings) it's worth opening an issue for discussion before putting in the effort of a full PR, just to make sure we're aligned.

---

## Get in touch

The best place for informal discussion is the OWASP Slack workspace, in the `#ai-ml-security` channel. You can join OWASP Slack at https://owasp.org/slack/invite.

For anything security-related (e.g. if you've found a problem with this project itself rather than the standard), please raise a GitHub issue marked `security`.

---

## Licence

By contributing to this project, you agree that your contributions will be licensed under the same licence as the project — CC BY 4.0 for documentation and Apache 2.0 for any code or tooling.
