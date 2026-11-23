# IAM configuration and evidence

## IAM groups observed

The AWS Console screenshot showed:

| Group | Users shown |
|---|---:|
| `developer` | 2 |
| `auditors` | 1 |
| `unauthorizeduser` | 0 |

## Developer group policy

The user confirmed that the Developer group has the policy represented by [developer-policy.json](developer-policy.json).

The policy contains two statements:

1. `S3FilesPermissions` allows `s3files:Get*` and `s3files:List*` on `*`.
2. `EC2ReadOnlyPermissions` allows selected EC2 `Describe*` operations on `*`, including subnet, network-interface, security-group, VPC and Availability Zone descriptions.

**Important:** This policy does not contain standard S3 object write permission such as `s3:PutObject`. The `s3files:*` action namespace is distinct from the standard `s3:*` namespace. Do not describe this policy as granting ordinary S3 object upload/write access.

## Auditor group policy

The user confirmed that the Auditor group has the policy represented by [auditor-policy.json](auditor-policy.json).

The policy allows:

- `s3:Get*`
- `s3:List*`
- `s3:Describe*`
- `s3-object-lambda:Get*`
- `s3-object-lambda:List*`

The policy uses `Resource: "*"`, so the JSON does not restrict these permissions to one named bucket. It is read/list-oriented and does not explicitly grant `s3:PutObject` or `s3:DeleteObject`.

## Interpretation and limitations

- These are the policy documents the user identified as attached to the Developer and Auditor groups.
- The files preserve the supplied policy statements; no permissions were added or removed.
- The policy documents alone do not show all effective permissions. Other attached identity policies, group memberships, resource policies, permission boundaries, SCPs and explicit denies can affect access.
- The Developer policy's S3 Files read/list permissions and EC2 Describe permissions should not be confused with standard S3 bucket/object permissions.
- Verify effective permissions with IAM Policy Simulator and controlled tests before claiming access outcomes.

## Other policy screenshots supplied

Other screenshots/documents included an S3 Files client mount/write policy, an IAM password-change policy and an SNS phone-number opt-out-check permission. They were not identified as the selected Developer or Auditor group policies, so they are not included as those groups' policy files.
