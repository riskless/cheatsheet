###  A Persistent Class
- A persistent class is a Class that follows the POJO rules (listed below) and whose objects or instances would be stored in to the database in its corresponding table/fields.
- POJO (Plain old Java object) rules
	-	All fields are private
	-	All fields would have public getter/setters
	-	Will have default constructor
	-	Should not extend/implement any specialized classes/interfaces
	-	Implement Serializable

### A Hibernate Configuration file
- The Hibernate Configuration file is where we would specify the Database connection parameters. So that, hibernate can connect to the database and do what it has to do!
```xml
<hibernate-configuration>
	<session-factory>

		<!-- Connection settings -->
		<property name="hibernate.connection.driver_class">org.postgresql.Driver</property>
		<property name="hibernate.connection.url">jdbc:postgresql://127.0.0.1:5432/postgres</property>
		<property name="hibernate.connection.username">postgres</property>
		<property name="hibernate.connection.password">postgres</property>

		<!-- SQL dialect -->
		<property name="hibernate.dialect">org.hibernate.dialect.PostgreSQLDialect</property>

		<!-- Print executed SQL to stdout -->
		<property name="show_sql">true</property>
		<property name="format_sql">true</property>
		
		<!-- Create/Update/Validate/Create-drop database -->
		<property name="hibernate.hbm2ddl.auto">update</property>

		<!-- Annotated entity classes <mapping class="site.haroldkim.entity.Employee"/> -->

		<mapping resource="Employee.hbm.xml" />
	</session-factory>
</hibernate-configuration>
```

### A Mapping XML file
- The mapping XML file would help us specify the mapping of class/fields with RDB table/columns. This will help Hibernate figure out which persistence class maps to with table in database. Alternatively, we could use annotations to do the same Job!
 ```xml
 <hibernate-mapping>
   <class name = "site.haroldkim.entity.Employee" table = "EMPLOYEE">
      
      <id name = "id" type = "long" column = "id">
         <generator class="identity"/>
      </id>
      
      <property name = "name" column = "name" type = "string"/>
      <property name = "age" column = "age" type = "int"/>
      
   </class>
</hibernate-mapping>
```
### Session
- state: tranient, persistent, detached

### Annotations
- Support of JPA annotations, will make it easier to switch to other ORM tools.

### SessionFactory
```java
private static SessionFactory sessionFactory = null;

static {
	try {
		loadSessionFactory();
	} catch (Exception e) {
		e.printStackTrace();
	}
}

public static void loadSessionFactory() {

	Configuration configuration = new Configuration();
	configuration.configure("hibernate.cfg.xml");
	configuration.addResource("Employee.hbm.xml");
	//configuration.addAnnotatedClass(site.haroldkim.entity.Employee.class);
	ServiceRegistry srvcReg = new StandardServiceRegistryBuilder().applySettings(configuration.getProperties())
			.build();
	sessionFactory = configuration.buildSessionFactory(srvcReg);
}

public static Session getSession() throws HibernateException {

	Session session = null;
	try {
		session = sessionFactory.openSession();
	} catch (Exception e) {
		System.err.println("Exception while opening a session ");
		e.printStackTrace();
	}

	return session;
}

public static void main(String[] args) {

	Session session = null;
	Transaction txn = null;

	try {
		session = getSession();
		txn = session.beginTransaction();

		Employee user = new Employee("Crystal", 40);
		session.save(user);

		txn.commit();
	} catch (Exception e) {
		if (txn != null)
			txn.rollback();
		e.printStackTrace();
	} finally {
		session.close();
	}
}
```


### CRUD operations
```java
static Session session;
static SessionFactory sessionFactory;

// This method build session factory and returns that object
private static SessionFactory loadSessionFactory() {
	if(sessionFactory != null) {
		return sessionFactory;
	}
	Configuration configuration = new Configuration();
	configuration.configure("postgresql.cfg.xml");
	configuration.addAnnotatedClass(com.company.hb_proj.Employee.class);
	ServiceRegistry srvcReg = new StandardServiceRegistryBuilder().applySettings(configuration.getProperties())
			.build();
	sessionFactory = configuration.buildSessionFactory(srvcReg);
	return sessionFactory;
}

// Create multiple entries in the table
public static void createEmp() {
	Employee empOjb = null;
	Transaction txn = null;
	try {
		session = loadSessionFactory().openSession();

		// Begin Transaction
		txn = session.beginTransaction();

		for (int i = 1; i <= 10; i++) {
			empOjb = new Employee(i, "Employee" + 1, 30 + i);
			session.save(empOjb);
		}

		// Flush and commit the transaction in to database
		txn.commit();

		System.out.println("\nEmplyee Records are added in to the Database!\n");
	} catch (Exception e) {
		if (txn != null) {
			System.out.println("\nTransaction is being rolled back...\n");
			txn.rollback();
		}
	} finally {
		if (session != null)
			session.close();
	}
}

// Read entries from employee table
public static List<Employee> readEmp() {

	Transaction txn = null;

	List<Employee> EmployeesList = new ArrayList<Employee>();
	try {
		session = loadSessionFactory().openSession();

		// Begin Transaction
		txn = session.beginTransaction();

		EmployeesList = session.createQuery("FROM Employee").list();

		// Flush and commit the transaction in to database
		txn.commit();
	} catch (Exception e) {
		if (txn != null) {
			System.out.println("\nTransaction is being rolled back...\n");
			txn.rollback();
		}
	} finally {
		if (session != null)
			session.close();
	}
	return EmployeesList;
}

// Update an entry in employee table based on the ID
public static void updateEmp(int emp_id) {

	Transaction txn = null;

	try {
		session = loadSessionFactory().openSession();

		// Begin Transaction
		txn = session.beginTransaction();

		// Updating entry
		Employee stuObj = session.get(Employee.class, emp_id);
		stuObj.setName("Updated Name");

		// Flush and commit the transaction in to database
		txn.commit();

		System.out.println("\nEmployee With Id?= " + emp_id + " is updated with a new name\n");
	} catch (Exception e) {
		if (txn != null) {
			System.out.println("\nTransaction is being rolled back...\n");
			txn.rollback();
		}
	} finally {
		if (session != null)
			session.close();
	}
}

// Delete an entry in employee table based on the ID
public static void deleteRecord(Integer emp_id) {
	Transaction txn = null;
	try {
		session = loadSessionFactory().openSession();

		// Begin Transaction
		txn = session.beginTransaction();

		Employee empObj = session.load(Employee.class, emp_id);
		session.delete(empObj);

		// Committing The Transactions To The Database
		txn.commit();
		System.out.println("\nEmployee With Id?= " + emp_id + " is deleted\n");
	} catch (Exception e) {
		if (txn != null) {
			System.out.println("\nTransaction is being rolled back...\n");
			txn.rollback();
		}
	} finally {
		if (session != null)
			session.close();
	}
}

// Delete all entries
public static void deleteAll() {
	Transaction txn = null;
	try {
		session = loadSessionFactory().openSession();

		// Begin Transaction
		txn = session.beginTransaction();

		session.createQuery("DELETE FROM Employee").executeUpdate();

		// Committing The Transactions To The Database
		txn.commit();
		System.out.println("\nAll employee records are deleted..\n");
	} catch (Exception e) {
		if (txn != null) {
			System.out.println("\nTransaction is being rolled back...\n");
			txn.rollback();
		}
	} finally {
		if (session != null)
			session.close();
	}
}

public static void closeSessionFactory() {
	if (sessionFactory != null)
		sessionFactory.close();
}


/* Test class */
public class Test {

	public static void main(String[] args) {

		System.out.println("Performing Hibernate CRUD Operations..\n");

		System.out.println("Creating Employee Records...\n");
		HibernateDAO.createEmp();

		System.out.println("Reading Employee Records...\n");
		List<Employee> entries = HibernateDAO.readEmp();
		if (entries != null & entries.size() > 0) {
			for (Employee empObj : entries) {
				System.out.println(empObj.toString());
			}
		}

		System.out.println("Updating Employe Record with id '5'...\n");
		HibernateDAO.updateEmp(5);
		System.out.println("Updated Records...\n");
		List<Employee> updatedEntries = HibernateDAO.readEmp();
		if (updatedEntries != null & updatedEntries.size() > 0) {
			for (Employee empObj : updatedEntries) {
				System.out.println(empObj.toString());
			}
		}
		
		System.out.println("Deleting a Employe Record with id '2'...\n");
		HibernateDAO.deleteRecord(2);
		System.out.println("Records after delete...\n");
		List<Employee> entriesAfterDelete = HibernateDAO.readEmp();
		if (entriesAfterDelete != null & entriesAfterDelete.size() > 0) {
			for (Employee empObj : entriesAfterDelete) {
				System.out.println(empObj.toString());
			}
		}
		
		System.out.println("Deleting all records...\n");
		HibernateDAO.deleteAll();	
		
		HibernateDAO.closeSessionFactory();

	}

}
```

### relations
- @OneToOne
```java
@Entity
@Table(name="employee")
public class Employee implements Serializable {
	@Id
	@Column(name="eid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
}


@Entity
@Table(name="emp_contact")
public class Contact {
	@OneToOne(targetEntity=Employee.class,cascade=CascadeType.ALL)
	@JoinColumn(name="emp_id",referencedColumnName="eid")
	private  Employee  employee;
}
```

- @OneToMany
```java
@Entity
@Table(name = "boss")
public class Boss implements Serializable {
	@Id
	@Column(name = "bid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@OneToMany(targetEntity = Employee.class, cascade = CascadeType.ALL)
	@JoinColumn(name = "boss_id", referencedColumnName = "bid")
	private Set<Employee> employees;
}

```

- @ManyToOne
```java
@Entity
@Table(name = "boss")
public class Boss implements Serializable {
	@Id
	@Column(name = "bid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
}

@Entity
@Table(name = "employee")
public class Employee {
	@ManyToOne(targetEntity = Boss.class, cascade = CascadeType.ALL)
	@JoinColumn(name = "boss_id", referencedColumnName = "bid")
	private Boss boss;
}
```

- bidirection
```java
@Entity
@Table(name = "boss")
public class Boss implements Serializable {
	@Id
	@Column(name = "bid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@OneToMany(targetEntity = Employee.class, cascade = CascadeType.ALL)
	@JoinColumn(name = "boss_id", referencedColumnName = "bid")
	private Set<Employee> employees;
}

@Entity
@Table(name = "employee")
public class Employee {
	@ManyToOne(targetEntity = Boss.class, cascade = CascadeType.ALL)
	@JoinColumn(name = "boss_id", referencedColumnName = "bid")
	private Boss boss;
}

/* Test */
Boss boss = new Boss("Jobs",45);

Employee emp1 = new Employee("John");
Employee emp2 = new Employee("Rohit");			
emp1.setBoss(boss);
emp2.setBoss(boss);

session = getSession();
txn = session.beginTransaction();

session.save(emp1);
session.save(emp2);

txn.commit();
```

- @ManyToMany
```java
@Entity
@Table(name = "employee")
public class Employee implements Serializable{
	@Id
	@Column(name = "empid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@ManyToMany(targetEntity=Course.class,cascade = CascadeType.ALL)
	@JoinTable(name = "EMPLOYEE_COURSE", joinColumns = { @JoinColumn(name = "empid") }, inverseJoinColumns = { @JoinColumn(name = "cid") })
	private Set<Course> course;
}

@Entity
@Table(name = "courses")
public class Course implements Serializable {
	@Id
	@Column(name = "cid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@ManyToMany(targetEntity=Employee.class,cascade = CascadeType.ALL)
	@JoinTable(name = "EMPLOYEE_COURSE", inverseJoinColumns = { @JoinColumn(name = "empid") }, joinColumns = { @JoinColumn(name = "cid") })
	private Set<Employee> employees;
}

/* Test */
// insert employee
Course course1 = new Course("Java");
Course course2 = new Course("Hibernate");

Set<Course> courseSet = new HashSet<Course>();
courseSet.add(course1);
courseSet.add(course2);

Employee emp1 = new Employee("John");
emp1.setCourse(courseSet);

session = getSession();
txn = session.beginTransaction();

session.save(emp1);

txn.commit();

// insert course
Course course = new Course("Java");

Employee emp1 = new Employee("Ben");
Employee emp2 = new Employee("Rahul");

Set<Employee> empSet = new HashSet<Employee>();
empSet.add(emp1);
empSet.add(emp2);

course.setEmployees(empSet);

session = getSession();

txn = session.beginTransaction();

session.save(course);

txn.commit();
```

### Collection Mapping (List and Set)
```java
@Entity
@Table(name = "boss")
public class Boss implements Serializable {
	@Id
	@Column(name = "bid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;

	@ElementCollection
	@CollectionTable(name="otherNames", JoinColumns=@JoinColumn(name="bid"))
	@Column(name="nickname")
	private List<String> nickNames;
	//private Set<String> nickNames;
}

/* Test */
session = getSession();
txn = session.beginTransaction();

// Set<String> nickNames = new HashSet<>();
List<String> nickNames = new ArrayList<>();
nickNames.add("Cat");
nickNames.add("Dog");
Boss boss = new Boss("Jobs",45, nickNames);

session.save(boss);
txn.commit();

/* in case of XML configuration */
<set name="nicknames" table="othernames">
	<key column="bid"></key>
	<element column="nicknames" type="string"></element>
</set>
```

### Collection Mapping (Map)
```java
@ElementCollection
@CollectionTable(name="otherNames")
@MapKeyColumn(name="Nickname")
@Column(name="whocreated")
private Map<String, String> nickNames;

/* Test */
Map<String,String> nickNames = new HashMap<>();
nickNames.put("Cat", "john");
nickNames.put("Dog", "john");

Boss boss = new Boss("Jobs",45, nickNames);
session.save(boss);
```

### Embedded Types
```java
/* Address */
@Embeddable
public class Address {
}

/* Boss */
@Embedded
private Address address;

/* Test */
Address address = new Address("street","city","state", "country");
Boss boss = new Boss("Jobs",45, address);
session.save(boss);
```

### Inheritance (Mapped Entity)
```java
/* Person */
@MappedSuperClass {
public class Person {
	@Column(name="name")
	private String name;
	private float age;
}

/* Employee */
@Entity 
@Table(name="employee")
public class Employee extends Person implements Serializable {
	@Id
	@Column(name = "empid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
}

/* Boss */
@Entity 
@Table(name="boss")
public class Boss extends Person implements Serializable {
	@Id
	@Column(name = "bid")
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private Long id;
}

/* Test */
// Table: boss(bid, name, age), employee(empid, name, age)
Employee emp = new Employee();
emp.setName("John");
emp.setAge(35);

Boss boss = new Boss();
boss.setName("Boss");
boss.setAge(45);

session.save(emp);
session.save(boss);
```

### Inheritance (Single Table Inheritance)
```java
/* Person */
@Entity
@Table(name = "person")  
@Inheritance(strategy=InheritanceType.SINGLE_TABLE)  
@DiscriminatorColumn(name="type", discriminatorType=DiscriminatorType.STRING)
@DiscriminatorValue(value="person")
public class Person {
	@Id
	@Column(name = "id")
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	
	@Column(name = "name")
	private String name;
	
	@Column(name = "age")
	private float age;
}

/* Employee */
@Entity 
@DiscriminatorValue(value="employee")
public class Employee extends Person implements Serializable {
	private String bossName;
}

/* Boss */
@Entity 
@DiscriminatorValue(value="boss")
public class Boss extends Person implements Serializable {
	private int empCount;
}

/* Test */
// Table: person(dtype,id,age,name,empcount,bossname)
Person person = new Person();
person.setName("Person");
person.setAge(27);

Employee emp = new Employee();
emp.setName("John");
emp.setAge(35);
emp.setBossName("Doe");

Boss boss = new Boss();
boss.setName("Boss");
boss.setAge(45);
boss.setEmpCount(100);

session.save(emp);
session.save(boss);
session.save(person);
```

### Inheritance (Joined Inheritance)
```java
/* Person */
@Entity
@Table(name = "person")  
@Inheritance(strategy=InheritanceType.JOINED)  
public class Person {
	@Id
	@Column(name = "id")
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	
	@Column(name = "name")
	private String name;
	
	@Column(name = "age")
	private float age;
}

/* Employee */
@Entity 
@PrimaryKeyJoinColumn(name="FK_Person")
public class Employee extends Person implements Serializable {
	private String bossName;
}

/* Boss */
@Entity 
@PrimaryKeyJoinColumn(name="FK_Person")
public class Boss extends Person implements Serializable {
	private int empCount;
}

/* Test */
// Table: person(id,age,name), bos(empcount,fk_person),employee(bossname,fk_person)
Person person = new Person();
person.setName("Person");
person.setAge(27);

Employee emp = new Employee();
emp.setName("John");
emp.setAge(35);
emp.setBossName("Doe");

Boss boss = new Boss();
boss.setName("Boss");
boss.setAge(45);
boss.setEmpCount(100);

session.save(emp);
session.save(boss);
session.save(person);
```

### Inheritance (Table Per Class)
```java
/* Person */
@Entity
@Table(name = "person")  
@Inheritance(strategy=InheritanceType.TABLE_PER_CLASS)
public class Person {
	@Id
	@Column(name = "id")
	@GeneratedValue(strategy = GenerationType.AUTO)
	private Long id;
	
	@Column(name = "name")
	private String name;
	
	@Column(name = "age")
	private float age;
}

/* Employee */
@Entity 
public class Employee extends Person implements Serializable {
	private String bossName;
}

/* Boss */
@Entity 
public class Boss extends Person implements Serializable {
	private int empCount;
}

/* Test */
// Table: person(id,age,name), bos(id,age,name,empcount),employee(id,age,name,bossname)
Person person = new Person();
person.setName("Person");
person.setAge(27);

Employee emp = new Employee();
emp.setName("John");
emp.setAge(35);
emp.setBossName("Doe");

Boss boss = new Boss();
boss.setName("Boss");
boss.setAge(45);
boss.setEmpCount(100);

session.save(emp);
session.save(boss);
session.save(person);
```
