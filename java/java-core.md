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
