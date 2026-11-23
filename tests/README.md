# Security test evidence

No IAM Policy Simulator results or end-to-end security-test screenshots were supplied in the evidence reviewed so far.

Record tests only after running them. Suggested test records:

| Test | Identity / method | Expected result | Actual result | Evidence |
|---|---|---|---|---|
| Developer object upload | Developer identity | As granted by its policy | Pending | Add screenshot |
| Auditor object read | Auditor identity | As granted by its policy | Pending | Add screenshot |
| Auditor write/delete attempt | Auditor identity | Denied if read-only | Pending | Add screenshot |
| Insecure HTTP request | Test client | Denied by HTTPS-only policy | Pending | Add screenshot |
| Public object access | Unauthenticated client | Denied only if public access controls prevent it | Pending | Add screenshot |
| Object-version recovery | Authorized test identity | Earlier version retrievable if versioning is enabled | Pending | Add screenshot |

Also save IAM Policy Simulator allow/deny results. The simulator evaluates IAM policy logic; it does not prove every live request outcome because other AWS controls can affect access.

Do not upload secrets, access keys, session tokens, passwords or unredacted customer data.
