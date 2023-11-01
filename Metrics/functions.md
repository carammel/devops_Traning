## The rate() function
The rate() function returns the average per-second increase rate for the counter passed to it. The average rate is calculated over the lookbehind window passed in square brackets to rate().

For example, rate(unifi_devices_wireless_received_bytes_total[5m]) calculates the average per-second increase rate over the last 5 minutes. It returns lower than expected rate when 100MB of data in transferred in 10 seconds, because it divides those 100MB by 5 minutes and returns the average data transfer speed as 100MB/5minutes = 333KB/s instead of 10MB/s.

Unfortinately, using 10s as a lookbehind window doesn't work as expected - it is likely the rate(unifi_devices_wireless_received_bytes_total[10s]) would return nothing. This is because rate() in Prometheus expects at least two raw samples on the lookbehind window. This means that new samples must be written at least every 5 seconds or more frequently into Prometheus for [10s] lookbehind window. The solution is to use irate() function instead of rate():

irate(unifi_devices_wireless_received_bytes_total[5m])
It is likely this query would return data transfer rate, which is closer to the expected 10MBs if the interval between raw samples (aka scrape_interval) is lower than 10 seconds.

Unfortunately, it isn't recommended to use irate() function in general case, since it tends to return jumpy results when refreshing graphs on big time ranges. Read this article for details.

So the ultimate solution is to use rollup_rate function from VictoriaMetrics - the project I work on. It reliably detects spikes in counter rates by returning the minimum, maximum and average per-second increase rate across all the raw samples on the selected time range.

## The average() function
