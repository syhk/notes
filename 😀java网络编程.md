# java网络编程

```java

  public static void main(String[] args) {
//查询本机地址
      try {
     InetAddress inetAddress= InetAddress.getByName("127.0.0.1");
          System.out.println(inetAddress);
      } catch (UnknownHostException e) {
          e.printStackTrace();
      }
      try {
          InetAddress   inetAddress= InetAddress.getLocalHost();
      System.out.println(inetAddress);
      } catch (UnknownHostException e) {
          e.printStackTrace();
      }

//     查询网站ip地址
      InetAddress inetAddress= null;
      try {
          inetAddress = InetAddress.getByName("www.baidu.com");
      } catch (UnknownHostException e) {
          e.printStackTrace();
      }
      System.out.println(inetAddress);

    //      常用方法
    System.out.println(inetAddress.getCanonicalHostName());//规范的名字
    System.out.println(inetAddress.getHostAddress());//ip地址
    System.out.println(inetAddress.getHostName());//域名或者自己电脑名字
  }
}
```

> 端口


表示计算机上的一个程序的进程：

- 
不同的进程有不同的端口号！

- 
一般被规定0-65535

- 
TCP/UDP ： 65535*2 ，单个协议下端口号不能冲突


端口分类：HTTP 80 HTTPS 43  FTP 21   SSH 22  TELENT 23

```bash
netstat -ano #查看所有的端口
```

```java
  public static void main(String[] args) {
InetSocketAddress socketaddress=    new InetSocketAddress("127.0.0.1",8080);
    System.out.println(socketaddress);
    System.out.println(socketaddress.getAddress());
    System.out.println(socketaddress.getAddress().getHostAddress());
    System.out.println(socketaddress.getAddress().getHostName());
    System.out.println(socketaddress.getPort());
}}
```

TCP

服务端：

```java
package Java反射;


import java.io.ByteArrayOutputStream;
import java.io.InputStream;
import java.net.ServerSocket;
import java.net.Socket;

//服务端
public class sy007s {

  public static void main(String[] args) throws Exception{
      //    要有一个地址
   ServerSocket ss = new ServerSocket(6666);
//           等待客户端连接过来
      Socket s =ss.accept();

//      读取客户端的信息
      InputStream in= s.getInputStream();

//      管道流
      ByteArrayOutputStream out = new ByteArrayOutputStream();
      byte[] b = new byte[1024];
          int len = 0;
      while ((len=in.read(b))!= -1){
              out.write(b,0,len);
      }
    System.out.println(out.toString());

//      关闭资源，先开的后关，后开的先关
      out.close();
      in.close();
      s.close();
      ss.close();
  }
}
```

客户端：

```java
package Java反射;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
//客户端
public class sy007c {
  public static void main(String[] args) throws Exception{
//      知道服务器地址
      InetAddress   serverIP= InetAddress.getByName("127.0.0.1");
        int    port=6666;
//      创建一个Socket连接
          Socket socket = new Socket(serverIP,port);
//      发送信息
      OutputStream os=socket.getOutputStream();
      os.write("hello world".getBytes());
//      关闭连接
      socket.close();
  }
}
```
