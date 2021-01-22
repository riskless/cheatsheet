### Contents
- SOAP
- Spring-WS
- RESTful Webservice
- Before Spring Boot
- Spring Boot
- Content Negotiation in RESTful Webservices
- HTTP Cache
- @PatchMapping
- File Upload and Download
- Exception Handling
- Validation
- RestTemplate
- HATEOAS (Hypermedia as the Engine of Applicaiton State)

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

### @PatchMapping
```java
@PatchMapping(value = "/{id}")
public @ResponseBody void saveManager(@PathVariable Long id, @RequestBody Map<Object, Object> fields) {
	Product product = productService.getProduct(id);
		// Map key is field name, v is value
		fields.forEach((k, v) -> {
			 // use reflection to get field k on manager and set it to value k
				Field field = ReflectionUtils.findField(Product.class, (String) k);
				field.setAccessible(true);
				ReflectionUtils.setField(field, product, v);
		});
		productService.updateProduct(product);
}

/* Test */
// http://localhost:8080/product/1
{
	name: "new product"
}
```

### File Upload and Download
```java
/* application.properties */
# Enable multipart uploads
spring.servlet.multipart.enabled=true

# Threshold after which files are written to disk.
spring.servlet.multipart.file-size-threshold=2KB

# Max file size.
spring.servlet.multipart.max-file-size=10MB

# Max Request Size
spring.servlet.multipart.max-request-size=40MB

/* FileStorageService */
@Service
public class FileStorageService {

	private final Path fileStorageLocation;

	// @Value("${file.upload-dir}") private String uploadDir;
	
	@Autowired
	public FileStorageService() {
		this.fileStorageLocation = Paths.get("C:/Users/Admin/Desktop/uploads")
						.toAbsolutePath().normalize();

		try {
			Files.createDirectories(this.fileStorageLocation);
		} catch (Exception ex) {
			//throw Execption
		}
	}

	public String storeFile(MultipartFile file) {
			String fileName = StringUtils.cleanPath(file.getOriginalFilename());

			try {
					// Check if the file's name contains invalid characters
					if(fileName.contains("..")) {
							//throw exception
					}

					// Copy file to the target location (or Replacing if already existing)
					Path targetLocation = this.fileStorageLocation.resolve(fileName);
					Files.copy(file.getInputStream(), targetLocation, StandardCopyOption.REPLACE_EXISTING);

					return fileName;
			} catch (IOException ex) {
					//throw new FileStorageException("Could not store file " + fileName + ". Please try again!", ex);
			}
			return fileName;
	}

	public Resource loadFileAsResource(String fileName) {
			try {
					Path filePath = this.fileStorageLocation.resolve(fileName).normalize();
					Resource resource = new UrlResource(filePath.toUri());
					if(resource.exists()) {
							return resource;
					} else {
							//throw new MyFileNotFoundException("File not found " + fileName);
					}
			} catch (MalformedURLException ex) {
					//throw new MyFileNotFoundException("File not found " + fileName, ex);
			}
	return null;
	}
}

/* UploadFileResponse */
public class UploadFileResponse {
	private String fileName;
	private String fileDownloadUri;
	private String fileType;
	private long size;
}

/* controller */
@Autowired
private FileStorageService fileStorageService;

@PostMapping("/uploadFile")
public UploadFileResponse uploadFile(@RequestParam("file") MultipartFile file) {
		String fileName = fileStorageService.storeFile(file);

		String fileDownloadUri = ServletUriComponentsBuilder.fromCurrentContextPath()
						.path("/product/downloadFile/")
						.path(fileName)
						.toUriString();

		return new UploadFileResponse(fileName, fileDownloadUri,
						file.getContentType(), file.getSize());
}

@PostMapping("/uploadFiles")
public List<UploadFileResponse> uploadMultipleFiles(@RequestParam("files") MultipartFile[] files) {
		return Arrays.asList(files)
						.stream()
						.map(file -> uploadFile(file))
						.collect(Collectors.toList());
}

@GetMapping("/downloadFile/{fileName:.+}")
public ResponseEntity<Resource> downloadFile(@PathVariable String fileName, HttpServletRequest request) {
		// Load file as Resource
		Resource resource = fileStorageService.loadFileAsResource(fileName);

		// Try to determine file's content type
		String contentType = null;
		try {
				contentType = request.getServletContext().getMimeType(resource.getFile().getAbsolutePath());
		} catch (IOException ex) {
				System.out.println(("Could not determine file type."));	
		}

		// Fallback to the default content type if type could not be determined
		if(contentType == null) {
				contentType = "application/octet-stream";
		}

		return ResponseEntity.ok()
						.contentType(MediaType.parseMediaType(contentType))
						.header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + resource.getFilename() + "\"")
						.body(resource);
}

// {variable_name:regular_expression}
// .+ (where . means 'any character' and + means 'one or more times')
```

### Exception Handling
- Redirect the user to a dedicated error view
- Build a totally custom error response
- Log the exception

```java
/* CustomError */
public class CustomError {
	private HttpStatus status;
	private String message;
}

/* ErrorHandler */ 
@ControllerAdvice
public class GlobalErrorHandler {
	@ExceptionHandler({Exception.class})
	public ResponseEntity<Object> handleError(HttpServletRequest req, Exception ex) {
		CustomError error = new CustomError(HttpStatus.BAD_REQUEST, ex.getMessage());
		return new ResponseEntity<Object>(error, new HttpHeaders(), error.getStatus());
	}
}
```

### Validation
- Spring Default Validation
```java
@GetMapping("/{id}")
public Product getProduct(@PathVariable(value="id",required=true) Long id) {
	return productService.getProduct(id);
}

/* client */
// localhost:8080/products/abc -> Invalid-400 Bad Request Error Response
```

- Spring Request Body Validation
```java
/* Controller */
@PutMapping
public Product updateProductUsingJson(@RequestBody @Valid Product product) {
	productService.updateProduct(product);
	return product;
}

/* Product */
public class Product {
	private Long productId;
	@NotBlank(message="Name is a mandatory field")
	private String productName;
	@NotNull(message="Price needs to be specified")
	private Integer productPrice;
}

/* client */
{
	"productId": 1,
	"productName":"",
	"productPrice":0
}
// Invalid - 400 Bad Request Error Response
```
### RestTemplate
- Json Jackson To Retrieve Specific Info
```java
/* controller */
public class ProductController {
	@Autowired
	RestTemplate restTemplate;
	
	@GetMapping
	public String getProduct() throws JsonMappingException, JsonProcessingException {
		String resourceUrl = "http://localhost:8080/product";
		ResponseEntity<String> response = restTemplate.getForEntity(resourceUrl+"/1", String.class);

		ObjectMapper mapper = new ObjectMapper();
		JsonNode root = mapper.readTree(response.getBody());
		JsonNode name = root.path("productName");

		return name.asText(); // iPhone
	}
}

/* result */
{
	"productId": 1,
	"productName":"iPhone",
	"productPrice":1000
}

/* SpringBootApplication */
@Bean
public RestTemplate restTemplate() {
	return new RestTemplate();
}
```

- POST Using REST Template
```java
@PostMapping
public Map<String,Object> createProduct(@RequestBody Product product) {
	String resourceUrl = "http://localhost:8080/product";
	/*
	// headers
	HttpHeaders headers = new HttpHeaders();
	headers.set("Accept", MediaType.APPLICATION_JSON_VALUE);
	HttpEntity<?> request = new HttpEntity<>(headers);
	*/

	UriComponentsBuilder builder = UriComponentsBuilder.fromHttpUrl(resourceUrl)
		.queryParam("id", product.getProductId())
		.queryParam("name", product.getProductName())
		.queryParam("price", product.getProductPrice());

	Map<String,Object> response = restTemplate.postForObject(builder.toUriString(), HttpEntity.EMPTY, Map.calss);
	
	// restTemplate.exchange(builder.toUriString(), HttpMethod.POST, HttpEntity.EMPTY, Map.class);
	
	return response;
}
```

- PUT Using REST Template
```java
@PostMapping
public ResponseEntity<Product> updateProduct(@RequestBody Product product) {
	String resourceUrl = "http://localhost:8080/product";
	HttpEntity<Product> request = new HttpEntity<>(product);
	
	ResponseEntity<Product> response = restTemplate.exchange(resourceUrl, HttpMethod.PUT, request, Product.class);
	
	return response;
}
```

- DELETE Using Rest Teamplate
```java
@DeleteMapping("/{id}")
public ResponseEntity<Map> deleteProduct(@PathVariable("id") Long id) {
	String resourceUrl = "http://localhost:8080/product/" + id;

	ResponseEntity<Map> response = restTemplate.exchange(resourceUrl, HttpMethod.DELETE, HttpEntity.EMPTY, Map.class);
	
	return response;
}
```

- Rest Template Error Handling
```java
/* ErrorHandler */
@Component
public class MyResponseErrorHandler implements MyResponseErrorHandler {
	@Override
	public boolean hasError(ClientHttpResponse httpResponse) throws IOException {
		System.out.println("Has Error");
		if (httpResponse.getStatusCode() != HttpStatus.OK) {
			System.out.println("Status Code: " + httpResponse.getStatusCode());
			System.out.println("Response: " + httpResponse.getStatusText());

			if (httpResponse.getStatusCode() != HttpStatus.FORBIDDEN) {
				System.out.println("403 Forbidden Response");
				return true;
			}
			return true;
		}
	}
}

/* controller */
public class TestController {
	@Autowired
	RestTemplate restTemplate;
	
	@GetMapping
	public void testGet() throws JsonMappingException, JsonProcessingException {
		restTemplate.setErrorHandler(new MyResponseErrorHandler());
		String resourceUrl = "https://httpstat.us/404";
		ResponseEntity<Product> response = restTemplate.exchange(resourceUrl, HttpMethod.GET, HttpEntity.EMPTY, Product.class);
	}
}
```


- Handling Time Out
```java
/* SpringBootApplication */
@Bean
public RestTemplate restTemplate() {
	HttpComponentsClientHttpRequestFactory cliReqFact = new HttpComponentsClientHttpRequestFactory();
	cliReqFact.setConnectTimeout(5000);
	cliReqFact.setReadTimeout(5000);

	return new RestTemplate(cliReqFact);
}

/* controller */
public class TestController {
	@Autowired
	RestTemplate restTemplate;
	
	@GetMapping
	public void testGet() throws JsonMappingException, JsonProcessingException {
		restTemplate.setErrorHandler(new MyResponseErrorHandler());
		String resourceUrl = "https://httpstat.us/200?sleep=10000";
		ResponseEntity<Product> response = restTemplate.exchange(resourceUrl, HttpMethod.GET, HttpEntity.EMPTY, Product.class);
	}
}
```

### HATEOAS (Hypermedia as the Engine of Applicaiton State)
- Client does not need to know the URI. Once the client gets initial URI, it must be able to navigate through API like we navigate a website. Hence it's called "Hypermedia as the Engine of Applicaiton State"
```java
/* pom.xml */
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-hateoas</artifactId>
</dependency>

/* Product */
public class Product extends ResourceSupport {
	private Long productID;
	private String productName;
	private Integer productPrice;

	public Product(Long productID, String productName, Integer productPrice) {
		this.productID = productID;
		this.productName = productName;
		this.productPrice = productPrice;
		
		add(new Link("/products/"+ productID).withSelfRel());
		add(linkTo(methodOn(ProductController.class).updateProductUsingJson(new Product())).withRel("update"));
		add(linkTo(methodOn(ProductController.class).getReviewsForProduct(productID)).withRel("Reviews"));
		add(linkTo(methodOn(ProductController.class).deleteProduct(productID)).withRel("delete"));
		// add(new Link("/products/delete"+ productID).withRel("delete"));
		// Json: "delete": {"href":"/products/delete/1"}
	}
}

/* ProductReviews */
public class ProductReviews extends ResourceSupport {
	private Long ID;
	private Long productID;
	private String review;
	
	public ProductReviews(Long id, Long productID, String review) {
		this.ID = id;
		this.productID = productID;
		this.review = review;
		
		add(linkTo(methodOn(ProductController.class).getProduct(productID)).withRel("product"));
    add(linkTo(methodOn(ProductController.class).getReviewsForProduct(productID)).withSelfRel());
	}
}

/* controller */
@RestController
@RequestMapping("/products")
public class ProductController {
	@GetMapping("")
	Resources<Product> getProducts() {
		Link selfRelLink = ControllerLinkBuilder
				.linkTo(ControllerLinkBuilder.methodOn(ProductController.class).getProducts()).withSelfRel();

		List<Product> productList = productService.getProducts();

		Resources<Product> resources = new Resources<>(productList);
		resources.add(selfRelLink);
		return resources;
	}

	@GetMapping(value = "/{productId}/reviews")
	public Resources<ProductReviews> getReviewsForProduct(@PathVariable final long productId) {

		List<ProductReviews> reviews = productService.getProductReviews(productId);
		return new Resources<ProductReviews>(reviews);
	}
}

/* JSON Response */
{
	"productId": 1,
	"productName":"iPhone",
	"productPrice":1000,
	"links": {
		"self": {"href":"/products/1"},
		"delete": {"href":"/products/delete/1"}
		"update": {"href":"/products/1"}
		"Reviews": {"href":"/products/1/reviews"}
	}
}

```
### References
- [Spring Boot REST & Angular + Full Stack Application](https://www.eduonix.com/spring-boot-rest-amp-angular-full-stack-application)
