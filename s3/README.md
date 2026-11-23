# S3 security configuration

## Bucket inventory

The S3 bucket-list screenshot showed two general-purpose buckets:

- `hemanth-1234` in Europe (Stockholm), `eu-north-1`. This is the project-storage candidate and matches the bucket name in the supplied HTTPS-only policy.
- A separate AWS-generated CloudTrail log-delivery bucket in Asia Pacific (Hyderabad), `ap-south-2`.

Inspect the `hemanth-1234` bucket for the application/project storage settings. Do not use the CloudTrail log bucket's settings as evidence for this bucket.

## HTTPS-only policy

A supplied bucket policy denies S3 requests when `aws:SecureTransport` is false. A sanitized example is provided in [https-only-bucket-policy.json](https-only-bucket-policy.json).

The supplied JSON names `hemanth-1234`, but the bucket's Permissions tab should be checked to confirm that this exact policy is currently attached.

This is a transport-security control only. It does not establish that Block Public Access, versioning or default encryption are enabled.

## Evidence still required for the project-storage bucket

Open S3 → Buckets → `hemanth-1234` and capture:

- Permissions → Block Public Access.
- Permissions → Bucket policy (show the attached JSON).
- Properties → Bucket Versioning.
- Properties → Default encryption.
