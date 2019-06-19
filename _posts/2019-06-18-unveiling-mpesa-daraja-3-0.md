---
layout: blog
title: Unveiling MPESA Daraja 2.0
date: 2019-06-18 13:08 +0300
categories: 
published: false
author: Tom Nyongesa
blog-image: 
intro: This never happened, Mpesa haven't launched a new API. What if they did? What would it mean? What would it be like? Same reliability of G2 SOAP API, accessibility of Daraja, simplicity of Stripe API and Security of Visa API?  
---
{{page.intro}}

Well, my thoughts? It would have been the perfect product the community never got. Perfect here sounds strong but it doesn't necessarily discredit [Daraja](https://developer.safaricom.co.ke/apis) as imperfect. Most of us in the Fintech space have integrated the Daraja REST API with a handful of us integrating the legacy G2 SOAPAPI. 

Daraja has been in existence for a while now and working relatively well. However, the general feeling of the Kenyan tech community specifically on MPESA integration clearly shows that the community in the Fintech space has just been 'surviving'. Our recent release of the Kenyan Payment Integration survey solidifies my arguement. Going through the survey exposes the fact that the current Daraja API has got a relatively poor documentation, lacks sample integration codes, not so reliable test bed, poor security implementation and many other reasons among them a complex onboarding process. Here's the link to the [survey findings](2019-03-21-payments-kenya.md).

Part of findings of the survey tells us that the integration or rather testing of the Daraja API isn't a walk in the park either:

![ease of integration](/assets/images/blog/daraja-3/ease_of_integration.png){: .img-responsive .center}

Can it be better? Is it even possible to improve on it? Can we make it simpler?Yes, a strong **YES**. But simple isn't simple enough unless we put it into context and define what simple really looks like. 

## The Pain of integrating APIs

Looking back at the [Payment survey](2019-03-21-payments-kenya.md) and [Payment gateways in Kenya](2019-01-14-payment-gateways-Kenya.md), the main challenges experienced by the community while integrating Payment  can be summarized to the following main points:

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

The design of any API, not only financial APIs, greatly affects the above issues. Since APIs are mostly developed for use by others, it is strongly recommended that a standardized and widely accepted format should be followed while designing it. The standardization runs right from overlooked things like naming conventions, data formats like date formats to technical things like API definition/specification. With the widespread acceptability of REST APIs. It's also evident from the Payments survey that more than 90% of the developers interviewed prefer using REST APIS. This simply dictates that Daraja 2.0 should be in REST. At this age and times, REST and standardization can never be in the same paragraph without mentioning JSONAPI. JSONAPI is a widely spread format or API specification that specifies how a a client should request for a resource and conversely how a server should respond with resources. The beauty of JSONAPI is that it excels at readability, flexibility and discoverability and greatly improves on efficiency by minimizing the number of requests and responses sent between the clients and the server. Daraja 2.0 should definitely be written according to the JSONAPI format, here's more on [why we think JSONAPI is great]().

With a good design and use of latest community accepted API design tools, we could address documentation, sample codes and standardization issues experienced. Here are some of the top [API design tools](https://openapi.tools/).

### Security

Security is another major concern of the community. Even though the current Daraja implementation has proved to be secure, it partly puts the security weight on developers which shouldn't be the case. Asking developers to make a combination of certain required variables, encrypting the result, uploading security certificates makes the whole process difficult and cumbersome. This should solely be the servers own doing with little input from the developers like perhaps including JWT authorization headers in the API requests. 

Another huge security issue and perhaps overlooked by Daraja implementors is securing the developers servers. This is specifically on those callback APIs like C2B APIs. The current implementation allows anyone who is has access to the callback URLs - results urls, confirmation urls and validation urls - to orchestrate a digital attack to the developers servers for example by posting any unexpected data to the urls. This is a security risk on the developers end. 

### Tests

Everyone likes testing out a product before putting it into commercial use. Testing helps in assuring the users about the reliability of the product, its ease of use and get a feeling of the commercial side of it. Test Dr

## Proposed Daraja 2.0

After thinking through Daraja 2.0 optimal implementation and the issues mentioned above, we've consolidated our thoughts into a beautiful, better, clearer, cleaner, more secure Daraja 2.0 proposal in terms of its API specification. If you think that you've experienced issues with the current [Daraja API](https://developer.safaricom.co.ke/apis) and would like to experience a better solution to MPESA integration feel free to checkout the following Daraja 2.0 implementation. So far, it proposes a better implementation of the B2C and C2B APIs - other APIs like C2B express checkout commonly know as STK Push, B2B transfers, Account Balance query, Transaction reversal and more.

You'll only need a Postman client to try it out. [Download](https://www.getpostman.com/downloads/) one if you don't have it installed.

[Daraja 2.0 API Specification](https://app.swaggerhub.com/apis/zegetech/mpesaUniAPI/1.0)<br>
[Daraja 2.0 Postman Collection](https://www.getpostman.com/collections/47afa78d656679d9f5d5)

Please click on the Postman collection link and run the collection on the Postman client. Please note that the responses aren't real MPESA responses but simply mock responses for demonstration purposes.

### A quick recap of the collection...

The APIs are grouped into Developer APIs set and MPESA APIs set. Developer APIs represent requests made to the developers system - commonly called the callback requests while Mpesa APIs represent requests that originate from the developers system to Mpesa system. The split was necessary for visualization reasons and make the proposal clearer. 

Let's have a peek into one of the APIs and try to benchmark it against the existing solutions. 

### B2C API

This is an API that allows businesses to send money to Customers directly to their MPESA wallets. It could be renumerations, salaries, bonuses or any other reasons. It is mostly used by NGOs, private companies like banking institutions and government institutions. 

Here is a sample request of the legacy G2 implementation of the API which is SOAP based:

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
                            <Value>https://10.8.0.10:8310/mpesatransaction/queue_timeout</Value>
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
                        <ResultURL>https://10.8.0.10:8310/mpesatransaction/result</ResultURL>
                    </Caller>
                    <Initiator>
                        <IdentifierType>11</IdentifierType>
                        <Identifier>SOMEInitiator</Identifier>
                        <SecurityCredential>sdscjGPSOfYT6xMEHEIdOVIT7RCQi1jy//dsdsdsdnMwvPeDhxeQlNngr8Qgh/FNlrj23p/+MKBhBGuerJnkICbctCWQ10qU355CWRreTpA==</SecurityCredential>
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
My feedback: 

- uses less attractive old age XML. Who does that nowadays??
- looks clumsy

Daraja implementation of the same B2C request which is REST based:

```
curl -X POST --header "Authorization: Bearer <Access-Token>" --header "Content-Type: application/json" -d "{
  \"InitiatorName\":\" \",
  \"SecurityCredential\":\" \",
  \"CommandID\":\" \",
  \"Amount\":\" \",
  \"PartyA\":\" \",
  \"PartyB\":\" \",
  \"Remarks\":\" \",
  \"QueueTimeOutURL\":\"http://your_timeout_url\",
  \"ResultURL\":\"http://your_result_url\",
  \"Occasion\":\" \"
}" "https://sandbox.safaricom.co.ke/mpesa/b2c/v1/paymentrequest"
```

My feedback:

- better improvement
- contains too much information that would otherwise be dropped
- puts too much security weight on developer(process of generating the Security Credential)
- confusing naming of keys. One can't easily diffrenciate Party A, from Party B and Initiator Name. In fact the description on the docs confuses newbies further

Proposed Daraja 2.0 implementation of the same:

```
curl -X POST --header "Authorization: Bearer <Access-Token>" --header "Content-Type: application/vnd.api+json" --header "Accept: application/vnd.api+json" -d "
{
    "type" : "pay_outs",
    "id" : 1,
    "attributes" : {
      "amount": 1000,
      "recipient_no": "25472264885",
      "posted_at": "2019-03-18T17:22:09.651011Z"
    }
  }"
```
My feedback:

- Simple, fewer lines
- easy to understand
- self descriptive
- good naming conventions

Winner: **Daraja 2.0**

## To conclude,

This isn't to say that this unveiling is perfect. This is to get us thinking as a community, share out our ideas on how Daraja 2.0 could be like, discuss the best approach to solving the challenges that the community faces and finally playing our roles in constantly improving the fintech space. 

Coming to this point must have got you thinking throughout the piece. I would be very much pleased to get a sense of your thoughts, lessons picked from the piece or simply a thumbs up in the comment section. 