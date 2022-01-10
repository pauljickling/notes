# Prometheus

Prometheus is an open source, monitoring and alerting service for applications and infrastructure. The big draw of Prometheus is it has a simple to use, but powerful query language (promQL) for analyzing metrics. It also plays well with Loki and Grafana. The data model gives each time series a name and a key-value pair called a label. You can use "label matchers" to restrict the type of metrics that are returned, e.g. `process_resdient_memory_bytes{job="node"}`.

Prometheus monitors metrics -- *the aggregation of event types that occur on a piece of infrastructure or application*. Prometheus collects metrics using pieces of software called exporters. Prometheus uses service discovery tools to determine where it can find these exporters. Collecting the metrics from these exporters is called *scraping*.

Prometheus is a pull-based monitoring service. Although it is technically possible to use Prometheus using a push-based approach, you are probably better off looking for a different monitoring solution.

Prometheus can create alert rules, and send alerts to Alertmanager, which turns those alerts into notifications.

## Running Prometheus

Prometheus is written in Go, and so using it just involves downloading and running the appropriate binary for your OS.

By default Prometheus runs on port 9090.

### Prometheus Components

In addition to the base Prometheus package, there are a number of components that you can include to extend the capabilities of Prometheus:

- Alertmanager
- Blackbox Exporter
- Consul Exporter
- Graphite Exporter
- Haproxy Exporter
- Memcached Exporter
- Mysqld Exporter
- Node Exporter
- Pushgateway
- Statsd Exporter

#### Alertmanager

After creating alerting rules in Prometheus, Alertmanager can handle the notification process for those alerts using a variety of messaging protocols. Alerting rules use PromQL expressions. For example, an alerting rule for when a node is down would be expressed this way: `up == 0`.

By default Alertmanager runs on port 9093.

#### Node Exporter

Node Exporter exposes kernel and machine level metrics on Unix systems such as CPU, memory usgae, disk space, IOPS, network bandwidth, etc. It does not cover the metrics for individual processes.

By default Node Exporter runs on port 9100.

You can import Node exporter metrics as a Grafana Dashboard by clicking the `+` icon on the side panel, and selecting import. Then enter `1860` under the ID number to import the `Node Exporter Full` dashboard from grafana.com. This is a nice and quick way to monitor common metrics in Grafana.

## Types of Metrics

**Gauge:** A gauge are a current absolute value. Gauge involves values that can increase or decrease over time. Gauges have three primary metrics, increment (`.inc()`), decrement (`.dec()`), and set (`.set()`).

**Counter:** A counter tracks how many events have occured, or the size of all the events. Counters are a number goes up type of metric that can accept non-negative integers and 64-bit floats. Counters have an increment (`.inc()`) method as well as some built in functionality for counting exceptions.

**Summary:** Summary metrics track a sample of observations, principally through the `.observe()` method which takes the conditions for observing as its argument. Note that the Python Prometheus client does not accept client-side quantiles.

**Histogram:** Histograms take observable events and count them based on configurable buckets. Like the summary metric it uses the `.observe()` method. Buckets can be defined as sorted lists. In the Python library you can use list comprehensions if the bucket has a defined increment.

[Best practices on designing Instrumentation](https://prometheus.io/docs/practices/instrumentation/#counter-vs-gauge-summary-vs-histogram).

## Some Basic PromQL Queries

**Disk Usage as Percentage:** `(1 - node_filesystem_avail_bytes / node_filesystem_size_bytes) * 100`

**CPU Utilization:** `avg by (instance,mode) (irate(node_cpu_seconds_total{mode!='idle'}[1m]))`

**Average Network Traffic Received:** `rate(node_network_receive_bytes)`
