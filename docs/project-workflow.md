# Project workflow and implementation guide

This guide consolidates the five-step project plan shown in the supplied handwritten project pages. It describes the intended workflow; check the evidence/status notes before presenting a task as completed.

## Project purpose

Build a secure cloud-storage solution on AWS using Amazon S3, IAM and CloudTrail. Apply access controls, monitor activity, detect risky behavior and remediate misconfigurations.

**Role:** Cloud Security Analyst

**Tools listed in the project plan:** AWS S3, IAM, CloudTrail, IAM Policy Simulator/Validator and S3 Storage Lens. The evidence reviewed so far confirms console use and selected IAM/S3/CloudTrail/CloudWatch configurations; it does not yet confirm that every listed tool or step was completed.

## Step 1: Set up the environment

The project plan calls for:
- Create an AWS account/free-tier environment.
- Enable MFA for the root user.
- Create a separate IAM user for administration rather than using the root user for daily work.
- Enable AWS CloudTrail in all regions.
- Create an S3 bucket in the selected region.

**Evidence/status:** An S3 project-storage candidate and a separate CloudTrail log-delivery bucket were visible in the supplied bucket inventory. The CloudTrail trail screenshot showed a multi-region trail, but trail logging was Off in that captured state. Root MFA and the separate administrator-user setup have not been evidenced in the supplied material.

## Step 2: Secure the S3 bucket

The project plan calls for:
- Block all public access.
- Enable bucket versioning.
- Encrypt objects using SSE-S3 or SSE-KMS.
- Create a restrictive bucket policy.
- Enforce HTTPS-only requests.

Planned validation:
- Attempt public access and confirm it fails.
- Upload a file and verify encryption.
- Upload a replacement object and verify versioning behavior.

**Evidence/status:** A supplied HTTPS-only bucket-policy JSON denies requests when `aws:SecureTransport` is false and names `hemanth-1234`. The bucket inventory showed `hemanth-1234` in `eu-north-1` as the likely project-storage bucket. Its current Block Public Access, versioning and default encryption settings have not been shown. The separate CloudTrail log bucket had Block Public Access On and versioning Disabled in the supplied screenshots. Do not transfer those settings to the project bucket.

## Step 3: Implement least-privilege IAM

The project plan proposes these access roles:
- Developers: read/write access.
- Auditors: read-only access.
- Unauthorized user: no access.

Planned tasks:
- Create IAM groups.
- Create least-privilege policies and attach them to the groups.
- Test each role's access.
- Use IAM Policy Simulator to validate permissions and remove unnecessary access.

**Evidence/status:** The console screenshot showed `developer` (2 users), `auditors` (1 user) and `unauthorizeduser` (0 users). The confirmed Developer policy grants S3 Files `Get*`/`List*` and selected EC2 `Describe*` actions; it does not contain standard S3 `s3:PutObject` permission. The confirmed Auditor policy grants S3 and S3 Object Lambda read/list/describe actions with `Resource: "*"`. Therefore, the supplied policies do not currently demonstrate the planned Developer standard-S3 read/write and Auditor bucket-scoped read-only matrix. Existing JSON files are preserved unchanged. See [IAM findings](../iam/README.md).

## Step 4: Monitor and detect

The project plan calls for:
- Configure CloudTrail to capture management events and S3 data events.
- Send CloudTrail logs to CloudWatch Logs.
- Create metric filters for sensitive events, including `DeleteBucket`, `PutBucketAcl` and `ConsoleLogin` without MFA.
- Create CloudWatch alarms for the resulting metrics and configure notification actions.

Planned flow:

```text
AWS CloudTrail -> CloudWatch Logs -> Metric Filters -> CloudWatch Alarm -> Notification
```

**Evidence/status:** A multi-region trail named `management-events` and a CloudWatch Logs destination were shown. Trail logging was Off in the supplied screenshot, and SNS notification delivery was disabled. A CloudWatch log group with three metric filters was shown, but their names and patterns were not visible. Alarm configuration and actual notifications have not been evidenced. Do not claim the three named detections are operational until the actual filter patterns and test results are captured.

## Step 5: Test, remediate and report

The project plan lists these scenarios:
1. Attempt to make the bucket public.
2. Use an unauthorized user to access the bucket.
3. Delete an object.
4. Sign in without MFA.
5. Verify that the expected CloudWatch alerts are triggered.

Possible remediation actions listed in the plan:
- Remove public access.
- Correct IAM permissions.
- Enable MFA.
- Update the bucket policy.
- Re-run tests to confirm remediation.

Record the actual outcome of each test. Do not perform destructive tests against important data or attempt to weaken a live production bucket. Use a disposable test object and a controlled test identity. Record whether the test was prevented, detected, both, or neither.

## Final report deliverables

The project pages ask the report to include:
- Architecture diagram.
- Steps taken.
- Screenshots.
- Issues found and how they were fixed.
- Key learnings.

Suggested report sections: objective and scope; architecture; AWS services; implementation steps; IAM access matrix; S3 security controls; CloudTrail/CloudWatch detection flow; test cases and results; remediation; limitations; lessons learned.

## Current evidence-based gaps

- Confirm root-user MFA and administrative IAM-user setup.
- Capture the project bucket's Block Public Access, versioning and default encryption.
- Confirm the HTTPS-only policy is attached to the project bucket.
- Review whether Developer is intended to have standard S3 object write access; the current supplied policy does not show it.
- Scope Auditor access to the intended bucket if that is the design, then validate the policy changes separately. Do not silently alter the supplied policy.
- Confirm CloudTrail management and S3 data event selectors, and whether trail logging is currently enabled.
- Identify the three CloudWatch metric filters and capture their exact patterns.
- Capture CloudWatch alarm settings and SNS topic/subscription/test-notification evidence.
- Run and record the security tests and remediation retests.

## Safety and publication notes

- Never commit AWS access keys, secret keys, session tokens, passwords or credentials.
- Redact account IDs, personal email addresses and sensitive resource identifiers from screenshots.
- Keep policy JSON aligned with the actual attached AWS policies. Do not replace an observed policy with the intended example policy.
- Treat this guide as a project plan and evidence tracker, not proof that every planned task has been completed.