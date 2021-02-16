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
