---
layout: blog
title: How Zegetans Make APIs with Swagger
date: 2019-02-13 13:35 +0300
categories: 
published: false
author: Tom Nyongesa, Melvin Atieno, Ngari Ndung'u
blog-image: swagger/swagger.jpg
intro: We recently covered [API deep dive](2018-12-08-api-dive.md), where we got into the whats of APIs, how to build effective, efficient and usable APIs and also looked into the various tools one can use in the API lifecycle.In this piece, we would like to focus on Swagger, a tool that has been adopted greatly by developers across the globe. 
---

![Swagger](/assets/images/blog/{{page.blog-image}}){:class="img-responsive center"}


{{page.intro}}

We can't mention swagger without mentioning OpenAPI Specification. What is it then? 

The OpenAPI specification is a standardized way of describing RESTful services that enables both humans and machines to understand the services without much hustle. It gives stakeholders a means to define the specifics of what to build, with the definition acting as both a design and documentation tool.

API definition has led to the adoption of API driven architecture where the server and client communicate through HTTP using JSON as the requests' and responses' payload and relying on the REST CRUD methods. This has brought the backend architecture to the frontend allowing both the frontend and backend teams to work in tandem and be sure that their code will integrate seamlessly hence acting as a bridge. This is the reason behind the rise of Single Page applications(SPAs). OpenAPI has tooling around it to generate sample client and server code enabling each team to test their code before the actual components are complete. 

![spapi](/assets/images/blog/swagger/spa.jpg){:class="img-responsive center"}

You might come across openapi and swagger being used interchangeably on the internet. This is a testament to OpenAPI specification that it's actually an evolution from swagger specification. The specification was first developed as the Swagger specification before being renamed in 2015 after its donation to the Linux Foundation, by [smartbear](https://smartbear.com/) - bits of history there!

![concur?](/assets/images/blog/swagger/concur.webp){:class="img-responsive center"}

Swagger today refers to the suite of API development tools covering the entire API development lifecycle, from design to testing and deployment.

So, simply the difference is:

Swagger - toolchain

OpenAPI - Specification

An OpenAPI document, allows you to describe the following features for your entire API:

1. Resources.
2. Authentication
3. Paths
4. Methods
5. Request parameters/bodies
6. Responses
7. Examples.
8. Contact information license, terms of use etc.


## Getting Started with Swagger toolchain
[Swaggerhub](app.swaggerhub.com) is an online collaborative platform that enables API developers team up to design, define, test, document and produce APIs. It contains tools that are most needed by API developers and applicable in almost all cases. Swaggerhub's toolchain consists of the following major functionalities:

- API Definition
- API Validation
- API documentation
- CodeGen

To keep things more fun and practical you can checkout the [petstore](http://petstore.swagger.io) or head to the [resource section](#resources) at the end and check out the blogging API specification. You can clone it to your own API specification.

### API Definition

This is the design and description of APIs. 

OpenAPI documents can be written in both JSON and yaml. Yaml is the primary format used on Swagger but json is fully supported and specification examples are provided in both formats. 

OpenAPI specification consists of 3 main parts:

I. General Info

This is the part where you can give descriptive information about your API including API version, Title, Description, Licence info, contact info etc

An example:
~~~
info:
    title: Blog API
    description: A blog Api server
    termsOfService: http://example.com/terms/
    contact:
        name: API Support
        url: http://www.example.com/support
        email: support@example.com
    version: 1.0.0
~~~

`N/B: title and version fields are REQUIRED`

II. Servers

The servers section is a list of hosts where your API can be reached on.
The hosts could be e.g a testing server, sandbox server and production server.

Example:
~~~
servers:
    - url: https://developmentserverexample.server.com/v1
      description: The development server
    - url: https://productionserverexample.server.com/v1
      description: The production server
~~~

`URL field is REQUIRED`

III. Paths

This is where you describe the individual endpoints where the clients would be sending their requests to. You'll be required to describe how to authenticate and authorize the requests, the content types and information that goes into the bodies of requests and responses.

Example: 
~~~
	paths:
	  /blog-resource/:
	    get:
	      summary: Endpoint summary
	        requestBody: 
	          GET request body	                  
	        responses:
	         '201':
	          description: Created
	          content: 
	          	Response body
	            example:
	              data:
	                - type: example type
	                  id: 1
	                  attributes:
	                    title: first blog
	                    text: some text
	                    author_id: "1"
~~~
<span style="font-size: 18px;">Components part</span>

This is an optional part. It holds information on entities or models that the API specification deals with including:

1. schemas : An object containing the definition of input and output data types.
2. responses : An object containing responses that can be reused through out the specification
3. parameters : An object containing operation parameters that can be reused.
4. examples : An object containing, quite obviously, reusable examples.
5. Requestbodies : An object containing reusable single requets models.
6. Headers : Reusable headers
7. SecuritySchemes : An object that defines a security scheme that can be used by the operations.

Example:
~~~
components:
  schemas:
    entity:
     type: entity type
     description: entity description
     properties:
       property1:
         type: #string, integer etc
       property2:
         type: #array, etc
     required:
       - required_property1
       - required_property2
~~~

An entity is like an object in OOP. It can be a blog entity, article entity, comment entity etc for our Blogging System API.

Here's part of the blogging system API specification. Jump to the [resources section](#resources) for a deeper peek at the specification. 

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
          content:
          	application/vnd.api+json
          		schema:
          		  oneOf:
          		    - $ref: '#/components/schemas/BlogItems'
          		    - $ref: '#/components/schemas/404List
                example:
                  data:
                    - type: blog
                      id: 1
                      attributes:
                        title: first blog
                        text: some text
                        author_id: "1"
components:
  schemas:
    User:
      type: object
      properties:
        id:
          type: integer
        username:
          type: string
        password:
          type: string
          format: password
        full_name:
          type: string
      required:
        - username
        - password
        - full_name
~~~

#### JSONAPI

Now that we have an idea of the design's basic structure, we need to find a way to format our requests and associated responses. This is where JSONAPI comes to play. While OpenAPI Specification is used to describe service model(the API in general, endpoints, request metadata like headers, authentication, responses) and JSONAPI describes the schema for HTTP request/response bodies, simply put as the standard format for requests and responses' bodies. 

[Why is JSONAPI winning?](https://nordicapis.com/the-benefits-of-using-json-api/)
- Increased productivity due to shared convention.
- Reduced number of requests.
- Reduced amount of data in each request.

Example of a JSONAPI request:

	GET /blog?include=author HTTP/1.1

To solidify the second point on why JSONAPI is winning, from the JSONAPI request above, we are requesting the server to retrieve all the blogs including their authors, all in one GET request. Otherwise, you would have made 2 requests. See? 

Example of a JSONAPI response:

~~~
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
{
  "data": [{
    "type": "blog",
    "id": "1",
    "attributes": {
      "title": "Getting started with jsonapi!",
      "body": "The shortest article. Ever."
    },
    "relationships": {
      "author": {
        "data": {"id": "1", "type": "users"}
      }
    }
  }],
  "included": [
    {
      "type": "users",
      "id": "1",
      "attributes": {
        "username": "Melvin"
      }
    }
  ]
}
~~~

For more on JSONAPI, visit [JSONAPI documentation](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md)

While creating OpenAPI documents could be considered fun, it gets tiring, especially if you are designing a large scale API. 

Solution? Visual api design tools. Here's a list of the commonly used OAS visual design tools:

- [Restlet](https://studio.restlet.com/)
- [Spotlight](https://stoplight.io/)
- [APIBldr](https://apibldr.com/)
- [OpenAPI designer](https://openapi.design/)
- [OpenAPI GUI](https://mermade.github.io/openapi-gui/)

Feel free to explore them and settle on one that best suits your needs if you find the visual definition simpler. 

### API validation

Another key Swagger feature is API validation. It's a feature that allows you to quickly and efficiently check the syntax of your API specification according to the OAS standards and point them out smartly. Swagger editor actually helps you with syntax validation.

You can also perform tests and mocks on the API spec you've created to help you get the feeling of how the server would respond to requests made to the API. This will help you verify that your API spec is solid and ready for consumption.

[Swagger Inspector](https://inspector.swagger.io/) is a useful tool for such validation.

### API Documentation

Swagger lets you create your API documentation right from the API specification, such a super gem there. This is especially useful for the ever evolving APIs, where you would need to update your API documentation each time you add a new feature to your API. 

You can even merge an existing documentation for an API be it in the cloud storage or local storage.

It is recommended that even after drafting the documentation from within the Swagger specification you need to expound it further with the help of technical writers so that the final document can be understandable by your end users because it really plays a huge role in breaking or moulding the uptake of your API.

### Swagger CodeGen
With the advent of so many programming languages, you wouldn't know the implementation language of your API consumers, the developers. It would only be prudent to write client SDKs for the various languages but it would be a real pain for many companies especially smaller companies. Even the bigger companies would feel the pain of writing client codes for more than 20 major languages. This would mean hiring top developers for all these major languages. This is where Swagger Codegen comes in.

Swagger Codegen is a tool that automatically generates client SDKs for more that 40 languages from your API specification. You simply describe your specification and Swagger Codegen does the rest. If you need to make any changes, you would only need to make the change once to your API spec then Codegen will propagate the change across your client SDKs. Another gem them there, right?

With all the swagger features mentioned here and others like cloud storage, integration with source control tools like Git and deployment to API management platforms like AWS, it would be correct to put swagger as an all in one tool for your API lifecycle. So, let swagger handle things for you! It will help you save time, minimize errors and get things done faster.

### Mocking with Swagger

Maybe you've been following through with practical API definition on your Swaggerhub, would you love to test your API specification? This needs a backend service that would actually respond to requests made. Don't worry if you don't have or can't setup a backend server for this, Swagger got you covered with its Virtserver mock server. It comes automatically integrated into your API specs on Swaggerhub. You can confirm this under the 'servers' part in your API definition.

See [our blogging system API specification](https://app.swaggerhub.com/apis/tomshy/Searails_Blog_API/1.0.0) under 'servers' part.

You can test if the swagger auto-mocking worked by testing out the API endpoints. The easiest would be to test a GET request from the browser. Paste the Swagger virtserver url into the URL field on your browser and append your GET request endpoint. You should get a JSON formatted response on your browser. 

Want to try out other mocking tools? Refer to [API deep dive](2018-12-08-api-dive.md) for a list of mocking tools.

### Finally, 

Are you a Postman fan? It is amazing how OpenAPI Specification works with Postman besides testing the mock Swagger server. With a single Postman import, you can sync your OpenAPI Specification into Postman and quickly convert it into a Postman collection. The new 'OAS-postman' collection created can also get automatic updates once you make changes to your OAS, meaning no manual updates on Postman. 

For an example testing exercise, let's use postman. 

Steps:
1. Launch postman
2. Click 'NEW' to create a request
3. Paste the Swagger virtual server's URL from your API definition to the url request field on Postman.
4. Then perform all the requests on the endpoints defined in the API definition. 

If successful, consider your API working and mocked.

PS: OpenAPI has a collection of tools that come in handy when defining your OAS. You can check out the various [OpenAPI tools here](https://openapi.tools/)

## Resources


1. [Blogging System API specs](https://app.swaggerhub.com/apis/tomshy/Searails_Blog_API/1.0.0)
2. [Try out Swagger](https://app.swaggerhub.com/)
3. [Understanding JSONAPI](https://jsonapi.org/format/)
4. [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md)