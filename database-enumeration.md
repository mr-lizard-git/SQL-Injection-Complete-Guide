# DBMS Identification and Enumeration

Enumeration is a follow-on activity, not the first test. In a professional assessment, identify only what is necessary to validate the issue and explain risk. Prefer non-invasive evidence and stop before accessing sensitive records.

## Identify cautiously

Potential clues include verbose error wording, known function behavior, driver error formats, and lab documentation. Each clue can be masked, customized, or produced by middleware. A database banner or version string should be treated as a hypothesis unless confirmed through authorized, corroborating evidence. Report uncertainty explicitly.

## Metadata concepts

DBMSs expose schema information through different catalogs or metadata views. Names, privileges, and query syntax differ. An application account may not be allowed to read all metadata. A denied metadata query does not disprove SQLi; successful access to a catalog does not imply access to every table.

For a training exercise, learn the metadata path from the lab's explanation and use it only to satisfy that lab objective. Do not provide a universal enumeration payload: query context, DBMS, and permissions make such a payload unreliable and potentially harmful.

## Column count and UNION compatibility

A UNION requires the same number of selected columns on both sides and compatible types by ordinal position. In a lab, testers commonly establish column count with a harmless NULL-based probe or an ORDER BY position technique where supported. These methods have dialect and context caveats. A failure may indicate syntax context or filtering rather than a guessed count being wrong.

Once the count is established, use non-sensitive constants to locate output positions. A successful visible marker demonstrates result rendering in that context; it does not justify dumping tables. Stop and document.

## Safe reporting

Record the evidence supporting DBMS identification separately from the fact of injection. Avoid statements such as “the server is MySQL” based only on a generic error or a payload that failed. State “behavior is consistent with…” where evidence is incomplete. Never include real user records, hashes, or secrets in portfolio artifacts.
