# IAM configuration and evidence

## Observed groups

The reviewed AWS Console screenshot showed three groups:

- `developer` — 2 users.
- `auditors` — 1 user.
- `unauthorizeduser` — 0 users.

## Policy review

The policy screenshots supplied during review included AWS-managed policies for S3 Files client access and IAM password changes, plus an SNS policy containing `sns:CheckIfPhoneNumberIsOptedOut`.

These are not sufficient evidence of custom S3 Developer and Auditor policies.

## Evidence still required

For each group, capture **IAM → User groups → group name → Permissions**. Open attached custom policies and save their JSON documents. Verify the intended allowed and denied actions with IAM Policy Simulator. Do not create policy files by guessing the permissions.
