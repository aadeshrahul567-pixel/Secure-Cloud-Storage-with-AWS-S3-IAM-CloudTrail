# Project report template

Use this document to assemble the final Cloud Security Analyst project report. Fill in actual implementation details and test outcomes; do not treat planned actions as completed results.

## 1. Project overview

- **Project:** Secure Cloud Storage with AWS S3, IAM and CloudTrail
- **Role:** Cloud Security Analyst
- **Objective:** Secure file storage, apply least-privilege access, monitor activity and detect/remediate security issues.
- **Tools listed in project plan:** AWS S3, IAM, CloudTrail, IAM Policy Simulator/Validator and S3 Storage Lens. CloudWatch Logs/metrics/alarms and SNS appear in the monitoring workflow.

## 2. Architecture diagram

Include the intended flow from [architecture.md](architecture.md). Label planned components separately from components confirmed operational.

## 3. Implementation steps

Document:
1. Environment setup: AWS account, root-user MFA, separate IAM administrator, multi-region CloudTrail and S3 bucket.
2. S3 hardening: Block Public Access, versioning, SSE-S3 or SSE-KMS, restrictive bucket policy and HTTPS-only requests.
3. IAM: Developer, Auditor and Unauthorized User groups; policies; group attachments; policy simulation.
4. Monitoring: CloudTrail management/data events, CloudWatch Logs, metric filters, alarms and notifications.
5. Testing/remediation: run controlled tests, record outcomes, remediate findings and retest.

For each control, include the actual AWS setting, region/resource scope, evidence reference and current status.

## 4. IAM access matrix

The handwritten project plan's intended matrix is:

| Identity | List bucket | Read object | Write object |
|---|---:|---:|---:|
| Developers | Allow | Allow | Allow |
| Auditors | Allow | Allow | Deny |
| Unauthorized User | Deny | Deny | Deny |

This is the design target only. Compare it with actual attached policy JSON and IAM Policy Simulator/live test results. Do not modify the committed policy JSON simply to make it match the target.

## 5. S3 security controls

Record Block Public Access, versioning, default encryption (SSE-S3 or SSE-KMS), restrictive bucket policy and HTTPS-only enforcement. Include public-access, encryption and versioning test evidence.

## 6. Monitoring and detection

The project notes identify these example sensitive events:
- `DeleteBucket`
- `PutBucketAcl`
- `ConsoleLogin` without MFA

Document the actual CloudTrail event selectors, CloudWatch metric filter patterns, metric namespace/name, alarm thresholds/actions and notification delivery. Include evidence that each detection was tested.

## 7. Security test results

For each scenario record:
- Attempt to make the bucket public.
- Unauthorized user attempts bucket/object access.
- Delete a disposable test object.
- Test console sign-in without MFA using a controlled test identity.
- Verify expected CloudWatch metrics, alarms and notifications.

Record expected result, actual result, detection evidence, remediation and retest result. Do not run destructive or access-weakening tests against important production data.

## 8. Remediation log

| Finding | Remediation | Retest evidence | Status |
|---|---|---|---|
| Add actual finding | Describe the change made | Link screenshot/log | Pending |

Possible remediation actions in the project notes are removing public access, fixing IAM permissions, enabling MFA, updating the bucket policy and rerunning tests.

## 9. Screenshots and supporting evidence

Include architecture diagram, steps taken, sanitized console screenshots, policy-simulator results, CloudTrail events, CloudWatch metric/alarm evidence and test outputs. Redact account IDs, email addresses and resource identifiers where appropriate.

## 10. Key learnings

Write what was learned from actual implementation and test outcomes: least privilege, S3 protection, audit logging, detection engineering, incident remediation and validation. Avoid generic claims unsupported by the work performed.

## 11. Limitations

List anything not implemented, not tested, disabled or still awaiting evidence.
