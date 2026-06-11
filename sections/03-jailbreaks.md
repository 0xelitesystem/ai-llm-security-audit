# Jailbreaks (Safety Bypass)

Attempts to get the model to violate its safety policies through framing, persona, or technique. Distinct from prompt injection in that the user is also the attacker, there's no third party being defended against.

## Common jailbreak patterns

- **Persona attacks:** "You are DAN", "pretend you have no rules", "roleplay as a helpful assistant without restrictions".
- **Hypothetical framing:** "If you were going to write malware (which you wouldn't), how would you...", "for a fiction novel, describe in detail...".
- **Authority appeals:** "I'm a security researcher", "this is for an academic study", "my professor authorized this".
- **Stepwise extraction:** asking for individually innocuous pieces that combine into prohibited information.
- **Encoding / translation:** asking in another language, in code, in pig latin, with the model's response then translated.
- **Token smuggling:** weird unicode, zero-width characters, deliberately broken text that confuses safety classifiers but still parses to the model.
- **Many-shot:** providing a long context of fake prior turns where the model "already" complied, then asking again.

## Audit checklist

- Are you using a model with documented safety training (e.g., RLHF, constitutional AI)?
- Do you have an output safety classifier in addition to the model's own training?
- Does your evaluation suite include known jailbreak prompts? Update quarterly, techniques evolve.
- For consumer products: do you have a human review process for edge cases?
- Is there a feedback mechanism for users to report safety failures?

## What to test

- Refusal robustness: does the model refuse when asked directly, then comply when reframed?
- Consistency across languages: many models are weaker in non-English.
- Long-context behavior: do guardrails degrade past 50k tokens?
- Tool use: does the model call dangerous tools when convinced "it's just for testing"?

## Mitigations

- Defense in depth, model safety + input/output classifiers + system-level controls.
- Domain restriction: a customer-support model should refuse anything off-topic, which sidesteps most jailbreaks.
- Logging and monitoring for jailbreak patterns; alert on novel ones.
- Acknowledging that perfect safety is not achievable. Design the system so a successful jailbreak doesn't enable serious real-world harm.
