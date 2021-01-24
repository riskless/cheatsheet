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
### CRUD Operations
- GET Request (WeatherAPI)
```java
// test url 
http://restapi.demoqa.com/utilities/weather/city/Hyderrabad
{
	"City": "Hyderrabad",
	"Temperature": ""
}

/* Test */
@Test
public void getWeatherDetails() {
	given()
	.when()
		.get("http://restapi.demoqa.com/utilities/weather/city/Hyderrabad")
	.then()
		.statusCode(200)
		.statusLine("HTTP/1.1 200 OK")
		.assertThat().body("City", equalTo("Hyderrabad"))
		.header("Content-Type","application/json");
}
```

- POST Request
```java
// test url
http://restapi.demoqa.com/customer

// request body
{
	"FirstName":"value",
	"LastName":"value",
	"UserName":"value",
	"Password":"value",
	"Email":"value",
}

// response body
{
	"SuccessCode": "OPERATION_SUCCESS",
	"Message": "Completed successfully"
}

/* Test */
public static HashMap map = new HashMap();

@BeforeClass
public void setUp () {
	map("FirstName","Harold");
	RestAssured.baseURI= "http://restapi.demoqa.com/customer";
	RestAssured.basePath= "/register";
}

@Test
public void postData() {
	given()
		.contentType("application/json")
		.body(map)
	.when()
		.post()
	.then()
		.statusCode(201)
		.and()
		.body("SuccessCode", equalTo("OPERATION_SUCCESS"))
		.and()
		.body("Message",equalTo("Completed successfully"));	
}
```

- PUT Request
```java
public static HashMap map = new HashMap();

@BeforeClass
public void setUp () {
	int empId = 101;
	map("name","Harold");
	RestAssured.baseURI= "http://dummy.restapiexample.com/api/v1";
	RestAssured.basePath= "/update/" + empId;
}

@Test
public void putData() {
	given()
		.contentType("application/json")
		.body(map)
	.when()
		.put()
	.then()
		.statusCode(200)
		.log().all();
}
```

- DELETE Request
```java
@Test
public void deleteData() {
	int empId = 101;
	RestAssured.baseURI= "http://dummy.restapiexample.com/api/v1";
	RestAssured.basePath= "/delete/" + empId;

	Response response = 
	when()
		.delete()
	.then()
		.statusCode(200)
		.statusLine("HTTP/1.1 200 OK")
		.log().all()
		.extract().response();

	// convert json to string format
	String result = response.asString(); 
	
	// verify
	Assert.assertEquals(result.contains("Deleted successfully"), true);
}
```


### Validations in JSON Response
```java
/* Test Status Code */
@Test(priority=1)
public void testStatusCode() {
	given()
	.when()
		.get("http://jsonplaceholder.typicode.com/posts/5")
	.then()
		.statusCode(200);
}

/* Log Response */
@Test(priority=2)
public void testLogResponse() {
	given()
	.when()
		.get("http://services.groupkt.com/country/get/iso2code/KR")
	.then()
		.statusCode(200)
		.log().all();
}

/* Verifying Single content in response body */
// Response
{
	"RestResponse": {
		"message": ["KR"],
		"result": {
			"name": "Korea"
		}
	}
}


@Test(priority=3)
public void testSingleContent() {
	given()
	.when()
		.get("http://services.groupkt.com/country/get/iso2code/KR")
	.then()
		.statusCode(200)
		.body("RestResponse.result.name", equalTo("Korea"));
		// we can use JSONPath Finder.
}

/* Verifying Multiple contents in response body */
// Response
{
	"RestResponse": {
		"message": ["Total"],
		"result": [
			{"name": "Korea"},
			{"name": "NZ"}
		]
	}
}

@Test(priority=4)
public void testMultipleContents() {
	given()
	.when()
		.get("http://services.groupkt.com/country/get/all")
	.then()
	 .	body("RestResponse.result.name", hasItems("Korea","NZ"));
}

/* Setting parameters & headers */
@Test(priority=5)
public void testParametersAndHeaders() {
	given()
		.param("MyName", "Harold")
		.header("MyHeader", "Korea");
	.when()
		.get("http://services.groupkt.com/country/get/iso2code/KR")
	.then()
		.statusCode(200)
		.log().all();
}
```

### Validation in XML Response
```java
/* Test XML */
<CUSTOMER>
	<ID>15</ID>
	<FIRSTNAME>Bill</FIRSTNAME>
	<LASTNAME>Clancy</LASTNAME>
</CUSTOMER>

<INVOICEList>
	<INVOICE>1</INVOICE>
	<INVOICE>2</INVOICE>
	<INVOICE>30</INVOICE>
</<INVOICEList>>

/* Verifying single content in XML response */
@Test(priority=1)
public void testSingleContent() {
	given()
	.when()
		.get("http://thomas-bayer.com/sqlrest/CUSTOMER/15/")
	.then()
		.body("CUSTOMER.ID", equalTo("15"))
		log().all();
}

/* Verifying multiple contents in XML response */
@Test(priority=2)
public void testMultipleContents() {
	given()
	.when()
		.get("http://thomas-bayer.com/sqlrest/CUSTOMER/15/")
	.then()
		.body("CUSTOMER.ID", equalTo("15"))
		.body("CUSTOMER.FIRSTNAME", equalTo("Bill"))
		.body("CUSTOMER.LASTNAME", equalTo("Clancy"));
}


/* Verifying all the contents in response body in one go */
@Test(priority=3)
public void testAllContents() {
	given()
	.when()
		.get("http://thomas-bayer.com/sqlrest/CUSTOMER/15/")
	.then()
		.body("CUSTOMER.text()", equalTo("15BillClancy"));
}

/* Find values using XML Path in XML Response */
@Test(priority=4)
public void testFindValueByXPath1() {
	given()
	.when()
		.get("http://thomas-bayer.com/sqlrest/CUSTOMER/15/")
	.then()
		.body(hasXPath("/CUSTOMER/FIRSTNAME", containsString("Bill")));

	given()
	.when()
		.get("http://thomas-bayer.com/sqlrest/INVOICE/")
	.then()
		.body(hasXPath("/INVOICEList/INVOICE[text()='30']"))
		.log().all();
}

/* XPath with text() function */
@Test(priority=5)
public void testFindValueByXPath2() {
	given()
	.when()
		.get("http://thomas-bayer.com/sqlrest/INVOICE/")
	.then()
		.body(hasXPath("/INVOICEList/INVOICE[text()='30']"))
		.log().all();
}


### Serialization & Deserialization
- Serialization
	-  The process of writing state of an object to a file is called serialization. But strictly speaking it is the process of converting an object from Java supported form to either file supported form or network supported form.
	- By using FileOutputStream and ObjectOutputStream classes we can achieve Serialization.
- Deserialization
	-  The process of reading state of an object from a file is called Deserialization. But strictly speaking it is the process of converting an object from either file or network supported form into Java supported form .
	- By using FileInputStream and ObjectInputStream classes we can achieve Deserialization.

```java
class Target implements Serialiable {
	int i=10, j=20;
}

public class TestApp {
	public static void main(String[] args) throws IOException {
		Target d = new Test();

		// Serialization
		// from d to abc.ser
		FileOutputStream fos = new FileOutputStream("abc.ser");
		ObjectOutputStream oos = ObjectOutputStream(fos);
		oos.writeObject(d);

		// Deserialization
		// from abc.ser to d
		FileInputStream fis = new FileInputStream("abc.ser");
		ObjectInputStream ois = ObjectInputStream(fis);
		Target t = (Target)ois.readObject();
	}
}
```

### Serialization & Deserialization (JSON)
```java
/* request & response - JSON Format */
[
	{"id":1,"name":"Harold","courses":["Java","C"]},
	{"id":2,"name":"Crystal","courses":["PHP","C"]}
]

/* post request without Serialization & Deserialization */
HashMap map = new HashMap();

@Test(priority=1)
public void createStudent() {
	map.put("id", 101);
	map.put("name", "Harold");

	ArrayList<String> courses = new ArrayList<String>();
	courses.add("Java");
	courses.add("C");
	map.put("courses", courses);

	given()
		.contentType(ContentType.JSON)
		.body(map)
	.when()
		.post("http://localhost:8080/student")
	.then()
		.statusCode(201)
		.assertThat()
		.body("msg", equalTo("Student added"));
}

/* get request without Serialization & Deserialization */
@Test(priority=2)
public void getStudent() {
	given()
	.when()
		.get("http://localhost:8080/student/101")
	.then()
		.statusCode(200)
		.assertThat()
		.body("id", equalTo(101))
		.log().all();
}

/* post request using Serialization & Deserialization */
// Student
public class Student implements Serialize {
	Integer id;
	String name;
	List<String> courses;
	// getter, setter, toString
}

@Test(priority=3)
public void createStudentWithSerializaton() {
	ArrayList<String> courses = new ArrayList<String>();
	courses.add("Java");
	courses.add("C");
	
	Student s = new Student();
	s.setId(102);
	s.setName("Harold");
	s.setCourses(courses);

	given()
		.contentType(ContentType.JSON)
		.body(s)
	.when()
		.post("http://localhost:8080/student")
	.then()
		.statusCode(201)
		.assertThat()
		.body("msg", equalTo("Student added"));
}

/* get request using Serialization & Deserialization */
@Test(priority=4)
public void getStudentWithSerializaton() {
	Student s = 
	get("http://localhost:8080/student/102").as(Student.class);

	Assert.assertEquals(s.getId(), 102);
}
```

### Serialization & Deserialization (XML)
```java
/* Post & Get Request using Serialization & Deserialization */
// VideoGame
@XmlAccessorType(XmlAccessType.FIELD)
@XmlRootElement
public class VideoGame implements Serializable { 
	// dependencies: jaxb-api
	// id, name, releaseDate, reviewScore
	// getter, setter
}

/* post request */
// create VideoGame object
given()
	.contentType("application/xml")
	.body(video_object)
.when()
	.post("resource_url")
.then()
	.log().all()
	.assertThat()
	.body(equalTo("{\"status\" : \"Record Added Successfully\")");

/* get request */
Student student = get("resource_url").as(VideoGame.class);
//verification
Assert.assertEquals(expected, actual);
```

### References
- [REST Assured Tutorial](https://www.guru99.com/rest-assured.html)
- [API Automation Testing using RestAssured BDD Approach](https://www.youtube.com/watch?v=n3UITFRJ9KU&list=PLUDwpEzHYYLskkglxoXd0L6DKu4uOfh-m)
