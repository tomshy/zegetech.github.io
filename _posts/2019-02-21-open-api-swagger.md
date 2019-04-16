---
layout: blog
title: "Open API / Swagger: The missing link"
date: 2019-02-21 13:35 +0300
categories: developer
author: Tom Nyongesa
blog-image: swagger/swagger.png
intro: We recently [examined APIs](/blog/2018/12/08/api-dive.html), where we got into what APIs are, how to build effective, efficient and usable APIs and also looked into the various tools one can use in the API lifecycle. In this post, we would like to focus on Swagger / Open API, a tool that has been adopted greatly by developers across the globe.
---

![Swagger](/assets/images/blog/{{page.blog-image}}){:class="img-responsive center"}

{{page.intro}}

We can't mention swagger without mentioning Open API Specification 3.0

The Open API specification (OAS) is a standardized way of describing RESTful services that enables both humans and machines to understand the services clearly. It gives stakeholders a means to define the specifics of what to build, with the definition acting as both a design and documentation tool.

API definition has led to the adoption of API driven architecture where the server and client communicate through HTTP with JSON payloads in the requests and responses. This has brought the backend architecture to the frontend allowing both the frontend and backend teams to work in tandem and be sure that their code will integrate seamlessly hence acting as a bridge. As a result, Single Page applications (SPAs) have become popular. Open API has tooling around it to generate sample client and server code enabling each team to test their code before the actual components are complete.

![spapi](/assets/images/blog/swagger/spa.jpg){:class="img-responsive center"}

You might come across Open API and Swagger being used interchangeably on the internet. The specification was first developed as the Swagger specification before being renamed Open API in 2015 after it was donated to the Linux Foundation, by [smartbear](https://smartbear.com/) - bits of history there!

![concur?](/assets/images/blog/swagger/concur.webp){:class="img-responsive center"}

Swagger today refers to the suite of API development tools covering the entire API development lifecycle, from design to testing and deployment.

> So, simply the difference is:
> Swagger is a toolchain, OpenAPI is Specification

An Open API document, allows you to describe the following features for your entire API:

1. Resources.
2. Authentication
3. Paths
4. Methods
5. Request parameters / bodies
6. Responses
7. Examples.
8. Contact information license, terms of use etc.

## Getting Started with Swagger toolchain

[Swaggerhub](https://app.swaggerhub.com) is an online collaborative platform that enables API developers to collaborate in design, definition, testing, documentation and production of APIs. It contains tools that are most needed by API developers and applicable in almost all cases. Swaggerhub's toolchain consists of the following major functionalities:

- API definition
- API validation
- API documentation
- CodeGen

### API Definition

This is the design and description of APIs. Open API documents can be written in both JSON and Yaml. Yaml is the primary format used on Swagger but JSON is fully supported and specification examples are provided in both formats.

OpenAPI specification consists of 3 main parts:

### _I. General Info_

This is the part where you can give descriptive information about your API including API version, title, description, licence info, contact info etc

An example:

```yaml
info:
  title: Blog API
  description: A blog Api server
  termsOfService: http://example.com/terms/
  contact:
    name: API Support
    url: http://www.example.com/support
    email: support@example.com
  version: 1.0.0
```

**N/B: title and version fields are REQUIRED**

### _II. Servers_

The servers section is a list of hosts where your API can be reached on.
The hosts could be e.g a testing server, sandbox server and production server.

Example:

```yaml
servers:
  - url: https://developmentserverexample.server.com/v1
    description: The development server
  - url: https://productionserverexample.server.com/v1
    description: The production server
```

**URL field is REQUIRED**

### _III. Paths_

This is where you describe the individual endpoints where the clients would be sending their requests to. You'll be required to describe how to authenticate and authorize the requests, the content types and information that goes into the bodies of requests and responses.

Example:

```yaml
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
```

### _IV. Components (optional)_

This is an optional part. It holds information on entities or models that the API specification deals with including:

1. schemas : An object containing the definition of input and output data types.
2. responses : An object containing responses that can be reused through out the specification
3. parameters : An object containing operation parameters that can be reused.
4. examples : An object containing, quite obviously, reusable examples.
5. Requestbodies : An object containing reusable single requets models.
6. Headers : Reusable headers
7. SecuritySchemes : An object that defines a security scheme that can be used by the operations.

Example:

```yaml
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
```

An entity is like an object in OOP. It can be a blog entity, article entity, comment entity etc for a Blogging API.

Here's part of the blogging system API specification. Jump to the [resources section](#resources) for a deeper peek at the specification.

```yaml
openapi: 3.0.0
info:
  version: '1.0.0'
  title: 'searails'
  description: 'This is an API for searails blogging syst'
servers:
  - description: SwaggerHub API Auto Mocking. You can setup a real server to host your API.
    url: /yourserverurl.ext/

paths: # Path info goes here
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
          application/vnd.api+json:
            schema:
              oneOf:
                - $ref: '#/components/schemas/BlogItems'
                - $ref: '#/components/schemas/404List'
                example:
                  data:
                    - type: blogs
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
```

**JSONAPI Format**

Now that we have an idea of the design's basic structure, we need to find a way to format our requests and associated responses. This is where JSONAPI comes to play. While Open API Specification is used to describe the service model (the API in general, endpoints, request metadata like headers, authentication, responses), JSONAPI describes the schema for HTTP request/response bodies.

[Why is JSONAPI winning?](https://nordicapis.com/the-benefits-of-using-json-api/)

- Increased productivity due to shared convention.
- Reduced number of requests.
- Reduced amount of data in each request.

Example of a JSONAPI request:

    GET /blog?include=author HTTP/1.1

To solidify the second point on why JSONAPI is winning, from the JSONAPI request above, we are requesting the server to retrieve all the blogs including their authors, all in one GET request. Otherwise, you would have made 2 requests. See?

Example of a JSONAPI response:

```json
HTTP/1.1 200 OK
Content-Type: application/vnd.api+json
{
  "data": [{
    "type": "blogs",
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
```

For more on JSONAPI, visit [JSONAPI documentation](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md)

While creating Open API documents could be considered fun, it gets tiring, especially if you are designing a large scale API.

Solution? Visual api design tools. Here's a list of the commonly used OAS visual design tools:

- [Restlet](https://studio.restlet.com/)
- [Spotlight](https://stoplight.io/)
- [APIBldr](https://apibldr.com/)
- [OpenAPI designer](https://openapi.design/)
- [OpenAPI GUI](https://mermade.github.io/openapi-gui/)
- [API Curio by Redhat](https://www.apicur.io/) (opensource)
- [Apigility](https://apigility.org/) (opensource)

Feel free to explore them and settle on one that best suits your needs if you find the visual definition simpler.

### API validation

Another key Swagger feature is API validation. It's a feature that allows you to quickly and efficiently check the syntax of your API specification according to the OAS standards and point them out smartly. Swagger editor actually helps you with syntax validation.

You can also perform tests and mocks on the API spec you've created to help you get the feeling of how the server would respond to requests made to the API. This will help you verify that your API spec is solid and ready for consumption.

[Swagger Inspector](https://inspector.swagger.io/) is a useful tool for such validation.

### API Documentation

Swagger lets you create your API documentation right from the API specification. This is especially useful for the ever evolving APIs, where you would need to update your API documentation each time you add a new feature to your API.

It is recommended that even after drafting the documentation from within the Swagger specification you need to expound and simplify it further by adding clear detail so that the final documentation can be well understood by your end users because it really plays a huge role in moulding the uptake of your API.

### Swagger CodeGen

With the advent of so many programming languages, your developers would build API clients in any language. It would be helpful to write client SDKs for the major languages although it may be tasking for many companies especially smaller ones. Even larger companies would feel the pain of writing client codes for more than 20 major languages. This is where Swagger Codegen comes in.

Swagger Codegen is a tool that automatically generates client SDKs for more that 40 languages from your API specification. You simply describe your specification and Swagger Codegen does the rest. If you need to make any changes, you would only need to make the change once to your API spec then Codegen will propagate the change across your client SDKs.

With all the swagger features mentioned here and others like cloud storage, integration with source control tools like Git and deployment to API management platforms like AWS, it would be correct to say that "swagger is an all in one tool for your API lifecycle". So, let swagger handle things for you! It will help you save time, minimize errors and get things done faster.

### Mocking with Swagger

Once you have your API defined, it only makes sense to start the development process. The backend team will be tasked at reverse engineering the specificatin to produce the API responses, and the frontend team will be tasked at building an app that will consume the API. Mocking allows the api consumers to get going before the backend is ready with the actual api. Swagger definition can be used to build mock servers that produce valid API responses according to the definitions. Tools for doing this include swaggerhub, apiary and others [listed here](2018-12-08-api-dive.md#6-api-mocking).

In testing an API server, you might use a popular api client tool like Postman. As a matter of fact, Open API Specification works with Postman besides testing the mock Swagger server. With a single Postman import, you can sync your Open API Specification into Postman and quickly convert it into a Postman collection. The new 'OAS-postman' collection created can also get automatic updates once you make changes to your OAS, meaning no manual updates on Postman.

PS: OpenAPI has a collection of tools that come in handy when defining your OAS. You can check out the various [Open API tools here](https://openapi.tools/)

## Resources

1. [Blogging System API specs](https://app.swaggerhub.com/apis/Melvin1Atieno/blog-api/1.0.0)
2. [Try out Swagger](https://app.swaggerhub.com/)
3. [Understanding JSONAPI](https://jsonapi.org/format/)
4. [OpenAPI Specification](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md)
