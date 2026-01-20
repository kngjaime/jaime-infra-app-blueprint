# UI and API Services logging ingestion

## Status
Propposed [date: 01/18/26]

## Context
Define service's log ingestion to forward logs to external services such NewRelic/Splunk/Datadog.

## Decision
1. Fluentbit sidecar container to filter/process and create log chunks to forward logs to NewRelic (here some documentation). 

2. Cloudwatch logs + Kinesis subscription: Requires aws agent configuration and proper IAM permissions, the subscription would allow services such Kinesis Firehose to stream Application Logs to NewRelic/Splunk/Datadog.

## Consequences
Option 1 is better, it would reduce Infrastructure costs, computation consumption (no aws agent processing logs), and this implementation would also improve performance during the log ingestion process, and you can leverage feature to filter and send logs in chunks.