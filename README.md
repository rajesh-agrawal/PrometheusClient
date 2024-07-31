# 👋 Getting Started with <img height="30" src="https://user-images.githubusercontent.com/25181517/182534182-c510199a-7a4d-4084-96e3-e3db2251bbce.png" width="30"/> Prometheus, <img height="30" src="https://user-images.githubusercontent.com/25181517/182534075-4962068b-4407-46c2-ac67-ddcb86af30cc.png" width="30"/> Grafana

### How to Run?

1. Create a docker network interface for prometheus,grafana and Java application to talk to each other.
```
docker network create grafana-prometheus
```
1. Run the prometheus container with [prometheus.yaml](prometheus/prometheus.yaml), I am exposing it the port to be 9095 instead of 9090 as its already occupied on my machine

```
docker run --name my-prometheus  --network grafana-prometheus --network-alias prometheus   --mount type=bind,source=C:/work/setup/prometheus/prometheus.yml,destination=/etc/prometheus/prometheus.yml -p 9095:9090 prom/prometheus
```

PROMETHEUS SCRAPES metrics from application by polling

1.  Confirm if prometheus runs and shows docker metrics  [Prometheus local setup](http://localhost:9095/targets?search=)
1. Run the grafana container to persist data across multiple restarts of docker. You need to create a C:/work/setup/prometheus/grafana directory to persist data on main machine.
```
docker run --rm --name grafana2 --network grafana-prometheus --network-alias grafana --publish 3000:3000 -v C:/work/setup/prometheus/grafana:/var/lib/grafana  --detach grafana/grafana-oss:latest
```
2. Confirm if Grafana is running [Grafana local setup](http://localhost:3000) 
1.  To display Prometheus metrics in Grafana:
     - Log into Grafana (default username "admin" and password "admin").
     - Go to Configuration > Data sources
     - Click Add data source
     - Pick Prometheus as the data type
     - Set the URL as <http://prometheus:9090> and click Save & test down the bottom.
     - We use "prometheus" instead of "localhost" because that's the internal network name for our Prometheus container (within the network "grafana-prometheus").

1.  Run The spring Boot app

1. Grafana Screenshots for built in ☕ JVM metrics 
![JVM Dashboard - Grafana](Grafana_dashboard.png)
1. Grafana Screenshots for custom visitor count
![Visitor Count](VisitorCount.png)


### Reference Documentation
For further reference, please consider the following sections:

* [Official Apache Maven documentation](https://maven.apache.org/guides/index.html)
* [Spring Boot Maven Plugin Reference Guide](https://docs.spring.io/spring-boot/3.3.2/maven-plugin)
* [Create an OCI image](https://docs.spring.io/spring-boot/3.3.2/maven-plugin/build-image.html)
* [Spring Boot Actuator](https://docs.spring.io/spring-boot/docs/3.3.2/reference/htmlsingle/index.html#actuator)
* [Prometheus](https://docs.spring.io/spring-boot/docs/3.3.2/reference/htmlsingle/index.html#actuator.metrics.export.prometheus)

### Guides
The following guides illustrate how to use some features concretely:

* [Building a RESTful Web Service with Spring Boot Actuator](https://spring.io/guides/gs/actuator-service/)
