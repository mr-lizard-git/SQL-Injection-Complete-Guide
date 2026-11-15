# Prevention and Secure Query Design

## 1. Parameterize values

Keep SQL structure fixed and bind each dynamic value through the database driver's parameter API.

Unsafe pattern:

```text
sql = "SELECT id FROM users WHERE email = '" + email + "'"
```

Safer pattern (pseudocode):

```text
statement = prepare("SELECT id FROM users WHERE email = ?")
statement.bind(1, email)
execute(statement)
```

Use the framework's documented placeholders and binding calls. Do not interpolate a value into the SQL string before preparing it. Parameters are for values, not table names, column names, sort directions, or arbitrary SQL fragments.

## 2. Map dynamic structure to fixed choices

If a user may select a sort field or direction, map the request to a small application-owned set of literal SQL fragments, reject unknown choices, and bind all values. Do not assume quoting an identifier authorizes arbitrary user-selected identifiers.

## 3. Review every query path

Search for concatenation and interpolation in raw SQL, ORM query languages, reports, search filters, stored procedures, and background jobs. A parameterized call to a stored procedure does not make unsafe dynamic SQL inside that procedure safe. Review second-order flows where stored untrusted data is later used to build query text.

## 4. Defense in depth

- Give each application database account only the permissions it needs; separate read and write roles where practical.
- Avoid returning unnecessary columns and rows.
- Keep detailed SQL errors in access-controlled logs; return generic errors to clients.
- Use input validation for business rules and types, while still binding values.
- Add code review guidance and regression tests for query construction.
- Keep database drivers and frameworks supported and configured securely.

Escaping, filtering, WAF rules, and error suppression are not substitutes for parameterization. A stored procedure is not automatically safe if it concatenates dynamic SQL. Least privilege reduces impact but does not remove the vulnerability.

## Review checklist

- Are all untrusted values passed through bind variables?
- Are dynamic identifiers drawn from fixed allow-lists?
- Do stored procedures construct dynamic query text? If so, are values bound internally?
- Are errors generic externally and useful in protected logs?
- Does the application account have only required permissions?
- Does a regression check exercise the affected request path?

See the OWASP SQL Injection Prevention Cheat Sheet for language and framework examples.
