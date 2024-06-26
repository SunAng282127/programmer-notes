# 一、Netty介绍

## 一、Netty概述

1. Netty是由JBOSS提供的一个Java开源框架，现为Github上的独立项目
2. Netty是一个异步的、基于事件驱动的网络应用框架，用以快速开发高性能、高可
   靠性的网络lO程序
3. Netty主要针对在TCP协议下，面向Clients端的高并发应用，或者Peer-to-Peer场景下的大量数据持续传输的应用
4. Netty本质是一个NIO框架，适用于服务器通讯相关的多种应用场景

## 二、Netty的应用场景

1. 互联网行业：在分布式系统中，各个节点之间需要远程服名调用，高性能的RPC框架必不可少，Netty作为**异步高性能的通信框架**，往往作为基础通信组件被这些RPC框架使用。如阿里分布式服务框架Dubbo的RPC框架使用Dubbo协议进行节点间通信，Dubbo协议默认使用Netty作为基础通信组件，用于实现各进程节点之间的内部通信
2. 游戏行业：无论是手游服务端还是大型的网络游戏Java语言得到了越来越广泛的应用。Netty 作为高性能的基础通信组件，提供了TCP/UDP和HTTP协议栈，方便定制和开发私有协议栈，账号登录服务器。地图服务器之间可以方便的通过Netty进行高性能的通信
3. 大数据领域：经典的Hadoop的高性能通信和序列化组件（AVRO实现数据文件共享）的RPC框架，默认采用Netty进行跨界点通信，它的Netty Service基于Netty框架二次封装实现

## 三、IO模型基本说明

1.  I/O模型简单的理解：就是用什么样的通道进行数据的发送和接收，很大程度上决定了程序通信的性能
2. Java共支持3种网络编程模型IO模式：BIO、NIO、AIO
3.  Java BIO：同步并阻塞（**传统阻塞型**）， 服务器实现模式为一个连接一个线程，即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事情会造成不必要的线程开销
4. Java NIO：**同步非阻塞**，服务器实现模式为一个线程处理多个请求/连接，即客户端发送的连接请求都会注册到多路复用器上，多路复用器轮询到连接有I/O请求就进行处理
5. Java AIO（NIO.2）：**异步非阻塞**，AIO引入异步通道的概念，采用了Proactor模式， 简化了程序编写，有效的请求才启动线程，它的特点是先由操作系统完成后才通知服务端程序启动线程去处理，一般适用于连接数较多且连接时间较长的应用

## 四、BIO、NIO、AIO适用场景分析

1. BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，并发局限于应用中，JDK1 .4以前的唯一选择，但程序简单易理解
2.  NIO方式适用于连接数目多且连接比较短（轻操作）的架构，比如聊天服务器，弹幕系统，服务器间通讯等。编程比较复杂，JDK1.4开始支持
3. AIO方式使用于连接数目多且连接比较长（重操作）的架构，比如相册服务器，充分调用OS参与并发操作，编程比较复杂，JDK7开始支持

# 二、Java BIO基本介绍

## 一、Java BIO概述

1. Java BIO就是传统的java io编程，其相关的类和接口在java.io
2. BIO（blocking I/O）：**同步阻塞**，服务器实现模式为一个连接一个线程， 即客户端有连接请求时服务器端就需要启动一个线程进行处理，如果这个连接不做任何事倩会造成不必要的线程开销，可以通过线程池机制改善（实现多个客户连接服务器）
3. BIO方式适用于连接数目比较小且固定的架构，这种方式对服务器资源要求比较高，
   并发局限于应用中，JDK1.4以前的唯一选择， 程序简单易理解

## 二、Java BIO问题分析

1. 每个请求都需要创建独立的线程，与对应的客户端进行数据Read+业务处理，数据Write
2. 当并发数较大时，需要创建大量线程来处理连接，系统资源占用较大
3. 连接建立后，如果当前线程暂时没有数据可读，则线程就阻塞在Read操作上，造成线程资源浪费

## 三、BIO应用实例

1. 使用BIO模型编写一个服务器端。监听6666端口，当有客户端连接时，就启动一个线程与之通讯

2. 要求使用线程池机制改善，可以连接多个客户端

3. 服务器端可以接收客户端发送的效据（telnet方式即可，启动服务端，通过telnet方式，连接服务IP和断后，输入Ctrl+]，Send+需要发送的内容，就能发消息被服务端监听到）

   ```java
   package com.luojia.netty.nettypro.bio;
   
   import java.io.IOException;
   import java.io.InputStream;
   import java.net.ServerSocket;
   import java.net.Socket;
   import java.util.concurrent.ExecutorService;
   import java.util.concurrent.Executors;
   
   public class BIOServer {
       public static void main(String[] args) throws IOException {
           // 创建一个线程池
           ExecutorService newCachedThreadPool = Executors.newCachedThreadPool();
           // 创建 serverSocket
           ServerSocket serverSocket = new ServerSocket(6666);
           System.out.println("服务器启动了");
   
           while (true) {
               System.out.println("等待连接");
               // 监听，等待客户端连接, BIO,一直阻塞在等待连接部分
               Socket accept = serverSocket.accept();
               System.out.println("连接到一个客户端");
               newCachedThreadPool.execute(new Runnable() {
                   @Override
                   public void run() {
                       handler(accept);
                   }
               });
           }
       }
   
       // 编写一个handler和客户端通信
       public static void handler(Socket socket) {
           System.out.println("线程信息 ID = " + Thread.currentThread().getId() +
                   "线程名字 = " + Thread.currentThread().getName());
           byte[] bytes = new byte[1024];
           try (InputStream inputStream = socket.getInputStream()) {
               // 循环读取客户端发送的请求
               while (true) {
                   System.out.println("等待读取...");
                   int read = inputStream.read(bytes);
                   if (read != -1) {
                       // 输出客户端发送的数据
                       System.out.println(new String(bytes, 0, read));
                   } else {
                       break;
                   }
               }
           } catch (IOException e) {
               throw new RuntimeException(e);
           }
       }
   }
   ```

# 三、Java NIO基本介绍

## 一、Java NIO概述

1. Java Nl0全称 java non-blocking lO，是指JDK提供的新API。从JDK1.4开始，Java提供了一系列改进的输入/输出的新特性，被统称为NIO（即NewIO），是同步非阻塞的
2. NIO相关类都被放在java.nio包及子包下，并且对原java.io包中的很多类进行改写
3. NIO有三大核心部分：Channel即通道，Buffer即缓冲区，Selector即选择器
4. NIO是面向缓冲区，或者面向块编程的。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动，这就增加了处理过程中的灵活性，使用它可以提供非阻塞式的高伸缩性网络
5. Java NIO的非阻塞模式，使一个线程从某通道发送请求或者读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取，而**不是保持线程阻塞**，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。非阻塞写也是如此，一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情
6. 通俗理解为NIO是可以做到用一个线程来处理多个操作的。假设有10000个请求过来根据实际情况，可以分配50或者100个线程来处理。不像之前的阻塞IO那样，非得分配10000
7. HTTP2.0使用了多路复用的技术，做到同一个连接并发处理多个请求，而且并发请求的数量比HTTP1.1大了好几个数量级

## 二、NIO和BIO的比较

1. BI0以流的方式处理数据，而NIO以块的方式处理数据，块I/0的效率比流I/0高很多
2. BIO是阻塞的，NIO则是非阻塞的
3. BIO基于字节流和字符流进行操作，而NI0基于Channel（通道）和Buffer（缓冲区）进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。Selector（选择器）用于监听多个通道的事件（比如：连接请求，数据到达等），因此使用**单个线程就可以监听多个客户端通道**

## 三、selector、channel、buffer关系说明

![](../../../TyporaImage/20200516111445972.png)

1. 每个channel都会对应一个Buffer
2. 一个Selector对应一个线程，一个Selector对应多个channel/连接
3. 程序切换到哪个channel是由事件决定的，Event就是一个重要的概念
4. Selector会根据不同的事件，在各个通道上切换
5. 数据的读取写入是通过Buffer，这个和BIO有区别，BIO中要么是输入流，或者是输出流，不能双向，但是NIO的Buffer是可以读也可以写，需要fip方法切换
6. channel是双向的，可以返回底层操作系统的情况，比如Linux，底层的操作系统通道就是双向的

## 四、Buffer缓冲区

1. Buffer缓冲区：缓冲区本质上是一个可以读写数据的内存块，可以理解成是一个容器对象（含数组），该对象提供了一组方法，可以更轻松地使用内存块，缓冲区对象内置了一些机制，能够跟踪和记录缓冲区的状态变化情况。Channel提供从文件、网络读取数据的渠道，但是读取或写入的数据都必须经由Buffer，如图：

   ![](../../../TyporaImage/1.%E7%BC%93%E5%86%B2%E5%8C%BA%E8%AF%BB%E5%86%99.jpg)

2. Bufter类定义了所有的缓冲区都具有的四个属性来提供关于其所包含的数据元素的信息

   ```java
   // Invariants: mark <= position <= limit <= capacity
   // 标记
   private int mark = -1;
   // 位置，下一个要被读或写的元素的索引，每次读写缓冲区数据时都会改变该值，为下次读写做准备
   private int position = 0;
   // 表示缓冲区的当前终点，不能对缓冲区超过极限的位置进行读写操作。且极限是可以修改的
   private int limit;
   // 容量，即可以容纳的最大数源量，在缓冲区创建时被设定并且不能改变
   private int capacity;
   ```

## 五、Channel通道

1. NIO的通道类似于流，但有些区别如下：
   - 通道可以同时进行读写，而流只能读或者只能写
   - 通道可以实现异步读写数据
   - 通道可以从缓冲读数据，也可以写数据到缓冲
2. BIO中的stream是单向的，例如FilelnputStream对象只能进行读取数据的操作，而NIO中的Channel通道是双向的，可以读操作，也可以写操作
3. Channel在NIO中是一个接口public interface Channel extends Closeable{}
4. 常用的Channel类有：FileChannel、DatagramChanhel、ServerSocketChannel和SocketChannel
5. FileChannel用于文件的数据读写；DatagramChannel用于UDP的数据读写；ServerSocketChannel和SocketChannel用于TCP的数据读写

## 六、关于Buffer和Channel的注意事项和细节

1. ByteBuffer支持类型化的put和get，put放入的是什么数据类型，get就应该使用相应的数据类型来取出，否则可能有BufferUnderflowException异常
2. 可以将一个普通Buffer转成只读Buffer
3. NIO还提供了MappedByteBuffer，可以让文件直接在内存（堆外的内存）中进行修改，而如何同步到文件由NIO来完成
4. 前面的读写操作，都是通过一个Buffer完成的，NIO还支持通过多个Buffer（即Buffer数组）完成读写操作，即Scattering和Gatering

## 七、Selector选择器

1. Selector选择器基本介绍

   - Java的NIO，用非阻塞的IO方式。可以用一个线程，处理多个的客户端连接，就会使用到Selector选择器
   - Selector能够检测多个注册的通道上是否有事件发生（注意：多个Channel以事件的方式可以注册到同一个Selector），如果有事件发生，便获取事件然后针对每个事件进行相应的处理。这样就可以只用一个单线程去管理多个通道，也就是管理多个连接和请求
   - 只有在连接/通道
   - 真正有读写事件发生时，才会进行读写，就大大地减少了系统开销，并且不必为每个连接都创建一个线程，不用去维护多个线程
   - 避免了多线程之间的上下文切换导致的开销

2. Selector选择器特点说明

   - Netty的IO线程NioEventLoop（聚合了Selector选择器，也叫多路复用器），可以同时并发处理成百上千个容户端连接
   - 当线程从某客户端Socket通道进行读写数据时，若没有数据可用时，该线程可以进行其他任务
   - 线程通常将非阻塞IO的空闲时间用于在其他通道上执行I0操作，所以单独的线程可以管理多个输入和输出通道
   - 由于读写操作都是非阻塞的，这就可以充分提升IO线程的运行效率，避免由于频繁IO阻塞导致的线程挂起
   - 一个I/O线程可以并发处理N个客户端连接和读写操作，这从根本上解决了传统同步阻塞I/O一连接一线程模型，架构的性能、弹性伸缩能力和可靠性都得到了极大的提升

3. NIO非阻塞网络编程原理分析图：NIO非阻塞网络编程相关的（Selector、SelectionKey、ServerSocketChannel和SocketChannel）关系梳理

   ![](../../../TyporaImage/2.%E9%9D%9E%E9%98%BB%E5%A1%9EIO.jpg)

   - 当客户端连接时，会通过ServerSocketChannel得到SocketChannel
   - Selector开始监听
   - 将SocketChannel注册到Selector上，调用register(Selector sel, int ops)，一个Selector上可以注册多个SocketChannel
   - 注册后返回一个SelectionKey，会和该Selector关联（集合关系）
   - Selector实例进行监听select方法，返回有事件发生的通道个数
   - 进一步得到各个selectionKey（有事件发生的selectionKey）
   - 再通过SelectionKey反向获取到SocketChannel ，方法为Channel()
   - 通过得到的SocketChannel，完成业务处理

4. NIOServer

   ```java
   package com.luojia.netty.nettypro.nio;
   
   import java.io.IOException;
   import java.net.InetSocketAddress;
   import java.nio.ByteBuffer;
   import java.nio.channels.*;
   import java.util.Iterator;
   import java.util.Set;
   
   public class NIOServer {
       public static void main(String[] args) throws IOException {
           // 创建ServerSocketChannel -> serverSocket
           ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
           // 得到一个Selector 对象
           Selector selector = Selector.open();
           // 绑定一个端口6666，在服务端监听
           serverSocketChannel.socket().bind(new InetSocketAddress(6666));
           // 设置为非阻塞
           serverSocketChannel.configureBlocking(false);
           // 把 serverSocketChannel 注册到Selector 关心事件为 OP_ACCEPT
           serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
   
           // 循环等待客户链接
           while (true) {
               // 非阻塞，立即返回
               // if (selector.selectNow() == 0) {
               // select，阻塞指定时间毫秒，无链接就阻塞指定时间返回
               if (selector.select(1000) == 0) {
                   System.out.println("服务器等待了1秒，无连接");
                   continue;
               }
   
               // 如果返回的 >0，就获取到了selectionKey集合
               // 1.如果返回大于0，表示已经获取到了关注的集合
               // 2.selector.selectedKeys()返回关注时间的集合
               Set<SelectionKey> selectionKeys = selector.selectedKeys();
               // 通过 selectionKeys 反向获取通道
               Iterator<SelectionKey> keyIterator = selectionKeys.iterator();
               while (keyIterator.hasNext()) {
                   SelectionKey key = keyIterator.next();
                   // 根据key，对应的通道发生的事件做相应的处理
                   if (key.isAcceptable()) {
                       // serverSocketChannel的accept()是不阻塞的
                       SocketChannel sockerChannel = serverSocketChannel.accept();
                       sockerChannel.configureBlocking(false);
                       // 将SocketChannel 注册到Selector，关注事件为 OP_READ
                       sockerChannel.register(selector, SelectionKey.OP_READ, ByteBuffer.allocate(1024));
                   }
                   // 发生事件 OP_READ
                   if (key.isReadable()) {
                       // 通过key，反向获取到Channel
                       SocketChannel socketChannel = (SocketChannel)key.channel();
                       // 获取到该Channel关联的Buffer
                       ByteBuffer buffer = (ByteBuffer)key.attachment();
                       socketChannel.read(buffer);
                       System.out.println("form 客户端 " + new String(buffer.array()));
                   }
   
                   // 手动从集合中移动当前的selectionKey，防止迭代器中重复遍历
                   keyIterator.remove();
               }
           }
       }
   }
   ```

5. NIOClient

   ```java
   package com.luojia.netty.nettypro.nio;
   
   import java.io.IOException;
   import java.net.InetSocketAddress;
   import java.nio.ByteBuffer;
   import java.nio.channels.SocketChannel;
   
   public class NIOClient {
       public static void main(String[] args) throws IOException {
           // 得到一个网络通道
           SocketChannel socketChannel = SocketChannel.open();
           // 设置非阻塞
           socketChannel.configureBlocking(false);
           // 绑定服务器ip和端口
           InetSocketAddress inetSocketAddress = new InetSocketAddress("127.0.0.1", 6666);
           // 连接服务器
           if (!socketChannel.connect(inetSocketAddress)) {
               while (!socketChannel.finishConnect()) {
                   System.out.println("暂未连接上服务器，客户端不会阻塞，可以做其他操作");
               }
           }
   
           // 如果链接成功，发送数据
           String str = "连接成功，发送数据";
           // ByteBuffer buffer = ByteBuffer.allocate(1024);
           ByteBuffer buffer = ByteBuffer.wrap(str.getBytes());
           socketChannel.write(buffer);
           // 让客户端停留在此处
           System.in.read();
       }
   }
   ```

## 八、NIO与零拷贝

### 一、零拷贝基本介绍

1. 零拷贝是网络编程的关键，很多性能优化都离不开
2. 在Java程序中，常用的零拷贝有mmap（内存映射）和sendFile

### 二、传统IO数据读写

![](../../../TyporaImage/3.%E4%BC%A0%E7%BB%9FIO%E5%92%8C%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B.png)

![](../../../TyporaImage/4.%E4%BC%A0%E7%BB%9FIO.jpg)

### 三、mmap优化

- mmap通过内存映射，将文件映射到内核缓冲区，同时，用户空间可以共享内核空间的数据。这样，在进行网络传输时，就可以减少内核空间到用户空间的拷贝次数。如下图

  ![](../../../TyporaImage/7.mmap.jpg)

### 四、sendFile

- Linux在2.4版本中，做了一些修改，避免了从内核缓冲区拷贝到Socketbuffer的操作，直接拷贝到协议栈，从而再一次减少了数据拷贝。具体如下图和小结：

  ![](../../../TyporaImage/5.sendFile.jpg)

### 五、mmap和sendFile的区别

1. mmap适合小数据量读写，sendFile适合大文件传输

2. mmap需要4次上下文切换，3次数据拷贝；sendFile需要3次上下文切换，最少2次数据拷贝

3. sendFile可以利用DMA方式，减少CPU拷贝，mmap则不能（必须从内核拷贝到Socket缓冲区）

   channel.transferTo(0, channel.size(), socketChannel);该方法就使用到了零拷贝

### 六、代码实现

1. NIOServer

   ```java
   package com.luojia.netty.nettypro.nio.zerocopy;
   
   import java.io.IOException;
   import java.net.InetSocketAddress;
   import java.nio.ByteBuffer;
   import java.nio.channels.ServerSocketChannel;
   import java.nio.channels.SocketChannel;
   
   public class NIOServer {
       public static void main(String[] args) throws IOException {
           InetSocketAddress address = new InetSocketAddress(7001);
           ServerSocketChannel serverSocketChannel = ServerSocketChannel.open();
           serverSocketChannel.socket().bind(address);
   
           // 创建Buffer
           ByteBuffer buffer = ByteBuffer.allocate(4096);
           while (true) {
               SocketChannel socketChannel = serverSocketChannel.accept();
               int readCount = 0;
               while (-1 != readCount) {
                   readCount = socketChannel.read(buffer);
               }
               buffer.rewind(); // 倒带，让数据可以重读
           }
       }
   }
   ```

2. NIOClient

   ```java
   package com.luojia.netty.nettypro.nio.zerocopy;
   
   import java.io.FileInputStream;
   import java.io.IOException;
   import java.net.InetSocketAddress;
   import java.nio.channels.FileChannel;
   import java.nio.channels.SocketChannel;
   
   public class NIOClient {
       public static void main(String[] args) throws IOException {
           SocketChannel socketChannel = SocketChannel.open();
           socketChannel.connect(new InetSocketAddress("127.0.0.1", 7001));
           String fileName = "F:\\nginx-1.23.4.zip";
           FileChannel channel = new FileInputStream(fileName).getChannel();
           
           // 准备发送
           long start = System.currentTimeMillis();
           // 在Linux下一个transferTo 方法就可以完成传输
           // 在Windows下一个transferTo 只能发送8M，需要分段传输文件，需要注意传输的位置
           // transferTo 底层使用到零拷贝
           long transferCount = channel.transferTo(0, channel.size(), socketChannel);
   
           System.out.println("发送的总的字节数 = " + transferCount + "  耗时：" + (System.currentTimeMillis() - start));
           channel.close();
       }
   }
   ```

# 四、Netty详解

## 一、原生NIO存在的问题

1. NIO的类库和API繁杂，使用麻烦：需要熟练掌握Selector、ServerSocketChannel、SocketChannel、ByteBuffer等
2. 需要具备其他的额外技能：要熟悉Java多线程编程，因为NIO编程涉及到Reactor模式，必须对多线程和网络编程非常熟悉，才能编写出高质量的NIO程序
3. 开发工作量和难度都非常大：例如客户端面临断连重连、网络闪断、半包读写、失败缓存、网络拥塞和异常流的处理等等
4. JDK NIO的Bug：例如臭名昭著的Epoll Bug，它会导致Selector空轮询，最终导致CPU 100%。直到JDK1.7版本该问题仍旧存在，没有被根本解决

## 二、Netty优点

1. Netty对JDK自带的NIO的APL进行了封装，解决了上述问题
2. 设计优雅：适用于各种传输类型的统一 API阻塞和非阻塞Socket；基于灵活且可扩展的事件模型，可以清晰地分离关注点；高度可定制的线程模型， 单线程，一个或多个线程池
3. 使用方便：详细记录的Javadoc，用户指南和示例；没有其他依项，JDK5（Netty3.x）或JDK6（Netty4.x）就足够了
4. 高性能、吞吐量更高：延迟更低；减少资源消耗；最小化不必要的内存复制
5. 安全：完整的SSL/TLS和StartTls支持
6. 社区活跃、不断更新：社区活跃，版本迭代周期短，发现的Bug可以被及时修复，同时，更多的新功能会被加入

## 三、Netty架构设计

1. 线程模型基本介绍

   - 不同的线程模式，对程序的性能有很大影响
   - 目前存在的线程模型有：传统阻塞I/O服务模型、Reactor模式
   - 根据Reactor的数量和处理资源池线程的数量不同，有3种典型的实现
     - 单Reactor、单线程
     - 单Reactor、多线程
     - 主从Reactor、多线程
   - Netty线程模式（Netty主要**基于主从Reactor多线程模型**做了一定的改进，其中主从Reactor多线程模型有多个Reactor）

2. 传统阻塞I/O服务模型

   - 工作原理图：黄色的框表示对象，蓝色的框表示线程，白色的框表示方法(API)

     ![](../../../TyporaImage/1.%E4%BC%A0%E7%BB%9F%E9%98%BB%E5%A1%9EIO%E6%A8%A1%E5%9E%8B.jpg)

   - 模型特点：采用阻塞IO模式获取输入的数据；每个连接都需要独立的线程完成数据的输入，业务处理，数据返回

   - 问题分析：当并发数很大，就会创建大量的线程，占用很大系统资源；连接创建后，如果当前线程暂时没有数据可读，该线程会阻塞在read操作，造成线程资源浪费

## 四、Reactor模式

1. 针对传统阻塞I/O服务模型的2个缺点，解决方案

   - 基于I/O复用模型：多个连接共用一个阻塞对象，应用程序只需要在一个阻塞对象等待，无需阻塞等待所有连接。当某个连接有新的数据可以处理时，操作系统通知应用程序，线程从阻塞状态返回，开始进行业务处理
   - 基于线程池复用线程资源：不必再为每个连接创建线程，将连接完成后的业务处理任务分配给线程进行处理，一个线程可以处理多个连接的业务

2. I/O复用结合线程池，就是Reactor模式基本设计思想，如图：

   ![](../../../TyporaImage/2.reactor%E6%A8%A1%E5%BC%8F.jpg)

   - Reactor模式，通过一个或多个输入同时传递给服务处理器的模式（基于事件驱动）
   - 服务器端程序处理传入的多个请求，并将它们同步分派到相应的处理线程，因此Reactor模式也叫Dispatcher模式（观察者模式）
   - Reactor模式使用IO复用监听事件收到事件后，分发给某个线程/进程，这点就是网络服务器高并发处理关键

3. Reactor模式中核心组成

   - Reactor：Reactor在一个单独的线程中运行，负责监听和分发事件，分发给适当的处理程序来对IO事件做出反应。它就像公司的电话接线员，它接听来自客户的电话并将线路转移到适当的联系人
   - Handlers：处理程序执行I/O事件要完成的实际事件，类似于客户想要与之交谈的公司中的实际官员。Reactor通过调度适当的处理程序来响应I/O事件，处理程序执行非阻塞操作

4. 单Reactor、单线程

   ![](../../../TyporaImage/3.%E5%8D%95reactor%E5%8D%95%E7%BA%BF%E7%A8%8B.jpg)

   - Select是前面I/O复用模型介绍的标准网络编程API，可以实现应用程序通过一个阻塞对象监听多路连接请求
   - Reactor对象通过Select监控客户端请求事件，收到事件后通过Dispatch进行分发
   - 如果是建立连接请求事件，则由Acceptor通过Accept处理连接请求，然后创建一个Handler对象处理连接完成后的后续业务处理
   - 如果不是建立连接事件，则Reactor会分发调用连接对应的Handler来响应
   - Handler会完成Read→业务处理→Send的完整业务流程
   - 结合实例：服务器端用一个线程通过多路复用搞定所有的IO操作（包括连接，读、写等），编码简单，清晰明了，但是如果客户端连接数量较多，将无法支撑
   - 优点：模型简单，没有多线程、进程通信、竞争的问题，全部都在一个线程中完成
   - 缺点：性能问题，只有一个线程，无法完全发挥多核CPU的性能。Handler在处理某个连接上的业务时，整个进程无法处理其他连接事件，很容易导致性能瓶颈；可靠性问题，线程意外终止，或者进入死循环，会导致整个系统通信模块不可用，不能接收和处理外部消息，造成节点故障
   - 使用场景：客户端的数量有限，业务处理非常快速，比如Redis在业务处理的时间复杂度 0(1) 的情况

5. 单Reactor、多线程

   ![](../../../TyporaImage/4.%E5%8D%95Reactor%E5%A4%9A%E7%BA%BF%E7%A8%8B.jpg)

   - Reactor对象通过select监控客户端请求事件，收到事件后，通过dispatch进行分发
   - 如果建立连接请求，则由Acceptor通过accept处理连接请求，然后创建一个Handler对象处理完成连接后的各种事件
   - 如果不是连接请求，则由reactor分发调用连接对应的handler来处理
   - handler只负责响应事件，不做具体的业务处理，通过read读取数据后，会分发给后面的worker线程池的某个线程处理业务
   - worker线程池会分配独立线程完成真正的业务，并将结果返回给handler
   - handler收到响应后，通过send将结果返回给client
   - 优点：可以充分的利用多核cpu的处理能力
   - 缺点：多线程数据共享和访问比较复杂，reactor处理所有的事件的监听和响应，在单线程运行，在高并发场景容易出现性能瓶颈

6. 主从Reactor、多线程

   ![](../../../TyporaImage/5.%E4%B8%BB%E4%BB%8EReactor%E5%A4%9A%E7%BA%BF%E7%A8%8B.jpg)

   - 针对单Reactor多线程模型中，Reactor在单线程中运行，高并发场景下容易成为性能瓶颈，可以让Reactor在多线程中运行
   - Reactor主线程MainReactor对象通过select监听连接事件，收到事件后，通过Acceptor处理连接事件
   - 当Acceptor处理连接事件后，MainReactor将连接分配给SubReactor
   - subReactor将连接加入到连接队列进行监听，并创建handler进行各种事件处理
   - 当有新事件发生时，subReactor就会调用对应的hander处理
   - handler 通过read读取数据，分发给后面的worker线程处理
   - worker线程池分配独立的worker线程进行业务处理，并返回结果
   - handler收到响应的结果后，再通过send将结果返回给client
   - Reactor主线程可以对应多个Reactor子线程，即MainRecator可以关联多个SubReactor





