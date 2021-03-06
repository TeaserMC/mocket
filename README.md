Mocket
======

[![Build Status](https://travis-ci.org/Nishant-Pathak/mocket.svg?branch=master)](https://travis-ci.org/Nishant-Pathak/mocket)
[![Join the chat at https://gitter.im/_mocket/Lobby](https://badges.gitter.im/_mocket/Lobby.svg)](https://gitter.im/_mocket/Lobby?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)


    Lightweight, Typesafe, Reliable, Guaranty delivery, Ordered, High performant java nio sockets build 
    on top of udp.

Download
--------
 * Use jitpack as repository
 * Add mocket to dependencies

As shown below:

1. Maven
```xml
<project>
...
    <repositories>
    ...
        <repository>
            <id>jitpack.io</id>
            <url>https://jitpack.io</url>
        </repository>
    </repositories>
</project>
```

```xml
    <dependencies>
        ...
        <dependency>
            <groupId>com.github.Nishant-Pathak</groupId>
            <artifactId>mocket</artifactId>
            <version>v1.0</version>
        </dependency>
    </dependencies>
```

2. Gradle
```groovy
    allprojects {
        repositories {
            ...
            maven { url 'https://jitpack.io' }
        }
    }
```
```groovy
    dependencies {
        compile 'com.github.Nishant-Pathak:mocket:v1.0'
    }
```

Simple Server
-------------
To build server:
```java
   ServerBuilder<byte []> serverBuilder = new ServerBuilder<byte []>()
        .port(serverPort);
   Server<byte[]> server = serverBuilder.build();
```
To read:
```java
    while (true) {
      // blocking read
      Pair<SocketAddress, byte[]> readBytes = server.read();
    }
```
To write:
```java
    // write to server
    server.write(read.getSecond(), read.getFirst());
```
Write signature:
```java
  /**
   * writed data to the given address
   * @param data to send to addredd
   * @param address of the client connected
   * @throws IOException if error occurs while writing to the socket
   * @throws InterruptedException if write is interrupted
   */
  void write(T data, SocketAddress address) throws IOException, InterruptedException;

```
Simple Client
-------------
To build client:
```java
    ClientBuilder<byte []> builder = new ClientBuilder<byte []>()
        .host("127.0.0.1", 8080);
    Client<byte []> client = builder.build();
```
To read:
```java
    // blocking read
    byte[] data = client.read();
```
To write:
```java
   client.write(data);
```
Write signature:
```java
  /**
   * write object of type T on the wire
   * it used @{@link com.network.mocket.handler.MocketStreamHandler} to parse the object
   * @param data object to send on wire to the server.
   * @throws IOException if error occurs while writing to the socket
   * @throws InterruptedException if write is interrupted
   */
  void write(T data) throws IOException, InterruptedException;
```

To write custom mapper
----------------------
Implement java interface and add it to the client and server builder as

```java
        builder.addHandler(new MyCustomHandler())
```

```java
public interface MocketStreamHandler<P> {
  /**
   * Used to encode object to byte stream
   * @param in inout object of tye {P}
   * @return byte array representing object
   * @throws ParseException throws if fails to encode or decode
   */
  byte [] encode(P in) throws ParseException;

  /**
   * Used to decode bytestream to object
   * @param out inout object of tye {P}
   * @return byte array representing object
   * @throws ParseException throws if fails to encode or decode
   */
  P decode(byte [] out) throws ParseException;
}
```

Example
-------
Sample server client can be found here [example](/src/main/java/com/network/mocket/example).

Android App Demo
================
![Mocket android app](https://github.com/Nishant-Pathak/mocket_android_demo)

Use Case
--------
1. Service-Service communication inside data center.
2. Use in android app for better performance in flaky networks.
3. Use in senser based Android things device to push* periodic data to the server.

*Some gateways might block UDP traffic.

Next Milestone
--------------
1. Add ssl support.
2. Have support for TCP.
3. Multipart TCP support.

Contributing
------------
I am more than happy to accept external contributions to the project in the form of feedback, bug reports and even better - pull requests :)

If you would like to submit a pull request, please make an effort to follow the guide in [CONTRIBUTING.md](CONTRIBUTING.md).

License
=======
Copyright (C) 2016 - 2017 Nishant Pathak

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
