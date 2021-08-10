# Prometheus

Prometheus is an open source, monitoring and alerting service for applications and infrastructure. The big draw of Prometheus is it has a simple to use, but powerful query language (promQL) for analyzing metrics. It also plays well with Loki and Grafana. The data model gives each time series a name and a key-value pair called a label.

Prometheus monitors metrics -- *the aggregation of event types that occur on a piece of infrastructure or application*. Prometheus collects metrics using pieces of software called exporters. Prometheus uses service discovery tools to determine where it can find these exporters. Collecting the metrics from these exporters is called *scraping*.

Prometheus is a pull-based monitoring service. Although it is technically possible to use Prometheus using a push-based approach, you are probably better off looking for a different monitoring solution.

Prometheus can create alert rules, and send alerts to Alertmanager, which turns those alerts into notifications.
