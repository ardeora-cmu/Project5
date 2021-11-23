```

// The following illustrates poor documentation.
// See below for a better version.
//***********************************************************
import java.net.*;
import java.io.*;

public class UDPClient {

public static void main(String args[]) {

if(args.length != 3) {

    System.out.println("usage:java UDPClient operand operator operand");
    System.exit(0);
}

DatagramSocket aSocket = null;

try {
    aSocket = new DatagramSocket();

    byte m[] = (args[0] +" " + args[1] + " " + args[2]).getBytes();

    InetAddress aHost = InetAddress.getByName("localhost");
    int serverPort = 6502;

    DatagramPacket request = new DatagramPacket(
    m, m.length, aHost, serverPort);

    aSocket.send(request);

    byte buffer[] = new byte[40];

    DatagramPacket reply = new DatagramPacket(buffer,buffer.length);
    aSocket.receive(reply);

    System.out.println(new String(reply.getData()));

}
catch(SocketException e) {
    System.out.println("Socket: " + e.getMessage());
}
catch(IOException e) {
    System.out.println("IO: " + e.getMessage());
}
finally {
    if(aSocket != null) aSocket.close();
    }
  }
}

//****************************************************************
// A better version.


/**
* Author: Michael McCarthy
* Last Modified: May 27, 2009
*
* This program demonstrates a very simple UDP client.
* The command line contains two operands and an operator.
* The command line is packaged and placed in a single UDP packet.
* The packet is sent to the server asynchronously.
* The program then blocks waiting for the server to perform
* the requested operation. When the response packet arrives,
* a String object is created and the reply is displayed.
* The program illustrates the marshaling and un-marshaling
* of requests and replies. It also demonstrates that UDP
* style sockets are quite different from TCP style sockets.
*/

// imports required for UDP/IP

import java.net.*;
import java.io.*;

public class UDPClient {

/**
* Command line arguments.
*
* @param args[0] operand (double)
* @param args[1] operator (+, *, -, /)
* @param args[2] operand (double)
*/

public static void main(String args[]) {

    if(args.length != 3) {

    System.out.println("usage:java UDPClient operand operator operand");
    System.exit(0);
}

// define a Datagram (UDP style) socket
DatagramSocket aSocket = null;

try {

    aSocket = new DatagramSocket();

    // build packet contents by string concatenation
    byte m[] = (args[0] +" " + args[1] + " " + args[2]).getBytes();

    // build an InetAddress object from a DNS name  
    InetAddress aHost = InetAddress.getByName("localhost");

    // the testing port is 6502
    int serverPort = 6502;

    // build the packet holding the destination address, port and
    // byte array constructed from the command line arguments
    DatagramPacket request = new DatagramPacket(
    m, m.length, aHost, serverPort);

    // send the Datagram on the socket
    aSocket.send(request);

    // prepare room for the reply
    byte buffer[] = new byte[40];

    // build a datagram for the reply
    DatagramPacket reply = new DatagramPacket(buffer,buffer.length);

    // block and wait
    aSocket.receive(reply);

    // show the result to the client
    System.out.println(new String(reply.getData()));

  }
  // handle socket exceptions
    catch(SocketException e) {
         System.out.println("Socket: " + e.getMessage());
   }
   // handle general I/O exceptions
   catch(IOException e) {
        System.out.println("IO: " + e.getMessage());
   }
finally {
    // always close the socket
     if(aSocket != null) aSocket.close();
    }
  }
}
```
