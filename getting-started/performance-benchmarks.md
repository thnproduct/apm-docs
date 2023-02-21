# Performance Benchmarks

### Node.js

We've run the benchmark with a AWS Lambda function who makes two DynamoDB call using the data passed with the event.&#x20;

We've run the experiment for 1000 times to see the average and excluded the cold started invocations.&#x20;

| **Memory Size (MB)** | **Baseline Latency (ms)** | **Thundra Enabled Latency (ms)** | **Thundra + Offline Debugging Enabled Latency (ms)** |
| -------------------- | ------------------------- | -------------------------------- | ---------------------------------------------------- |
| 512                  | 16.8                      | 24.4                             | 25.5                                                 |
| 1024                 | 16.6                      | 23.6                             | 25                                                   |
| 1536                 | 15.5                      | 22.7                             | 23.2                                                 |
| 2024                 | 15.7                      | 21.8                             | 22.7                                                 |
| 3072                 | 15.3                      | 22.1                             | 22.4                                                 |

###
