# Security test matrix

This matrix is based on Step 3 and Step 5 of the supplied project pages. The expected outcomes below describe the intended design, not test results already observed.

| Test | Identity / action | Intended result | Evidence to capture |
|---|---|---|---|
| Developer bucket listing | Developer lists the project bucket | Allowed for the intended project bucket | IAM Policy Simulator result and controlled console/CLI test |
| Developer object read | Developer downloads a test object | Allowed | Successful read evidence |
| Developer object write | Developer uploads a test object | Allowed only if the Developer role is intended to be read/write and the effective policy grants it | Policy evaluation and upload result |
| Auditor bucket listing | Auditor lists the project bucket | Allowed | IAM Policy Simulator result and controlled test |
| Auditor object read | Auditor downloads a test object | Allowed | Successful read evidence |
| Auditor object write | Auditor attempts to upload a test object | Denied under the intended read-only design | Explicit access-denied result |
| Unauthorized access | Unauthorized identity attempts list/read/write | Denied | Access-denied evidence for each tested action |
| Public access | Unauthenticated client attempts object access | Denied | Block Public Access and controlled request result |
| Insecure transport | Test request uses HTTP | Denied by the HTTPS-only bucket policy | Access-denied result; do not send credentials over HTTP |
| Object deletion | Controlled identity deletes a disposable test object | Event should be recorded if the relevant CloudTrail S3 data event selector and logging are enabled | CloudTrail event and related metric/alarm state |
| Bucket ACL change | Controlled test of a non-production bucket ACL | Restricted or detected according to the configured policy and monitoring | API result and CloudTrail/CloudWatch evidence |
| Console sign-in without MFA | Controlled test identity without MFA | Sign-in policy and alert behavior must be verified | CloudTrail sign-in event and alarm evidence |
| Alert delivery | Trigger a configured test event | Matching metric/alarm and notification should occur | Metric, alarm history and received SNS notification |
| Version recovery | Upload two versions of a disposable object | Earlier version can be restored if versioning is enabled | Object versions and recovery result |
| Encryption check | Upload a disposable object | Object is encrypted using the configured default encryption | Object properties or API response showing encryption |

## Test execution record

For every test, record: date/time; AWS region; test identity (redacted); resource (redacted); exact action; expected result; actual result; CloudTrail event ID if available; metric/alarm state; remediation; retest result; screenshot/file reference.

**Important:** The current Developer policy JSON does not show standard S3 `s3:PutObject`; the current Auditor policy uses `Resource: "*"`. Evaluate the actual effective permissions before claiming the intended role matrix has been implemented.

Use a disposable bucket/object and test identities. Do not make a production bucket public or delete important data just to generate an alert.