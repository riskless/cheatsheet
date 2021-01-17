### Hibernate Configuration file
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

		<!-- Annotated entity classes <mapping class="com.company.hb_proj.Employee"/> -->

		<mapping resource="Employee.hbm.xml" />
	</session-factory>
</hibernate-configuration>
```
