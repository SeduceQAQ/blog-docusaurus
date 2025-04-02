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

[](https://kafka.apache.org/22/documentation.html#brokerconfigs)

> After a consumer group loses all its consumers (i.e. becomes empty) its offsets will be kept for this retention period before getting discarded. For standalone consumers (using manual assignment), offsets will be expired after the time of last commit plus this retention period.

By default, the retention period is 7 days (10080 minutes).
