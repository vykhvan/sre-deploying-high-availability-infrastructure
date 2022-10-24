# API Service

| Category | SLI | SLO |
|----------|-----|-----|
| Availability | Number of successful requests/total number of requests over the last 5 minutes | 99% |
| Latency | Bucket of requests in a histogram showing the 90th percentile over the last 5 minutes | 90% of requests below 100ms |
| Error Budget | Remaining percentage of the error budget | Error budget is defined at 20%. This means that 20% of the requests can fail and still be within the budget |
| Throughput | Total number of successful requests over 5 minutes | 5 RPS indicates the application is functioning |
