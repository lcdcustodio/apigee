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

* Going back to Container Registry and then click on application image

<kbd>![Alt text](/pictures/05.png "Flask application")</kbd> 

* Choose the latest image version and then deploy on GKE

<kbd>![Alt text](/pictures/06.png "Flask application")</kbd> 

* Click on Continue

<kbd>![Alt text](/pictures/07.png "Flask application")</kbd> 

* Type the deployment name

<kbd>![Alt text](/pictures/08.png "Flask application")</kbd> 

* And finally click on deploy

<kbd>![Alt text](/pictures/09.png "Flask application")</kbd> 

* The application is up and running inside GKE Cluster. However, there is no way to access externally until this moment. So let's click on Expose button:

<kbd>![Alt text](/pictures/10.png "Flask application")</kbd> 

* By default, flask application runs at 5000 port. At this moment is time to map destination port properly.

<kbd>![Alt text](/pictures/11.png "Flask application")</kbd> 

* That is it! The application is exposed by public IP address.

<kbd>![Alt text](/pictures/12.png "Flask application")</kbd> 

<kbd>![Alt text](/pictures/13.png "Flask application")</kbd> 

GKE steps was ended. Now let's take a look on ApiGee side.

## Step by step to switch on ApiGee

Getting Started with Apigee API Management [Setting up your Apigee environment](https://www.youtube.com/watch?v=4jxAcZdeZqk&t=91s).

A quite simple approach to enable ApiGee as API proxy for GKE application.

* On GCP Navigation Menu select Apigee API Management option and then ApiGee. In the ApiGee page click on Open ApiGee Console

<kbd>![Alt text](/pictures/14.png "Flask application")</kbd> 

* Click on Api Proxies

<kbd>![Alt text](/pictures/15.png "Flask application")</kbd>

* Create a new Api Proxy

<kbd>![Alt text](/pictures/16.png "Flask application")</kbd>

* Choose Reverse proxy (most commom)

<kbd>![Alt text](/pictures/17.png "Flask application")</kbd>

* Define base path. Proxy will handle request on hostname/base-path.

* Policies setting. For a quite simple approach was chosen Pass through option

<kbd>![Alt text](/pictures/19.png "Flask application")</kbd>

* Review setting through summary view and then click on Create and Deploy

<kbd>![Alt text](/pictures/19.png "Flask application")</kbd>

* That is it! Proxy API is up and running

<kbd>![Alt text](/pictures/20.png "Flask application")</kbd>