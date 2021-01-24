### Introduction of TestNG
- TestNG is a testing framework inspired from JUnit and NUnit but introducing some new functionalities that make it more powerful and easier to use.
- TestNG is designed to cover all categories of tests; unit, functional, end-to-end, integration, etc.
- TestNG is an open source automated testing framework. In TestNG, NG stands for "Next Generation".

### Advantages of TestNG
- Manages test suites and test cases
- Helps in prioritizing of tests
- Helps in grouping of tests
- Parallel execution
- Reporting

### Features of TestNG
- Multiple Before and After annotation options
- XML-based test configuration
	- Testng.xml file is used to organize and run the test suites.
- Dependent methods
	- Dependency is a feature of Testng that allows a test method to depend on the single or group of test methods.
- Groups / group of groups
	- TestNG groups allow you to group the test methods.
- Dependent groups
	- Test methods in a group can depend on the test methods of another group.
- Parameterization of test methods
	- We can pass the parameters to test methods in two ways, i.e., testng.xml file and DataProviders.
- Data-driven testing
	- This testing allows users to execute the same test multiple times with multiple sets of data.
- Multithreaded execution
	- Multithreaded execution is the parallel execution of tests.
- Better reporting
	- Testng provides XML and HTML reports by default for test execution.
- Open API
	- This feature allows you to create your custom extensions in your framework when required.

### How to Install TestNG in Eclipse IDE
- Three ways
1. Adding the TestNG jar file in the Build Path
2. Maven dependency
3. Installing the TestNG plug-in
	- Install via Eclipse Marketplace
		- Go to the [TestNG page on the Eclipse Market Place](https://marketplace.eclipse.org/content/testng-eclipse) and drag the icon called "Install" onto your workspace. 
	- Install from update site
		- Select Help / Install New Software...
    - Enter the update site URL in "Work with:" field:
			- Update site for release: https://dl.bintray.com/testng-team/testng-eclipse-release/.
    - Make sure the check box next to URL is checked and click Next.
    - Eclipse will then guide you through the process.
- References
	- http://testng.org/doc/
	- https://github.com/cbeust/testng-eclipse/
### Running test cases in TestNG without java compiler
- steps
1. Create the java project
2. create the Java class
3. If you install TestNG framework or TestNG library, then you do not need to compile on Java compiler as TestNG itself is a java compiler that compiles the test cases. In order to achieve this, we need to install the TestNG plug-in and then add the TestNG plug-in in a project.
4. Create a simple program
```java
@Test
public void test1() {}
```
5. Run the program by using TestNG. (Run As > TestNG Test)

### testng.xml
- In TestNG, you can define multiple test cases in a single class whereas, in Java, you can define only one test in a single class in the main() method.
- You can trigger all the test cases from a single file known as xml file. Xml file is the heart of TestNG framework.
- How to create a xml file
	- Right click on the project. Move your cursor down, and you will see TestNG and then click on the Convert to TestNG.
- We do not need to run the java files individually. We have to run the XML file which will automatically execute all the test cases as we have configured all the class files inside the XML file that are containing test cases.
	- Right click on the testng.xml file and then move down to the Run As and then click on the TestNG Suite.
- testng.xml
```html
<suite name="">  
	<test name="">  
		<classes>  
			<class name=""/>  
		</classes>  
  	</test>
 	<test name="">  
		<classes>  
			<class name=""/>
		</classes>  
	</test>  
</suite>
```
### Exclude/Include test cases
- In TestNG, test cases can be disabled in two ways.
	1. You can disable the test case in a @Test annotation.
	2. You can disable the test case in the XML file.

- You can disable or exclude the test cases by using the enable attribute to the @Test annotation and assign False value to the enable attribute.
```java
@Test(enabled=false)
public void test1() {}

@Test
public void test2() {}
```

- Disable the enable attribute in the XML file. (exclude/include)
```html
<suite name="">  
	<test name="">  
	<classes>  
		<class name=""> 
			<methods><include name =""/></methods>   
		</class>
	</classes>  
	</test>
	<test name="">  
	<classes>  
		<class name="">  
			<methods><exclude name =""/></methods>  
		</class>  
	</classes>  
	</test>  
</suite>
```

### Running test cases with Regex
- Include all the test cases represented by the starting keyword 'Mobile'
```html
<suite name="">  
  <test name="">  
		<classes>  
			<class name=""> 
				<methods><include name="Mobile.*"/></methods>   
			</class>
		</classes>  
  </test>
</suite>
```

### TestNG Groups
- TestNG Groups allow you to perform groupings of different test methods. Grouping of test methods is required when you want to access the test methods of different classes.
- Test cases within Groups
```java
/* PersonalLoan.java */
public class PersonalLoan {  
 @Test(groups= {"SmokeTest"})  
 public void WebLoginPersonalLoan() {}
 
 @Test  
 public void MobileLoginPersonalLoan() {} 

 @Test  
 public void APILoginPersonalLoan() {}
}  

/* HomeLoan.java */
public class HomeLoan {  
 @Test
 public void WebLoginHomeLoan() {}
 
 @Test(groups= {"SmokeTest"})  
 public void MobileLoginHomeLoan() {} 

 @Test  
 public void APILoginHomeLoan() {}
}  

/* CarLoan.java */
public class CarLoan {  
 @Test
 public void WebLoginCarLoan() {}
 
 @Test  
 public void MobileLoginCarLoan() {} 

 @Test(groups= {"SmokeTest"})  
 public void APILoginCarLoan() {}
} 
```

```html
<!-- testng.xml -->
<suite name="test_suite">  
	<groups>  
		<run>  
			<include name="SmokeTest"/>  
		</run>  
	</groups>  

	<test name="Personal Loan">  
		<classes>  
			<class name="PersonalLoan"/>  
		</classes>  
	</test>
	
	<test name="Home Loan">  
		<classes>  
			<class name="HomeLoan"/>  
		</classes>  
	</test> 

	<test name="Car Loan">  
		<classes>  
			<class name="CarLoan"/>  
		</classes>  
	</test>
</suite>

<!-- testng.xml -->
<suite name="test_suite">  
	<test name="Loan">  
		<groups>  
			<run>  
				<include name="SmokeTest"/>  
			</run>  
		</groups>  
		<classes>  
			<class name="PersonalLoan"/>  
			<class name="HomeLoan"/>  
			<class name="CarLoan"/>
		</classes>  
	</test> 
</suite>
```

### Tests belonging to multiple Groups
```java
// Groups.java
public class Groups {  
	@Test(groups= {"Group A"})  
	public void testcase1() {}  

	@Test(groups= {"Group A","Group B"})  
	public void testcase2() {}
 
	@Test(groups= {"Group B"})  
	public void testcase3() {}  
}  
```

```html
<!-- testng.xml -->
<suite name="test_suite">  
	<test name="Group A">  
		<groups>  
			<run>  
				<include name="Group A"/>  
			</run>  
		</groups>  
		<classes>  
			<class name="Groups"/>  
		</classes>  
	</test>

	<test name="Group B">  
		<groups>  
			<run>  
				<include name="Group B"/>  
			</run>  
		</groups>  
		<classes>  
			<class name="Groups"/>  
		</classes>  
	</test>
</suite>
```

### Including/Excluding Groups
```java
// Groups.java
public class Groups {  
	@Test(groups= {"Include Group"})  
	public void test_case1() {}  
 
	@Test(groups= {"Include Group"})  
	public void test_case2() {}
 
	@Test(groups= {"Exclude Group"})  
		
	public void test_case3() {}
}  
```

```html
<!-- testng.xml -->
<suite name="test_suite">  
	<test name="Include and Exclude Group">  
		<groups>  
		<run>  
			<include name="Include Group"/>  
			<exclude name="Exclude Group"/>  
		</run>  
		</groups>  
		<classes>  
			<class name="Groups"/>  
		</classes>  
	</test>
</suite>
```

### Using Regular Expressions
```java
// RegularExpression
public class RegularExpression {  
	@Test(groups= {"Include test case1"})  
	public void test_case1() {}   

	@Test(groups= {"Include test case2"})  
	public void test_case2() {}   
 
	@Test(groups= {"Exclude test case3"})  
	public void test_case3() {}
} 
```
```html
<!-- testng.xml -->
<suite name="test_suite">  
	<test name="Including test cases">  
		<groups>  
			<run>  
				<include name="Include.*"/>  
			</run>  
		</groups>  
		<classes>  
			<class name="RegularExpression"/>  
		</classes>  
	</test>
</suite>
```

### Groups in Groups
- We can also specify a group within another group. The groups which are defined in another groups are known as Meta Groups.
```java
// GroupsInGroups
public class GroupsInGroups {  
 @Test(groups= {"Smoke"})  
 public void test1() {}  
 @Test(groups= {"Regression"})  
 public void test2() {} 
 @Test  
 public void test3() {}  
}  
```
```html
<!-- testng.xml -->
<suite name="test_suite">  
	<test name="Groups in Groups">  
		<groups>  
			<define name="Group 1">  
				<include name="Smoke"/>  
				<include name="Regression"/>  
			</define>  
			<run>  
				<include name="Group 1"/>  
			</run>  
		</groups>  
		<classes>  
			<class name="GroupsInGroups"/>  
		</classes>  
	</test>
</suite>
```

### TestNG Annotation
- TestNG Annotation is a piece of code which is inserted inside a program or business logic used to control the flow of execution of test methods.
- @BeforeSuite
	- The @BeforeSuite annotated method will run before the execution of all the test methods in the suite.
- @AfterSuite 	
	- The @AfterSuite annotated method will run after the execution of all the test methods in the suite.
- @BeforeTest 	
	- The @BeforeTest annotated method will be executed before the execution of all the test methods of available classes belonging to that folder.
- @AfterTest 	
	- The @AfterTest annotated method will be executed after the execution of all the test methods of available classes belonging to that folder.
- @BeforeClass 	
	- The @BeforeClass annotated method will be executed before the first method of the current class is invoked.
- @AfterClass 	
	- The @AfterClass annotated method will be invoked after the execution of all the test methods of the current class.
- @BeforeMethod 	
	- The @BeforeMethod annotated method will be executed before each test method will run.
- @AfterMethod
	- The @AfterMethod annotated method will run after the execution of each test method.
- @BeforeGroups
	- The @BeforeGroups annotated method run only once for a group before the execution of all test cases belonging to that group.
- @AfterGroups
	- The @AfterGroups annotated method run only once for a group after the execution of all test cases belonging to that group.

### TestNG Annotation Attributes
- description
	-  The "description" attribute provides information about the test.
```java
@Test(description="This is test1")  
public void test1() {}  
```

- dependsOnMethods
	- When a single value is passed in a parameter.
	- We want WebStudentLogin() method to be executed before the execution of the APIStudentLogin() method.
```java
@Test
public void WebStudentLogin() {}  

@Test(dependsOnMethods= {"WebStudentLogin"}) 
public void APIStudentLogin() {}  

// When multiple values are passed in a parameter.
@Test(dependsOnMethods= {"testcase3","testcase2"})  
public void testcase1() {}

@Test  
public void testcase2() {} 
 
@Test  
public void testcase3() {} 
```

- priority
	- TestNG will run the test cases in alphabetical order by default.
	- The priority can hold the integer values between -5000 and 5000. 
	- When the priority is set, the lowest priority test case will run first and the highest priority test case will be executed last.
	- If priority is not specified, then the default priority will be 0.
```java
@Test
public void test1() {}  

@Test(priority=2) 
public void test2() {}  

@Test(priority=1)
public void test3() {}  
```

- enabled
	- The 'enabled' attribute contains the boolean value. By default, its value is true. If you want to skip some test method, then you need to explicitly specify 'false' value.
```java
@Test
public void test1() {}  

@Test(enabled=false)
public void test2() {}  
```

- groups
	- The 'groups' attribute is used to group the different test cases that belong to the same functionality.
```java
@Test(groups= {"software company"})  
public void infosys() {}
```

- timeOut
	- The timeOut is a time period provided to the test case to completely execute its test case.
```java
@Test(timeOut=200)  
public void test1() throws InterruptedException {  
	Thread.sleep(500);  
	System.out.println("This is test1");  
} 

@Test(timeOut=500)
public void test2() {}  
```

### TestNG Parameters
- TestNG Parameters are the arguments that we pass to the test methods. There are two ways through which we can pass the parameters to the test methods.
1. TestNG Parameters
2. TestNG DataProviders

- When Parameters are applied below the tag.
```java
/* Sum */
public class Sum {  
	@Test  
	@Parameters({"a","b"})  
	public void add(int c, int d) {  
		int sum=c+d;  
		System.out.println("Sum of two numbers : "+sum);  
	}  
}  

/* Subtract */
public class Subtract {  
	@Test  
	@Parameters({"a","b"})  
	public void add(int c, int d) {  
		int subtract=c-d;  
		System.out.println("Subtraction of two numbers : "+subtract);  
	}  
}  

/* Multiply */
public class Multiply {  
	@Test  
	@Parameters({"a","b"})  
	public void add(int c, int d) {  
		int mul=c*d;  
		System.out.println("Multiplication of two numbers : "+mul);  
	}  
}  
```
```html
<!-- testng.xml -->
<suite name="Suite">  
	<parameter name="a" value="5"/>  
	<parameter name="b" value="3"/>  
	<test name="Sum">   
		<classes>  
			<class name= "Sum"/>  
		</classes>  
	</test>
	<test name="Subtract">  
		<classes>  
			<class name="Subtract"/>  
		</classes>  
	</test>  
	<test name="Multiply">  
		<classes>  
			<class name="Multiply"/>  
		</classes>  
	</test>  
</suite>
```

- When parameters are specific.
```java
/* Fruits */
public class Fruits {  
	@Test  
	@Parameters("mango")  
	public void mango(String m)	{  
		System.out.println("Fruits names are:  ");  
		System.out.println(m);  
	}  
	@Test  
	@Parameters("orange")  
	public void orange(String o) {  
		System.out.println(o);  
	}  
}  

/* Vegetable */
public class Vegetable {  
	@Test  
	@Parameters("Cauliflower")  
	public void c(String m) {  
		System.out.println("Vegetable names are :");  
		System.out.println(m);  
	}  
	@Test  
	@Parameters("Ladyfinger")	
	public void orange(String l) {  
		System.out.println(l);  
	}  
}  
```
```html
<!-- testng.xml -->
<suite name="Suite">  
	<test name="Fruits">   
		<parameter name="mango" value="mango"/>  
		<parameter name="orange" value="orange"/>  
		<classes>  
			<class name= "site.haroldkim.Fruits"/>  
		</classes>  
	</test>  
	<test name="Vegetables">   
		<parameter name="Cauliflower" value="Cauliflower"/>  
		<parameter name="Ladyfinger" value="Ladyfinger"/>  
		<classes>  
			<class name= "site.haroldkim.Vegetable"/>  
		</classes>  
	</test>  
</suite>
```

### TestNG Listeners
- TestNG provides the @Listeners annotation which listens to every event that occurs in a selenium code.
- Listeners are implemented by the ITestListener interface.
- Methods of ITestListener
	- onTestStart(): An onTestStart() is invoked only when any test method gets started.
	- onTestSuccess(): An onTestSuccess() method is executed on the success of a test method.
	- onTestFailure(): An onTestFailure() method is invoked when test method fails.
	- onTestSkipped(): An onTestSkipped() run only when any test method has been skipped.
	- onTestFailedButWithinSuccessPercentage(): This method is invoked each time when the test method fails but within success percentage.
	- onStart(): An onStart() method is executed on the start of any test method.
	- onFinish(): An onFinish() is invoked when any test case finishes its execution.
- How to create the TestNG Listeners
1. We can use the @Listeners annotation within the class.
2. We create the listeners by using testng.xml file.

- We can use the @Listeners annotation within the class.
	- When sum() test case has been passed, then TestNG calls the onTestSuccess() listener. When testTofail() test case has been failed, then TestNG calls the onTestFailure() listener.
```java
/* Class1 */
@Listeners(Listener.class)  
public class Class1 {  
	@Test  
	public void sum() {  
		int sum=0;  
		int a=5;  
		int b=7;  
		sum=a+b;  
		System.out.println(sum);  
	}  
	@Test  
	public void testTofail() {  
		System.out.println("Test case has been failed");  
		Assert.assertTrue(false);  
	}  
} 

/* Listener.java */
public class Listener implements ITestListener {  
	@Override  
	public void onTestStart(ITestResult result) {}  
		
	@Override  
	public void onTestSuccess(ITestResult result) {}  
		
	@Override  
	public void onTestFailure(ITestResult result) {}  
		
	@Override  
	public void onTestSkipped(ITestResult result) {}  
		
	@Override  
	public void onTestFailedButWithinSuccessPercentage(ITestResult result) {}  
		
	@Override  
	public void onStart(ITestContext context) {}  
		
	@Override  
	public void onFinish(ITestContext context) {}  
}  

```html
-- testng.xml
<suite name="Suite">  
	<test name="Listeners">  
		<classes>  
			<class name="Class1"></class>  
		</classes>  
	</test>  
</suite>
```
- We create the listeners by using testng.xml file.
	- When listeners are added to multiple classes then it could be an error prone. If listeners are implemented through the testng.xml file, then they are applied to all the classes.

```java
/* Testcases */
public class Testcases {  
	@Test  
	public void testtopass() {}  

	@Test  
	public void testtofail() {}  
}  

/* Listener.java */
public class Listener implements ITestListener {  
	@Override  
	public void onTestStart(ITestResult result) {}  
		
	@Override  
	public void onTestSuccess(ITestResult result) {}  
		
	@Override  
	public void onTestFailure(ITestResult result) {}  
		
	@Override  
	public void onTestSkipped(ITestResult result) {}  
		
	@Override  
	public void onTestFailedButWithinSuccessPercentage(ITestResult result) {}  
		
	@Override  
	public void onStart(ITestContext context) {}  
		
	@Override  
	public void onFinish(ITestContext context) {}  
}  
```
```html 
<!-- testng.xml -->
<suite name="Suite">  
	<listeners>  
		<listener class-name="Listener"/>  
	</listeners>  
	<test name="Listeners_program">  
		<classes>  
			<class name="Testcases"></class>  
		</classes>  
	</test>  
</suite>   
```
### References
- [TestNG Tutorial (Software Testing Mentor)](https://www.youtube.com/watch?v=KegGpNMhGF0&list=PLL34mf651faMJ3uO8RNEh1GM5uLVXWq2Z)
