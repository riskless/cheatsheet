
### List
```java
/* Arrays.asList */
public static void main(String[] args) {
	// java.util
	// Collection<E> --- Map<K, V>

	// List<E>, Set<E>, Deque<E>, Queue<E>

	// List<E>  ---> ArrayList<E>, LinkedList<E>
	// Set<E>  ---> HashSet<E>, LinkedHashSet<E>, TreeSet<E>
	// Deque<E> ---> ArrayDeque<E>
	// Map<K, V> ---> HashMap<K,V>, LinkedHashMap<K,V>, TreeMap<K,V>

	List<String> list1 = Arrays.asList("ABC", "QWE", "AAA");
	List<String> list2 = List.of("ABC", "QWE", "AAA"); // OCP 11

	List<String> list3 = new ArrayList<>();
	List<String> list4 = new LinkedList<>();

	Collection<String> c1 = Arrays.asList("ABC", "QWE", "AAA");

	System.out.println(list1); // [ABC, QWE, AAA]

	String [] a1 = {"ABC", "QWE", "AAA"};

	System.out.println(a1); // [Ljava.lang.String;@36baf30c
}

/* size(), add()*/
List<Integer> list = new ArrayList<>();
System.out.println(list.size()); // 0

list.add(10); // 0
list.add(20); // 1

System.out.println(list.size()); // 2

list.add(34); // 2
list.add(1);  // 3

System.out.println(list.size()); // 4

list.add(20); // 4
list.add(20); // 5

System.out.println(list); // [10, 20, 34, 1, 20 ,20]

/* get() */
List<Integer> list = new ArrayList<>();

list.add(10); // 0
list.add(20); // 1
list.add(34); // 2
list.add(1);  // 3
list.add(20); // 4
list.add(20); // 5

Integer x1 = list.get(3); // 1
System.out.println(x1); // 1

Integer x2 = list.get(5); // 20
System.out.println(x2); // 20

// Integer x3 = list.get(6); // IndexOutOfBoundsException
// System.out.println(x2);

/* loop (for) */
List<Integer> list = new ArrayList<>();

list.add(10); // 0
list.add(20); // 1
list.add(34); // 2
list.add(1);  // 3
list.add(20); // 4
list.add(20); // 5

for (int i=0; i<list.size(); i++) {
	Integer x = list.get(i);
	System.out.println(x);
}

/* loop (for) */
List<Integer> list = new ArrayList<>();

list.add(10); // 0
list.add(20); // 1
list.add(34); // 2
list.add(1);  // 3
list.add(20); // 4
list.add(20); // 5

for (Integer x: list) {
	System.out.println(x);
}

/* loop (while) */
List<Integer> list = new ArrayList<>();

list.add(10); // 0
list.add(20); // 1
list.add(34); // 2
list.add(1);  // 3
list.add(20); // 4
list.add(20); // 5

Iterator<Integer> i = list.iterator();

while (i.hasNext()) {
	Integer x = i.next(); // moves the iterator to the next value and returns it
	System.out.println(x);
}

/* forEach */
List<Integer> list = new ArrayList<>();

list.add(10); // 0
list.add(20); // 1
list.add(34); // 2
list.add(1);  // 3
list.add(20); // 4
list.add(20); // 5

//list.forEach(x -> System.out.println(x));
list.forEach(System.out::println);

/* remove */
List<Integer> list = new ArrayList<>();

list.add(10); // 0
list.add(20); // 1
list.add(34); // 2
list.add(1);  // 3
list.add(20); // 4
list.add(20); // 5

list.remove((Integer) 20); // 20

System.out.println(list); // [10,34,1,20,20]
```
### Set
```java
/* HashSet, LinkedHashSet, TreeSet */
Set<Integer> set1 = Set.of(10, 20 ,50);
Set<Integer> set2 = new HashSet<>(); // not order and not sorted
Set<Integer> set3 = new LinkedHashSet<>(); // ordered
Set<Integer> set4 = new TreeSet<>(); // sorted

Set<Integer> set5 = Arrays.asList(10,20,30)
	.stream().collect(Collectors.toSet());

// list                vs           set (Set) / SortedSet / NavigableSet
// orders                           not (always) ordered
// indexed                          not indexed
// do allow duplicated              doesn't allow duplicates

/* loop (for) */
Set<Integer> set1 = Set.of(10, 1000, 23, 300, 256);

for (Integer x : set1) {
	System.out.println(x);
}

/* HashSet */
Set<Integer> set = new HashSet<>(); // uses hashCode() / equals()
set.add(10);
set.add(1000);
set.add(23);
set.add(1000); // this one isn't added to the collection
set.add(300);
set.add(256);

set.forEach(x -> System.out.println(x)); // System.out::println

/* LinkedHashSet */
Set<Integer> set = new LinkedHashSet<>(); // ordered
set.add(10);
set.add(1000);
set.add(23);
set.add(1000);
set.add(300);
set.add(256);

set.forEach(x -> System.out.println(x));

/* TreeSet */
NavigableSet<Integer> set = new TreeSet<>(); // ordered
set.add(10);
set.add(1000);
set.add(23);
set.add(1000);
set.add(300);
set.add(256);

set.forEach(x -> System.out.println(x));

/* SortedSet*/
public static void main(String[] args) {
	SortedSet<Integer> set = getSomeSet();
	// no I can assume that elements are sorted
}

static SortedSet<Integer> getSomeSet() {
	return new TreeSet<>(); // sorted
}
```


### Comparable & Comparator
```java
/* example */
//-- Test
var set1 = new TreeSet<>();
set1.add(new Cat(4));
set1.add(new Cat(1));
set1.add(new Cat(6));
set1.add(new Cat(2));

set1.forEach(System.out::println);

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
	public int compareTo(Cat o) {
		// <0 if this < o
		// 0 if this == o
		// >0 if this > o

		return o.age - this.age;
	}

	@Override
	public boolean equals(Object o) {
		if (this == o) return true;
		if (o == null || getClass() != o.getClass()) return false;
		Cat cat = (Cat) o;
		return age == cat.age;
	}

	@Override
	public int hashCode() {
		return Objects.hash(age);
	}

	@Override
	public String toString() {
		return "Cat{" +
						"age=" + age +
						'}';
	}
}

/* example */
//-- Test 
/*
Comparable<Dog>  ->  int compareTo(Dog d)
Comparator<Dog>  ->  int compare(Dog d1, Dog d2)
 */

Comparator<Dog> c1 = (d1,d2) -> d1.getAge() - d2.getAge();
Comparator<Dog> c2 = (d1,d2) -> d1.getName().compareTo(d2.getName());

var set = new TreeSet<Dog>(c2);

set.add(new Dog("A", 3));
set.add(new Dog("D", 2));
set.add(new Dog("C", 1));
set.add(new Dog("F", 6));

set.forEach(System.out::println);

// Collections vs Collection
// Collection.sort() // does not compile!!!
// Executor vs Executors
// File vs Files

//-- Dog
public class Dog {
	private String name;
	private int age;

	public Dog(String name, int age) {
			this.name = name;
			this.age = age;
	}

	public String getName() {
			return name;
	}

	public void setName(String name) {
			this.name = name;
	}

	public int getAge() {
			return age;
	}

	public void setAge(int age) {
			this.age = age;
	}

	@Override
	public String toString() {
			return "Dog{" +
							"name='" + name + '\'' +
							", age=" + age +
							'}';
	}
}
```

### Deque
```java
/* push(), addFirst(), addLast(), offer() */
Deque<Integer> d1 = new ArrayDeque<>();
var d2 = new ArrayDeque<>();
ArrayDeque<Integer> d3 = new ArrayDeque<>();

// stacks and queues

// adding an element at the beginning
// adding and element at the end
// retrieve an element from the beginning
// retrieve an element from the end

// stack ... LIFO
// stacks have layers

//        d1.push(3);
//        d1.push(8);
//        d1.push(9);
//        Integer e1 = d1.pop();

//        d1.addFirst(3);
//        d1.addFirst(8);
//        d1.addFirst(9);
//        d1.removeFirst();
// 9 8 3

//        d1.addLast(10);
//        d1.addLast(78);
//        d1.addLast(50);

//        d1.offer(10);
//        d1.offer(78);
//        d1.offer(50);

// 10 78 50
System.out.println(d1); // any collection in the collections fw implements a toStirng()
//        d1.forEach(System.out::println);

/* queue */
// Deque -> stack -> LIFO
// Deque -> queue -> FIFO

var d1 = new ArrayDeque<>(); // like ArrayList, ArrayDeque is also backed by an array

// queue

//        d1.offer(10);
//        d1.offer(20);
//        d1.offer(30);
//
//        d1.pop();

//        d1.push(10);
//        d1.push(20);
//        d1.removeLast();
```

### Map
```java
/* LinkedHashMap */
Map<Integer, String> m1 = new LinkedHashMap<>();
// keys are unique

m1.put(10, "ABC");
m1.put(20, "QWE");
m1.put(10, "WWW");
m1.put(30, "WWW");

// [10=>WWW, 20=>QWE, 30=>WWW]

//        m1.forEach((k,v) -> System.out.println(k + " " + v));
System.out.println(m1); // is does implement a toString() method

/* TreeMap */
Map<Integer, String> m1 = new TreeMap<>(
				Collections.reverseOrder());
// keys are unique

m1.put(10, "ABC");
m1.put(20, "QWE");
m1.put(10, "WWW");
m1.put(30, "WWW");

// [10=>WWW, 20=>QWE, 30=>WWW]

//        m1.forEach((k,v) -> System.out.println(k + " " + v));
System.out.println(m1); // is does implement a toString() method

/* Object */
//-- Test
var m1 = new TreeMap<Cat, String>();

m1.put(new Cat(10), "Tom");
m1.put(new Cat(15), "Leo");

System.out.println(m1);

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
	public boolean equals(Object o) {
			if (this == o) return true;
			if (o == null || getClass() != o.getClass()) return false;
			Cat cat = (Cat) o;
			return age == cat.age;
	}

	@Override
	public int hashCode() {
			return Objects.hash(age);
	}

	@Override
	public int compareTo(Cat o) {
			return this.age - o.age;
	}
}

/* Map vs Set */
// collection of associations (pairs)
// (key, value)

Map<Integer, String> m1 = new HashMap<>(); // (k,v) doesn't guarantee an order
Map<Integer, String> m2 = new LinkedHashMap<>(); // (k,v) in the order in which you've added them
Map<Integer, String> m3 = new TreeMap<>(); // (k,v) are sorted by their keys

Set<Integer> s1 = new HashSet<>(); // doesn't guarantee an order
Set<Integer> s2 = new LinkedHashSet<>(); // keeps the elements in the order in which you've added them
Set<Integer> s3 = new TreeSet<>(); // sorts the elements by a given rule (Comparable / Comparator)
```
