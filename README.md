# Cloudeye-exporter

Prometheus cloudeye exporter for [Cloud.ru](https://cloud.ru) Advanced.

## Introduce

Prometheus is an open source visualization tool used to display large-scale measurement data. It also has a broad user base in fields such as industrial monitoring, weather monitoring, home automation, and process management. After connecting the Cloudeye service to prometheus, you can use prometheus to better monitor and analyze data from the Cloudeye service.


## Install and configure cloudeye-exporter

1. Install cloudeye-exporter
```
git clone https://github.com/sbercloud-terraform/cloudeye-exporter
cd cloudeye-exporter
```
2. Configure clouds.yml file
```
global:
  prefix: "cloudru"
  port: ":8087"
  metric_path: "/metrics"
  scrape_batch_size: 10
auth:
  auth_url: "https://iam.ru-moscow-1.hc.sbercloud.ru/v3"
  project_name: "{project_name}"
  access_key: "{access_key}"
  secret_key: "{secret_key}"
  region: "ru-moscow-1"
```
or
```
auth:
  auth_url: "https://iam.ru-moscow-1.hc.sbercloud.ru/v3"
  project_name: "{project_name}"
  user_name: "{username}"
  password: "{password}"
  region: "ru-moscow-1"
  domain_name: "{domain_name}"

```

3. Start cloudeye-exporter
```
./cloudeye-exporter
```

Visit metrics in http://localhost:8087/metrics?services=SYS.VPC,SYS.ELB

## Help
```
Usage of ./cloudeye-exporter:
  -config string
        Path to the cloud configuration file (default "./clouds.yml")
  -debug
        If debug the code.
 
```
The default port is 8087, default config file location is ./clouds.yml.

## Configure prometheus to connect to cloudeye
1. Configure access to cloudeye exporter node

   Modify the prometheus.yml file configuration in prometheus. As shown in the following configuration, add a node with job_name named 'cloudru' under scrape_configs. Among them, targets are configured with the IP address and port number for accessing the cloudeye-exporter service, and services are configured with the services you want to monitor, such as SYS.VPC and SYS.RDS.
```
global:
  scrape_interval: 1m # Set the scrape interval to every 1 minute seconds. Default is every 1 minute.
  scrape_timeout: 1m
scrape_configs:
  - job_name: 'cloudru'
    static_configs:
    - targets: ['10.0.0.10:8087']
    params:
      services: ['SYS.VPC,SYS.ELB']
```
2. Start prometheus to monitor Cloud.ru Advanced services
```
./prometheus
```
* Log in to http://127.0.0.1:9090/graph
* View the monitoring results of specified indicators
