### What is a Microservice?
- Microservices are a software development technique - a variant of the service-oriented architecture structural style that structures an application as a collection of loosely coupled services.
- In a microservices architecture, services are fine-grained.
- The benefit of decomposing an application into different smaller services is that it improves modularity. This makes the application easier to understand, develop, test, and become more resilient to architecture erosion.
- It parallelizes development by enabling small autonomous teams to develop, delploy and scale their respective services independently.
- RESTful Web Service
- Small and Responsible for one thing (Search, Password Reset, Email Verification)
- Configured to work in the Cloud and is easily scalable

### Eureka Discovery Service
- Euerka will know the address of each Mircoservice. When a new instance of a microservice starts up, it then registers itself with Eureka.
```java
/* pom.xml */
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
</dependency>

/* PhotoAppDiscoveryServiceApplication */
@SpringBootApplication
@EnableEurekaServer
public class PhotoAppDiscoveryServiceApplication {}

/* application.properties */
server.port=8010
spring.application.name=discoveryservice
eureka.client.registerWithEureka=false
eureka.client.fetchRegistry=false
eureka.client.serviceUrl.defaultZone = http://localhost:8010/eureka
eureka.instance.preferIpAddress=true

/* Eureka dashboard */
// url: http://localhost:8010
// System Status
// Instances currently registered with Eureka
// General Info
// Instance Info
```

### Building a User Microservice
- Responsibilites
	- Create new user (Registration)
	- User login
	- Get user details
	- Update user details
	- Delete user details

```java
/* pom.xml */
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>

<!-- optional -->
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<scope>runtime</scope>
</dependency>

/* PhotoAppApiUsersApplication */
@SpringBootApplication
@EnableDiscoveryClient
public class PhotoAppApiUsersApplication {}

/* application.properties */
server.port=0
spring.application.naem=users-ws
eureka.client.serviceUrl.defaultZone=http://localhost:8010/eureka
spring.devtools.restart.enabled=true

//-- server.port=0
//A random port number to be assigned to this application

/* UsersController */
@RestController
@RequestMapping("/users")
public class UsersController {
	
	@Autowired
	private Environment env;
	
	@Autowired
	UsersService usersService;

	@GetMapping("/status/check")
	public String status() {
		return "Working on port " + env.getProperty("local.server.port") + ", with token = " + env.getProperty("token.secret");
	}
 
	@PostMapping(
			consumes = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE },
			produces = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE }
			)
	public ResponseEntity<CreateUserResponseModel> createUser(@RequestBody CreateUserRequestModel userDetails) {
		ModelMapper modelMapper = new ModelMapper(); 
		modelMapper.getConfiguration().setMatchingStrategy(MatchingStrategies.STRICT);
		
		UserDto userDto = modelMapper.map(userDetails, UserDto.class);
		
		UserDto createdUser = usersService.createUser(userDto);
		
		CreateUserResponseModel returnValue = modelMapper.map(createdUser, CreateUserResponseModel.class);
		
		return ResponseEntity.status(HttpStatus.CREATED).body(returnValue);
	}
	
	@GetMapping(value="/{userId}", produces = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE })
	public ResponseEntity<UserResponseModel> getUser(@PathVariable("userId") String userId) {
		 
			UserDto userDto = usersService.getUserByUserId(userId); 
			UserResponseModel returnValue = new ModelMapper().map(userDto, UserResponseModel.class);
			
			return ResponseEntity.status(HttpStatus.OK).body(returnValue);
	}
}
```

######################################################################
*** Building an Account Manageemnt Microservice
```java
/* depenndencies */
spring-boot-starter-web
spring-cloud-starter-netflix-eureka-client
spring-boot-devtools (optional)

/* AccountManagementApplication */
@SpringBootApplication
@EnableDiscoveryClient
public class AccountManagementApplication {}

/* application.properties */
server.port=0
spring.application.name=account-ws
eureka.client.serviceUrl.defaultZone = http://localhost:8010/eureka
spring.devtools.restart.enabled = true

/* AccountController */
@RestController
@RequestMapping("/account")
public class AccountController {

    @GetMapping("/status/check")
    public String status() {
        return "Working..";
    }	
}
```

### Zuul API gateway
- A single entry point to our multiple instances of running microsevices.
- It will be accepting HTTP requests coming from a client application and then it will be routing HTTP requests to instances of microservices running behind the gateway.
- Zuul will also perform user authorization for us. If request to a protected web services does not contain a valid JSON Web Token in the authorization header, Zuul API gateway will not let that HTTP requests pass through and it will return a proper error message.

```java
/* pom.xml */
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-netflix-zuul</artifactId>
</dependency>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-devtools</artifactId>
	<scope>runtime</scope>
</dependency>

/* PhotoAppApiZuulApiGatewayApplication */
@SpringBootApplication
@EnableEurekaClient
@EnableZuulProxy
public class PhotoAppApiZuulApiGatewayApplication {}

/* application.properties */
spring.application.name=zuul
server.port=8011
eureka.client.serviceUrl.defaultZone = http://localhost:8010/eureka

/* Access Microservices via API Gateway */
http;//localhost:8011/users-ws/users
http;//localhost:8011/account-ws/account
```
