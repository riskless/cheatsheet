### Contents
- Stream API
- Function
- Java IO
- Thread

### Stream API
```java
/* filter(), forEach() */ 
List<Integer> list = List.of(1,5,6,3,8,9);

for (Integer i : list) {
	if (i % 2 == 0) {
		System.out.println(i);
	}
}

list.stream()
	.filter(x -> x % 2 == 0) // I1
	.forEach(System.out::println); // ::  x -> System.out.println(x)


/* generate(), iterate() */
public static void main(String[] args) {
	Stream<Integer> s1 = Stream.empty(); // finite

	Stream<Integer> s2 = Stream.of(3,5,6,7,8,9); // finite
	//s2.forEach(System.out::println);

	Supplier<Integer> sup = () -> new Random().nextInt();
	Stream<Integer> s3 = Stream.generate(sup); // inifinite

	// s3.limit(10).forEach(System.out::println);

	Stream<Integer> s4 = Stream.iterate(1, i -> i + 1); // infinite
	// s4.limit(10).forEach(System.out::println);
	
	// // starting with java 9
	Stream<Integer> s5 = Stream.iterate(1, i -> i <= 10, i -> i + 1);  // finite
	s5.forEach(System.out::println);
}

public List<Integer> m1() {
	// this meth does smth
	return Collections.emptyList();
}

public Stream<Integer> m2() {
	// this meth does smth
	return Stream.empty();
}

/* anyMatch(), allMatch(), nonMatch() */
List<Integer> list = List.of(1,5,6,3,8,9);

boolean b1 = list.stream().anyMatch(x -> x % 2 == 0); // true
boolean b2 = list.stream().allMatch(x -> x % 2 == 0); // false
boolean b3 = list.stream().noneMatch(x -> x % 2 == 0); // false

System.out.println(b1);
System.out.println(b2);
System.out.println(b3);

Stream<Integer> s1 = Stream.iterate(1, i -> i + 1); // 1 2 3 4 5 6 ...
Stream<Integer> s2 = Stream.iterate(1, i -> i + 2); // 1 3 5 7 9 11 ...


/* map(), reduce() */
List<String> list = List.of("abcd", "qwerty", "asdfg"); // 15

// java.util.function -> Function<T,R>
// Function<T,T>  --> UnaryOperator<T>
// Function<T,Boolean> --> Predicate<T>

var x = list.stream() // "abcd", "qwerty", "asdfg"
	.map(s -> s.length()) // 4, 6, 5
	.reduce(0, (a,b) -> a+b);
	// 0 + 4 = 4
	// 4 + 6 = 10
	// 10 + 5 = 15
System.out.println(x);

/* map(), forEach() */
List<String> list = List.of("abcd", "qwerty", "asdfg");

list.stream() // "abcd", "qwerty", "asdfg"
	.map(s -> new StringBuilder(s).reverse().toString()) // "dcba", "ytrewq", "gfdsa"
	.forEach(s -> System.out.println(s));


/* map(), mapToInt(), mapToLong(), mapToDouble(), mapToObj() */
List<String> list = List.of("abcd", "qwerty", "asdfg");

// Stream
// map(Function)  Stream -> Stream
// mapToInt(ToIntFunction) Stream -> IntStream
// mapToLong(ToLongFunction) Stream -> LongStream
// mapToDouble(ToDoubleFunction) Stream -> DoubleStream

// IntStream
// map(ToIntFunction)  IntStream -> IntStream
// mapToLong(IntToLongFunction) IntStream -> LongStream
// mapToDouble(IntToDoubleFunction) IntStream -> DoubleStream
// mapToObj(IntFunction) IntStream -> Stream

// LongStream, DoubleStream

var x = list.stream() // "abcd", "qwerty", "asdfg"
	.mapToInt(s -> s.length()) // 4, 6, 5   Stream  -> IntStream
	.mapToObj(s -> s)  // 4, 6, 5 IntStream -> Stream
	.mapToInt(s -> s)  // 4, 6, 5 Stream - IntStream
	.sum();

System.out.println(x);

/* flatMap(), reduce() */
// map  x -> y

// flatMap
// flatMapToInt
// flatMapToLong
// flatMapToDouble

List<List<Integer>> list = List.of(
	List.of(1,2,3,4,5),
	List.of(4,5,6),
	List.of(1,3)
);

var sum1 = list.stream() // List<Integer> [1,2,3,4,5], [4,5,6], [1,3]
	.flatMap(q -> q.stream()) // Integers [1,2,3,4,5,4,5,6,1,3]
	.reduce(0, (a,b) -> a + b);

System.out.println(sum1);


/* flatMap(), filter(), count() */
List<String> list = List.of("ab4n3kdk4", "2n3n2nnm", "mko","102a");

// 9 digits
String digits = "0123456789";

var res = list.stream() // "a4bn3kdk4", "2n3n2nnm", "mko","102a"
	.flatMap(s -> Arrays.stream(s.split("")))  // "a", "b", "4", "n", "3" ...
	.filter(s -> digits.contains(s)) // "4", "3"
	.count(); // long

System.out.println(res);


/* distinct(), sorted(), forEach() */
List<Integer> list = List.of(3,5,2,7,9,8,2,1,5,4);
// distinct()
// sorted() / sorted(Comparator)
list.stream()
	.distinct()
	.sorted() // Comparable
	.forEach(System.out::println);

/* sorted(Comparator) */
List<Integer> list = List.of(3,5,2,7,9,8,2,1,5,4);
// distinct()
// sorted() / sorted(Comparator)
Comparator<Integer> c = Collections.reverseOrder();
list.stream()
	.distinct()
	.sorted(c)
	.forEach(System.out::println);

/* sorted (Object) */
Stream<Cat> s = Stream.of(
	new Cat(3),
	new Cat(1),
	new Cat(5),
	new Cat(4),
	new Cat(2)
);

s.sorted().forEach(c -> System.out.println(c.getAge()));

//-- Cat
public class Cat implements Comparable<Cat> {

	private int age;

	public Cat(int age) {
		this.age = age;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	@Override
	public int compareTo(Cat c) {
		return this.getAge() - c.getAge();
	}
}

/* sorted (Object) */
Stream<Dog> s = Stream.of(
	new Dog(3),
	new Dog(1),
	new Dog(5),
	new Dog(4),
	new Dog(2)
);

Comparator<Dog> comp = (d1,d2) -> d2.getAge() - d1.getAge();
s.sorted(comp).forEach(c -> System.out.println(c.getAge()));

//-- Dog
public class Dog {

	private int age;

	public Dog(int age) {
		this.age = age;
	}

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}
}

/* skip() */
List<Integer> list = List.of(1,2,3,4,5,6,7,8,9,10);
list.stream() // [1,2,3,4,5,6,7,8,9,10]
	.filter(n -> n % 2 == 0) // [2,4,6,8,10]
	.skip(4) // [10]
	.forEach(System.out::println);

/* peek() */
List<Integer> list = List.of(1,2,3,4,5,6,7,8,9,10);
list.stream() // [1,2,3,4,5,6,7,8,9,10]
	.peek(n -> System.out.println(n))  // [1,2,3,4,5,6,7,8,9,10]
	.filter(n -> n % 2 == 0)  //  [2,4,6,8,10]
	.forEach(System.out::println); // [2,4,6,8,10]

List<Integer> input = List.of(1,2,3,4,5,6,7,8,9,10);
List<Integer> output = new ArrayList<>(); // [2,4,6,8,10]
input.stream() // [1,2,3,4,5,6,7,8,9,10]
	 .filter(n -> n % 2 == 0) // [2,4,6,8,10]
	 .peek(n -> output.add(n))   // not recommended
	 .forEach(System.out::println); // collect()  / Collectors

System.out.println(output); // [2,4,6,8,10]


/* collect() */
List<Integer> input = List.of(1,2,3,4,5,6,7,8,9,10);

List<Integer> output = input.stream()
	.filter(n -> n % 2 == 0) // [2,4,6,8,10]
	.collect(Collectors.toList());

System.out.println(output); // [2,4,6,8,10]

/* takeWhile() */
List<Integer> list = List.of(1,2,3,50,70,100,130);

list.stream()
	.takeWhile(n -> n <= 100) // while(condition)
	.forEach(System.out::println);

/* dropWhile() */
List<Integer> list = List.of(1,2,3,50,70,100,130);

list.stream()
	.dropWhile(n -> n <= 100) // [130]
	.forEach(System.out::println);


/* Collectors.toList()  */
List<Integer> res = Stream.of(1,2,3,4,5,1,2) // [1,2,3,4,5]
	.map(n -> 2*n) // [2,4,6,8,10,2,4]
	.collect(Collectors.toList());

System.out.println(res);

/* Collectors.toSet() */
Set<Integer> res = Stream.of(1,2,3,4,5,1,2) // [1,2,3,4,5]
	.map(n -> 2*n) // [2,4,6,8,10,2,4]
	.collect(Collectors.toSet()); // [2,4,6,8,10]

System.out.println(res); // [2,4,6,8,10]

/* intermediary operations */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF");

List<String> res1 = list.stream()
	// intermediary ops
	.collect(Collectors.toList()); // Collectors

Set<String> res2 = list.stream()
	// intermediary ops
	.collect(Collectors.toSet());

TreeSet<String> res3 = list.stream()
	// intermediary ops
	.collect(Collectors.toCollection(TreeSet::new));

/* Collectors.toMap() */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF");

Map<String, Integer> map1 = list.stream()
	.collect(
					Collectors.toMap(
					s -> s,
					s -> s.length())
	);

System.out.println(map1);

/* Collectors.toMap() */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

Map<String, Integer> map1 = list.stream()
	.collect(
					Collectors.toMap(
					 s -> s,
					 s -> s.length(),
					 (a,b) -> a + b
					)
	);

System.out.println(map1); // {AAA=6, B=1, DDD=3, CCCCC=5, FFFFFF=6}


/* */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

TreeMap<String, Integer> map1 = list.stream()
	.collect(
					Collectors.toMap(
					 s -> s,
					 s -> s.length(),
					 (a,b) -> a + b,
					 () -> new TreeMap<>()
					)
	);

System.out.println(map1); // {AAA=6, B=1, CCCCC=5, DDD=3, FFFFFF=6}


/* joining() */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

String res1 =
				list.stream()
				 .collect(Collectors.joining());

System.out.println(res1); // AAABCCCCCDDDFFFFFFAAA

String res2 =
				list.stream()
								.collect(Collectors.joining(","));

System.out.println(res2); // AAA,B,CCCCC,DDD,FFFFFF,AAA

String res3 =
				list.stream()
								.collect(Collectors.joining(",", "[", "]"));

System.out.println(res3); // [AAA,B,CCCCC,DDD,FFFFFF,AAA]

/* */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

var res = list.stream()
	.collect(Collectors.teeing( // jdk12
					Collectors.counting(), // Long
					Collectors.joining(), // String
					(a,b) -> List.of(a,b)
	));

System.out.println(res); // [6, AAABCCCCCDDDFFFFFFAAA]

/* Collectors.mapping() */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

List<Integer> res = list.stream()
.collect(
	Collectors.mapping(s -> s.length(), // string -> int
		Collectors.toList())); // downstreaming

System.out.println(res);  // [3, 1, 5, 3, 6, 3]

/* Collectors.filtering() */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

List<Integer> res1 = list.stream()
	.collect(Collectors.mapping(s -> s.length(),
		Collectors.filtering(n -> n % 2 == 0,
			Collectors.toList())));

System.out.println(res1); // [6]

List<Integer> res2 = list.stream()
	.map(s -> s.length())
	.filter(n -> n % 2 == 0)
	.collect(Collectors.toList());

System.out.println(res2); // [6]



/* Collectors.partitioningBy() */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

Map<Boolean, List<String>> map1 =
	list.stream()
			.collect(
				 Collectors.partitioningBy(s -> s.length() % 2 == 0));

System.out.println(map1); // {false=[AAA, B, CCCCC, DDD, AAA], true=[FFFFFF]}

Map<Boolean, Set<String>> map2 =
	list.stream()
			.collect(
				Collectors.partitioningBy(s -> s.length() % 2 == 0,
								Collectors.toSet()));

System.out.println(map2); // {false=[AAA, B, DDD, CCCCC], true=[FFFFFF]}

Map<Boolean, String> map3 =
	list.stream()
					.collect(
									Collectors.partitioningBy(s -> s.length() % 2 == 0,
													Collectors.joining()));

System.out.println(map3); // {false=AAABCCCCCDDDAAA, true=FFFFFF}

Map<Boolean, String> map4 =
	list.stream()
					.collect(
									Collectors.partitioningBy(s -> s.length() % 2 == 0,
													Collectors.mapping(s -> String.valueOf(s.length()),
																	Collectors.joining())));

System.out.println(map4); // {false=31533, true=6}


/* Collectors.groupingBy() */
List<String> list = List.of("AAA","B","CCCCC","DDD", "FFFFFF", "AAA");

Map<Integer, List<String>> map1 =
	list.stream()
			.collect(
				 Collectors.groupingBy(s -> s.length()));

System.out.println(map1); // {1=[B], 3=[AAA, DDD, AAA], 5=[CCCCC], 6=[FFFFFF]}
// 3 -> ["AAA", "DDD", "AAA"]
// 1 -> ["B"]
// 5 -> ["CCCCC"]
// 6 -> ["FFFFFF"]

Map<Integer, String> map2 = list.stream()
	.collect(
		 Collectors.groupingBy(s -> s.length(),
			 Collectors.mapping(s -> String.valueOf(s.length()),
				 Collectors.joining())));

System.out.println(map2); // {1=1, 3=333, 5=5, 6=6}


/* sum(), summingInt(), summingLong() */
List<String> list = List.of("AAA", "BB", "C", "DDDDDD", "E");

int res1 = list.stream() // ["AAA", "BB", "C", "DDDDDD", "E"]
	.mapToInt(s -> s.length())    // [3,2,1,6,1]
	.sum();
System.out.println(res1); // 13

int res2 = list.stream()
	.collect(Collectors.summingInt(s -> s.length()));

System.out.println(res2); // 13

long res3 = list.stream()
	.collect(Collectors.summingLong(s -> (long) s.length()));

System.out.println(res3); // 13

/* summingInt() */
List<String> list = List.of("AAA", "BB", "C", "DDDDDD", "E");

int res1 = list.stream()
	.collect(Collectors.mapping(s -> s.length(),
		Collectors.summingInt(n -> n)));

// summingInt, summingLong, summingDouble

System.out.println(res1); // 13

/* averagingInt */
List<String> list = List.of("AAA", "BB", "C", "DDDDDD", "E");

double res1 = list.stream()
	.collect(Collectors.averagingInt(s -> s.length()));

// averagingInt, averagingLong, averagingDouble

System.out.println(res1); // 2.6


/* summaryStatistics() */
List<String> list = List.of("AAA", "BB", "C", "DDDDDD", "E");

long res1 = list.stream()
	.mapToInt(s -> s.length())
	.count();

System.out.println(res1); // 5

long res2 = list.stream()
	.mapToInt(s -> s.length())
	.sum();

System.out.println(res2); // 13

var res3 = list.stream()
	.mapToInt(s -> s.length())
	.average();

	
System.out.println(res3); // OptionalDouble[2.6]

IntSummaryStatistics res4 = list.stream()
	.mapToInt(s -> s.length())
	.summaryStatistics();

System.out.println(res4); // IntSummaryStatistics{count=5, sum=13, min=1, average=2.600000, max=6}

/* summarizingInt() */
List<String> list = List.of("AAA", "BB", "C", "DDDDDD", "E");

IntSummaryStatistics res1 = list.stream()
	.mapToInt(s -> s.length())
	.summaryStatistics();

System.out.println(res1); // IntSummaryStatistics{count=5, sum=13, min=1, average=2.600000, max=6}

IntSummaryStatistics res2 = list.stream()
	.collect(Collectors.summarizingInt(s -> s.length()));

System.out.println(res2);

/* NullPointerException */
public static void main(String[] args) {
	// java 8
	String s1 = null;
	int n1 = s1.length(); // :( NullPointerException
}

static Optional<String> m1() {
	// more complex code
	return Optional.empty();  // empty box
}

/* empty(), of(), ofNullable() */
public static void main(String[] args) {
	Optional<Integer> o1 = Optional.empty(); // empty box
	Optional<Integer> o2 = Optional.of(10); // 10 is inside the box here

	//Optional<Integer> o2 = Optional.of(null); // <--- using null with of() throws an exception
	//Optional<Integer> o2 = Optional.of(m()); // throws an exception wherever m() return null

	Optional<Integer> o3 = Optional.ofNullable(10);
	Optional<Integer> o4 = Optional.ofNullable(m());

	boolean b1 = o1.isPresent(); // false
	boolean b2 = o2.isPresent(); // true

	System.out.println(b1);
	System.out.println(b2);
}

static Integer m() {
	return null;
}

/* isPresent() */
public static void main(String[] args) {
	Optional<Integer> o1 = Optional.empty(); // empty box
	Optional<Integer> o2 = Optional.of(10); // 10 is inside the box here

	if (o2.isPresent()) {
		Integer i1 = o2.get(); // make sure the Optional you use isn't empty
	}
}

static Integer m() {
		return null;
}

/* orElse(), orElseGet(), orElseThrow(), ifPresentOrElse() */
public static void main(String[] args) {
	Optional<Integer> o1 = Optional.empty(); // empty box
	Optional<Integer> o2 = Optional.of(10); // 10 is inside the box here

	Integer i1 = o1.orElse(-1); // -1
	Integer i2 = o2.orElse(-1); // 10

	Supplier<Integer> s1 = () -> -1;
	Integer i3 = o1.orElseGet(s1);

	Integer i4 = o2.orElseThrow(() -> new RuntimeException()); // 10

	Integer i5 = o1.or(() -> o2).orElse(-1); // 10  Java 9

	o1.ifPresentOrElse(x -> {
		System.out.println(x);
	}, () -> {
		System.out.println("There's no value!");
	});
}

static Integer m() {
	return null;
}

/* flatMap().orElse() */
public static void main(String[] args) {
	Optional<Integer> o1 = Optional.empty(); // empty box
	Optional<Integer> o2 = Optional.of(10); // 10 is inside the box here

	// map()   x -> y
	// flatMap() x -> Optional(y)

	var res1 = o2.map(x -> 2*x).orElse(-1);
	System.out.println(res1); // 20

	var res2 = o2.flatMap(x -> twice(x)).orElse(-1);
	System.out.println(res2); // 20
}

static Optional<Integer> twice(int x) {
	return Optional.of(2*x);
}

/* ifPresentOrElse() */
public static void main(String[] args) {
	Optional<Integer> o1 = Optional.empty(); // empty box
	Optional<Integer> o2 = Optional.of(10); // 10 is inside the box here

	o2.ifPresent(x -> { // Java 8
		System.out.println(x);
	});

	o2.ifPresentOrElse(x -> { // Java 9

	}, () -> {

	});
}

static Optional<Integer> twice(int x) {
	return Optional.of(2*x);
}

/* Collectors.counting() */
public static void main(String[] args) {
	Optional<Integer> o1 = Optional.empty(); // empty box
	Optional<Integer> o2 = Optional.of(10); // 10 is inside the box here

	var n = o2.stream() // stream of one element
		.collect(Collectors.counting()); // only a stupid example

	var r = o2.filter(x -> x % 2 == 0).orElse(-1);
}

static Optional<Integer> twice(int x) {
	return Optional.of(2*x);
}

/* findFirst() */
// findFirst(); // terminal
// findAny();
// Optional

Stream<Integer> s1 = Stream.empty(); // empty
Stream<Integer> s2 = Stream.of(10,20,50,30);

var o1 = s1.findFirst();

System.out.println(o1.isPresent()); // false

Optional<Integer> o2 = s2.findFirst();

System.out.println(o2.isPresent()); // true

int x = o2.get();
System.out.println(x);

/* min(), max() */
// Comparable and Comparator
// min()
// max()

List<Integer> list1 = List.of(3,1,5,7,2,5,7);

Comparator<Integer> c = (x,y) -> x - y; // int - 0 +
Optional<Integer> o1 = list1.stream().min(c);

o1.ifPresent(System.out::println); // x -> System.out.println(x); // 1

List<Integer> list2 = List.of(3,1,5,7,2,5,7);

Optional<Integer> o2 = list2.stream()
	.max((x,y) -> x - y);

o2.ifPresent(System.out::println); // 7

Optional<Integer> o3 = List.of(1,5,9,7,3).stream() // 1,5,9,7,3
	.filter(x -> x % 2 == 0) // []
	.max((x,y) -> x - y);
System.out.println(o3.isPresent()); // false

/* reduce() */
List<Integer> list1 = List.of();
Stream<Integer> s1 = Stream.of(3,4,5,6,7,8);

int res1 = list1.stream() // there're no elements -> 0
	.reduce(0, (x,y) -> x + y);

Optional<Integer> res2 = list1.stream()
	.reduce((x,y) -> x + y);

System.out.println(res1); // 0
System.out.println(res2.isPresent()); // false

/* primitive stream */
// IntStream, LongStream, DoubleStream -> map(), mapToInt()
// OptionalInt, OptionalLong, OptionalDouble
// average(), min(), max(), reduce()

OptionalDouble res1 = IntStream.of(3,4,5,6,7,8,9)
	.average(); // 2 3 ... 2.5

/* minBy(), maxBy() */
List<Integer> list = List.of();

Optional<Integer> res1 = list.stream()
	.collect(Collectors.minBy((x,y) -> x-y));

System.out.println(res1); // Optional.empty

Optional<Integer> res2 = list.stream()
	.collect(Collectors.maxBy((x,y) -> x-y)); // 3 < 5  3 - 5 --> negative

System.out.println(res2); // Optional.empty

Optional<Integer> res3 = list.stream()
	.collect(Collectors.minBy((x,y) -> y-x)); // max

System.out.println(res3); // Optional.empty

Optional<Integer> res4 =list.stream()
	.collect(Collectors.reducing((a,b) -> a + b));

System.out.println(res4); // Optional.empty

/* Exercise */
//-- Client
import java.util.Optional;

public class Client {

	private String name;
	private String address;
	private Optional email;

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getAddress() {
		return address;
	}

	public void setAddress(String address) {
		this.address = address;
	}

	public Optional getEmail() {
		return email;
	}

	public void setEmail(Optional email) {
		this.email = email;
	}

	@Override
	public String toString() {
		return "Client{" +
						"name='" + name + '\'' +
						", address='" + address + '\'' +
						", email=" + email +
						'}';
	}
}

//-- 
import java.time.LocalDate;

public class Ticket {

	private String departure;
	private String destination;
	private LocalDate date; // java.time
	private Client client;
	private double price;

	public String getDeparture() {
		return departure;
	}

	public void setDeparture(String departure) {
		this.departure = departure;
	}

	public String getDestination() {
		return destination;
	}

	public void setDestination(String destination) {
		this.destination = destination;
	}

	public LocalDate getDate() {
		return date;
	}

	public void setDate(LocalDate date) {
		this.date = date;
	}

	public Client getClient() {
		return client;
	}

	public void setClient(Client client) {
		this.client = client;
	}

	public double getPrice() {
		return price;
	}

	public void setPrice(double price) {
		this.price = price;
	}

	@Override
	public String toString() {
		return "Ticket{" +
						"departure='" + departure + '\'' +
						", destination='" + destination + '\'' +
						", date=" + date +
						", client=" + client +
						", price=" + price +
						'}';
	}
}

//-- Test1
public static void main(String[] args) {
	Client c1 = new Client();

	Ticket t1 = new Ticket();
	t1.setDestination("Paris");

	Ticket t2 = new Ticket();
	t2.setDestination("Paris");

	Ticket t3 = new Ticket();
	t3.setDestination("London");

	String dest = "Paris";

	List<Ticket> list = List.of(t1,t2,t3);

	long res1 = list.stream()
		.filter(t -> t.getDestination().equals(dest))
		.count();

	System.out.println(res1); // 2
} 

//-- Test2
public static void main(String[] args) {
	Client c1 = new Client();

	Ticket t1 = new Ticket();
	t1.setDate(LocalDate.of(2020, 5, 2));

	Ticket t2 = new Ticket();
	t2.setDate(LocalDate.of(2020, 5, 2));

	Ticket t3 = new Ticket();
	t3.setDate(LocalDate.of(2020, 6, 1));

	LocalDate date = LocalDate.of(2020, 5, 2);

	List<Ticket> list = List.of(t1,t2,t3);

	list.stream()
		.filter(t -> t.getDate().isEqual(date))
		.forEach(System.out::println);
	// Ticket{departure='null', destination='null', date=2020-05-02, client=null, price=0.0}
	// Ticket{departure='null', destination='null', date=2020-05-02, client=null, price=0.0}

}

//-- Test3
public static void main(String[] args) {
	Client c1 = new Client();
	c1.setName("Bob");

	Client c2 = new Client();
	c2.setName("John");

	Ticket t1 = new Ticket();
	t1.setClient(c1);

	Ticket t2 = new Ticket();
	t2.setClient(c1);

	Ticket t3 = new Ticket();
	t3.setClient(c2);

	String name = "Mary";

	List<Ticket> list = List.of(t1,t2,t3);

	boolean b = list.stream()
		.map(t -> t.getClient().getName())
		.anyMatch(n -> n.equals(name));
	System.out.println(b); // false
}

//-- Test4
public static void main(String[] args) {
	Client c1 = new Client();
	c1.setName("Bob");

	Client c2 = new Client();
	c2.setName("John");

	Ticket t1 = new Ticket();
	t1.setClient(c1);
	t1.setPrice(10);

	Ticket t2 = new Ticket();
	t2.setClient(c1);
	t2.setPrice(10);

	Ticket t3 = new Ticket();
	t3.setClient(c2);
	t3.setPrice(20);


	List<Ticket> list = List.of(t1,t2,t3);

	OptionalDouble res1 = list.stream()
		.mapToDouble(t -> t.getPrice())
		.average();

	System.out.println(res1); // OptionalDouble[13.333333333333334]

	DoubleSummaryStatistics s = list.stream()
		.mapToDouble(t -> t.getPrice())
		.summaryStatistics();

	System.out.println(s.getAverage()); // 13.333333333333334

	Double res3 = list.stream()
		.collect(Collectors.averagingDouble(t -> t.getPrice()));

	System.out.println(res3); // 13.333333333333334
}

//-- Test5
public static void main(String[] args) {
	Client c1 = new Client();
	c1.setName("Bob");
	c1.setEmail(Optional.of("bob@bob.com"));

	Client c2 = new Client();
	c2.setName("John");
	c2.setEmail(Optional.of("john@john.com"));

	Ticket t1 = new Ticket();
	t1.setClient(c1);
	t1.setPrice(10);

	Ticket t2 = new Ticket();
	t2.setClient(c1);
	t2.setPrice(10);

	Ticket t3 = new Ticket();
	t3.setClient(c2);
	t3.setPrice(20);


	List<Ticket> list = List.of(t1,t2,t3);

	boolean res = list.stream()
		.map(t -> t.getClient())
		.allMatch(c -> c.getEmail().isPresent());

	System.out.println(res); // true
}

//-- Test6
public static void main(String[] args) {
	Client c1 = new Client();
	c1.setName("Bob");
	c1.setEmail(Optional.of("bob@bob.com"));

	Client c2 = new Client();
	c2.setName("John");
	c2.setEmail(Optional.of("john@john.com"));

	Ticket t1 = new Ticket();
	t1.setClient(c1);
	t1.setPrice(10);
	t1.setDestination("Paris");

	Ticket t2 = new Ticket();
	t2.setClient(c1);
	t2.setPrice(10);
	t2.setDestination("Paris");

	Ticket t3 = new Ticket();
	t3.setClient(c2);
	t3.setPrice(20);
	t3.setDestination("London");

	List<Ticket> list = List.of(t1,t2,t3);

	String res = list.stream()
		.map(t -> t.getDestination())
		.distinct()
		.collect(Collectors.joining(","));

	System.out.println(res); // Paris,London

}
```

### Function
```java
/* FunctionInterface */
@FunctionalInterface
public interface Eatable {
	void eat();
}

/* Test */
public static void main(String[] args) {
	Eatable e1 = new Eatable() {
		@Override
		public void eat() {
			System.out.println("hap hap!");
		}
	};

	Eatable e2 = () -> System.out.println("hap hap");
}

/* Consumer, BiConsumer */
Consumer<String> c1 = s -> System.out.println(s);
c1.accept("Hello!"); // Hello!

BiConsumer<Integer, String> c2 = (a,b) -> System.out.println(a + " " + b);
c2.accept(10, ":)");

//-- foreach
// List<Integer> list = List.of(10,20,30,40);
// Consumer<Integer> c = x -> System.out.println(x);
// list.forEach(c);

Map<Integer, String> map = Map.of(
	10, "A",
	20, "B",
	30, "C"
);

BiConsumer<Integer, String> c2 = (k, v) -> System.out.println(k + " " +v);
map.forEach(c2);

/* Supplier */
Random r = new Random();
Supplier<Integer> s = () -> r.nextInt(1000); // [0, 999]
Integer x = s.get();
System.out.println(x);

/* Function */
Function<String, Integer> f1 = s -> s.length();

int x = f1.apply("HELLO!"); // 6

System.out.println(x);


/* BiFunction */
BiFunction<Integer, Integer, String> f = (a,b) -> a + "" + b;

String res = f.apply(5, 9); // 59

System.out.println(res);

/* Predicate */ 
Predicate<Integer> p1 = x -> x % 2 == 0;

boolean b1 = p1.test(10); // true
boolean b2 = p1.test(5); // false

System.out.println(b1);
System.out.println(b2);

/* BiPredicate */
BiPredicate<String, Integer> p = (s, i) -> s.length() == i;

boolean b1 = p.test("HELLO", 5); // true
boolean b2 = p.test("HELLO", 7); // false

System.out.println(b1);
System.out.println(b2);

/* UnaryOperator */
UnaryOperator<Integer> u1 = n -> n + 1;

int res1 = u1.apply(10); // 11

UnaryOperator<String> u2 = n -> new StringBuilder(n).reverse().toString();

String res2 = u2.apply("HELLO"); // OLLEH

System.out.println(res1);
System.out.println(res2);

/* BinaryOperator */
BinaryOperator<Integer> b1 = (a,b) -> a + b;
int res = b1.apply(10, 30); // 40
System.out.println(res);
```

### Java IO
- FileInputStream/FileOutputStream
```java
// byte stream
FileInputStream in = new FileInputStream("C:\\javaio\\test.txt");
FileOutputStream out = new FileOutputStream("C:\\javaio\\test_new.txt");
int i = 0;
while((i = in.read())!= -1){
	out.write(i);
	System.out.println(i + ":" + (char)i);
}
```


- InputStreamReader/OutputStreamWriter
```java
// character stream
FileInputStream in = new FileInputStream("C:\\javaio\\test.txt");
InputStreamReader isr = new InputStreamReader(in,"UTF-8");

FileOutputStream out = new FileOutputStream("C:\\javaio\\test_new.txt");
OutputStreamWriter osw = new OutputStreamWriter(out,"UTF-8");

int i = 0;
while((i = isr.read())!= -1){
	osw.write(i);
}
osw.flush();
```

- FileReader/FileWriter
```java
// use the encoding of OS by default
FileReader fr = new FileReader("C:\\javaio\\test.txt");
FileWriter fw = new FileWriter("C:\\javaio\\test_new.txt");

int i = 0;
while((i = fr.read())!= -1){
	fw.write(i);
}
fw.flush();
```

- BufferReader/BufferWriter
```java
// try-catch-finally
FileInputStream in = new FileInputStream("C:\\javaio\\test.txt");
InputStreamReader isr = new InputStreamReader(in);
BufferReader br = new BufferReader(isr, 1024*8);

FileOutputStream out = new FileOutputStream("C:\\javaio\\test_new.txt");
OutputStreamWriter osw = new OutputStreamWriter(out);
BufferWriter bw = new BufferWriter(osw);

String line;
while((line = br.readLine()) != null){
	//System.out.println(line);
	bw.write(line);
	bw.newLine();
}

if(br != null) br.close();
if(bw != null) bw.close(); // including bw.flush()
```

- ByteArrayInputStream
```java
byte[] byteArySource = { 65, 66, 87, 37 };

ByteArrayInputStream bais = new ByteArrayInputStream(byteArySource);

int i = 0;
while ((i = bais.read()) != -1) {
	System.out.print((char) i);
}
```

- ByteArrayOutputStream
```java
byte[] byteAryDestination = {};

String s = "Apple";
byte[] byteArySource = s.getBytes();

ByteArrayInputStream bais = new ByteArrayInputStream(byteArySource);
ByteArrayOutputStream out = new ByteArrayOutputStream();

int i = 0;

while ((i = bais.read()) != -1) {
	out.write(i);
}

byteAryDestination = out.toByteArray();

for (int j = 0; j < byteAryDestination.length; j++) System.out.print((char) byteAryDestination[j]);
```

- CharArrayReader
```java
char[] charAry = { 'a', 'p', 'p', 'l', 'e' };

CharArrayReader car = new CharArrayReader(charAry);

int i = 0;

while ((i = car.read()) != -1) {
	System.out.print((char) i);
}
```


- ObjectOutputStream
```java
// public class DogObject implements Serializable{}
DogObject dogObj = new DogObject("Brown");
FileOutputStream fos = new FileOutputStream("C:\\javaio\\object.txt");
ObjectOutputStream oos = new ObjectOutputStream(fos);

oos.writeObject(dogObj);
oos.close();
```
- ObjectInputtStreamExample
```java
DogObject dogObj = null;
FileInputStream fis = new FileInputStream("C:\\javaio\\object.txt");
ObjectInputStream ois = new ObjectInputStream(fis);

dogObj = (DogObject)ois.readObject();

dogObj.bark();
System.out.println(dogObj.getColor());
```

- File
```java
File newDir = new File("C:\\javaio\\subDirectory");

if(!newDir.exists()){
	newDir.mkdirs();
	System.out.println("File Created : "+newDir.getName());
	System.out.println("At : "+newDir.getAbsolutePath());
}
```

### Thread
- ReenterantLock
```java
Lock lock = new ReentrantLock();
public void display() {
	lock.lock();
	// code
	lock.unlock();
}
```
- synchronized
```java
// synchronized
public synchronized void display() {
	// code
}

// static synchronized
public static synchronized void display() {
	// code
}

// synchronized(this) {}
// synchronized(obj) {}
// synchronized(*.class) {}
```
- volatile
	- If the variable is volatile, both read/write operations happen directly on memory, not on local cache.

- wait(), notify(), notifyAll()
```java
// Thread 1
synchronized (obj) {
	obj.wait();
}

// Thread 2
synchronized (obj) {
	obj.wait();
}

// Thread 3
synchronized (obj) {
	obj.notify();
	//obj.notifyAll();
}
```
- ThreadGroup: activeCount(), list()

- DeadLock

- Thread Pool
```java
List<Runnable> tasks = new ArrayList<>();
tasks.add(() -> System.out.println("Task1 executed.."));
tasks.add(() -> System.out.println("Task2 executed.."));
tasks.add(() -> System.out.println("Task3 executed.."));	
for(Runnable task : tasks) {
	new Thread(task).start();
}

// stream
tasks.stream().forEach(task -> threadPool.execute(task));


// Thread Pool
ExecutorService threadPool = Executors.newFixedThreadPool(5);
tasks.stream().forEach(task -> threadPool.execute(task));		
threadPool.shutdown();
```
- Callable
```java
ExecutorService execService = Executors.newFixedThreadPool(5);
List<Future<Integer>> futureList = new ArrayList<Future<Integer>>();

// return value unlike Runnable
Callable task = () -> {
	Integer x = 10;
	Integer y = 20;
	return x + y;
};

for (int i = 0; i < 10; i++) {
	Future<Integer> future = execService.submit(task);
	futureList.add(future);
}

for (Future<Integer> future : futureList) {
	try {
		System.out.println(future.get());
	} catch (InterruptedException e) {
		e.printStackTrace();
	} catch (ExecutionException e) {
		e.printStackTrace();
	}
}

execService.shutdown();
```
- Semaphore
```java
static Semaphore semaphore = new Semaphore(2);

public static void main(String[] args) {
	new Thread(() -> method(), "T1").start();
	new Thread(() -> method(), "T2").start();
	new Thread(() -> method(), "T3").start();
}

static void method() {
	try {
		semaphore.acquire();
		// Some code here
		System.out.println("Current Thread : " + Thread.currentThread().getName());
		Thread.sleep(5000);
		semaphore.release();
	} catch (InterruptedException e) {
		e.printStackTrace();
	}
}
```

- BlockingQueue
```java
// produce & consume
static BlockingQueue<Integer> bQueue = new ArrayBlockingQueue<Integer>(3);
static int itemCount = 10;
public static void main(String[] args) {
	new Thread(() -> produce()).start();
	new Thread(() -> consume()).start();
}

static void produce() {
	try {
		for (int i = 0; i <= itemCount; i++) {
			bQueue.put(i);
			System.out.println("Put : " + i);
		}
	} catch (InterruptedException e) {
		e.printStackTrace();
	}
}

static void consume() {
	try {
		Integer item;
		while (!((item = bQueue.take()) == itemCount)) {
			Thread.sleep(1000);
			System.out.println(item);
		}
	} catch (InterruptedException e) {
		e.printStackTrace();
	}
}
```

### References
- [Java Programming Masterclass](https://www.udemy.com/course/java-the-complete-java-developer-course/)
- [Java Fundamentals](https://www.youtube.com/watch?v=on2Kdqe6tVU&list=PLEocw3gLFc8WMfp7fGqvWkQnBwC__Dv4K&index=1)
