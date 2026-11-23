# AWS Secure Cloud Storage with IAM, CloudTrail and CloudWatch

A hands-on AWS cloud-security project exploring secure object storage, identity-based access control, audit logging and monitoring.

> **Evidence-based project status:** This repository documents configurations and evidence observed during the project review. Some services were configured but are not currently shown as active. Items marked **Not yet verified** must not be presented as completed implementation.

## Project objectives

- Protect object storage against unintended public access.
- Apply identity-based permissions to Developer, Auditor and unauthorized-user groups.
- Enforce HTTPS for S3 requests.
- Record AWS activity using CloudTrail.
- Explore centralized log monitoring and alerting using CloudWatch and SNS.

## AWS services and tools

- Amazon S3
- AWS Identity and Access Management (IAM)
- AWS CloudTrail
- Amazon CloudWatch Logs, metric filters and alarms
- Amazon Simple Notification Service (SNS)
- IAM Policy Simulator
- AWS Management Console

## Architecture (logical)

```text
IAM identities and groups
  |-- developer
  |-- auditors
  |-- unauthorizeduser
          |
          v
    IAM permissions
          |
          v
      Amazon S3
  - Bucket access policy
  - HTTPS-only transport control
  - Public-access and object settings (verification pending)
          |
          v
      AWS CloudTrail
  - Multi-region trail configured
  - S3 log destination configured
  - Logging status must be checked before relying on new events
          |
          v
    CloudWatch Logs
  - CloudTrail log group
  - 3 metric filters observed
  - Alarm configuration not yet verified
          |
          v
       Amazon SNS
  - Notification workflow not yet verified
```

This is a logical view of the components, not proof that every connection is currently operational.

## Configurations evidenced

### IAM

The AWS account contains three IAM user groups:

| Group | Users shown in screenshot | Observation |
|---|---:|---|
| `developer` | 2 | Group exists |
| `auditors` | 1 | Group exists |
| `unauthorizeduser` | 0 | Group exists, no users shown |

The reviewed policy screenshots included AWS-managed `AmazonS3FilesClientReadWriteAccess`, `IAMUserChangePassword`, and an `sns-read` policy. These screenshots do **not** establish the existence or contents of custom Developer and Auditor S3 policies. Those policies still need to be captured from the relevant group Permissions tabs.

### S3 transport security

A bucket policy was supplied that denies all S3 actions when `aws:SecureTransport` is `false`. A sanitized copy is in [s3/https-only-bucket-policy.json](s3/https-only-bucket-policy.json).

This policy enforces secure transport. It does not, by itself, configure S3 Block Public Access, encryption, versioning or least-privilege access.

### CloudTrail

The reviewed trail was named `management-events` and showed:

- Multi-region trail: enabled.
- Trail logging: **Off** in the screenshot.
- S3 log destination: configured.
- Last log file delivered shown in screenshot: 19 August 2026.
- SNS notification delivery: disabled.
- Log file validation: disabled.
- CloudWatch Logs destination/log group: present.

The trail's S3 log-delivery bucket policy was supplied and sanitized in [cloudtrail/log-delivery-bucket-policy.json](cloudtrail/log-delivery-bucket-policy.json).

**Operational note:** enablement and event selectors must be rechecked in the AWS Console before describing CloudTrail as actively collecting current events. The supplied screenshot is a point-in-time view.

### CloudWatch Logs

A CloudTrail-related log group was observed in the `ap-south-2` region with:

- Three metric filters.
- Retention: Never expire.
- Deletion protection: Off.

The screenshot does not show the metric-filter names or patterns. Alarm names, thresholds, alarm state and SNS actions have not yet been evidenced.

### SNS

The reviewed CloudTrail trail details showed SNS notification delivery disabled. A separate SNS policy screenshot showed the action `sns:CheckIfPhoneNumberIsOptedOut`; this action alone is not evidence of a publish/notification workflow.

## Evidence and outstanding verification

| Area | Current evidence | Status |
|---|---|---|
| IAM groups | Three groups visible | Evidenced |
| Custom Developer S3 policy | Not supplied | Not yet verified |
| Custom Auditor S3 policy | Not supplied | Not yet verified |
| HTTPS-only bucket policy | JSON supplied | Evidenced |
| CloudTrail log-delivery bucket policy | JSON supplied | Evidenced |
| CloudTrail multi-region setting | Enabled in screenshot | Evidenced |
| CloudTrail logging | Off in screenshot | Inactive in captured state |
| CloudWatch log group | Present, three filters | Evidenced |
| Metric filter patterns | Not supplied | Not yet verified |
| CloudWatch alarms | Not supplied | Not yet verified |
| SNS delivery/notifications | Trail delivery disabled | Not yet verified |
| S3 Block Public Access | No screenshot supplied | Not yet verified |
| S3 versioning | No screenshot supplied | Not yet verified |
| S3 default encryption | No screenshot supplied | Not yet verified |
| IAM Policy Simulator tests | No results supplied | Not yet verified |
| End-to-end security tests | No test evidence supplied | Not yet verified |

## Repository structure

```text
.
├── README.md
├── iam/
│   └── README.md
├── s3/
│   ├── README.md
│   └── https-only-bucket-policy.json
├── cloudtrail/
│   ├── README.md
│   └── log-delivery-bucket-policy.json
├── cloudwatch/
│   └── README.md
├── sns/
│   └── README.md
└── tests/
    └── README.md
```

## Safe handling

- Never commit AWS access keys, secret keys, session tokens, passwords or credentials.
- Do not publish real account IDs, personal email addresses or sensitive resource identifiers.
- Replace bucket names, account numbers and ARNs with clearly marked placeholders in examples.
- Review screenshots before committing them; redact account IDs, email addresses and other sensitive details.
- Do not execute example policies in a production account without reviewing their scope and impact.

## Next evidence to add

1. Developer and Auditor group attached-policy screenshots and policy JSON.
2. S3 Block Public Access, versioning and default-encryption screenshots.
3. CloudTrail management/data event selectors and current logging status.
4. The three CloudWatch metric-filter names and patterns, plus alarm configurations.
5. SNS topic/subscription evidence and a successful test notification, if performed.
6. IAM Policy Simulator allow/deny results and actual security-test evidence.

## Scope and limitations

This repository currently contains sanitized policy examples and documentation based on the evidence reviewed. It does not contain deployment automation, Terraform/CloudFormation templates, custom IAM Developer/Auditor policies, or verified end-to-end alert tests. Add these only when the corresponding implementation or test artifacts are available.
