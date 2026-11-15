# SQL Injection Testing Methodology

## 1. Establish scope

Confirm target hosts, accounts, time window, permitted techniques, data-handling rules, and stop conditions. Use the training labs referenced in this project for practice. Do not test an out-of-scope host or infer authorization from public availability.

## 2. Map inputs and establish baselines

Browse the authorized application normally. Record parameters in URL paths, query strings, forms, JSON, cookies, and relevant headers. Note expected behavior and repeat one representative request to understand normal variation. Avoid changing multiple values at once.

## 3. Probe minimally

Use a harmless syntax perturbation or logical comparison appropriate to the parameter context. Begin with the least intrusive test. Watch for response changes, errors, content, redirects, and timing. A failure to observe an effect is not proof of safety: filters, parameterization, error handling, and context can obscure signals.

## 4. Confirm with a second observation

Repeat a candidate signal with matched controls. Prefer a benign marker or true/false differential over data retrieval. For timing, alternate baseline and conditional requests and keep the test rate low. Consider caches, session state, CSRF tokens, load balancers, and validation as alternative explanations.

## 5. Characterize only what is needed

Determine injection context and whether results/errors are observable only to the extent needed to substantiate risk. Use UNION compatibility or a blind oracle in a training lab when the exercise requires it. Do not dump tables, access unrelated records, or pursue privilege escalation when a minimal proof already establishes the finding.

## 6. Report clearly

Include affected endpoint and parameter; prerequisites; concise sanitized request/response evidence; repeatability; demonstrated impact and limits; confidence; remediation based on parameter binding; and retest guidance. Redact tokens, passwords, personal information, and unrelated response data.

## Common mistakes and limitations

- Calling any 500 response SQLi without a control.
- Assuming database type from a single error string or syntax response.
- Ignoring encoding, quoting context, and application validation.
- Mistaking dynamic content, caching, or session state for a boolean oracle.
- Treating one slow response as time-based evidence.
- Overusing payload lists without a hypothesis.
- Continuing into sensitive data access after the finding is established.
- Claiming that a WAF or generic errors prove a query is safe.
- Treating a negative black-box test as proof that all code paths are safe.

## Burp Suite Community Edition notes

Use Proxy history to capture the normal request, Repeater to make controlled edits, and the response comparison features available in your installed edition. Burp's built-in browser simplifies proxy setup. Manual Community Edition testing is sufficient for these exercises; do not imply that an automated scan was run. Keep the scope restricted to the lab or explicitly authorized target.
