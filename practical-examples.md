# Practical Examples (Training Labs)

These examples are intentionally schematic. Run them only in the named lab or an isolated application you own. Do not copy a lab request to an unrelated host. The lab's DBMS, query context, encoding, and filtering determine the exact syntax.

## Burp Community workflow

1. Start Burp Suite Community Edition and use its built-in browser, or configure a browser to use Burp Proxy.
2. Open a PortSwigger SQLi lab or local DVWA instance and complete the normal action once. Keep the request in Proxy HTTP history.
3. Send the request to Repeater. Preserve cookies, CSRF tokens, and unrelated fields; edit one input at a time.
4. Compare baseline and test responses: status, response length, rendered content, redirects, and timing. Repeat suspicious observations.
5. Save only the minimal evidence needed for a finding. Redact credentials, session tokens, and personal data.

Burp Community supports a manual workflow. Feature availability can differ from Professional; these examples need no automated scanner.

## Example A: Boolean behavior in a category filter

Suppose the lab's intended request is conceptually `GET /products?category=books`. The application may issue a query such as:

```sql
SELECT name, description FROM products WHERE category = '<value>'
```

In the lab, compare a normal value with a value that makes the predicate logically true and one that makes it false, using the syntax taught by that lab. If the true case consistently resembles the baseline while the false case differs, that is evidence of a boolean oracle. It is not enough that one response happens to be shorter: repeat with controls and account for caching, pagination, and dynamic page content.

**Conclusion:** report the parameter and repeatable differential. Do not claim data extraction or a specific database unless separately demonstrated.

## Example B: UNION result visibility

Conceptually, the application's query returns two columns:

```sql
SELECT name, price FROM products WHERE category = '<value>'
```

A compatible UNION must return two columns, and their types must be compatible by position. In a training lab, use a harmless constant marker in successive candidate positions and inspect the page for the marker. If the marker is shown, it identifies an output position. If not, possible causes include wrong count, incompatible types, non-rendered rows, query context, or filtering.

Do not use this demonstration to enumerate or retrieve real account data. PortSwigger's lab provides a controlled target and objective.

## Example C: Authentication query logic

A vulnerable login can be conceptualized as:

```sql
SELECT id FROM users WHERE username = '<user>' AND password = '<password>'
```

If input can alter the predicate, authentication decisions may be affected. In a lab, use the exact exercise instructions and a test account. A successful session demonstrates impact in that lab only. See [authentication-bypass.md](authentication-bypass.md).

## Example D: Conditional timing

A time-based lab may let a tester compare a normal request with a conditional database operation that sleeps only when a predicate is true. Use a short, lab-prescribed delay and repeat both conditions. A single slow request is not reliable evidence; network jitter, server load, throttling, and queues affect timing.

## Lab selection

- [PortSwigger Web Security Academy: SQL injection](https://portswigger.net/web-security/sql-injection): guided labs for error, UNION, blind, and authentication cases.
- [DVWA](https://github.com/digininja/DVWA): run locally in an isolated environment and select a security level deliberately. It is intentionally vulnerable and should not be exposed to the public internet.

## Recording an example

For each exercise, note: authorized lab and objective; endpoint and parameter; baseline request; exact test variation; repeat count; observed response difference; interpretation and confidence; remediation. Keep the report reproducible without including unnecessary extracted data.
