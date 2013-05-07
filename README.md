What is MockServer
==================

MockServer is an API to enable the mocking of any system you integrate with via HTTP (i.e. services, web sites, etc)

This API allows you to setup expectations using an example HTTP request and a HTTP response.  When the MockServer then receives a matching request it will provide the specified response.

Requests can be matched on:
* path (regex or plain text)
* body (regex or plain text)
* headers
* cookies
* query parameters (GET)
* body parameters (POST)

Responses can contain:
* status code
* body
* headers
* cookies

MockServer has already been used on multiple large commercial projects to run large test suits that execute in parallel.

Why use MockServer
==================

MockServer allows you to mock any server or service that you connect to over HTTP, such as REST or RPC service.  

This is useful in the following scenarios:
* testing 
 * easily recreate all types of responses for HTTP dependencies such as REST or RPC services to test applications easily and affectively 
 * isolate the system under test to ensure tests run reliably and only fail when there is a genuine bug.  It is important only the system under test is tested and not its dependencies to avoid tests failing due to irrelevant external changes such as network failure or a server being rebooted / redeployed.
 * easily setup mock responses independently for each test to ensure test data is encapsulated with each test.  Avoid sharing data between tests that is difficult to manage and maintain and risks test infecting each other.
* de-coupling development
 * start working against a service API before the service is available.  If an API or service is not yet fully developed MockServer can mock the API allowing the teams who is using the service to start work without being delayed. 
 * isolate development teams particularly critical during the initial development phases when the APIs / services may be extremely unstable and volatile.  Using the mock server allows development work to continue even when an external service fails.

How to use MockServer
=====================

To use the MockServer:
 1. create mock responses
 2. setup mock expectations
 3. run test

A system with service dependancies as follows:

![System In Production](/SystemInProduction.png)

Could be tested with MockServer, mocking the service dependancies, as follows:

![Mocking service dependancies with Mock Server](/SystemUnderTest.png)

1. create mock response
-----------------------

The mock response can include any aspects of a HTTP request including: 
* response code i.e. 200, 302, 404, etc
* body - a string containing any content
* cookies - each with a name and with one or more values, more complex cookies can be modeled by using the a Set-Cookie header
* headers - each with a name and one or more values
* delay - including both the time unit (java.util.concurrent.TimeUnit) and value

Java API

The org.mockserver.model.HttpResponse class is used to specify the details of each HTTP response with a fluent API:

    public class HttpResponse extends ModelObject {

        public HttpResponse withStatusCode(Integer responseCode);

        public HttpResponse withBody(String body);

        public HttpResponse withCookies(List<Cookie> cookies);

        public HttpResponse withCookies(Cookie... cookies);

        public HttpResponse withHeaders(List<Header> headers);

        public HttpResponse withHeaders(Header... headers);

        public HttpResponse withDelay(Delay delay);
    }
    
For example:

        HttpResponse httpResponse =
                new HttpResponse()
                        .withStatusCode(200)
                        .withHeaders(new Header("Content-Type", "application/json; charset=utf-8"))
                        .withBody("{ message: 'a simple json response' ");

JavaScript API

TODO

Requirements
============

* Java 7 - because this API uses Jetty 9 to increase reliability, simplicity and flexibility which in turn requires Java 7 (http://webtide.intalio.com/2012/09/jetty-9-features/)
