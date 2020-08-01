---
layout: post
title: Java网络编程的常用类和方法
categories: JavaSE
description: 
keywords: Internet,Java
---

重新复习一下 java 网络编程的常用类和方法
# 1、 IP 地址

Java 中使用 `InetAddress` 类表示 IP 地址，该类没有构造器，通过静态方法来得到对象

```java
        try {
            //获得网络主机
            InetAddress ip1 = InetAddress.getByName("www.baidu.com");
            System.out.println(ip1);
            //获得本地主机
            InetAddress ip2 = InetAddress.getLocalHost();
            System.out.println(ip2);

            //常用方法
            System.out.println(ip1.getHostAddress());   //ip
            System.out.println(ip1.getHostName());      //域名
        } catch (UnknownHostException e) {
            e.printStackTrace();
        }

```

# 2、 端口
Java 中使用 `InetSocketAddress` 类表示套接字，通过构造器创建对象，注意指定 ip 和端口。

```java
		InetSocketAddress inetSocketAddress = new InetSocketAddress("www.baidu.com",443);
        System.out.println(inetSocketAddress);

        System.out.println(inetSocketAddress.getHostName());    //域名
        System.out.println(inetSocketAddress.getPort());        //端口
```



# 3、 通信协议

## 3.1、 TCP 

+ **server 端**

在客户/服务器通信模式中，服务器端需要创建监听特定端口的 `ServerSocket`，`ServerSocket` 负责接收客户连接请求，并生成与客户端连接的 `Socket` 。

```java
//服务器端
public class TcpServerDemo01 {
    public static void main(String[] args) {
        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream is = null;
        ByteArrayOutputStream baos = null;

        try {
            //1. 服务端利用ServerSocket来监听端口，等待客户端的Socket来连接
            serverSocket = new ServerSocket(9999);//监听9999端口
            //2. 阻塞式监听，会一直等待客户端连接过来,并且得到客户端Socket
            socket = serverSocket.accept();

            //3. 读取客户端的消息
            is = socket.getInputStream();

            //通过管道流确保数据接收正确
            baos = new ByteArrayOutputStream();
            byte[] buffer = new byte[1024];
            int len;
            while((len = is.read(buffer))!= -1){
                baos.write(buffer,0,len);
            }
            
            //得到Client数据
            System.out.println(baos.toString());
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            //关闭流 依次为 baos, is, socket,  serverSocket
            //因为代码太多了，这里只展示 baos，其他一样
            if(baos != null){
                try {
                    baos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            //is.close();socket.close();serverSocket.close();
        }
    }
}
```



+ **Client 端**

```java
//客户端
public class TcpClientDemo01 {
    public static void main(String[] args) {
        Socket socket = null;
        OutputStream os = null;

        try {
            //1. 创建与Server端的Socket连接
            socket = new Socket("127.0.0.1",9999);
            //2. 通过IO流发送消息
            os = socket.getOutputStream();
            os.write("Hello,this is Client!".getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }finally{
            //关闭流 os 和 socket,具体代码和 Server 一样。
            //os.close(); socket.close();
        }
    }
}
```

## 3.2、UDP

++ ** server 端**
准确来说， UDP 没有 Server 和 Client 的概念，因为每一个机器都可以为 Server 或 Client，应该称之为发送端和接收端。

```java
public class UdpServerDemo01 {
    public static void main(String[] args) throws Exception {
        //监听端口
        DatagramSocket socket = new DatagramSocket(9999);

        //接收数据包
        byte[] buffer = new byte[1024];
        DatagramPacket packet = new DatagramPacket(buffer, 0, buffer.length);
        socket.receive(packet);     //阻塞接收

        System.out.println(new String(packet.getData()));	//输出一下接收的数据

        //关闭连接
        socket.close();
    }
}

```

+ ** Client 端**
```java
//不需要连接服务器
public class UdpClientDemo01 {
    public static void main(String[] args) throws Exception {
        //1. 建立一个socket,用于发包
        DatagramSocket datagramSocket = new DatagramSocket();

        //2. 建个包
        String msg = "Hello!";
        //参数：数据，数据的长度，要发给谁
        DatagramPacket packet = new DatagramPacket(msg.getBytes(),0,msg.getBytes().length,new InetSocketAddress("127.0.0.1",9999));

        //3. 发送
        datagramSocket.send(packet);

        //4.关闭流
        datagramSocket.close();
    }
}

```

# 4、 文件上传

+ ** server 端**

```java
public class TcpServerDemo02 {
    //文件上传Server端
    public static void main(String[] args) throws Exception {
        ServerSocket serverSocket = new ServerSocket(9000);
        Socket socket = serverSocket.accept();
        InputStream is = socket.getInputStream();

        FileOutputStream fos = new FileOutputStream("yukino1.jpeg");
        byte[] buffer = new byte[1024];
        int len;
        while((len = is.read(buffer)) != -1){
            fos.write(buffer,0,len);
        }

        //通知客户端我已接受完毕
        OutputStream os = socket.getOutputStream();
        os.write("我已接收完毕，可以断开了".getBytes());

        //关闭流,这里简写了
        fos.close();
        is.close();
        socket.close();
        serverSocket.close();
    }
}

```

+ ** client 端**

```java
public class TcpClientDemo02 {
    //文件上传Client端
    public static void main(String[] args) throws Exception {
        Socket socket = new Socket("127.0.0.1",9000);

        FileInputStream fis = new FileInputStream("Yukino.jpeg");
        OutputStream os = socket.getOutputStream();
        byte[] buffer = new byte[1024];
        int len;
        while((len = fis.read(buffer)) != -1){
            os.write(buffer,0,len);
        }//图片上传完毕

        /**
        	通知服务器，文件上传已完成 ,如果不主动告诉服务器传输已完成，则服务器会一直处于等待接收
        	服务器端 len 如果为 -1 只表示现在没有东西传过来了，但是不确定之后会不会再有东西传过来，所以会处于一直等待而不是主动释放
        	而之前的 TcpClientDemo01 没有写 socket.shutdownOutput() 但是 Server 和 Client 两端也会关闭，是因为 client 会在 socket.close() 直接把连接断开了，那么服务器端自然就知道之后不会再有数据传输过来。
        	注意 socket.shutdownOutput() 只是告诉服务端文件上传完成，但是此次 Socket 连接并没有关闭，依然可以通过此次 Socket 连接来得到服务器发送的确认信号
        **/
        socket.shutdownOutput();

        //确认服务器接收完毕，才能够断开连接
        InputStream is = socket.getInputStream();
        ByteArrayOutputStream baos = new ByteArrayOutputStream();

        byte[] buffer2 = new byte[1024];
        while((len = is.read(buffer2)) != -1){
            baos.write(buffer2,0,len);
        }
        System.out.println(baos.toString());

        //关闭流,这里简写了
        os.close();
        fis.close();
        socket.close();
    }
}


```

# 5、 URL

利用 URL 下载网易云的音乐
```java
public class URLDemo01 {
    public static void main(String[] args) throws Exception {

        //1. 下载地址
        URL url = new URL("https://m701.music.126.net/20200717191610/dd9c893464029294adc3860a64061ce0/jdyyaac/535b/0208/5358/d1223043d95be596c846d23417cee968.m4a");

        //2. 连接到这个资源 http
        HttpURLConnection urlConnection = (HttpURLConnection) url.openConnection();

        InputStream is = urlConnection.getInputStream();

        FileOutputStream fos = new FileOutputStream("loser.m4a");

        byte[] buffer = new byte[1024];
        int len;
        while((len = is.read(buffer,0,buffer.length)) != -1){
            fos.write(buffer,0,len);
        }

        //关闭流
        fos.close();
        is.close();
        urlConnection.disconnect();
    }
}

```