## Availability SLI
### The percentage of successful requests over the last 5m
sum (rate(flask_http_request_total{job="ec2",status!~"5.."}[5m]))/sum (rate(flask_http_request_total{job="ec2"}[5m]))

## Latency SLI
### 90% of requests finish in these times
histogram_quantile(0.90, sum(rate(flask_http_request_duration_seconds_bucket{job="ec2"}[5m])) by (le, verb))

## Throughput
### Successful requests per second
sum(rate(flask_http_request_total{job="ec2",status=~"2.."}[5m]))

## Error Budget - Remaining Error Budget
### The error budget is 20%
1 - ((1 - (sum(increase(flask_http_request_total{job="ec2", status!~"5.."}[5m])) by(verb)) / sum(increase(flask_http_request_total{job="ec2"}[5m])) by (verb)) / (1 - .80))
