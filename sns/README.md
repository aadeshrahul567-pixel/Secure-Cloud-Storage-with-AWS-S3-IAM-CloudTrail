# SNS notification evidence

The reviewed CloudTrail trail details showed SNS notification delivery disabled. A separate policy screenshot showed `sns:CheckIfPhoneNumberIsOptedOut`, which does not establish permission to publish notifications.

To document a notification workflow, capture the SNS topic, confirmed subscriptions, CloudWatch alarm action, and a test notification actually received.
