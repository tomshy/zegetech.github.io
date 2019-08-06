---
layout: blog
title: Unveiling MPESA Daraja 2.0
date: 2019-06-18 13:08 +0300
categories: 
published: false
author: Tom Nyongesa
blog-image: daraja-2/daraja1.png
intro: This never happened, Mpesa haven't launched a new API. What if they did? What would it mean? What would it be like? Same reliability of G2 SOAP API, accessibility of Daraja, simplicity of Stripe API and Security of Visa API?  
---
![daraja](/assets/images/blog/{{page.blog-image}}){:.img-responsive .center}<br>
{{page.intro}}

Well, my thoughts? It would have been the perfect product the community never got. Perfect here sounds strong but it doesn't necessarily discredit [Daraja](https://developer.safaricom.co.ke/apis) as imperfect. Most of us in the Fintech space have integrated the Daraja REST API with a handful of us integrating the legacy G2 SOAP API. 

The legacy G2 SOAP API is the reliable MPESA API. However, it has attracted less interest among the developers perhaps because of its clumsy nature, the WSDL format and the fact that it wasn't really public. Going public led to the development of Daraja, an improved REST based version. 

![daraja](/assets/images/blog/daraja-2/daraja.jpg){: .img-responsive .center}

Daraja has been in existence for a while now and working relatively well. However, the general feeling of the Kenyan tech community specifically on MPESA integration clearly shows that the community in the Fintech space is just 'surviving'. Our recent release of the Kenyan Payment Integration survey solidifies my arguement. Going through the survey exposes the fact that the current Daraja API has got a relatively poor documentation, lacks sample integration codes, not so reliable test bed, poor security implementation and many other reasons among them a complex onboarding process. Here's the link to the [survey findings](2019-03-21-payments-kenya.md).

Part of findings of the survey tells us that the integration or rather testing of the Daraja API isn't a walk in the park either:

![ease of integration](/assets/images/blog/daraja-2/ease_of_integration.png){: .img-responsive .center}

Can it be better? Is it even possible to improve on it? Can we make it simpler?Yes, a strong **YES**. But simple isn't simple enough unless we put it into context and define what simple really looks like. 

## The Pain of integrating APIs

Looking back at the [Payment survey](2019-03-21-payments-kenya.md) and [Payment gateways in Kenya](2019-01-14-payment-gateways-Kenya.md), the main challenges experienced by the community while integrating Payments can be summarized to the following main points:

1. Lack of Documentation and sample code
2. Lack of testing platforms(widely known as sandboxes)
3. Lack of standardization
4. Poor Security
5. Long frustrating onboarding process
6. Reliability
7. Lack of technical support(customer support)

Simplicity simply means addressing these issues and other unlisted issues experienced while integrating the current Daraja APIs.

Let's now jump straight into addressing them, think through them and try to determine whether we can design a beautiful Daraja API that's easy to use, standardized, secure, easy to connect to, *consitent in terms of data formats and naming and one with self descriptive requests and responses.*

### Design

The design of any API, not only financial APIs, greatly affects the above issues. Since APIs are mostly developed for use by others, it is strongly recommended that a standardized and widely accepted format should be followed while designing it. The standardization runs right from overlooked things like naming conventions, data formats like date formats to technical things like API definition/specification and the widespread acceptability of REST APIs. 

It's also evident from the Payments survey that more than 90% of the developers interviewed prefer using REST APIS. This simply dictates that Daraja 2.0 should be in REST. At this age and times, REST and standardization can never be in the same paragraph without mentioning JSONAPI. JSONAPI is a widely spread format or API specification that specifies how a client should request for a resource and conversely how a server should respond with resources. The beauty of JSONAPI is that it excels at readability, flexibility and discoverability and greatly improves on efficiency by minimizing the number of requests and responses sent between the clients and the server. Daraja 2.0 should definitely be written according to the JSONAPI format, here's more on [why we think JSONAPI is great](2019-06-18-why-use-json-api-format.md).

With a good design and use of latest community accepted API design tools, we could address documentation, sample codes and standardization issues experienced. Here are some of the top [API design tools](https://openapi.tools/).

### Security

Security is another major concern of the community. Even though the current Daraja implementation has proved to be secure, it partly puts the security weight on developers which shouldn't be the case. 

For example, asking developers to make a combination of certain required variables, encrypting the result and including it in the request body further complicates the request payload. Conventionally, security credentials should be included in the headers and not having many mini credentials like how it's done in Mpesa Daraja - *[see example below](#mpesa_daraja)*

Another security complexity also happens when integrating callback APIs like C2B API. Developers are required to secure their callback endpoints by only accepting SSL certificates from Mpesa broker SSL certificates - meaning any communication without the broker SSL certificate would fail. This works but thinking conventionally, this is not so well thought out since it adds security implementation complexity and cumbersome.

Security implementation should hugely depend on the API creators with little input from the developers like perhaps including JWT authorization headers in the API requests.

### Tests

Everyone likes testing out a product before putting it into commercial use. Testing helps in assuring the users about the reliability of the product, its ease of use and get a feeling of the commercial side of it. Tests drive high acceptance, rapid and instant feedback, clearer scope and a higher Return on Investment(ROI).

## Proposed Daraja 2.0

After thinking through Daraja's optimal implementation and the issues mentioned above, we've consolidated our thoughts into a beautiful, better, clearer, cleaner, more secure Daraja API proposal. We will call it **Daraja 2.0** 

### Proposed Structure...
- uses JSONAPI format. Treats everything as a resource.
- uses JWT for authentication. The JWT token embeds static developer data like MPESA account credentials and developer endpoints. 
- secures developer endpoints
- uses conventional naming styles
- uses globally accepted data formats like time formats which are always in UTC format.
- keeps the API simple. Developer only needs to focus on the business goals. 

To make this proposal practical, the following assumptions were made:

- developer endpoints for receiving payment notification do not change with every call and will already have been pre-registered
- JWT token will embed/co-relate static information of MPESA credentials and developer endpoints that the developer would have provided through a different registration process
- developer already has MPESA account for relevant endpoints i.e C2B or B2C paybills

For those who would like to have a deeper understanding and look at the proposed Daraja 2.0, it's only prudent that I share the backend design of it, its API definitions and the Postman collections. 

*Please note that we only have a proposal for the B2C and C2B APIs. Be on the look out for proposals of other APIs like C2B express checkout commonly know as STK Push, B2B transfers, Account Balance query, Transaction reversal and more.*

![ERD](/assets/images/blog/daraja-2/ERD_v4.png){: .img-responsive .center}

*You can also contribute to this design by editting its StarUML file found [here](/assets/images/blog/daraja-2/uniapi.mdj)*

[Daraja 2.0 API Specification](https://app.swaggerhub.com/apis/zegetech/mpesaUniAPI/1.0)<br>
[Daraja 2.0 Postman Collection](https://www.getpostman.com/collections/47afa78d656679d9f5d5)

If you think that you've experienced issues with the current [Daraja API](https://developer.safaricom.co.ke/apis) and would like to experience a better solution to MPESA integration feel free to try out the proposed solution and design. 

You'll only need a Postman client to try it out. [Download](https://www.getpostman.com/downloads/) one if you don't have it installed.

Please click on the Postman collection link and run the collection on the Postman client. Also, note that the responses aren't real MPESA responses but mock responses for demonstration purposes.

### A quick recap of the collection...

The APIs are grouped into Developer APIs set and MPESA APIs set. Developer APIs represent requests made to the developers system - commonly called the callback requests while Mpesa APIs represent requests that originate from the developers system to Mpesa system. The split was necessary for visualization reasons and make the proposal clearer. 

Let's have a peek into one of the APIs and try to benchmark it against the existing solutions. 

### B2C API

This is an API that allows businesses to send money to Customers directly to their MPESA wallets. It could be renumerations, salaries, bonuses or any other reasons. It is mostly used by NGOs, private companies like banking institutions and government institutions. 

Here is a sample request and response of the legacy G2 implementation of the API which is SOAP based:

#### Request

```
<!-- Request Body -->
<?xml version="1.0" encoding="UTF-8"?>
<soapenv:Envelope xmlns:xsd="http://www.w3.org/2001/XMLSchema" 
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
    xmlns:req="http://api-v1.gen.mm.vodafone.com/mminterface/request" 
    xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Header>
        <tns:RequestSOAPHeader xmlns:tns="http://www.huawei.com.cn/schema/common/v2_1">
            <tns:spId>100274</tns:spId>
            <tns:spPassword>somepassword==</tns:spPassword>
            <tns:timeStamp>20190318172239</tns:timeStamp>
            <tns:serviceId>10027400323230</tns:serviceId>
        </tns:RequestSOAPHeader>
    </soapenv:Header>
    <soapenv:Body>
        <req:RequestMsg>
            <![CDATA[
            <request xmlns="http://api-v1.gen.mm.vodafone.com/mminterface/request">
                <Transaction>
                    <Remark>0</Remark>
                    <ConversationID></ConversationID>
                    <LanguageCode>0</LanguageCode>
                    <CommandID>BusinessPayment</CommandID>
                    <OriginatorConversationID>1669083056</OriginatorConversationID>
                    <Parameters>
                        <Parameter>
                            <Key>Amount</Key>
                            <Value>1000.0</Value>
                        </Parameter>
                    </Parameters>
                    <ReferenceData>
                        <ReferenceItem>
                            <Key>QueueTimeoutURL</Key>
                            <Value>https://10.8.9.11:8310/mpesatransaction/queue_timeout</Value>
                        </ReferenceItem>
                    </ReferenceData>
                    <Timestamp>2019-03-18T17:22:09.651011Z</Timestamp>
                </Transaction>
                <Identity>
                    <Caller>
                        <CallerType>2</CallerType>
                        <ThirdPartyID>345612</ThirdPartyID>
                        <Password>Password0</Password>
                        <CheckSum>CheckSum0</CheckSum>
                        <ResultURL>https://10.8.9.11:8310/mpesatransaction/result</ResultURL>
                    </Caller>
                    <Initiator>
                        <IdentifierType>11</IdentifierType>
                        <Identifier>SOMEInitiator</Identifier>
                        <SecurityCredential>sdscjGPSOfYT6xMEHIT7RCQi1jy//dsdsdsdnMwvPeDhxeQlNngr8Qgh/FNlrj23sasasp/+MKBhBGuerJnkICbctCWQ10qU355CWRreTpA==</SecurityCredential>
                        <ShortCode>123456</ShortCode>
                    </Initiator>
                    <PrimaryParty>
                        <ShortCode>123456</ShortCode>
                        <IdentifierType>4</IdentifierType>
                        <Identifier>123456</Identifier>
                    </PrimaryParty>
                    <ReceiverParty>
                        <ShortCode>254722000000</ShortCode>
                        <IdentifierType>1</IdentifierType>
                        <Identifier>254722000000</Identifier>
                    </ReceiverParty>
                    <AccessDevice>
                        <IdentifierType>1</IdentifierType>
                        <Identifier>Identifier3</Identifier>
                    </AccessDevice>
                </Identity>
                <KeyOwner>1</KeyOwner>
            </request>
            ]]>
        </req:RequestMsg>
    </soapenv:Body>
</soapenv:Envelope>
```

#### Response

```
<!--Response body-->
<?xml version='1.0' encoding='UTF-8'?>
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/">
    <soapenv:Body>
        <req:ResponseMsg xmlns:req="http://api-v1.gen.mm.vodafone.com/mminterface/request">
            <![CDATA[
            <?xml version="1.0" encoding="UTF-8"?>
            <response xmlns="http://api-v1.gen.mm.vodafone.com/mminterface/response">
                <ResponseCode>0</ResponseCode>
                <ConversationID>AG_20190417_000049hjde14ae88580c48</ConversationID>
                <ResponseDesc>Accept the service request successfully.</ResponseDesc>
                <OriginatorConversationID>uuid8893834399</OriginatorConversationID>
                <ServiceStatus>0</ServiceStatus>
            </response>
            ]]>
        </req:ResponseMsg>
    </soapenv:Body>
</soapenv:Envelope>
```
My feedback: 

- uses less attractive old age XML. Where is it used at this age and time??
- looks clumsy
- requires more bandwidth hence expensive
- confusing and vague xml tag naming
- can pose a difficulty especially when trying to parse `CDATA`

<a name="mpesa_daraja"></a>
The REST based Daraja implementation of the same B2C request:

#### Request

```
curl -X POST --header "Authorization: Bearer <Access-Token>" --header "Content-Type: application/json" -d "{
    "InitiatorName":"SOMEInitiator",
    "SecurityCredential":"sdscjGPSOfYT6xMEHEIsaDEsasAdOVIT7RCQi1jy//dsdsdsdnMwvPeDhxeQlNngr8Qgh/FNlrj/+MKBhBGuerJnkICbctCWQ10qU355CWRreTpA==",
    "CommandID":"BusinessPayment",
    "Amount":"1000",
    "PartyA":"123456",
    "PartyB":"254722000000",
    "Remarks":"some remarks",
    "QueueTimeOutURL":"http://your_timeout_url",
    "ResultURL":"http://your_result_url",
    "Occasion":"some occassion"
}" "https://sandbox.safaricom.co.ke/mpesa/b2c/v1/paymentrequest"
```

#### Response

```
{  
   "Result":{  
      "ResultType":0,
      "ResultCode":2001,
      "ResultDesc":"The initiator information is invalid.",
      "OriginatorConversationID":"7488-6256766-1",
      "ConversationID":"AG_20190104_00004ce9fc41b15aa227",
      "TransactionID":"NA43XZPDE3",
      "ReferenceData":{  
         "ReferenceItem":{  
            "Key":"QueueTimeoutURL",
            "Value":"http:\/\/internalapi.safaricom.co.ke\/mpesa\/b2cresults\/v1\/submit"
         }
      }
   }
}
```

My feedback:

- better improvement
- contains too much information that would otherwise be dropped
- puts too much security weight on developer(process of generating the Security Credential)
- confusing naming of keys. One can't easily differentiate Party A, from Party B and Initiator Name. In fact the description on the docs confuses newbies further.

Proposed Daraja 2.0 implementation of the B2C API :

```
curl -X POST --header "Authorization: Bearer <Access-Token>" --header "Content-Type: application/vnd.api+json" --header "Accept: application/vnd.api+json" -d "
{
    "data":{
        "type" : "pay_outs",
        "id" : 1,
        "attributes" : {
            "category": "BusinessPayment",
            "amount": 1000,
            "recipient_no": "25472264885",
            "posted_at": "2019-03-18T17:22:09.651011Z"
        }
    }
}
```

#### Response 

```
{
  "data": {
    "type": "pay_outs",
    "id": 1,
    "attributes": {
      "msg_id": "AG_20190417_000049de14ae0c48"
    }
  }
}

```
My feedback:

- Simple, fewer lines
- easy to understand
- self descriptive
- good naming conventions
- lightweight

Winner: **Daraja 2.0**

## To conclude,

This isn't to say that this unveiling is perfect. This is to get us thinking as a community, share out our ideas on how Daraja 2.0 could be like, discuss the best approach to solving the challenges that the community faces and finally playing our roles in constantly improving the fintech space. 

Coming to this point must have got you thinking throughout the piece. I would be very much pleased to get a sense of your thoughts, lessons picked from the piece or simply a thumbs up in the comments section. 