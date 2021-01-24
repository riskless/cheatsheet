### Rest Assured
- Rest Assured enables you to test REST APIs using java libraries and integrates well with Maven. 
- Rest Assured has methods to fetch data from almost every part of the request and response no matter how complex the JSON structures are.
- Rest Assured is a open source with a lot of additional methods and libraries being added has made it a great choice for API automation.

### Setup Rest Assured
- For Java version < 9 users
```html
<!-- pom.xml -->
<dependency>
<groupId>io.rest-assured</groupId>
<artifactId>json-path</artifactId>
<version>4.2.0</version>
<scope>test</scope>
</dependency>


<dependency>
<groupId>io.rest-assured</groupId>
<artifactId>xml-path</artifactId>
<version>4.2.0</version>
<scope>test</scope>
</dependency>


<dependency>
<groupId>io.rest-assured</groupId>
<artifactId>json-schema-validator</artifactId>
<version>4.2.0</version>
<scope>test</scope>
</dependency>
```

- For Java version < 9 users
```html
<!-- pom.xml -->
<dependency>
<groupId>io.rest-assured</groupId>
<artifactId>rest-assured-all</artifactId>
<version>4.2.0</version>
<scope>test</scope>
</dependency>
```

### Syntax
```java
Given()
	.param("x", "y")
	.header("z", "w")
.when()
.Method()
.Then()
	.statusCode(XXX)
	.body("x, "y", equalTo("z"));
```

- Given()
	- 'Given' keyword, lets you set a background, here, you pass the request headers, query and path param, body, cookies. This is optional if these items are not needed in the request
- When()
	- 'when' keyword marks the premise of your scenario. For example, 'when' you get/post/put something, do something else.
- Method()
	- Substitute this with any of the CRUD operations(get/post/put/delete)
- Then()
	- Your assert and matcher conditions go here

###  Getting the response Body
```java
public static void getResponseBody() {
	given()
	.when()
	.get("http://demo.guru99.com/V4/sinkministatement.php?CUSTOMER_ID=68195&PASSWORD=1234!&Account_No=1")
	.then()
	.log().all();
}

// log().all()
// Once all the response is fetched, log response, headers, essentially everything that the request returns to you.

/* For using query param */
public static void getResponseBody(){
 
	given()
		.queryParam("CUSTOMER_ID","68195")
		.queryParam("PASSWORD","1234!")
		.queryParam("Account_No","1")
	.when().get("http://demo.guru99.com/V4/sinkministatement.php")
	.then()
	.log().body();
}
//-- body()
// this helps us to extract only the body of the response.
```

### Getting the response status code
```java
final static String url="http://demo.guru99.com/V4/sinkministatement.php?CUSTOMER_ID=68195&PASSWORD=1234!&Account_No=1";

public static void getResponseStatus(){
	int statusCode = 
	given()
		.queryParam("CUSTOMER_ID","68195")
		.queryParam("PASSWORD","1234!")
		.queryParam("Account_No","1")
	.when().get("http://demo.guru99.com/V4/sinkministatement.php")
	.getStatusCode();
   
	System.out.println("The response status is "+statusCode);

	given().when().get(url).then().assertThat().statusCode(200);
}

//-- getStatusCode()
// Built-in method of Rest Assured to fetch the status code value

//-- In order to assert that your status code is 200
// assertThat().statusCode(expectedCode)
```

### Fetching headers
```java
public static void getResponseHeaders(){
	System.out.println("The headers in the response "+
		get(url).then().extract().headers());
}

// If here is no precondition or verification, 'given().when()' can be skipped
```

### Response Time
```java
public static void getResponseTime(){
	System.out.println("The time taken to fetch the response "+get(url)
		.timeIn(TimeUnit.MILLISECONDS) + " milliseconds");
}

// A very important feature of testing APIs is their response time, to measure the performance of the application.
```

### Content-Type
```java
public static void getResponseContentType(){
	System.out.println("The content type of response "+
		get(url).then().extract().contentType());
}
```

### Fetch Individual JSON Element
- Rest Assured, provides a mechanism to reach the values in the API using "path" 
```java
public static void getSpecificPartOfResponseBody(){
	ArrayList<String> amounts = when().get(url).then().extract().path("result.statements.AMOUNT") ;
	int sumOfAll=0;
	for(String a:amounts){
		System.out.println("The amount value fetched is "+a);
    sumOfAll=sumOfAll+Integer.valueOf(a);
	}
	System.out.println("The total amount is "+sumOfAll);
}
```

### References
- [REST Assured Tutorial](https://www.guru99.com/rest-assured.html)
- [API Automation Testing using RestAssured BDD Approach](https://www.youtube.com/watch?v=n3UITFRJ9KU&list=PLUDwpEzHYYLskkglxoXd0L6DKu4uOfh-m)
