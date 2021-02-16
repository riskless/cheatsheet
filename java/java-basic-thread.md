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
