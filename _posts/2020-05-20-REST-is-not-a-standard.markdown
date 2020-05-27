---
layout: post
title: "REST API guidelines are outdated and insufficient"
author: madan
categories: [ Software]
featured: true
description: A quick overview on why REST is outdated and does not solve the business problems today. We are ariving closed to the need to invent something new.
image: /assets/images/posts/2020-05-20-REST-is-not-a-standard/restful-not-a-standard.png
---

A year after your project execution you realize that the APIs are messy and inconsistent. Things looked good when you started the project, so where did you go wrong? Was it you or was it REST?

## Why did we invent REST APIs?

The internet was largely composed of static websites around 2000. Visiting a website like foobar.com/some/path would return HTML content back which the browsers knew how to render. 

You clicked and the server hurdled back another new page. This back and forth pingpong was largely how the World Wide Web was pieced together.  

These were indeed happy and simple times, however, not as exciting and chaotic as today - just 20 years later.

Yahoo homepage in 2000 loaded minimal html with static content, but the 2020 website has *infinite scrolling, rotating news, covid updates, dynamically rotating advertisement and a dynamic trending news section and much more*. We also went from large desktop computers to IoTs.

![yahoo website in 2000](/assets/images/posts/2020-05-20-REST-is-not-a-standard/yahoo-comparision.png){: .center-image }

A number of things influenced the application layer development.


##### Medium of access

Your million dollar weather app business in 2000 had a single web page to serve to your customers through their desktops. 

Contrast that to this day - a VC funded silicon valley company with a brilliant weather app. The first version works seamlessly on a native IOs app, native android App, native desktop application, native laptop application and on IoT device by your bedside that runs on an iPad like device and oh ... finally all the different web browsers. Here is the kicker! 

All devices must be in sync with the cities I configured the weather. Who does't love getting weather notifications across 5 devices a few seconds apart!

##### Not a one person job

The apps are so rich that sometimes, a single company might not be powering all the data that a customer sees. 

In the example of yahoo, the weather app is likely developed by another third party company that yahoo has no control over. This is also true of large companies with thousands of engineers too.

##### Languages and Development Frameworks

App development could happen in swift, objectiveC, Javascript, C#, Java.

Server might be running in CGI scripts, PHP, JAVA, Ruby, Python, C#, Golang, Rust, Node.js and others. 

We have to make them all understand each other.


## The Internet standards pyramid

Interoperability - A cornerstone requirement for the web to function. In order for different devices, teams, languages to interact, they need to understand each other.

Standards allows the interoperability of multiple devices developed by different teams to interact with multiple web services written in different languages. Some ideas become concrete enough to become a true standard, while others die trying. 

In late 1990s we saw browser wars where their capabilities allowed innovation but left us with a largely broken ecosystem - a feature would work in internet explorer but not on netscape. Each new rendering or scripting feature addition to netscape was a way for web developers to build richer experience for netscape customers. This ultimately resulted in a state where a website would either work on netscape or on internet explored. 

Standardization bodies like the IETF, W3C and the now ceased The Web Standards Project stepped in to establish standards that are the minimal set of capabilities that all browsers should adhere to by standardizing the specs for fundamental systems like HTML, HTTP and others.

HTML or HTTP is one tiny piece of the puzzle. If you think about all the technologies that are needed for your cat video to show up, it is truly incredible. Along the layers of technology stack, there are numerous standardizations. 

##### Application layer standardization


![standards pyramid](/assets/images/posts/2020-05-20-REST-is-not-a-standard/standards-pyramid.png){: .center-image }


Enforcing standards are hard where there is room for a lot of creativity. The bottom most layers of the pyramid above provide relatively less creative space. 
Most engineers do not delve to that level during a normal day or product development. 

If you deviate from the spec/standard of DNS, there is change the internet is not going to work. 

However, the highest layers are areas of creative playground. It is really hard to set an exact spec/standard on a website layout. One may prescribe the accepted colors and tone of the copy. But the final outcome from two developers will be different. 

Same is true for REST APIs. Let us get into what REST is and how it has failed us but it survives us too.

## Introduction of REST

While APIs have existed even before the formalization of HTTP in 1997. XMLHttpRequest (XHR) in browsers in the mid 2000’s was a pivotal moment when APIs became mainstream. This allowed a browsers to speak multiple requests to the server and fetch the information they need without reloading the entire view. This essentially gave birth to the the now commonly known term web APIs. 

APIs became the contract between the backend and the frontend systems to “transfer state”.

HTTP was already providing the core component of data transfer protocol between server and client. The primary anatomy of a HTTP had 3 components

Representational State Transfer (REST) was an architectural proposal of how to use the 3 HTTP components (uri, header, body) to create some uniformity when building business applications that represent server state on the client.

REST was introduced In 2000 by Roy fielding as part of a PHD thesis.

##### Theoretical Principles

REST was originally a guidance, the foundational pieces were concrete but the final implementation was open ended. Let us start with the principles of REST as originally proposed

<table>
    <colgroup>
        <col width="30%" />
        <col width="70%" /> </colgroup>
    <thead>
        <tr class="header">
            <th>Principle</th>
            <th>Outcome</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td markdown="span">
                **Client-Server**
            </td>
            <td markdown="span">
                 <span class="success text-padding">Success</span><br>
                 *A separation of client vs server allowing us to swap either of them* - We see android, IOS devices, IOTs, laptops and desktop driven by mostly similar APIs. However this comes out of the box from HTTP 1.1
            </td>
        </tr>
        <tr>
            <td markdown="span">
                **Layered system**
            </td>
            <td markdown="span">
                <span class="success text-padding">Success</span><br>
                The client truly is un aware of the layers serving the data.
            </td>
        </tr>
        <tr>
            <td markdown="span">
                **Stateless**
            </td>
            <td markdown="span">
                <span class="warning text-padding">Partial Success</span><br>
                *Each request from client to server must contain all of the information necessary to understand the request* - The areas where it deviates are in cases where we keep server session data for security and authentication
            </td>
        </tr>
        <tr>
            <td markdown="span">
                **Cacheable**
            </td>
            <td markdown="span">
                <span class="warning text-padding">Partial Success</span><br>
                *Each request from client to server must contain all of the information necessary to understand the request* - The areas where it deviates are in cases where we keep server session data for security and authentication
            </td>
        </tr>
        <tr>
            <td markdown="span">
                **Uniform interface**
            </td>
            <td markdown="span">
                <span class="danger text-padding">Failure</span><br>
                Not much value beyond HTTP 1.1 specs
            </td>
        </tr>
    </tbody>
</table>

## Shortcomings of REST APIs

The promise of each of these principles were only partially fulfilled. REST provides us parts and components and some suggestions, but nothing stops us from reinventing the wheel or REST is straight up not a good fit. If we drew up the needs of the business and what REST satisfies, it's a small subset.

REST did not aspire to be an all encompassing standards or guidance for building the business application. We have just embraced it blindly to be that. 

![rest api standards](/assets/images/posts/2020-05-20-REST-is-not-a-standard/http-rest-business-overlap.png){: .center-image }


##### REST in practice is a lie (mostly)

Here are some scenarios which occurs 80% of the time when developing complex web applications. We will list down the choices. It clearly shows the gaps in REST.


* **Large get URL** - Sometimes a get URL has too many parameters. We see developers convert these semantically get calls into a POST request. We can see this done in elastic search queries. 
* **GET side effects** - In practical applications we see GET APIs emitting events (Kafka or any other system). If we were to calculate the airline price using events with GET, we are clearly changing the state of the system with each GET access.
* **Complex URL** - a GET request does not have the ability to send arrays, hash, or set via query URI. Different languages & frameworks interpret it differently. The REST principles of uniformity or client-server interoperability is broken in these cases.
* **Variations in resource access** - Complex business cases have many ways to access a resource. E.g., firstHotel, lastHotel, firstHotelVieableByCustomer, firstHotelForKayakIntegration, firstHotelForPricelineIntegration. While it is idealistic to want to put it into /hotel?[parameters]. I bet you to convince your engineer to do that. Testing, blast radius isolation, legacy code, or just completely different semantics might push us to have multiple URIs for the same resource.
* **Bidirectional data** - There are APIs where the server sends clients data via HTTP2 or SSE. Example, if a server wants to send price changes to the connected app. The request originates from the server by calling an API which sends the data via a persistent connection. There is no constructs for this in REST.
* **Caching** - For large APIs which return a huge amount of information, caching is likely needed at a more granular level than at the API. This is especially true in the case of bidirectional data. There is no REST guidance to rest upon.
* **Business Errors** - A user’s credit card payment API errors due to credit card failure, should the API return a 200 or 400 error? you might think 400 as the card is wrong. What if it was due to previous arrears and not due to any input in the current request, would it still be a 400 even through it was not a client error w.r.t this request?. As you can see, issues like this are very common in practical applications with no clear guidelines.
* **HATEOAS** - The concept of recursive explication might work for some basic graph data models but it is not practical for a majority of business applications. it further creates chattiness between the server and the client. This is the worst for of N+1 query between a highly latency prone network layer.    Most practical applications stay away from this ‘REST’ feature.
* **APIs per consumer** - As the number of devices and experiences increase, the flow between those devices start diverting. E.g., your experience of booking an airline from the app might be different from that on a desktop. As the experiences change, the APIs will need to be different. With this the reusability is out of the window and the need for multiple APIs looking similar happens. 
* **Versioning** -  No one has come to terms with one approach that is easy and has meaning. Unlike server for software, versioning is not as easy in APIs as it spans multiple dimensions like schema, logic.

##### Guarantees in a REST API

When using REST, here are the lowest common denominator that you can safely assume. Anything beyond the following assumption could anytime turn out to be a lie.

* **URL** - Do not assume the semantic meaning. Use it just to locate the handler function
* **GET** - All parameters are in the URL and headers
* **POST** - Parameters can be in URL, header and body
* **Never assume 400, 500, or 200 as business semantics** - Always just assume they are system (client, server) issues. Introspect how business errors are surfaced in the specific implementation as it will be different between different projects.

## Conclusion

We have relied on REST for almost 20 years and it is time for a change. We have to consider the complexities we see in our business today and ideate a new standard from he ground up. This time, we should really put practice into theory instead of the other way around.