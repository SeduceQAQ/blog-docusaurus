---
sidebar_position: 1
title: Get records from Kinesis cross-count
description: Get records from Kinesis cross-count
keywords: [aws, lambda, kinesis, cross-account]
---

# Get records from Kinesis cross-count

## Architecture

![](https://images.blog.seduceqaq.com/images/2025/03/16/20250316213530458.webp)

## Setup

### Kinesis data stream sharing policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "StreamReadStatementID",
      "Effect": "Allow",
      "Principal": {
        "AWS": "arn:aws:iam::167439876321:role/LambdaRoleForCrossAccountKinesisDataStream"
      },
      "Action": [
        "kinesis:GetRecords",
        "kinesis:GetShardIterator",
        "kinesis:DescribeStream",
        "kinesis:DescribeStreamSummary",
        "kinesis:ListShards"
      ],
      "Resource": "arn:aws:kinesis:ap-south-1:842676014633:stream/KinesisDataStreamCrossAccount"
    }
  ]
}
```

### KMS key policy

```json
{
  "Sid": "Allow Lambda Role",
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::167439876321:role/LambdaRoleForCrossAccountKinesisDataStream"
  },
  "Action": "kms:Decrypt",
  "Resource": "*"
}
```

### Lambda role permissions

1. Attach the `AWSLambdaKinesisExecutionRole` AWS managed policy to the Lambda execution role

2. Attach the `kms:Decrypt` permission to the Lambda execution role

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "VisualEditor0",
      "Effect": "Allow",
      "Action": "kms:Decrypt",
      "Resource": "arn:aws:kms:ap-south-1:842676014633:key/83220705-71c1-4000-9d0f-d041c25675d6"
    }
  ]
}
```

## Test

```bash
aws kinesis put-record \
    --stream-name KinesisDataStreamCrossAccount  \
    --data aGVsbG8= \
    --partition-key 0
```
