---
title: Extend nginx functionality with JavaScript
tags: [healthcare, observability]
categories: [mirth-connect]
author: Benny Tops
type: blackbee
draft: true
---

<h2>{{ $.Page.Title }}</h2>

**nginx is an amazing open source reverse proxy, load balancer, HTTP cache and web server. It offers extensive functionality, but as your setup grows, so will your requirements and at a certain point in time, you will want to extend the functionality of nginx. This article will take a look at using JavaScript to do so.**
 
# Introduction

This article will describe what you need to do to let nginx output 'Hello, world!' using JavaScript. When this works, we will proceed to use JavaScript to log all incoming header values with the datetime and the source of the incoming request. The reason for this exercise is a warmup for an authentication functionality extension that we will need for a next article: the introspection and caching of OAuth 2.0 JWT Tokens.

# njs

The official documentation of [njs](https://nginx.org/en/docs/njs/ "njs")

# Setup

{{< highlight Bash >}}
docker run --rm test-njs mm
{{< / highlight >}}

# 'Hello, world!' from njs
