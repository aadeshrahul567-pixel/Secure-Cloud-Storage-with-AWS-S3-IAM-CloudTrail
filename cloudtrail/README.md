# CloudTrail configuration and evidence

The reviewed console screenshot showed a multi-region trail named `management-events`, with an S3 log destination. Trail logging was **Off** in that screenshot, and CloudTrail SNS notification delivery was disabled. The last log-file delivery shown was 19 August 2026.

A CloudTrail S3 log-delivery bucket policy was supplied; a sanitized copy is in [log-delivery-bucket-policy.json](log-delivery-bucket-policy.json).

## Important distinction

A configured trail is not necessarily collecting events now. Recheck the trail's logging status and event selectors before relying on it.

## Evidence still required

- Current trail status.
- Management event selectors.
- S3 data event selectors for object-level activity, if configured.
- CloudWatch Logs integration.
- Event History examples showing the events actually recorded.
