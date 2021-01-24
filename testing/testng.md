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
