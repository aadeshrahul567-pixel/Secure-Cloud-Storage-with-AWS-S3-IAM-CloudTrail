# S3 security configuration

## HTTPS-only policy

A supplied bucket policy denies S3 requests when `aws:SecureTransport` is false. A sanitized example is provided in [https-only-bucket-policy.json](https-only-bucket-policy.json).

This is a transport-security control only. It does not establish that Block Public Access, versioning, default encryption or access logging are enabled.

## Evidence still required

Capture the target bucket's:

- Permissions → Block Public Access.
- Permissions → Bucket policy.
- Properties → Bucket Versioning.
- Properties → Default encryption.

Confirm the correct project bucket before treating any screenshot or policy as belonging to this project.
