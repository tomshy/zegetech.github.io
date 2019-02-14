---
layout: blog
title: How Zegetans Make APIs with Swagger
date: 2019-02-13 13:35 +0300
categories: 
published: false
author: Tom Nyongesa
blog-image: swagger/swagger.jpg
intro: We recently covered [API deep dive](2018-12-08-api-dive.md), where we got into the whats of APIs, how to build effective, efficient and usable APIs and also looked into the various tools one can use in API lifecycle.In this piece, I would like to focus on Swagger, a tool that has been adopted greatly by developers across the blog. 
---

![Swagger](/assets/images/blog/{{page.blog-image}}){:class="img-responsive center"}


{{page.intro}}

This is a walkthrough of the standardized and modern way of creating APIs that will help speed things up to allow you to focus on the business logic, swagger is based on this principle. 

We can't mention swagger without mentioning OpenAPI Specification. What is it then? 

OpenAPI specification is a standardized way of describing RESTful services that enables both humans and machines to understand the services without much hustle. If this is the first time you are seeing this, it may be confusing to distinguish between OpenAPI and Swagger. OpenAPI specification is a Swagger specification evolution. It got separated as a standalone initiative back in 2015 after its donation to the Linux Foundation. Currently, Swagger is just but an API tooling service.

## Getting Started with Swagger toolchain
[Swaggerhub](app.swaggerhub.com) is an online collaborative platform that enables API developers team up to design, define, test, document and produce APIs. It contains tools that are most needed by API developers and applicable in almost all cases. Swaggerhub's toolchain consists of the following major functionalities:

- API Definition
- API Validation
- API documentation
- CodeGen

To keep things more fun and practical you can checkout the [petstore](http://petstore.swagger.io) or head to the [resource section](#resources) at the end and check out the blogging API specification, clone it to your own API specification.

### API Definition

This is the design and description of APIs. Since Swagger is primarily focused on creating RESTful web services, it would be important to quickly look at how an HTTP request and response looks like! 

Here is an example HTTP/1.1 request

~~~
	GET /blogs HTTP/1.1
	Date: Mon, 13 Feb 2019 12:28:53 EAT
	User-Agent: Chrome/71.031
	Host: apihost
	Accept-Language: en-us
	Connection: Keep-Alive
	
~~~

To efficiently design an API that would serve such a request, a developer needs to clearly define the METHOD spefication, HEADER specification, BODY specification and the URL specification for a simple consumption of requests which would be sent to application servers for computation and eventually responses. 

Here is an example of an HTTP response.

~~~
HTTP/1.1 200 OK
Date: Mon, 13 Feb 2019 12:28:53 EAT
Server: Apache/2.2.14 (Win32)
Content-Length: 88
Content-Type: application/json
Connection: Closed
~~~

[More on HTTP requests and responses](https://www.tutorialspoint.com/http/)

Swagger comes in to ensure a seamless communication between the client and the server.

How?

OpenAPI documents can be written in both JSON and yaml. Yaml is the primary format used on Swagger but json is fully supported and specification examples are provided in both formats.

[JSON API specification standard format](https://jsonapi.org/format/)

## Structure of an OpenAPI Definition Document

Any definition consists of 3 main parts:
- General Info
This is the part where you can give descriptive information about your API including API version, Title, Description, Licence info, contact info etc

- Servers
The servers section is a list of hosts where your API can be reached on.
The hosts could be e.g a testing server, sandbox server and production server.

- Paths

This is where you describe the individual endpoints where the clients would be sending their requests to.
You'll be required to describe how to authenticate and authorize the requests, the content types of both requests and responses and the structure of the information that goes into the bodies of requests and responses.

Here's part of the blogging system API specification. You can jump to the [resources section](#resources) for a deeper peek into the specification. 

~~~
openapi: 3.0.0
info:
  version: '1.0.0'
  title: 'searails'
  description: 'This is an API for searails blogging syst'
servers:
  - description: SwaggerHub API Auto Mocking. You can setup a real server to host your API.
    url: /yourserverurl.ext/

paths:# Path info goes here
	/users:
    post:
      summary: Creates a user.
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/User'
            example:
              username: user01
              password: superstrongpassword
              full_name: "super long name"
                  
      responses:
        '201':
          description: Created

~~~

## Request/Response Structure
It is almost guaranteed that if you and I were to write a definition for the same API, we would have different data structures for the requests and most certainly for the responses.
We use a JSON specification to mitigate against endless discussions around how to structure JSON data.
The [JSON:API](https://jsonapi.org/format/) specification provides a well defined structure and rules to write consistent JSON data.
Besides enabling a team to produce a consistent definition even while working in parallel, the requests and responses will be immediately familiar to developers familiar with the JSON:API specification.
~~~ json
{
  "data": {
    "type": "blog",
    "id": "1",
    "attributes": {
      "title": "quite the title",
      "text": "and the text to go with it"
    }
  }
}
~~~
The above represents a single blog *resource*, which would be the response to a `GET /blogs/1` request. This is the typical structure for a JSON:API resource.

Besides recommended request/response structures, the specification also gives guidance on expected HTTP response codes and what data should accompany them.
It also includes a structure for errors and a means to model *relationships* between your API's resources.

## Writing your Definition
Swaggerhub provides an [editor](https://editor.swagger.io/) where you could write in your API definition. However, you could also create the definitions using GUI tools to help you create the specification visually. Some of the most common GUI API specification editors include:

- [Restlet](https://studio.restlet.com/)
- [Spotlight](https://stoplight.io/)
- [APIBldr](https://apibldr.com/)
- [OpenAPI designer](https://openapi.design/)
- [OpenAPI GUI](https://mermade.github.io/openapi-gui/)

Feel free to explore them and settle on one that best suits your needs if you find the visual definition simpler. 

### API validation

Another key Swagger feature is API validation. It's a feature that allows you to quickly and efficiently check the syntax of your API specification according to the OAS standards and point them out smartly. 

You can also perform tests and mocks on the API spec you've created to help you get the feeling of how the server would respond to requests made to the API. This will help you verify that your API spec is solid and ready for consumption.

### API Documentation

Swagger lets you create your API documentation right from the API specification, such a super gem there. This is especially useful for the ever evolving APIs, where you would need to update your API documentation each time you add a new feature to your API. 

You can even merge an existing documentation for an API be it in the cloud storage or local storage.

It is recommended that even after drafting the documentation from within the Swagger specification you need to expound it further with the help of technical writers so that the final document can be understandable by your end users because it really plays a huge role in breaking or moulding the uptake of your API.

### Swagger CodeGen
With the advent of so many programming languages, you wouldn't know the implementation language of your API consumers, the developers. It would only be prudent to write client SDKs for the various languages but it would be a real pain for many companies especially smalle companies. Even the bigger companies would feel the pain of writing client codes for more than 20 major languages. This would mean hiring top developers for all these major languages. This is where Swagger Codegen comes in.

Swagger Codegen is a tool that automatically generates client SDKs for more that 40 languages from your API specification. You simply describe your specification and Swagger Codegen does the rest. If you need to make any changes, you would only need to make the change once to your API spec then Codegen will propagate the change across your client SDKs. Another gem them there, right?


With all the swagger features mentioned here and others like cloud storage, integration with source control tools like Git and deployment to API management platforms like AWS, it would be correct to put swagger as an all in one tool for your API lifecycle. So, let swagger handle things for you! It will help you save time, minimize errors and get things done faster.

## Resources

1. [Blogging System API specs](https://app.swaggerhub.com/apis/tomshy/Searails_Blog_API/1.0.0)
2. [Try out Swagger](https://app.swaggerhub.com/)
3. [Understanding JSONAPI](https://jsonapi.org/format/)
4. [OpenAPI Specification](https://github.com/tomshy/OpenAPI-Specification/blob/master/versions/3.0.2.md)
