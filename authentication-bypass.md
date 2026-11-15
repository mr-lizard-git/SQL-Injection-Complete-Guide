# Authentication Logic and SQL Injection

## Why login queries can be sensitive

A login handler often checks whether a username and password match a row. If either value is concatenated into SQL, attacker-controlled syntax may change the predicate. The issue is not a magic bypass string; it is a failure to keep data separate from query structure.

Conceptual unsafe form:

```sql
SELECT user_id FROM users
WHERE username = '<input>' AND password = '<input>'
```

This illustration omits hashing, schema, and driver details. Modern applications should verify a password hash in application code after retrieving a candidate user with a parameterized username lookup, or use another well-reviewed authentication mechanism. They should not build a SQL predicate from raw credentials.

## Controlled validation

In PortSwigger Academy or a local DVWA setup, use a lab-provided account and exercise. Compare normal failure behavior with the lab's controlled test. Check whether the response establishes an authenticated session, not merely a changed message or status code. Verify access only to a benign page intended by the lab.

A different error message can be caused by validation or account state and is not proof of bypass. A success response without a valid authenticated session is also insufficient. Never test this on a real login endpoint without explicit authorization.

## Impact and limitations

A demonstrated bypass may undermine authentication for the affected flow, but the scope depends on query, account privileges, session handling, and other controls. Do not infer administrator access, arbitrary account access, or data exposure without evidence. Avoid changing passwords, accessing other users' data, or creating persistent accounts during validation.

## Remediation

Bind username and other values as parameters. Store salted password hashes using a purpose-built password hashing algorithm and verify them with the platform's safe password verification function. Apply generic login errors and rate limiting as supplementary controls. Neither hashing nor rate limiting fixes SQL injection; query parameterization does.
