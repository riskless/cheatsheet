### Networking
- LAN (Local Area Network) : Hub, Swtich
- WAN (Wide Area Network) : Router
- Packet
- Protocol
	- TCP (Transmission Control Protocol) / UDP (User Datagram Protocol)
	- IP (Internet Protocol)
	- HTTP (Hypertext Transfer Protocol)
	- FTP (File Transfer Protocol)
	- SMTP (Simple Mail Transfer Protocol)
- OSI Model (Open Systems Interconnection Model) : Application, Presentation, Session, Transport, Network, Data Link, Physical

- ServerSocket (Server)
```java
/*** Simple Server ***/
ServerSocket server = new ServerSocket(8798); // Listen to port
Socket serverClientSocket = server.accept();

System.out.println("Server started..");

DataInputStream inStream = new DataInputStream(serverClientSocket.getInputStream());
DataOutputStream outStream = new DataOutputStream(serverClientSocket.getOutputStream());

outStream.writeUTF("Echo from server : " + inStream.readUTF());
outStream.flush();

inStream.close();
outStream.close();
serverClientSocket.close();

/*** Echo Server ***/
ServerSocket server = new ServerSocket(8698);
Socket serverClientSocket = server.accept();

DataInputStream inStream = new DataInputStream(serverClientSocket.getInputStream());
DataOutputStream outStream = new DataOutputStream(serverClientSocket.getOutputStream());

String clientMessage = "";

while(!clientMessage.equals("end")) {
	clientMessage = inStream.readUTF();
	outStream.writeUTF("Echo from server : " + clientMessage);
	outStream.flush();
}
```

- Socket (Client)
```java
/*** Simple Client ***/
// Create connection to server socket
Socket socket = new Socket("localhost", 8798);

// Create streams to read/write data
DataInputStream inStream = new DataInputStream(socket.getInputStream());
DataOutputStream outStream = new DataOutputStream(socket.getOutputStream());

Scanner scanner = new Scanner(System.in);
String clientMessage = "";
String serverMessage = "";

// Prompt user to enter some number or 'end'
System.out.println("Enter something :");
clientMessage = scanner.nextLine();

// Send the entered number to server
outStream.writeUTF(clientMessage);
outStream.flush();

// Read data from socket input stream
serverMessage = inStream.readUTF();
System.out.println(serverMessage);

// Close resources
outStream.close();
outStream.close();
socket.close();

/*** Echo Client ***/
// Create connection to server socket
Socket socket = new Socket("localhost", 8698);

// Create streams to read/write data
DataInputStream inStream = new DataInputStream(socket.getInputStream());
DataOutputStream outStream = new DataOutputStream(socket.getOutputStream());

Scanner scanner = new Scanner(System.in);
String clientMessage = "";
String serverMessage = "";

System.out.println("Prime number checker!");

while (!clientMessage.equals("end")) {

	// Prompt user to enter some number or 'end'
	System.out.println("Enter something : ");
	clientMessage = scanner.nextLine();

	// Send the entered number to server
	outStream.writeUTF(clientMessage);
	outStream.flush();

	// Read data from socket input stream
	serverMessage = inStream.readUTF();
	System.out.println(serverMessage);
}

// Close resources
outStream.close();
outStream.close();
socket.close();
```
- DatagramSocket
```java
/*** Receiver ***/
//Opens a datagram socket on the specified port
DatagramSocket ds = new DatagramSocket(5000);


byte[] buf = new byte[1024];

//Constructs a datagram packet for receiving the packets of specified length
DatagramPacket dp = new DatagramPacket(buf, 1024);

ds.receive(dp);
String str = new String(dp.getData(), 0, dp.getLength());

System.out.println(str);

//Closing the Datagram socket
ds.close();

/*** Sender ***/
// Datagram socket that binds to any available port in localhost
DatagramSocket ds = new DatagramSocket();

String message = "Hello Message using UDP";
InetAddress ip = InetAddress.getByName("localhost");

//Create Datagram packet and send
DatagramPacket dp = new DatagramPacket(message.getBytes(), message.length(), ip, 5000);
ds.send(dp);

//Close the socket
ds.close();
```

- MulticastSocket
```java
/*** Multicasting Receriver ***/
String group = args[0];

//Opens a Multicast socket on the specified port
MulticastSocket ms = new MulticastSocket(5000);
ms.joinGroup(InetAddress.getByName(group));

byte[] buf = new byte[1024];

//Constructs a datagram packet for receiving the packets of specified length
DatagramPacket dp = new DatagramPacket(buf, 1024);

ms.receive(dp);
String str = new String(dp.getData(), 0, dp.getLength());

System.out.println(str);

//leave the group
ms.leaveGroup(InetAddress.getByName(group));
//Closing the Datagram socket
ms.close();	


/*** Multicasting Sender ***/
//Pick any multicast address within the range 224.0.0.0 to 239.255.255.255
String group = "226.4.5.6";

// Multicast socket that binds to any available port in localhost
MulticastSocket ms = new MulticastSocket();

String message = "Hello Message using Multicast";

// Create Datagram packet and send
DatagramPacket dp = new DatagramPacket(message.getBytes(), message.length(), InetAddress.getByName(group), 	5000);

ms.send(dp);

// Close the socket
ms.close();
```

- URL
```java
// Parse through the URL Details
URL URL = new URL("http://howtostudyit.com:80/docs/books" + "/index.html?name=networking#sometext");
System.out.println("protocol = " + URL.getProtocol());
System.out.println("authority = " + URL.getAuthority());
System.out.println("host = " + URL.getHost());
System.out.println("port = " + URL.getPort());
System.out.println("path = " + URL.getPath());
System.out.println("query = " + URL.getQuery());
System.out.println("filename = " + URL.getFile());
System.out.println("ref = " + URL.getRef());

// URLConnection
URL oracle = new URL("http://how-to-host-a-website.com/");
URLConnection yc = oracle.openConnection();
BufferedReader in = new BufferedReader(new InputStreamReader(yc.getInputStream()));
String inputLine;
while ((inputLine = in.readLine()) != null) { 
	System.out.println(inputLine); 
}
in.close();
}
```
