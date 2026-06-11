# Evaluation and Red-Teaming

Security audit is a process, not a one-time check. The model, the prompt, the dependencies, and the threat landscape all change. Continuous evaluation is the only durable mitigation.

## What to evaluate

- **Refusal behavior** on a curated set of prohibited requests.
- **Jailbreak robustness** against current public jailbreak techniques.
- **Prompt injection resistance** for direct and indirect cases.
- **Tool use safety**, agentic scenarios where the model could be manipulated into harm.
- **Output safety**, does the model emit harmful content unprompted?
- **Hallucination rates** for factual claims.
- **Bias and fairness** for the deployment domain.
- **Performance regressions** as prompts and model versions change.

## Building an eval suite

- Start with a small, focused set you can run on every prompt change.
- Categorize by risk: critical (blocks deployment), warning (review needed), info (track over time).
- Include both positive cases (model should do X) and negative cases (model should refuse Y).
- Version your eval suite. When you add a case, note why.
- Run evals in CI. Deployment should fail on regressions in critical cases.

## Red team approach

- Periodic structured exercises against a deployed system (or staging clone with realistic data).
- Mix of internal staff and external testers; external is better at finding novel attacks.
- Include domain experts: a security pro for security risks, a clinician for medical AI, etc. Generic red-teamers miss domain-specific harms.
- Document findings, severity, and remediation. Track time-to-fix.

## Reporting and feedback

- Mechanism for users to report problems (button in UI, email address, support ticket category).
- Internal triage process: who looks at reports, with what SLA?
- Loop findings back into the eval suite as regression cases.

## Audit checklist

- Is there an eval suite that runs on prompt and model changes?
- What's the cadence of red-team exercises? Quarterly is a reasonable baseline; high-stakes systems need more.
- Are red-team findings tracked through to remediation, or are they dropped?
- Is there a public-facing security disclosure process for AI-specific issues?
- For systems with users: is there a clear path for users to report issues, and is it acted on?

## Pitfalls

- "We tested 100 prompts and they were fine", proves nothing if attackers can generate the 101st.
- Confusing benchmark performance with safety. SOTA on MMLU tells you nothing about jailbreak resistance.
- Self-evaluation: asking the model to grade its own outputs is unreliable for the cases that matter.
- Stale evals: techniques from 2023 don't catch 2025 attacks.

## Closing note

Treat AI security like web application security circa 2005, the field is young, defenses are incomplete, and the published attacks are a small sample of the working ones. Plan for a steady stream of issues over years, not a final secured state.
