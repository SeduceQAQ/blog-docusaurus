---
sidebar_position: 2
title: Migrate the MSK trigger
description: Migrate the MSK trigger
keywords: [aws, lambda, msk, trigger]
---

# Migrate the MSK trigger

[](https://docs.aws.amazon.com/lambda/latest/dg/with-msk-process.html#services-msk-topic-add)

You can migrate ongoing Kafka record processing to a new trigger by using the same `consumer group ID`. Lambda will process records based on the committed offsets of the consumer group, ensuring no data loss occurs.

## Steps

1. Note down the `consumer group ID` of the existing trigger
2. Remove the old trigger
3. Create a new trigger using the same `consumer group ID`

## Additional Information

[](https://kafka.apache.org/documentation/#brokerconfigs_offsets.retention.minutes)

> For subscribed consumers, committed offset of a specific partition will be expired and discarded when 1) this retention period has elapsed after the consumer group loses all its consumers (i.e. becomes empty); 2) this retention period has elapsed since the last time an offset is committed for the partition and the group is no longer subscribed to the corresponding topic.

By default, the retention period is 7 days (10080 minutes).
