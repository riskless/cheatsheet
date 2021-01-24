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

### References
- [TestNG Tutorial (Software Testing Mentor)](https://www.youtube.com/watch?v=KegGpNMhGF0&list=PLL34mf651faMJ3uO8RNEh1GM5uLVXWq2Z)
