### Contents
- Resouce URIs
- CRUD Operations
- MySQL Database
- To be continued...

### Resouce URIs
```java
/* Traditional */
POST /RegisterUserServlet
{
	"name":"harold"
}

GET /GetAllUsersServlet

GET /GetUserDetailsServlet?userId=1

GET /DeleteUserDetailsServlet?userId=1

POST /UpdateUserServlet
{
	"name":"harold",
	"userId": "1"
}

/GetMessageComments?userId=1&messageId=5

/GetMessageComment?userId=1&messageId=5&commentId=1

/* REST */
POST /users
{
	"name":"harold"
}

GET /users

GET /users/1

DELETE /users/1

PUT /users/1
{
	"name":"harold"
}

GET /users/1/messages/5/comments

GET /users/1/messages/5/comments/1
```

- Naming Tips
	- Nouns as opposed to verbs or actions
		- /users
	- Plurals
		- /users/1
	- Predictable
		- /users/1/messages
		- /users/1/messages/2/likes


### CRUD Operations
```java
/* UserController */
@RestController
@RequestMapping("/users") // http://localhost:8080/users
public class UserController {
	@Autowired
	UserService userService;

	@Autowired
	AddressService addressService;

	@Autowired
	AddressService addressesService;
	
	@GetMapping(path = "/{id}", produces = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE })
	public UserRest getUser(@PathVariable String id) {
		UserRest returnValue = new UserRest();

		UserDto userDto = userService.getUserByUserId(id);
		ModelMapper modelMapper = new ModelMapper();
		returnValue = modelMapper.map(userDto, UserRest.class);

		return returnValue;
	}

	@PostMapping(consumes = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE }, produces = {
			MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE })
	public UserRest createUser(@RequestBody UserDetailsRequestModel userDetails) throws Exception {
		UserRest returnValue = new UserRest();
		
		// UserDto userDto = new UserDto();
		// BeanUtils.copyProperties(userDetails, userDto);
		
		ModelMapper modelMapper = new ModelMapper();
		UserDto userDto = modelMapper.map(userDetails, UserDto.class);

		UserDto createdUser = userService.createUser(userDto);
		returnValue = modelMapper.map(createdUser, UserRest.class);

		return returnValue;
	}

	@PutMapping(path = "/{id}", consumes = { MediaType.APPLICATION_XML_VALUE,
			MediaType.APPLICATION_JSON_VALUE }, produces = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE })
	public UserRest updateUser(@PathVariable String id, @RequestBody UserDetailsRequestModel userDetails) {
		UserRest returnValue = new UserRest();

		UserDto userDto = new UserDto();
		userDto = new ModelMapper().map(userDetails, UserDto.class);

		UserDto updateUser = userService.updateUser(id, userDto);
		returnValue = new ModelMapper().map(updateUser, UserRest.class);

		return returnValue;
	}

	@DeleteMapping(path = "/{id}", produces = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE })
	public OperationStatusModel deleteUser(@PathVariable String id) {
		OperationStatusModel returnValue = new OperationStatusModel();
		returnValue.setOperationName(RequestOperationName.DELETE.name());

		userService.deleteUser(id);

		returnValue.setOperationResult(RequestOperationStatus.SUCCESS.name());
		return returnValue;
	}

	@GetMapping(produces = { MediaType.APPLICATION_XML_VALUE, MediaType.APPLICATION_JSON_VALUE })
	public List<UserRest> getUsers(@RequestParam(value = "page", defaultValue = "0") int page,
			@RequestParam(value = "limit", defaultValue = "2") int limit) {
		List<UserRest> returnValue = new ArrayList<>();

		List<UserDto> users = userService.getUsers(page, limit);
		
		Type listType = new TypeToken<List<UserRest>>() {
		}.getType();
		returnValue = new ModelMapper().map(users, listType);

		/*for (UserDto userDto : users) {
			UserRest userModel = new UserRest();
			BeanUtils.copyProperties(userDto, userModel);
			returnValue.add(userModel);
		}*/

		return returnValue;
	}

	// http://localhost:8080/mobile-app-ws/users/jfhdjeufhdhdj/addressses
	@GetMapping(path = "/{id}/addresses", produces = { MediaType.APPLICATION_XML_VALUE,
			MediaType.APPLICATION_JSON_VALUE, "application/hal+json" })
	public Resources<AddressesRest> getUserAddresses(@PathVariable String id) {
		List<AddressesRest> addressesListRestModel = new ArrayList<>();

		List<AddressDTO> addressesDTO = addressesService.getAddresses(id);

		if (addressesDTO != null && !addressesDTO.isEmpty()) {
			Type listType = new TypeToken<List<AddressesRest>>() {
			}.getType();
			addressesListRestModel = new ModelMapper().map(addressesDTO, listType);

			for (AddressesRest addressRest : addressesListRestModel) {
				Link addressLink = linkTo(methodOn(UserController.class).getUserAddress(id, addressRest.getAddressId()))
						.withSelfRel();
				addressRest.add(addressLink);

				Link userLink = linkTo(methodOn(UserController.class).getUser(id)).withRel("user");
				addressRest.add(userLink);
			}
		}

		return new Resources<>(addressesListRestModel);
	}

	@GetMapping(path = "/{userId}/addresses/{addressId}", produces = { MediaType.APPLICATION_JSON_VALUE,
			MediaType.APPLICATION_XML_VALUE, "application/hal+json" })
	public Resource<AddressesRest> getUserAddress(@PathVariable String userId, @PathVariable String addressId) {

		AddressDTO addressesDto = addressService.getAddress(addressId);

		ModelMapper modelMapper = new ModelMapper();
		Link addressLink = linkTo(methodOn(UserController.class).getUserAddress(userId, addressId)).withSelfRel();
		Link userLink = linkTo(UserController.class).slash(userId).withRel("user");
		Link addressesLink = linkTo(methodOn(UserController.class).getUserAddresses(userId)).withRel("addresses");

		AddressesRest addressesRestModel = modelMapper.map(addressesDto, AddressesRest.class);

		addressesRestModel.add(addressLink);
		addressesRestModel.add(userLink);
		addressesRestModel.add(addressesLink);

		return new Resource<>(addressesRestModel);
	}
}

/* UserEntity */
@Entity
@Table(name="users")
public class UserEntity implements Serializable {
	private static final long serialVersionUID = 5313493413859894403L;
	
	@Id
	@GeneratedValue
	private long id;
	
	@Column(nullable=false)
	private String userId;

	@Column(nullable=false, length=50)
	private String firstName;
	
	@Column(nullable=false, length=50)
	private String lastName;
	
	@Column(nullable=false, length=120)
	private String email;
	
	@Column(nullable=false)
	private String encryptedPassword;
	
	private String emailVerificationToken;
	
	@Column(nullable=false)
	private Boolean emailVerificationStatus = false;
	
	@OneToMany(mappedBy="userDetails", cascade=CascadeType.ALL)
	private List<AddressEntity> addresses;
}

/* */
@Entity(name="addresses")
public class AddressEntity implements Serializable {

	private static final long serialVersionUID = 7809200551672852690L;
	
	@Id
	@GeneratedValue
	private long id;
	
	@Column(length=30, nullable=false)
	private String addressId;
	
	@Column(length=15, nullable=false)
	private String city;
	
	@Column(length=15, nullable=false)
	private String country;
	
	@Column(length=100, nullable=false)
	private String streetName;
	
	@Column(length=7, nullable=false)
	private String postalCode;
	
	@Column(length=10, nullable=false)
	private String type;
	
	@ManyToOne
	@JoinColumn(name="users_id")
	private UserEntity userDetails;
*/

/* UserRepository */
@Repository
public interface UserRepository extends PagingAndSortingRepository<UserEntity, Long> {
	UserEntity findByEmail(String email);
	UserEntity findByUserId(String userId);
	UserEntity findUserByEmailVerificationToken(String token); 
	
	@Query(value="select * from Users u where u.EMAIL_VERIFICATION_STATUS = 'true'", 
			countQuery="select count(*) from Users u where u.EMAIL_VERIFICATION_STATUS = 'true'", 
			nativeQuery = true)
	Page<UserEntity> findAllUsersWithConfirmedEmailAddress( Pageable pageableRequest );
	
	@Query(value="select * from Users u where u.first_name = ?1",nativeQuery=true)
	List<UserEntity> findUserByFirstName(String firstName);
	
	@Query(value="select * from Users u where u.last_name = :lastName",nativeQuery=true)
	List<UserEntity> findUserByLastName(@Param("lastName") String lastName);
	
	@Query(value="select * from Users u where first_name LIKE %:keyword% or last_name LIKE %:keyword%",nativeQuery=true)
	List<UserEntity> findUsersByKeyword(@Param("keyword") String keyword);
	
	@Query(value="select u.first_name, u.last_name from Users u where u.first_name LIKE %:keyword% or u.last_name LIKE %:keyword%",nativeQuery=true)
	List<Object[]> findUserFirstNameAndLastNameByKeyword(@Param("keyword") String keyword);
	
	@Transactional
	@Modifying
	@Query(value="update users u set u.EMAIL_VERIFICATION_STATUS=:emailVerificationStatus where u.user_id=:userId", nativeQuery=true)
	void updateUserEmailVerificationStatus(@Param("emailVerificationStatus") boolean emailVerificationStatus, 
			@Param("userId") String userId);
	
	@Query("select user from UserEntity user where user.userId =:userId")
	UserEntity findUserEntityByUserId(@Param("userId") String userId);
	
	@Query("select user.firstName, user.lastName from UserEntity user where user.userId =:userId")
	List<Object[]> getUserEntityFullNameById(@Param("userId") String userId);
	
    @Modifying
    @Transactional 
    @Query("UPDATE UserEntity u set u.emailVerificationStatus =:emailVerificationStatus where u.userId = :userId")
    void updateUserEntityEmailVerificationStatus(
    		@Param("emailVerificationStatus") boolean emailVerificationStatus,
            @Param("userId") String userId);

}

/* AddressRepository */
@Repository
public interface AddressRepository extends CrudRepository<AddressEntity, Long> {
	List<AddressEntity> findAllByUserDetails(UserEntity userEntity);
	AddressEntity findByAddressId(String addressId);
}

/* Test */
// POST http://localhost:8080/users
{
	"firstName":"Harold",
	"lastName":"Kim",
	"email":"test@test.com",
	"password":"1234",
	"addresses": [
		{
			"city":"Seoul","country":"Korea", "streetName":"123", "postalCode": "5011", "type":"home"
		},
		{
			"city":"Wellington","country":"NZ", "streetName":"234", "postalCode": "5012", "type": "office"
		}
	]
}
```

### MySQL Database
- Installation: https://dev.mysql.com/downloads/mysql/
- Login
	- mysql -u root -p
	- exit
- Creating MySQL Database
	- show databases;
	- create database mydb
	- use mydb
	- show tables;
- Creating a new User
	- create user 'harold'@'localhost' identified by password 'password';
	- grant all privileges on mydb.* to 'harold'@'localhost';
	- flush privileges;
	- exit
	- mysql -u harold -p
	- describe mytable;
- Delete user
	- drop user someuser;
	- flush privileges;	

- Adding MySQL Database Support
```java
/* pom.xml */
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
</dependency>

/* application.properties */
spring.datasource.username=harold
spring.datasource.password=password
spring.datasource.url=jdbc:mysql://localhost:3306/mydb
spring.jpa.hibernate.ddl-auto=update
```
