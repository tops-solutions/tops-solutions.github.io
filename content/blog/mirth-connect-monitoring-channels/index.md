---
title: Monitoring Mirth Connect channels with Grafana and Prometheus
tags: [healthcare, observability]
categories: [mirth-connect]
author: Benny Tops
type: blackbee
draft: true
---

**Once you start using Mirth Connect as an (healthcare) integration engine, you will want to monitor it. My preferred monitoring stack is Grafana, so we will be using that to setup a monitor for Mirth Connect's channels in this article.**

# Introduction

We will start by introducing the steps we need to take and the several components we need to bring this to a good end.

## What is Mirth Connect?

{{< figure src="images/mirth-logo.png">}}

**[Mirth Connect](https://www.nextgen.com/solutions/interoperability/mirth-integration-engine "Mirth Connect") by [NextGen Healthcare](https://www.nextgen.com/ "NextGen Healthcare") is an open source, cross-platform interface engine, primarily used in healthcare.**

## What is Grafana?

![Grafana Logo](images/grafana-logo.png)

**[Grafana](https://grafana.com/grafana/ "Grafana") by [Grafana Labs](https://grafana.com/ "Grafana Labs") is a multi-platform open source analytics and interactive visualisation web application.** It provides charts, graphs, and alerts for the web when connected to supported data sources.

## What is Prometheus?

![Prometheus Logo](images/prometheus-logo.svg)

**[Prometheus](https://prometheus.io/ "Prometheus") is an open source technology designed to provide monitoring and alerting functionality for cloud-native environments**

## What is Docker?

![Docker Logo](images/docker-logo.webp?width=100)

**[Docker](https://www.docker.com/ "Docker") is an open source platform for building, deploying, and managing containerized applications.**

## The setup

### Create a new channel

- Create a new channel and name it **metrics**
- Set all inbound and outbound data types to Raw

### Configure the source connector's filter

- Click the **Source** tab
    - Select **HTTP Listener** as Connector Type
    - Set a port
    - Set base context path to **/metrics**
    - Set Response Content Type to **text/plain**
    - We will have to return to this view later on to configure **Response** under **Source Settings** and **Response Status Code** under **HTTP Listener Settings**
    - Click **Edit Filter** under **Channel Tasks**
        - Add a rule "Accept HTTP calls with GET method only"
        - Add the following code:


{{< highlight javascript >}}
// Filter non-GET HTTP calls
var httpMethod = sourceMap.get('method');
if (httpMethod == 'GET') {
    return true;
} else {
    return false;
}
{{< / highlight >}}

On the other hand, the Prometheus metric format takes a flat approach to naming metrics. Instead of a hierarchical, dot separated name, you have a name combined with a series of labels or tags:
{{< highlight javascript >}}
  <metric name>{<label name>=<label value>, ...}
{{< / highlight >}}

A time series with the metric name http_requests_total and the labels service="service", server="pod50″ and env="production" could be written like this:

{{< highlight javascript >}}
http_requests_total{service="service", server="pod50", env="production"}
{{< / highlight >}}

- mirth_channel_deployed_state
- mirth_channel_error_count
- mirth_channel_filtered_count
- mirth_channel_queued_count
- mirth_channel_received_count
- mirth_channel_sent_count

### Configure the source connector's transformer

- Click **Edit Transformer** under **Channel Tasks**

