### Unit Testing
- Unit indicates a part of a project which can be a task/story.
- Once code is completed by developer, then that part must be tested.

### JUint5
- An Unit Test framework.
- An open source Java Library.
- JUnit Runtime is provided with 3 components and one platform runtime.
	- JUnit Platform Runtime
		- Implementation of the TestEngine API for JUnit Jupiter
	- JUnit Jupiter Engine (JUnit 5 API)
		- API for writing tests using JUnit Jupiter
	- JUnit Vintage Engine (JUnit 4 and 3 API)
		- A thin layer on top of JUnit 4 to allow running vintage tests
- Integration (TestNG, Mock and more)
- To test our code, we need to define a class that is called as test case.
- To define one test case we should use Annotations and Assert API.
- Test cases is executed by JUnit Platform Runtime.

### Example (JUnit 4)
```java
/* library: junit4.12.jar */

/* TestJunit */
public class TestJunit {
 @Test
 public void testAdd() {
		String str = "Junit is working fine";
		assertEquals("Junit is working fine",str);
 }
}

/* TestRunner */
public class TestRunner {
	public static void main(String[] args) {
		Result result = JUnitCore.runClasses(TestJunit.class);
		for (Failure failure : result.getFailures()) {
			 System.out.println(failure.toString());
		}
	
		System.out.println(result.wasSuccessful());
	}
}

```

### Example (JUnit 4)
```java
/* pom.xml */
<dependency>
	<groupId>junit</groupId>
	<artifactId>junit</artifactId>
	<version>4.11</version>
	<scope>test</scope>
</dependency>

/* RssServiceTest */
public class RssServiceTest {

	private RssService rssService;
	private Item item;

	@Before
	public void setUp() throws Exception {
		rssService = new RssService();
	}

	@Test
	public void testGetItemsFile() throws RssException {
		List<Item> items = rssService.getItems(new File("test-rss/javavids.xml"));
		assertEquals(10, items.size());
		Item firstItem = items.get(0);
		// System.out.println(firstItem.getTitle());
		assertEquals("How to solve Source not found error during debug in Eclipse", firstItem.getTitle());	
		
		// <pubDate>Sun, 22 Jun 2014 20:35:49 +0000</pubDate>
		assertEquals("23 06 2014 08:35:49", new SimpleDateFormat("dd MM yyyy HH:mm:ss").format(firstItem.getPublishedDate()));
	}

}

/* RssService */
@Service
public class RssService {
	public List<Item> getItems(File file) throws RssException {
		return getItems(new StreamSource(file));
	}

	public List<Item> getItems(String url) throws RssException {
		return getItems(new StreamSource(url));
	}

	private List<Item> getItems(Source source) throws RssException {
		ArrayList<Item> list = new ArrayList<Item>();
		try {
			JAXBContext jaxbContext = JAXBContext.newInstance(new Class[] { ObjectFactory.class });
			
			// convert XML to bunch of java objects
			Unmarshaller unmarshaller = jaxbContext.createUnmarshaller(); 
			
			// the result of this XML(the root element of the RSS files)
			JAXBElement<TRss> jaxbElement = unmarshaller.unmarshal(source, TRss.class); 
			
			// get RSS object
			TRss rss = (TRss) jaxbElement.getValue();

			List<TRssChannel> channels = rss.getChannel();
			for (TRssChannel channel : channels) {
				List<TRssItem> items = channel.getItem();
				for (TRssItem rssItem : items) {
					Item item = new Item();
					item.setTitle(rssItem.getTitle());
					item.setDescription(rssItem.getDescription());
					item.setLink(rssItem.getLink());
					Date pubDate = new SimpleDateFormat(
							"EEE, dd MMM yyyy HH:mm:ss Z", Locale.ENGLISH)
							.parse(rssItem.getPubDate());
					item.setPublishedDate(pubDate);
					list.add(item);
				}
			}
		} catch (JAXBException e) {
			throw new RssException(e);
		} catch (ParseException e) {
			throw new RssException(e);
		}
		return list;
	}
}
```

### Example (JUnit 5 + Mockito)
- Testing Service Layer Code
```java
/* pom.xml */
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-test</artifactId>
	<scope>test</scope>
</dependency>

<dependency>
	<groupId>org.junit.jupiter</groupId>
	<artifactId>junit-jupiter-engine</artifactId>
	<scope>test</scope>
</dependency>

/* UserServiceImplTest */
class UserServiceImplTest {
	//-- @InjectMocks
	// When a class is created, the framework will inject into the class the mock objects that it requires.

	@InjectMocks
	UserServiceImpl userService;

	@Mock
	UserRepository userRepository;
 
	@Mock
	Utils utils;
	
	@Mock
	AmazonSES amazonSES;

	@Mock
	BCryptPasswordEncoder bCryptPasswordEncoder;
 
	String userId = "hhty57ehfy";
	String encryptedPassword = "74hghd8474jf";
	
	UserEntity userEntity;
 
	@BeforeEach
	void setUp() throws Exception {
		MockitoAnnotations.initMocks(this);
		
		userEntity = new UserEntity();
		userEntity.setId(1L);
		userEntity.setFirstName("Harold");
		userEntity.setLastName("Kim");
		userEntity.setUserId(userId);
		userEntity.setEncryptedPassword(encryptedPassword);
		userEntity.setEmail("test@test.com");
		userEntity.setEmailVerificationToken("7htnfhr758");
		userEntity.setAddresses(getAddressesEntity());
	}

	@Test
	final void testGetUser() {
 		when(userRepository.findByEmail(anyString())).thenReturn(userEntity);
		UserDto userDto = userService.getUser("test@test.com");

		assertNotNull(userDto);
		assertEquals("Harold", userDto.getFirstName());

	}

	//-- JUnit 4
	// @Test(expected=UsernameNotFoundException.class)
	@Test
	final void testGetUser_UsernameNotFoundException() {
		when(userRepository.findByEmail(anyString())).thenReturn(null);

		assertThrows(UsernameNotFoundException.class,
				() -> {
					userService.getUser("test@test.com");
				}
		);
	}
	
	@Test
	final void testCreateUser_CreateUserServiceException()
	{
		when(userRepository.findByEmail(anyString())).thenReturn(userEntity);

		UserDto userDto = new UserDto();
		userDto.setAddresses(getAddressesDto());
		userDto.setFirstName("Harold");
		userDto.setLastName("Kim");
		userDto.setPassword("12345678");
		userDto.setEmail("test@test.com");
 	
		assertThrows(UserServiceException.class,
				() -> {
					userService.createUser(userDto);
				}
		);
	}
	
	@Test
	final void testCreateUser()
	{
		// Mock
		when(userRepository.findByEmail(anyString())).thenReturn(null);
		when(utils.generateAddressId(anyInt())).thenReturn("hgfnghtyrir884");
		when(utils.generateUserId(anyInt())).thenReturn(userId);
		when(bCryptPasswordEncoder.encode(anyString())).thenReturn(encryptedPassword);
		when(userRepository.save(any(UserEntity.class))).thenReturn(userEntity);
	
		// Exclude Integration Code from Unit Test
		Mockito.doNothing().when(amazonSES).verifyEmail(any(UserDto.class));
 		
		UserDto userDto = new UserDto();
		userDto.setAddresses(getAddressesDto());
		userDto.setFirstName("Harold");
		userDto.setLastName("Kim");
		userDto.setPassword("12345678");
		userDto.setEmail("test@test.com");

		UserDto storedUserDetails = userService.createUser(userDto);

		assertNotNull(storedUserDetails);
		assertEquals(userEntity.getFirstName(), storedUserDetails.getFirstName());
		assertEquals(userEntity.getLastName(), storedUserDetails.getLastName());
		assertNotNull(storedUserDetails.getUserId());
		assertEquals(storedUserDetails.getAddresses().size(), userEntity.getAddresses().size());

		verify(utils,times(storedUserDetails.getAddresses().size())).generateAddressId(30);
		verify(bCryptPasswordEncoder, times(1)).encode("12345678");
		verify(userRepository,times(1)).save(any(UserEntity.class));
	}
	
	private List<AddressDTO> getAddressesDto() {
		AddressDTO addressDto = new AddressDTO();
		addressDto.setType("shipping");
		addressDto.setCity("Vancouver");
		addressDto.setCountry("Canada");
		addressDto.setPostalCode("ABC123");
		addressDto.setStreetName("123 Street name");

		AddressDTO billingAddressDto = new AddressDTO();
		billingAddressDto.setType("billling");
		billingAddressDto.setCity("Vancouver");
		billingAddressDto.setCountry("Canada");
		billingAddressDto.setPostalCode("ABC123");
		billingAddressDto.setStreetName("123 Street name");

		List<AddressDTO> addresses = new ArrayList<>();
		addresses.add(addressDto);
		addresses.add(billingAddressDto);

		return addresses;

	}
	
	private List<AddressEntity> getAddressesEntity()
	{
		List<AddressDTO> addresses = getAddressesDto();
		
	    Type listType = new TypeToken<List<AddressEntity>>() {}.getType();
	    
	    return new ModelMapper().map(addresses, listType);
	}
}
```

### Example (JUnit 5 + Mockito)
- Testing Rest Controller
```java
class UserControllerTest {
	@InjectMocks
	UserController userController;
	
	@Mock
	UserServiceImpl userService;
	
	UserDto userDto;
	
	final String USER_ID = "bfhry47fhdjd7463gdh";
	
	@BeforeEach
	void setUp() throws Exception {
		MockitoAnnotations.initMocks(this);
		
		userDto = new UserDto();
        userDto.setFirstName("Harold");
        userDto.setLastName("Kim");
        userDto.setEmail("test@test.com");
        userDto.setEmailVerificationStatus(Boolean.FALSE);
        userDto.setEmailVerificationToken(null);
        userDto.setUserId(USER_ID);
        userDto.setAddresses(getAddressesDto());
        userDto.setEncryptedPassword("xcf58tugh47");
	}

	@Test
	final void testGetUser() {
	    when(userService.getUserByUserId(anyString())).thenReturn(userDto);	
	    
	    UserRest userRest = userController.getUser(USER_ID);
	    
	    assertNotNull(userRest);
	    assertEquals(USER_ID, userRest.getUserId());
	    assertEquals(userDto.getFirstName(), userRest.getFirstName());
	    assertEquals(userDto.getLastName(), userRest.getLastName());
	    assertTrue(userDto.getAddresses().size() == userRest.getAddresses().size());
	}
	
	private List<AddressDTO> getAddressesDto() {
		AddressDTO addressDto = new AddressDTO();
		addressDto.setType("shipping");
		addressDto.setCity("Vancouver");
		addressDto.setCountry("Canada");
		addressDto.setPostalCode("ABC123");
		addressDto.setStreetName("123 Street name");

		AddressDTO billingAddressDto = new AddressDTO();
		billingAddressDto.setType("billling");
		billingAddressDto.setCity("Vancouver");
		billingAddressDto.setCountry("Canada");
		billingAddressDto.setPostalCode("ABC123");
		billingAddressDto.setStreetName("123 Street name");

		List<AddressDTO> addresses = new ArrayList<>();
		addresses.add(addressDto);
		addresses.add(billingAddressDto);

		return addresses;
	}
}
```

### Example (JUnit Integration Test)
- Testing JWT Tokens and UserId.

### References
- [JUnit Tutorial](https://www.tutorialspoint.com/junit/index.htm)
- [Apps Developer Blog](https://www.appsdeveloperblog.com/)
