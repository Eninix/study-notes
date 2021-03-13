[TOC]

---

# 网络编程

## UDP

### server端

```java
package InternetCoding.UDP;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;

public class UDPServer {
    public static void main(String[] args) throws IOException {
        // 创建服务器,监听端口
        DatagramSocket socket = new DatagramSocket(9090);

        //准备接收文件
        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);
        //接收文件
        socket.receive(packet);//阻塞接受

        System.out.println(packet.getAddress());
        System.out.println(new String(packet.getData(), 0, packet.getLength()));

        //关闭
        socket.close();
    }
}
```

### client端

```java
package InternetCoding.UDP;

import java.io.IOException;
import java.net.*;

public class UDPClient {
    public static void main(String[] args) throws IOException {
        //创建服务 监听端口,(也可以不监听)
        DatagramSocket socket = new DatagramSocket();

        //准备打出信息
        String msg = "吾主在此!";
        InetAddress localhost = InetAddress.getByName("localhost");
        int port = 9090;

        DatagramPacket packet = new DatagramPacket(msg.getBytes(),0,msg.getBytes().length,localhost,port);
        //发出信息
        socket.send(packet);
        //关闭
        socket.close();
    }
}
```

## TCP

### server端

```java
package InternetCoding.TCP;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPServer {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;
        try {
            // 先要有一个地址
            serverSocket = new ServerSocket(9999);
            while (true) {
                // 等待连接
                socket = serverSocket.accept();
                // 读取客户端消息
                is = socket.getInputStream();
                // 管道流
                baos = new ByteArrayOutputStream();
                byte[] buffer = new byte[1024];
                int len;
                while ((len = is.read(buffer)) != -1) {
                    baos.write(buffer, 0, len);
                }
                System.out.println(baos.toString());
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (baos != null) {
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (serverSocket != null) {
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

### server传文件

```java
package InternetCoding.TCP;

import java.io.*;
import java.net.ServerSocket;
import java.net.Socket;

public class TCPFileServer {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        FileOutputStream baos = null;
        try {
            // 先要有一个地址
            serverSocket = new ServerSocket(9999);
            while (true) {
                // 等待连接
                socket = serverSocket.accept();
                // 读取客户端消息
                is = socket.getInputStream();
                // 管道流
                baos = new FileOutputStream(new File("src/copytext.txt"));
                byte[] buffer = new byte[1024];
                int len;
                while ((len = is.read(buffer)) != -1) {
                    baos.write(buffer, 0, len);
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (baos != null) {
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (serverSocket != null) {
                try {
                    serverSocket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```



### client端

```java
package InternetCoding.TCP;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.Socket;

public class TCPClient {
    public static void main(String[] args) {
        Socket socket = null;
        OutputStream os = null;
        try {
            // 先要知道 地址和端口号
            InetSocketAddress socketAddress = new InetSocketAddress("127.0.0.1", 9999);
            // 创建socket连接
            socket = new Socket(socketAddress.getAddress(), socketAddress.getPort());
            // 发送消息 IO流
            os = socket.getOutputStream();
            os.write("此方はエニニ、お返事ください！".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (os != null) {
                try {
                    os.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (socket != null) {
                try {
                    socket.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

```

### client传文件

```java
package InternetCoding.TCP;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;

public class TCPFileClient {
    public static void main(String[] args) {
//        1.创建socket连接
        Socket socket = null;
        try {
            socket = new Socket(InetAddress.getByName("127.0.0.1"), 9999);
//        2.创建输出流
            OutputStream os = socket.getOutputStream();
//        3.读取文件
            FileInputStream fis = new FileInputStream(new File("src/text.txt"));
//        4.写出文件
            byte[] buffer = new byte[1024];
            int len;
            while ((len = fis.read(buffer)) != -1) {
                os.write(buffer,0,len);
            }
//            5.关闭
            fis.close();
            os.close();
            socket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```

# 多线程

