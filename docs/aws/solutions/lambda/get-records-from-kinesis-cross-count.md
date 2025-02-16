---
sidebar_position: 1
---

# 跨账户获取 Kinesis 消息

## 架构

![](https://images.blog.seduceqaq.com/images/2025/02/16/20250216161154971.webp)

## 设置

### Kinesis 数据流共享策略

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

### KMS 密钥策略

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

### Lambda 角色权限

1. 为 Lambda 执行角色附加 `AWSLambdaKinesisExecutionRole` AWS 托管策略

2. 为 Lambda 执行角色附加 `kms:Decrypt` 权限

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

## 测试

```bash
aws kinesis put-record \
    --stream-name KinesisDataStreamCrossAccount  \
    --data aGVsbG8= \
    --partition-key 0
```
