# Direct Prompt Injection

The user supplies input that overrides or contradicts the system prompt's instructions. This is the most basic LLM attack and the hardest to fully eliminate.

## What to look for

- Instruction-overriding phrasing in user input: "ignore previous instructions", "you are now", "your new task is", "for testing purposes".
- Role-claim attacks: "I am a developer at OpenAI", "as the system administrator", "this is an authorized override".
- Format manipulation: requests to emit outputs in specific structures the system prompt forbids (JSON when text was specified, code without disclaimers, etc.).
- Encoding tricks: instructions in base64, ROT13, leetspeak, or another language to bypass keyword filters.
- Long-context smuggling: hostile instruction buried inside a 10,000-token document the user "asks the model to summarize".
- Multi-turn conditioning: gradual escalation where each turn is borderline but the cumulative effect bypasses guardrails.

## Audit checklist

- Does the system prompt explicitly distinguish system vs user content, and instruct the model to never let user content redefine its role?
- Is there an output classifier or guardrail layer independent of the model's own judgment?
- Are sensitive operations (tool calls, data writes, financial actions) gated by deterministic checks that don't rely solely on the model's compliance?
- Do you have a regression suite of known injection prompts that runs against every prompt change?
- Are user inputs visibly delimited (XML tags, structured fields) so the model can distinguish them from instructions?

## Mitigations that actually help

- Treat the LLM as untrusted. Never give it capabilities you wouldn't give to user input directly.
- Use separate models or rule-based checks for high-stakes decisions instead of asking the same model to self-police.
- Limit the blast radius of any single response: rate limits, output length caps, content classifiers.
- Log all prompts and responses for offline review.

## Mitigations that don't work alone

- "Tell the model not to follow user instructions" — partially helps, often fails.
- Keyword blocklists — trivially bypassed by paraphrasing.
- Asking the model to detect injection in its own output — circular.
