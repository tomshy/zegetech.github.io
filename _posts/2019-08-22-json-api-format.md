---
layout: blog
title: JSON:API - REST from the Chaos
date: 2019-08-22 16:24 +0300
categories: developer, api
published: true
author: Ngari Ndung'u
blog-image: jsonapi/jsonapi.png
intro: |
  If you're a developer you've probably consumed or built more than one API.
  It is also probable that any two of those APIs differed considerably in how their requests and responses are structured.
  But what if they were not? What if you could have a consistent and instantly recognizable structure?
  Well, with JSON:API, you can.
---
![Hallway with concrete pillars](/assets/images/blog/{{page.blog-image}}){:.img-responsive .center}

{{page.intro}}
## What it is
[**JSON:API**](https://jsonapi.org/format/) specification is exactly what it reads like. But just so we're on the same page...
First, lets decompose the name; JSON - the data format we all know and love(or hate) and API - the lifeblood of today's technology industry.
The JSON:API specification dictates request and response formats to clients that make JSON requests and the servers that respond to those requests.

JSON:API provides a standard structure that makes the different components of a response/request easy to pick out. Here's are two example JSON:API document:
``` json
  {
    "data": {
      "type": "",
      "id": "1",
      "attributes": {}
    },
    "links": {},
    "included": [],
    "meta": {}
  }
``` 

```json
{
  "errors": [
    {
      "status": 400,
      "detail": "Something went wrong there",
      "id": 134324,
      "meta": {
        "resource_id": "1",
        "other_field": "NH90000000"
      }
    }
  ]
}
``` 

`data` represents the *resource object(s)* or *primary data* requested or being manipulated. A document MUST contain at least one of the following top-level members: `data`, `errors` and `meta`. The members `data` and `errors` MUST NOT coexist in the same document.

The promise of JSON:API? That every compliant API you use will only ever have these components(plus what may be added to the specification).
Once you know what these keys - and their subkeys - mean, you'll know for all APIs!

## Why use it?
### Advantages for the consumer
**Reduced network calls** - resources related to the current resource being retrieved from the server can be included in the response using the `included` key.
Using a blog example, it is common for comments on an article to be displayed below it.
It makes sense therefore that an API responds to a request to e.g `/articles/1/` with the article's comments in tow.
``` json
  {
    "data": {
      "type": "articles",
      "id": "1",
      "attributes": {
        "title": "A title to end all titles"
      }
    },
    "included": [
      {
        "type": "comments",
        "id": "1",
        "attributes": {
          "text": "What's with the lame title?!!"
        }
      }
    ]
  }
```
**Easier API discovery with [HATEOAS](https://restfulapi.net/hateoas/) support** - 
Hypermedia As The Engine Of Application State is a REST concept that allows the inclusion of context-relevant links in a response.
This allows the client to navigate the API without much prior knowledge of its capabilities.
It effectively makes the API self-documenting making manual exploration of the API much easier.
This functionality is provided via the optional `links` key of a jsonapi document.

**Easier testing with well defined structures** - 
The structure of a jsonapi document renders itself well for testing.
While testing for high-level success/error responses, we can simply check that the API responds with a data/errors body respectively.
We all do some Postman/Curl/whatever testing right? The readability of a JSON:API response really shines here:
``` json
{
  "errors": [
    {
      "status": "400",
      "summary": "Invalid request parameters",
      "detail": "You passed an invalid date, didn't you?"
    }
  ]
}
```
``` json
{
  "date": "Invalid date"
}
```
Now, I might be biased here, but with the JSON:API response, it is immediately obvious that the request failed and the HTTP status code.
With the flat JSON object you might need to go fishing.

### For the API developer
**Time saver**
> If you’ve ever argued with your team about the way your JSON responses should be formatted, JSON:API can be your anti-bikeshedding tool.

APIs are usually the public facing aspects of the products we build. As such it is only natural to want to make them beautiful.
Problem is, everyone will have an opinion as to what beauty is. And, that opinion might change the next day.
JSON:API saves time by focusing the conversation away from structure to content.

![User style guide](/assets/images/blog/jsonapi/code_quality.png){:.img-responsive .center}

**Convention over configuration**
JSON:API makes a number of pragmatic choices that the developer would normally have to spend time thinking over.
Among the decisions that are made for you include what HTTP response codes to use and how to support sorting, pagination and filtering.

**Errors are a first-class citizen**
`data` for successful requests, `errors` for failed requests.
Errors, not one, but as many as you might want to inform the consumer of.
It is only so easy for us to send back plain error status codes. And keep the developer guessing as to how they wronged the gods.
But we can do better, and JSON:API gives us an [error object](https://jsonapi.org/format/1.1/#errors) with enough detail to eliminate the guesswork.
This adds to the overall feel of quality for an API.

## What are the challenges?
For the API consumer, I don't really see any challenges.
If anything, there's just the minor inconvenience of typing out an extra level of nesting during manual testing.

The API developer has a bit more work to do, but nothing I would call a deal breaker;
- While you can go through the entire specification in one sitting, it's something you'll need to refer back to constantly.
- The JSON:API media type `application/vnd.api+json` will usually require some extra work to support server-side.
- The organization of the specification around resource objects can feel limiting. The requirement for a `type` and `id` on `data` objects can be especially un-intuitive for some endpoints.
- The extra level of nesting translates to more typing when writing your [API definition](2019-02-21-open-api-swagger.md).

## But, where do I start?
From [the specification](https://jsonapi.org/format) of course.
The documentation is clear, concise and easily navigable. It's like they knew we'll keep going back!
While it is conceivable that you could build a compliant API from scratch, you don't have to.
There is [tooling](https://jsonapi.org/implementations/) built around the spec to ease the transition.
There are both client and server-side implementations, and chances are, your favorite language is covered.

## So...
We've been transitioning to JSON:API, and I can't claim that it was an easy sell.
There were times when we question the suitability of the specification, mostly when we're feeling lazy or shackled by old structure thinking. 
Overall we are sticking with it because we feel that it gives us consistency, a clean structure and provides room to extend our APIs in the future. In our quest to find a solution to ground as through the [challenges of API development](2018-12-08-api-dive.md), we evaluated a number of options and JSON:API won. In a sense, we are bringing order and efficiency to our API development process, not fogetting our mantra as zegetech... **Be Kind to Future Self**