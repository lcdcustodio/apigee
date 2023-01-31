Setting up your Apigee environment
--------------

What is Apigee?
===========================
Apigee is a platform for developing and managing APIs. By fronting services with a proxy layer, Apigee provides an abstraction or facade for your backend service APIs and provides security, rate limiting, quotas, analytics, and more.

In this Demo Apigee was connected to Google Kubernetes Engine (GKE) cluster with REST API up and running in order to expose its services (Flask application) through Proxy API.

## Step by step to switch on GKE

* Enable Google Container Registry
* Enable Google Cloud Shell and then upload the Flask application


<kbd>![Alt text](/pictures/01.png "Flask application")</kbd>

* Run the following commands at Google Cloud Shell prompt:

```sh
    cd webapp
    docker build -t webapp .
    gcloud projects list  #Get gcloud project name
    docker tag webapp gcr.io/<GCLOUD PROJECT NAME>/webapp     
    docker push gcr.io/<GCLOUD PROJECT  NAME>/webapp
```

* Enable Google Kubernetes Engine (GKE)
* Create a GKE Cluster 

<kbd>![Alt text](/pictures/02.png "Flask application")</kbd>
<kbd>![Alt text](/pictures/03.png "Flask application")</kbd>
<kbd>![Alt text](/pictures/04.png "Flask application")</kbd>



As you can see, we have a scrape_configs root key where we can define a list of jobs and specify the URL, metrics path, and the interval. If you'd like to read more about Prometheus configurations, feel free to visit the [official documentation](https://prometheus.io/docs/prometheus/latest/configuration/configuration/).

**Note:** Since we are going to use Docker to run Prometheus, Docker network that won't understand localhost as you might expect. Since our app is going to run on localhost, and for the Docker container, localhost means its own network, we have to specify our system IP in place of it.

So instead of using locahost:8080, 192.168.0.20:8080 is used where 192.168.0.20 is my PC IP at the moment.

To check your system IP you can run ipconfig or ifconfig in your terminal, depending upon your OS.

## Grafana

Grafana is a visualization layer that offers a rich UI where you can build up custom graphs quickly and create a dashboard out of many graphs faster. You can also import many community built dashboards for free and get going.

Grafana can pull data from various data sources like Prometheus, Elasticsearch, InfluxDB, etc. It also allows you to set rule-based alerts, which then can notify you over Slack, Email, Hipchat, and similar.


------------

2- Installation
===========================

First of all, let’s clone the repository and build the application:

```sh
    git clone https://github.com/lcdcustodio/springboot_demo_prometheus.git
    cd springboot_demo_prometheus
    mvn clean install
```    

Once the Maven build is finished, the deployment archive has been created in target folder. Now, we be able to create image and run the container:  

```sh
    # Create container
    docker build -t springboot_demo_prometheus .
```

```sh    
    # Run container
    docker run -p 8080:8080 springboot_demo_prometheus
```    

We can see the application up and running at:

```sh
    http://localhost:8080/hello-world
```    

All of metrics, including prometheus, are exposed by:

```sh
    http://localhost:8080/actuator
    http://localhost:8080/actuator/prometheus
```    

Now, we can run Prometheus using the Docker command:


```sh
    docker run -d -p 9090:9090 -v $PWD/prometheus.yml:/etc/prometheus/prometheus.yml prom/prometheus
```    


To see Prometheus dashboard, navigate your browser to **http://localhost:9090:**

<kbd>![Alt text](/pictures/prometheus.png "Welcome Prometheus")</kbd>

To check if Prometheus is actually listening to the Spring app, you can go to the /targets endpoint:

<kbd>![Alt text](/pictures/prometheus_target.png "Prometheus Target")</kbd>


Let's start  by running **Grafana** using Docker:


```sh
    docker run -d -p 3000:3000 grafana/grafana
```    

If you visit **http://localhost:3000**, you will be redirected to a login page. The default username is admin and the default password is admin. You can change these in the next step, which is highly recommended:

<kbd>![Alt text](/pictures/grafana.png "Grafana")</kbd>

Since Grafana works with many data sources, we need to define which one we're relying on. Select Prometheus as your data source:

<kbd>![Alt text](/pictures/grafana_ds_1.png "Grafana DataSource")</kbd>

**Note:** Since we are going to use Docker to run Grafana, Docker network that won't understand localhost as you might expect. Localhost means its own network, we have to specify our system IP in place of it in order to connect Prometheus Data Source

<kbd>![Alt text](/pictures/grafana_ds_2.png "Grafana Prometheus Ready")</kbd>

As previously said, Grafana has a ton of pre-built [dashboards](https://grafana.com/grafana/dashboards/). For Spring Boot projects, the [JVM dashboard](https://grafana.com/grafana/dashboards/4701-jvm-micrometer/) is popular:

<kbd>![Alt text](/pictures/grafana_import.png "Grafana Import")</kbd>

Input the URL for the dashboard, select "Already created Prometheus datasource" and then click Import:

<kbd>![Alt text](/pictures/grafana_outcome.png "Grafana Outcomes")</kbd>

---------

In this Demo, we used Micrometer to reformat the metrics data provided by Spring Boot Actuator and expose it in a new endpoint. This data was then regularly pulled and stored by Prometheus, which is a time-series database. Ultimately, we've used Grafana to visualize this information with a user-friendly dashboard.


[Reference](https://stackabuse.com/monitoring-spring-boot-apps-with-micrometer-prometheus-and-grafana/)
