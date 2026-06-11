# ai-llm-security-audit

Ten sections covering the practical attack surface of LLM-backed applications: prompt injection, output handling, model supply chain, agent tool use, and evaluation. Each section is structured for use in an actual audit, not a survey.

## Section structure

Every section has four parts:

1. **Risk surfaces**, where the vulnerability lives in a typical LLM stack
2. **Audit checklist**, questions to ask the team and answers to look for
3. **Specific tests**, concrete prompts, payloads, or queries to run
4. **Mitigations**, controls that reduce exposure, ordered by impact

## Contents

| # | Section | Focus |
|---|---------|-------|
| 01 | [Direct prompt injection](sections/01-direct-prompt-injection.md) | User input that overrides system prompts |
| 02 | [Indirect injection](sections/02-indirect-injection.md) | Injection via retrieved documents, web pages, emails |
| 03 | [Jailbreaks](sections/03-jailbreaks.md) | Pattern-based bypass of safety training |
| 04 | [RAG risks](sections/04-rag-risks.md) | Retrieval augmentation pipelines: poisoning, data leakage, embedding attacks |
| 05 | [Output handling](sections/05-output-handling.md) | Treating model output as untrusted: HTML rendering, code execution, SQL |
| 06 | [Model supply chain](sections/06-model-supply-chain.md) | Compromised weights, tokenizers, framework dependencies |
| 07 | [Training data](sections/07-training-data.md) | Poisoning, membership inference, PII leakage from training corpora |
| 08 | [Agent tool abuse](sections/08-agent-tool-abuse.md) | Confused-deputy, scope creep, lateral movement via tools |
| 09 | [Multimodal](sections/09-multimodal.md) | Image, audio, and document inputs as injection vectors |
| 10 | [Eval and red-team](sections/10-eval-and-redteam.md) | Building useful evals, structured red-team programs |

## Intended use

Use this when you are auditing an LLM application, your own or a vendor's. The checklist questions are written so you can paste them into a security review template; the tests are written so you can run them against a staging environment without setup.

This is not OWASP LLM Top 10 reformatted. It overlaps in topic but is structured for audit work rather than awareness, with specific tests that map to each finding.

## Contributing

If a test stops working because a model provider patched it, open a PR with the new variant. If you find a class of attack not covered, open an issue first so we can place it in the right section.

## Related repositories

Part of a 10-repo security audit set.

Browser-based audit tools:
- [iam-policy-analyzer](https://github.com/0xelitesystem/iam-policy-analyzer)
- [terraform-security-linter](https://github.com/0xelitesystem/terraform-security-linter)
- [kubernetes-manifest-security-scanner](https://github.com/0xelitesystem/kubernetes-manifest-security-scanner)
- [session-cookie-auditor](https://github.com/0xelitesystem/session-cookie-auditor)
- [regex-redos-checker](https://github.com/0xelitesystem/regex-redos-checker)

Reference collections:
- [incident-response-runbooks](https://github.com/0xelitesystem/incident-response-runbooks)
- [api-security-audit-checklist](https://github.com/0xelitesystem/api-security-audit-checklist)
- [secrets-leak-response-runbook](https://github.com/0xelitesystem/secrets-leak-response-runbook)
- [threat-modeling-worksheets](https://github.com/0xelitesystem/threat-modeling-worksheets)

## License

MIT. See [LICENSE](LICENSE).
