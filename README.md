# AWS Secure Cloud Storage with IAM, CloudTrail and CloudWatch

A hands-on AWS cloud-security project exploring secure object storage, identity-based access control, audit logging and monitoring.

> **Evidence-based status:** This repository documents configurations and policy documents reviewed so far. Some services were configured but are not currently shown as active. Items marked **Not yet verified** must not be presented as completed implementation.

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
  - Secure-storage bucket (not yet identified in evidence)
  - CloudTrail log-delivery bucket
  - HTTPS-only transport policy example
          |
          v
      AWS CloudTrail
  - Multi-region trail configured
  - S3 log destination configured
  - Logging status was Off in captured screenshot
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

| Group | Users shown in screenshot |
|---|---:|
| `developer` | 2 |
| `auditors` | 1 |
| `unauthorizeduser` | 0 |

The Developer group policy is documented in [iam/developer-policy.json](iam/developer-policy.json). It grants S3 Files `Get*`/`List*` operations and selected EC2 `Describe*` operations. It does **not** show standard S3 object upload permission such as `s3:PutObject`.

The Auditor group policy is documented in [iam/auditor-policy.json](iam/auditor-policy.json). It grants S3 and S3 Object Lambda read/list/describe actions, with `Resource: "*"`.

See [IAM documentation](iam/README.md) for policy interpretation and limitations. These policy documents do not by themselves establish all effective permissions for the users.

### S3 transport security

A bucket policy was supplied that denies all S3 actions when `aws:SecureTransport` is `false`. A sanitized copy is in [s3/https-only-bucket-policy.json](s3/https-only-bucket-policy.json).

This policy enforces secure transport. It does not, by itself, configure S3 Block Public Access, encryption, versioning or least-privilege access.

### CloudTrail logging bucket

The supplied S3 console screenshots show the bucket used as the CloudTrail log destination (bucket name redacted in this documentation) in the Asia Pacific (Hyderabad) region, `ap-south-2`.

- Block all public access: **On**.
- Bucket versioning: **Disabled**.
- MFA Delete: **Disabled**.
- Bucket ABAC: **Disabled**.
- Default encryption: **Not visible in the supplied screenshots; not yet verified**.
- The bucket policy editor is not shown with its JSON contents in the screenshot; the CloudTrail log-delivery policy supplied separately is documented in [cloudtrail/log-delivery-bucket-policy.json](cloudtrail/log-delivery-bucket-policy.json).

These settings apply to the CloudTrail log-delivery bucket shown in the screenshots. They do not establish the configuration of a separate application/secure-storage bucket. The project storage bucket has not yet been identified in the evidence.

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

**Operational note:** recheck trail logging status and event selectors before describing CloudTrail as actively collecting current events.

### CloudWatch Logs

A CloudTrail-related log group was observed in the `ap-south-2` region with three metric filters and retention set to Never expire. The screenshot does not show the filter names or patterns. Alarm names, thresholds, alarm state and SNS actions have not yet been evidenced.

### SNS

The reviewed CloudTrail trail details showed SNS notification delivery disabled. A separate SNS policy screenshot showed `sns:CheckIfPhoneNumberIsOptedOut`; this action alone is not evidence of a publish/notification workflow.

## Evidence and outstanding verification

| Area | Current evidence | Status |
|---|---|---|
| IAM groups | Three groups visible | Evidenced |
| Developer group policy | Supplied JSON identified by user | Documented |
| Auditor group policy | Supplied JSON identified by user | Documented |
| HTTPS-only bucket policy | JSON supplied | Evidenced |
| CloudTrail log-delivery bucket policy | JSON supplied | Evidenced |
| CloudTrail multi-region setting | Enabled in screenshot | Evidenced |
| CloudTrail logging | Off in screenshot | Inactive in captured state |
| CloudTrail log bucket Block Public Access | On | Evidenced |
| CloudTrail log bucket versioning | Disabled | Evidenced |
| CloudTrail log bucket default encryption | Not visible | Not yet verified |
| Secure-storage bucket identity/settings | Not yet identified | Not yet verified |
| CloudWatch log group | Present, three filters | Evidenced |
| Metric filter patterns | Not supplied | Not yet verified |
| CloudWatch alarms | Not supplied | Not yet verified |
| SNS delivery/notifications | Trail delivery disabled | Not yet verified |
| IAM Policy Simulator tests | No results supplied | Not yet verified |
| End-to-end security tests | No test evidence supplied | Not yet verified |

## Repository structure

```text
.
├── README.md
├── iam/
│   ├── README.md
│   ├── developer-policy.json
│   └── auditor-policy.json
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

1. Identify the separate secure-storage bucket, if one exists, and capture its Block Public Access, versioning, encryption and bucket policy.
2. Capture default encryption for the CloudTrail log bucket.
3. CloudTrail management/data event selectors and current logging status.
4. The three CloudWatch metric-filter names and patterns, plus alarm configurations.
5. SNS topic/subscription evidence and a successful test notification, if performed.
6. IAM Policy Simulator allow/deny results and actual security-test evidence.

## Scope and limitations

This repository contains sanitized policy examples and documentation based on evidence reviewed. It does not contain deployment automation, Terraform/CloudFormation templates, or verified end-to-end alert tests. The Developer policy supplied does not show standard S3 object upload permissions; validate the intended access design before describing Developer as an S3 object uploader.
