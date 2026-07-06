# Chapter 2
# Understand Metric Types of Prometheus and Grafana
    - Counter Metric : Type of metric that can only increase (for ex: http_request_total{methos="GET",path="/metrics"})
        tutorial: http_request_total(method="GET",path=~"item.*")
    - Guage Metric : Type of metric that can both increase and decrease (for ex : process_cpu_usage)
    - Histogram Metric: Type of metric that describe the history of speed of the amount of the request in the specific point (for ex: http_request_duration_seconds_bucket)
# Chapter 3
# Prometheus function 
    for counter data
        - rate() : average number of something per second over entire time window (checks for frequency) 
        - irate() : look at only the last two point of that time window and caculate the rate of the increase between these two point of that 
        time windows (check for volatility)
    for guage data :
        - delta() : count the diff between the first and last value in time series (for ex: delta(process_cpu_usage[5m])
        check the diff between the cpu of the last point and newest point in 5m)
# Chapter 4
# Aggregation Operators
    sum()
    sum by ()
    avg()
    min()
    max()
    
# Aggregation OverTime 
# (calculates all values avg in the specific range of time series)
   sum_over_time()
   avg_over_time()
   min_over_time()
   max_over_time()

# Chapter 5
# Histogram Series
http_request_duration_seconds_bucket(le="0,05") this amount of requests only take 0,05 seconds to exchange which is fast as shit    
histogram_quantile(0.95, sum by (le) (http_request_duration_seconds_bucket{path="/"}))

# Chapter 6
# Average Totals
 (http_request_duration_seconds_sum{path="/"}[5m])
# Totals
 increase(http_request_total[3m])

# Chapter 7
# Label
label_replace(http_request_total,"new label","$1","path","(/.*)") create a new label infront of path label in all the path spec 

# Chapter 8
# Resource Metrics
process_memory_usage_bytes / (1024 * 1024) covert byte (B) into megabyte (MB)

# Chapter 9
# Visualizing Data Trends
deriv(process_resident_memory_bytes[1h]) the amount of memories that actually used by the process 

# Chapter 10
# Advanced Function
    topk(): get top time series with the high number of something (request,cpu,etc..)
        for ex: get top 5 time series with the high number (for ex: topk(5, http_request_total))
    bottomk(): in opposite comparing to topk()






