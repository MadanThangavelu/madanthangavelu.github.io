---
layout: post
title: "REST URL design - An opinionated list of best practices"
author: madan
categories: [ Software]
description: A quick overview on why REST is not a standard but merely an architectural style that most of us use but do not understand.
image: assets/images/posts/2020-05-22-rest-url-design/title.png
---

REST APIs come in all flavors. A quick review of all APIs provided from twitter, facebook, google, twillio and other companies make it clear that there is no single standard. You can checkout my other post on why REST APIs are outdated. 

This article is a list of practical recommendations when developing new REST APIs design. Consider them as an oppunionated design guidance. There is no right and wrong, but pick one and stick with it. 

* TOC
{:toc}

#### Item 1: &nbsp; &nbsp;Use only GET & POST

HTTP and REST spec provide a variety of request verbs like GET, POST, PATCH, PUT and DELETE.

###### Recommended

* If you control your callers, stick to only GET and POST HTTP methods. This is true when all the callers are internal company products. Let us look at each of the HTTP verbs and understand why I suggest you to stay away from them.

###### NOT Recommended

* PUT
    - This verb puts too much knowledge of internals to the client. The client needs to reconstruct the exact payload with all the relationships which will force the client to understand all the foreign key relationships. 

    - Imagine you have a restaurant table backing a /restaurant object. A PUT during a name update will force you to understand the menu items foreignkeys. 

* PATCH
    - Absolutely no reason to know whether it was a partial update or a full update. Infact sometimes partial updates might put your backend into a logically inconsistent state. 
    - Imagine the restaurant patch updates the name, but new restaurant creation via POST has validationst that are not replicated into the PATCH API.
    - Not all frameworks implement this verb correctly. Here is an [old issue][1] from the laravel framework community. If your company is developing software on new languages or frameworks, it is better to stick with commonly used verbs.

* DELETE
    - This verb does [not support passing data via body][2]. This forces all parameters to be passed through query parameters and query parameters can sometimes be insufficient. This will result in some of your APIs using DELETE while others default back to POST resulting in inconsistencies.

#### Item 2:  &nbsp; &nbsp;Use PATCH, DELETE & PUT only if

If your company actively sells APIs for integrations to external vendors into your product, try to support all HTTP verbs. You may still choose to convert incoming requests to a POST before your serving layer.

Not doing so will result in [Cross Vendor integrations issues][3] as we see in the support ticket.


#### Item 3: &nbsp; &nbsp;Casing in the url

There are hand full of casing used by various companies and there appears to be no true consensus. I do recommend picking one and sticking with it.

###### Recommended

* [/use-kebab-case][4]
* keep the urls characters in lowercase [0-9][a-z]

###### NOT Recommended
* /underscores_between_urls
* /camelCase
* Special characters outside of [0-9][a-z]
* Do not use upper case in the URL (using them in query is fine)

###### Some industry usages
* Twitter - [snake case][5]
* Square - [kebab case][6]
* Google Maps - [no case][7]
* Stack overflow [kebab case][8]
* AWS API [cameCase][9]

#### Item 4: &nbsp; &nbsp;Do not have /deeply/nested/url/paths

Using pure REST semantics might force some APIs to be really nested.

###### Recommended
* Max depth 2 
    - GET or POST /area/1/restaurant/5
    - GET or POST /menu/6/menuitem/4
    - POST /menu-editor/update-menu
* Browser urls and proxies allow a max length. Set a smaller value and Respond with a 414 statuscode.

###### NOT Recommended
* /area/1/restaurant/5/menu/6/menuitem/4

#### Item 5: &nbsp; &nbsp;Specifying boolean

There are many ways to represent boolean as string, numbers in query, params and body. It is critical to pick one and stick with it. 

###### Recommended
* use "true", "false" in query - /?active=true
* use "true", "false" in body - {"active": true}

###### NOT Recommended
* "0" & "1" (string) - /?active=1
* 0 & 1 (int) - {"active": 1}

#### Item 6: &nbsp; &nbsp;Specifying arrays
There are times when you would want to pass an array to the server using a GET request. Pick one to stick with it. 

###### Recommended
* /?key[]=1&key[]=2

###### NOT Recommended
* /?keys[]=1&keys[]=2 (note plural)
* /?key=1&key=2
* /?key=1,2
* /?key=[1,2]

#### Item 7: &nbsp; &nbsp;Specifying hash or dictionary
Although relatively rare, there are times when you would want to pass a dictionary to the server using a GET request. Pick one to stick with it.

###### Recommended
* /person?details.age=5&details.gender=male

###### NOT Recommended
* /person?details[age]=5&details[gender]=male
* url encoded string - /person?hash=details%5Bage%5D%3D5
* multi level nesting - details.age.birthyear

#### Item 8: &nbsp; &nbsp;Design your batch API the same as single API
Very often your APIs start with returning a single item and operating on a single item. As performance bottlenecks arise, there will be a need for a bulk api. If you stick with this plan, you will not have to double implement a shim everytime you need a bulk operation on an existing API. 

###### Recommended
* Collection - /menu-items
* Getting a single Resource - /menu-items/a
* /menu/remove-items?id[]=20&id[]=30

###### NOT Recommended
* Collection - /bulk-menu-items or /all-menu-items
* Getting a single resource with - /menu-item
* /menu/remove-item?id[]=20&id[]=30 (note singlar name remove-item)

#### Item 9: &nbsp; &nbsp;Using + in query params
"+" is a special character that can be used in query. When used, your query will replace the "+" for a "space" when the query is read. 

* /books?q=a+5 server reads as “a 5”
* /books?q=a+++++5 server reads as “a&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5”

This will be handy in cases like sort, as we will see in the next item.

#### Item 9: &nbsp; &nbsp;Sort
Across all APIs use a standard query, body field name for sorting operations. Use the DESC and ASC keywords.

###### Recommended
* /books?**sort[]**=title**+DESC**&sort[]=author+ASC

###### NOT Recommended
* /menu?**sort**=title&**desc**=title or ?**sort**=title&**asc**=author
* /menu?**sortby**=itemname

#### Item 9: &nbsp; &nbsp;Filter & Exclude
A number of times you would want to filter (select) or exclude (unselect) the response data based on certain parameters. In those cases, reserve special query parameters. 

###### Recommended
* /menu-items?filter=key:value
* /menu-items?filter=ingredient:tomatoes
* /menu-items?filter[]=ingredient:tomatoes&filter[]=size:xl

There are cases where you will have to use lte, gte to represent 'less than equal' and 'greater than equal'. In those cases the following is recommended.

###### Recommended
* /menu-items?filter=key**_lte**:value
* /menu-items?filter=price**_lte**:117
* /menu-items?filter=price**_gte**:117

There are also caess where we will need boolean operations for our filters. In those cases we will have to introduce boolean operations into the filter query. 

###### Recommended
**A && B**

/menu-items?filter[]=key:A&filter[]=key:B

**A \|\| B**

/menu-items?filter[]=key:A&filter[]=key_or:B

**A \|\| B && C**

/menu-items?filter[]=key:A&filter[]=key_or:B&filter[]=key_and:B

**A && B \|\| C && D**

/menu-items?filter[]=key:A&filter[]=key_or:B&filter[]=key_and:B

###### NOT Recommended

* A complex long filter and exclude combination. In those cases consider using search API.

#### Item 10: &nbsp; &nbsp;Pagination
Almost all APIs returning a list needs pagination once the length of the return data is more than a few hundreds.

###### Recommended
* /menuitems?offset=5&limit=10

###### NOT Recommended
* /menuitems?page[number]=5&page[size]=10
* Doing the pagination through headers
    * Content-Range
    * Accept-Range

#### Item 10: &nbsp; &nbsp;Treat advanced search differently

A lot many times filter and sort are used to do complex searching operations via get API. This can get complex quickly. 


###### Recommended
* Use POST for all searches
* Cache the results in the backend based on query hash
* Allow for accessing query result using GET with a query hash

###### NOT Recommended
* GET for complex /search?itemname=salad&area=california&more


#### Item 11: &nbsp; &nbsp;Global search

###### Recommended
* ?q=value&scope=value
* GET https://domain.com/e/?q=value&scope=menu

###### NOT Recommended
* POST https://domain.com/?q=value&scope=menu use ***item 10 instead***

#### Item 12: &nbsp; &nbsp;Reserved URLs
* /health/*
* /debug/*
* /metrics/*
* /status/*

#### Item 13: &nbsp; &nbsp;Content type in URL
Different clients would require different response types, e.g, HTML, JSON APIs, binary reponse, XML. The client should be able to specify the response data it would need. 

###### Recommended
* use the content-type header to specify if you want xml or json

###### NOT Recommended
* adding it to the url like /device-management/managed-devices**.xml**
* adding it to the url like /device-management/managed-devices**.json**
* adding it to the url like /device-management/managed-devices**.html**


#### Item 14: &nbsp; &nbsp;Versioning
An API has versioning across two dimensions

###### Logic
A change where /menu-editor/update-menu restricts updating the menu of archived items, but it used to allow it for previous version of the mobile apps (that are still used by customers).
###### Schema
A change where /menu-editor/update-menu now optionally takes another value "rating" for each menu item.

Logic and schema can be a "breaking change" or a "non breaking change" and should be treated differently when versioning.

###### Recommended
* Attempt to remain backward compatible in logic and schema.
* Non-breaking changes (rarely used)
    - Send version in a separate header if needed. It might be optionally respected.
    * X-COMPANY-API-MINOR-VERSION:1.5
* Breaking change for internal APIs
    - Add a new version to the path of the API being changed
        * /v3/menu-editor/update-menu
        * /v4/menu-editor/menu-items
* Breaking change for external APIs
    - Accumulate changes into a single upgrade for all APIs
        * <s>/v3/menu-editor/update-menu</s>
        * /v4/menu-editor/menu-items
        * /v4/menu-editor/update-menu


###### NOT Recommended

* Accept: application/vnd.megacorp.bookings+json; **version=1.0**
* /menu-editor/update-menu?**version=v3**
* /**v3**/menu-editor/**v1**/update-menu

#### Item 14: &nbsp; &nbsp;Provide "pretty" query

With JSON APIs a developer would be accessing your API during development. For every query, add a ?pretty=true so that the backend will print a json in a nice readable manner. 


###### Recommended


###### NOT Recommended


#### Item 14: &nbsp; &nbsp;Naming the resource

Naming resources is one of the most complex topics of URL design. 


URL - Decide on the resources, they should not 100% map to your normalized table names
* Doing so will make the API client business heavy (resulting in tight coupling) and network will be chatty
* Resources are logical business construct
* Transaction across APIs will not be true, will leave the backend in inconsistent state
* Low level APIs put the logic into client if the APIs are too granular
* Mapping to 100% tables will make the APIs too chatty
* Use singular
    * Plural is different /goose vs /geese - why?
* Ask questions before choosing the resource
    * CustomerEnrollment vs (Customer & Enrollment)
    * Keep them as nouns
    * Use verbs for action or calculation
* Questions to validate what is a resource
    * Will the business be asking questions about this resource?
* Avoid using CRUD HTTP methods or verbs (e.g. GET, DELETE, etc.) in URL paths.

Worksheet:

* Imagine you have a food ordering website and you are building rest APIs for it. 
    * Here are tables in the database:
        * User
        * Account
        * PaymentAccount
        * Payment
        * SubscriptionType
        * Menu
        * MenuItem
        * MenuMenuItem
        * SubscriptionTypeMenuMenuItem
        * Cart
        * CartItem
        * CartItemMenuMenuItem
    * Questions to ask yourself
        * What are the top 5 interactions a customer does with the system
            * ______________________
            * ______________________
            * ______________________
        * Step 1: Hypothetical answers
            * Signup and Add payment
            * Create a new cart and add menu items
            * Update items on a card
            * Checkout
        * Step 2: Refine and consolidate
            * Account setup (signup, add payments)
            * Shopping (create a cart and add menu items)
            * Configure menu (creates menu, edit menu items)

Example:

* Imagine GitHub has the following urls
    * /users/:userName/repos (many to many)
    * /repos/:repoName/users (many to many)
    * Just use /contributions?user&repository
* Let us take a complex example
    * 
URL - More than 5 URLs look similar? You are doing something wrong
* /getHungerlevel, /getSmallDogHungerLevel, /getAllHungerLevels, /getTopFiveHungerLevels
    * /hunger?id=5
    * /hunger
    * /hunger?filter=type:small
    * /hunger?filter=type:small
    * /hunger?filter=type:small&limit=5
                We will see what to do when you have to do multiple filters
* If the number of filters > 5 stop and use the query API
* Alias for common queries are ok
    * /tickets/recently_closed

[1]: https://laravel.io/forum/02-13-2014-i-can-not-get-inputs-from-a-putpatch-request
[2]: https://stackoverflow.com/questions/15619075/webapi-delete-not-working-405-method-not-allowed
[3]: https://success.salesforce.com/ideaView?id=0873A000000PST0QAO
[4]: https://medium.com/better-programming/string-case-styles-camel-pascal-snake-and-kebab-case-981407998841
[5]: https://developer.twitter.com/en/docs/api-reference-index
[6]: https://developer.squareup.com/reference/square/labor-api
[7]: https://developers.google.com/places/web-service/query
[8]: https://api.stackexchange.com/docs
[9]: https://docs.aws.amazon.com/AWSEC2/latest/APIReference/API_AcceptVpcEndpointConnections.html
[10]: http://blog.luisrei.com/articles/rest.html
