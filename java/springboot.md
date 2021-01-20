### SOAP
- SOAP (Simple Object Access Protocol)
- WSDL (Webservice Description Langauge)
- Advantages
	- Interoperability - SOAP can be implemented in any language and can be executed in any platform
	- SOAP uses HTTP protocol and hence no issue with firewall
	- Works with other protocols like FTP, SMTP, TCP
	- Compared to RPC, SOAP works better with Single Page Applications and Distributed systems
- SOAP Message Format
```xml
<env:Envelop>
	<env:Header></env:Header>
	<env:Body>
		<location>
			<name>MyLocation</name>
			<temperature>35</temperature>
		</location>
	</env:Body>
</env:Envelop>
```
### Spring-WS
- Exposing Weather Forecast Service
	- Create XSD File
	- Use JAXB to create Domain Objects using XSD File
	- Create an Endpoint and send response using Domain Objects
	- Spring-WS will create and expose WSDL (Accessible over internet)
- Consuming Weather Forecast Service
	- Use JAXB to create Java classes using WSDL file
	- Use request/reponse domain classes to send the request and receive response
- XSD File
```xml
<xs:element name='getTemeratureRequest'>
	<xs:complexType>
		<xs:sequence>
			<xs:element name='location' type='xs:string'>
		</xs:sequence>
	</xs:complexType>
</xs:element>
<xs:element name='getTemeratureResponse'>
	<xs:complexType>
		<xs:sequence>
			<xs:element name='temperature' type='xs:string'>
		</xs:sequence>
	</xs:complexType>
</xs:element>
```


### RESTful Webservice
- A service which is built on the REST architecture is called a RESTful webservice
- Resource
	- In RESTful webservices, anything that you can manipulate with a URL is a Resource. (Ex: A document, A row in a database)
- RESTful Methods
	- RESTful webservices relies on HTTP methods to manipulate a resource on the server
	- GET - To retrieve a resource from the server
	- POST - To create a resource on the server
	- PUT - To change the state or to update a resource
	- DELETE - To remove or delete a resource from the server
- Elements
	- Request Headers - Includes some additional information. Like content type, authorization, authentication
	- Request Body - This is where you would send the data required. For example, to insert a record in database with POST method
	- Response Body - Constitute the response data, which can be used. For example, student details returned in JSON format.
	- Response Status Code - HTTP status code that will specify the status of the operation performed. For example,
		- 200 OK - Successful
		- 201 Created - Created
		- 401 Unauthorised - Invalid authentication
- Principles of REST
	- Stateless - Server should not retain any clinet specific information with it. Every request must be treated as a new request and client has to provided all the information.
	- Cache - Client will cache the response and so if the same request is sent again, the request doesn't have to go server.
	- Layered System - Gives flexibly to distribute the service into mulitple servers.
	- Uniform Contract - Identification Of Resources, Resource Manipulation through Representation, Self Descriptive Messages, Hypermedia as the Engine of Applicaiton State (HATEOAS)
	- Client-Server - Server will not care of presentation. Client won't care of data storage.

- Hypermedia as the Engine of Applicaiton State (HATEOAS)
```xml
<!-- Get account information -->
<account>
	<account_number>12345</account_number>
	<balance currency="usd">100.00</balance>
	<link rel="deposit" href="/accounts/12345/deposit" />
	<link rel="withdraw" href="/accounts/12345/withdraw" />
	<link rel="transfer" href="/accounts/12345/transfer" />
	<link rel="close" href="/accounts/12345/close" />
</account>

<!-- Get account when balance is low -->
<account>
	<account_number>12345</account_number>
	<balance currency="usd">-25.00</balance>
	<link rel="deposit" href="/accounts/12345/deposit" />
</account>
```

- RESTful vs SOAP Webservice
	- REST is an Architectural style. SOAP is a protocol
	- SOAP is based on XML. REST supports JSON, XML, Plain text
	- REST exposes API with URL Mapping. While SOAP use annoations.
	- REST works well with JS technologies compared SOAP
	- REST requires fewer resources and bandwidth as XML
	- REST reponse can be cached


### Before Spring Boot
1. Create a Maven/Gradle Project
2. Find the required dependencies and add them in POM.xml
3. Download the Tomcat Server, install it and configure in IDE
4. Download and install Database Software and run it
5. Write your application logic
6. Create a config file defining all the beans (Dispatcher servlet, View Resolver, etc)
7. If there is DB, configure database parameters, Datasource, Entity Manager (Or for any other service)
8. Build, Deploy the Artifact on to the server and run it

### Spring Boot
- Spring Boot makes it easy to create stand-alone, production-grade based Applications that you can "just run".
- We take an opinionated view of the Spring platform and third-party libraries so you can get started with minimum fuss. Most Spring Boot applications need very little Spring configuration.
- Features
	- Automatically configure components & third party libraries
	- Creates standalone spring applications with embedded servers
	- Provide opinionated starter dependencies
	- Provide externalized configuration
	- Provides developer tools to enhance developer experience
	- Use Actuator to monitor application health and gather metrics
	- Provides testing utilities for ease of testing


### Content Negotiation in RESTful Webservices
- How will server determine the format?
1. Path extension
	- http://company/products/list.pdf -> will return in PDF format
	- http://company/products.json -> will return in JSON format
2. URL Parameter
	- http://company/products/list?format=pdf -> will return in PDF format
	- http://company/products?format=xml -> will return in XML format
3. HTTP Accept Header
	- Setting header property to "ACCEPT=application/json" will return JSON data.

- Message Converters
	- Message
		- A Message in Webservice is simply a request/response having information to process/retrieve the data.
	- HTTP Converter
		- Jaxb2RootElementHttpMessageConverter - converts Java objects to/from XML
		- Mapping JacksonHttpMessageConverter - read and write JSON
	- Marshalling / Unmarshalling
		- Marshalling - The process of converting representation (json/xml) to Java object
		- Unmarshalling - The process of converting Java object to representation (json/xml)

- Example
```java
/* pom.xml */
<dependency>
    <groupId>com.fasterxml.jackson.dataformat</groupId>
    <artifactId>jackson-dataformat-xml</artifactId>
</dependency>

/* application.properties */
// URL Parameter
// http://company/products/list?format=xml
# Whether a request parameter ("format" by default) should be used to determine the requested media type.
spring.mvc.contentnegotiation.favor-parameter=true
# Query parameter name to use when "favor-parameter" is enabled.
spring.mvc.contentnegotiation.parameter-name=mediaType

// Path extension
// http://company/products/list.pdf
spring.mvc.contentnegotiation.favor-path-extension=true


/* generate PDF */
@GetMapping(value = "", produces = { MediaType.APPLICATION_PDF_VALUE })
public ResponseEntity<InputStreamResource> download() throws IOException {
	ClassPathResource pdfFile = new ClassPathResource("files/temp.pdf");
	HttpHeaders headers = new HttpHeaders();

	headers.setContentLength(pdfFile.contentLength());
	
	ResponseEntity<InputStreamResource> response = new ResponseEntity<InputStreamResource>(
			new InputStreamResource(pdfFile.getInputStream()), headers, HttpStatus.OK);
	
	return response;
}
```

### HTTP Cache
- Cache Max Age
```java
/ * Test */
@GetMapping(value="gettemperature")
public ResponseEntity<String> handle() {
	// Header -> Cache-Control:max-age=30
	CacheControl cacheControl = CacheControl.maxAge(30, TimeUnit.SECONDS);
	int temperature = (int) (Math.random()*(60-10)) + 10;
	String testBody = "<h3>Current temperature: " + temperature + " Degrees!! </h3>" + "<h3>Response from server received at : " + LocalDateTime.now().format(DateTimeFormatter.ofPattern("HH:mm:ss")) + "</h3><a href=''>Re-send the request</a>";
	return ResponseEntity.ok().cacheControl(cacheControl).body(testBody);
}
```
- ETAG
	- The ETag HTTP response header is an identifier for a specific version of a resource. It lets caches be more efficient and save bandwidth, as a web server does not need to resend a full response if the content has not changed.
```java
/* controller */
@GetMapping(value="getproducts")
public ResponseEntity<Product> handle() {
	// Header -> Cache-Control:no-cache
	CacheControl cacheControl = CacheControl.noCache();

	return ResponseEntity.ok().cacheControl(cacheControl).body(productService.getProducts());
}

/* SpringBootApplication */
@Bean
public Filter filter() {
	ShallowEtagHeaderFilter filter = new ShallowEtagHeaderFilter();
	return filter;
}

/* client */
// Response headers
Cache-Control:no-cache
ETag: "encrypt_key"

// Request headers
if-None-Match: "encrypt_key"
```

- Cache-Control:No-Store
	- Browsers are not allowed to cache. Crucial for scenarios like banking transactions.
- Cache-Control:Public
	- Resources can be cached. Including in intermediary nodes like Proxies, CDN, etc
- Cache-Control:Private
	- Resources can be cached only in the client device, but not in intermediary nodes.
