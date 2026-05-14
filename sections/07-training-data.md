# Training Data and Fine-Tuning

If you train or fine-tune models, the training pipeline is in scope for security review. Even if you only use vendor APIs, your prompt history may be used for vendor training, which has its own implications.

## Risk surfaces

- **Data poisoning:** an attacker contributes training examples that cause specific misbehavior. Requires write access to your training set, or influence over a public corpus you ingest.
- **Privacy leakage:** sensitive data in training set is memorized and emitted at inference time. Especially likely for rare/unique strings (PII, secrets, internal URLs).
- **Membership inference:** an attacker can determine whether a specific record was in the training set, with privacy implications.
- **Vendor data retention:** when you call a vendor API, what happens to the prompts and responses? Does the vendor train on them? For how long are they retained?
- **PII in fine-tuning data:** fine-tuning datasets often come from production logs and contain PII the model will later regurgitate.

## Audit checklist

- Who can write to your training dataset(s)? Are changes reviewed?
- Is training data scrubbed of secrets, PII, internal hostnames, customer-specific identifiers before training?
- For fine-tuning from production logs: is there a redaction pass? Is consent established?
- For vendor APIs: read the data processing terms. Is there a "no training" option (often called "zero data retention" or similar) and have you opted in?
- Is your evaluation set held out and never seen during training? (Data leakage from eval to train inflates metrics.)
- Do you have a process to remove individual records on request (relevant for GDPR right-to-erasure)?

## Specific tests

- Pick known-rare strings from your training data. Prompt the model to elicit them. If it can reproduce them verbatim, you have memorization.
- Try membership inference: query for confidence on records you know were in vs records you constructed but were not.
- Search vendor logs for instances where customers have submitted obvious secrets. (You'll find some.)

## Mitigations

- DLP scanning on training data ingestion.
- Deduplication: heavy duplicates increase memorization.
- Differential privacy in training (where the privacy/utility tradeoff is acceptable).
- For vendor APIs: select zero-retention tiers for sensitive data; use redaction proxies.
- Don't fine-tune on raw production data. Synthesize / sample / redact first.
