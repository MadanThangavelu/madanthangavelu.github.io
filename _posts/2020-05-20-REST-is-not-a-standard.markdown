---
layout: post
title: "REST is not a standard"
author: madan
categories: [ Software]
description: A quick overview on why REST is not a standard but merely an architectural style that most of us use but do not understand.
image: /assets/images/posts/2020-05-20-REST-is-not-a-standard/restful-not-a-standard.png
---

Around 2000 the internet was largely composed of static websites. We were comfortable going to foobar.com and happy receiving HTML content back which the browser would then render. We would then click on a link of submit a button and another page would be hurled back at us. This back and forth pingpong was largely how the World Wide Web was pieced together.  In former times, large desktop computers were the primary single medium of accessing these websites. These were indeed happy and simple times, however, not as exciting and chaotic as today - just 20 years later.


## The need for Standardiztion

To understand why we currently live in a spaghetti world of web technologies, it would be prudent to view the evolution across a few dimensions.

<!--excerpt_separator-->

##### Medium of access

The data from the server was only accessed by a bulky desktop. The million dollar weather app business had to develop a single web page to cater its customers. Contrast that with a VC funded sillicon valley company with a brilliant weather app idea. The first version must work seamlessly on a native IOs app, native android App, native desktop application, native laptop application, IoT device by your bedside that runs on an iPad mod and oh, finally all the different web browsers. Here is the kicker! All devices must be in sync with the cities I configured the weather for. 

I love getting the weather notifications across 5 devices a few seconds apart!

##### Attention Span

In order to keep the user engaged in 2020, you have to keep changing the content on the page they are viewing. If it is not dynamic the content will feel outdated. This necessitates the need to fetch more data from the browser as the user continues to stare at the device/app of choice. 

##### Technology Standardization 

In late 1990s we saw browser wars where their capabilities allowed innovation but left us with a largely broken ecosystem - a feature would work in internet explorer but not on netscape. Standardization bodies like the IETF, W3C and the now ceased The Web Standards Project did standardize the aspects of the infrastructure powering the internet - html, javascript, protocol like HTTP, WebRTC etc, but there was never success in standardizing the "application layer”. This of “application layer” as the “what” and the technology standardizations as the “how”.

What you speak (actual applications you build)
Grammar with allowed variations (REST)
Words (application protocols HTTP, HTTP2, )
Letters (network protocol)


To solve the attention span, the medium of access -  XMLHttpRequest (XHR) in browsers in the mid 2000’s was a huge win for the Web Platform. This allowed a browser to speak multiple requests to the server and fetch the information they need without reloading the entire view. This gave birth to APIs.  APIs became the contract between the backend and the frontend systems to “transfer state”.  

## REST was born

In 2000 Roy fielding proposed “Representational State Transfer” as a principled approach to transfer state from the backend to the frontend. As with all proposals, a good number of it was followed, along the way, we discovered and modified things as required. 

REST was originally a guidance, the foundational pieces were concrete but the final implementation was open ended. As an analogy, REST provides us parts and components and some suggestions, but nothing stops us from reinventing the wheel - literally. Imagine we provide the following proposal, a car has a steering wheel which should control the wheels. There should be three or four wheels. There should be three pedals controlling clutch, gear and the brakes. One can imagine how many types of cars would be innovated which fit the proposal. REST has evolved in somewhat in that direction. The basic constructs of URL, Query params, post params, HTTP Method, Some standard headers are available, but when the rubber hits the road (keeping the car analogy), every API is a snowflake. Knowing one company’s API does not automatically translate to using another company’s API. 