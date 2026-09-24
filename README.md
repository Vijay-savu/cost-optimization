# EBS Snapshot Cost Optimization

An AWS Lambda function that identifies orphaned Amazon EBS snapshots and deletes snapshots whose source volumes no longer exist or are not attached to an active EC2 instance. This helps reduce unnecessary monthly EBS snapshot storage costs.

## How It Works

The function:

1. Lists EBS snapshots owned by the AWS account.
2. Finds currently running EC2 instances.
3. Checks whether each snapshot's source volume still exists and has an attachment.
4. Deletes snapshots with no source volume or with a source volume that is not attached.

> Warning: Snapshot deletion is permanent. Test this function in a non-production account first and add retention rules or tags if your environment requires backups to be preserved.

## Files

- `lambda_function.py` - Lambda handler and EBS snapshot cleanup logic.

## Required IAM Permissions

Attach an execution role to the Lambda function with permissions similar to:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:DescribeSnapshots",
        "ec2:DescribeInstances",
        "ec2:DescribeVolumes",
        "ec2:DeleteSnapshot"
      ],
      "Resource": "*"
    }
  ]
}
```

The role also needs the standard Lambda logging permissions, such as `logs:CreateLogGroup`, `logs:CreateLogStream`, and `logs:PutLogEvents`.

## Deploy Using the AWS Console

1. Open **AWS Lambda** and choose **Create function**.
2. Select **Author from scratch** and choose Python 3.12 or a later supported Python runtime.
3. Select an execution role with the permissions above.
4. Copy the contents of `lambda_function.py` into the Lambda code editor.
5. Choose **Deploy** and configure a scheduled EventBridge trigger if cleanup should run automatically.

## Local Git Commands

This project is published at:

https://github.com/Vijay-savu/cost-optimization

To publish future changes:

```powershell
git add .
git commit -m "Describe the change"
git push
```