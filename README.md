# Monitoring

## This project is an example of monitoring Docker and Kubernetes in different environments with Prometheus - Kube Prometheus

## Monitoring - Prometheus - Alertmanager - Grafana - Node Exporter - MailHog

![Screenshot](/docker/docker-design.png)

### Prometheus

Prometheus is an open-source monitoring and alerting system. It is used to collect and store metrics from various systems and services, and can then be used to query and visualize the data. Prometheus includes a multi-dimensional data model, a flexible query language, and a built-in alerting system. It is often used in conjunction with other tools, such as Grafana, for visualizing the metrics it collects.

[Prometheus: The Documentary](https://www.youtube.com/watch?v=rT4fJNbfe14&t=2s&ab_channel=Honeypot)

[Prometheus Official channel - Julius Volz - Co founder Prometheus](https://www.youtube.com/@PromLabs)


### Alertmanager 

Alerting with Prometheus is separated into two parts. Alerting rules in Prometheus servers send alerts to an Alertmanager. The Alertmanager then manages those alerts, including silencing, inhibition, aggregation and sending out notifications via methods such as email, on-call notification systems, and chat platforms.

[Alertmanager Configuration](https://prometheus.io/docs/alerting/latest/configuration/)

### Grafana 


Grafana is an open-source data visualization and monitoring platform. It allows users to create and share dynamic dashboards for monitoring metrics such as system performance, application analytics, and real-time business data. Grafana can pull data from a variety of sources, including Prometheus, InfluxDB, Graphite, Elasticsearch, and more, and it supports alerting and anomaly detection.

[Grafana Dashboard for Prometheus](https://grafana.com/grafana/dashboards/?dataSource=prometheus)

### Node Exporter 

Node Exporter is a Prometheus exporter for server level and OS level metrics, and measures various server resources such as RAM, disk space, and CPU utilization.

[Exporter List](https://prometheus.io/docs/instrumenting/exporters/)

### MailHog 

MailHog is an email-testing tool with a fake SMTP server underneath. It encapsulates the SMTP protocol with extensions and does not require specific backend implementations. MailHog runs a super simple SMTP server that hogs outgoing emails sent to it

[MailHog - Github](https://github.com/mailhog/MailHog)



## Kube Prometheus

![Screenshot](/kind/architecture-min.png)
![Screenshot](/kind/prometheus.png)