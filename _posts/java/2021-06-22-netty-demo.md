---
layout: post
title: Java——使用 Netty 实现 CS 模型
categories: Java
description: Java——使用 Netty 实现 CS 模型
keywords: Java, Netty
---

Java——使用 Netty 实现 CS 模型

Netty 实现了对 **NIO** 的**封装**，使用起来更加方便

需要导入 **io.netty** 包

# 案例
使用 Netty 实现基本的 CS 通信。

包括两种通讯方式：
- 客户端和服务器**一对一**通信
- 服务器将某客户端的消息**转发**给所有客户端，实现客户端之间的同步

## Client
```java
public class Client {
    public static void main(String[] args) throws Exception {
        /*
         * 事件处理的线程池，无论是连接还是读写都由线程池中的线程处理
         * 参数为线程池的大小，无参默认为CPU的核心数*2
         * 客户端一般一个线程就够了
         */
        EventLoopGroup group = new NioEventLoopGroup(1);

        //复制启动类
        Bootstrap bootstrap = new Bootstrap();

        try {
            //设置线程池
            ChannelFuture channelFuture = bootstrap.group(group)
                    //设置连接类型
                    .channel(NioSocketChannel.class)
                    //channel上有事件时传给谁处理
                    .handler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            ch.pipeline().addLast(new ClientHandler());
                        }
                    })
                    .connect("localhost", 8890);

            //设置监听器，监听连接的返回
            channelFuture.addListener(new ChannelFutureListener() {
                @Override
                public void operationComplete(ChannelFuture future) throws Exception {
                    if (future.isSuccess()) {
                        System.out.println("connected");
                    } else {
                        System.out.println("not connected");

                    }
                }
            });

            //会永远等待close()方法的调用,实现长时间启动
            channelFuture.channel().closeFuture().sync();
        } finally {
            group.shutdownGracefully();
        }
    }
}

class ClientHandler extends ChannelInboundHandlerAdapter{
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        //channel第一次连上可用，写出一个字符串
        ByteBuf buf = Unpooled.copiedBuffer("hello".getBytes());
        ctx.writeAndFlush(buf);
    }

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ByteBuf buf = null;
        try{
            buf = (ByteBuf) msg;
            byte[] bytes = new byte[buf.readableBytes()];
            buf.getBytes(buf.readerIndex(),bytes);
            System.out.println(new String(bytes));
        }finally {
            //关闭资源
            if(buf!=null){
                ReferenceCountUtil.release(msg);
            }
        }
    }
}
```

## Server
```java
public class Server {

    //开启一个客户端通道组，用一个默认的线程处理事件，实现转发
    public static ChannelGroup clients = new DefaultChannelGroup(GlobalEventExecutor.INSTANCE);

    public static void main(String[] args) throws Exception{
        //只负责客户端的连接
        EventLoopGroup bossGroup = new NioEventLoopGroup(1);
        //负责每个Socket上的事件的处理
        EventLoopGroup workerGroup = new NioEventLoopGroup(2);

        try{
            ServerBootstrap serverBootstrap = new ServerBootstrap();
            ChannelFuture channelFuture = serverBootstrap.group(bossGroup,workerGroup)
                    .channel(NioServerSocketChannel.class)
                    .childHandler(new ChannelInitializer<SocketChannel>() {
                        @Override
                        protected void initChannel(SocketChannel ch) throws Exception {
                            ChannelPipeline channelPipeline = ch.pipeline();
                            channelPipeline.addLast(new ServerChildHandler());
                        }
                    })
                    .bind(8890)
                    .sync();

            System.out.println("server started");

            //会永远等待close()方法的调用,实现长时间启动
            channelFuture.channel().closeFuture().sync();
        }finally {
            workerGroup.shutdownGracefully();
            bossGroup.shutdownGracefully();
        }
    }
}

class ServerChildHandler extends ChannelInboundHandlerAdapter{

    //把通道加入通道组
    @Override
    public void channelActive(ChannelHandlerContext ctx) throws Exception {
        Server.clients.add(ctx.channel());
    }

    @Override
    public void channelRead(ChannelHandlerContext ctx, Object msg) throws Exception {
        ByteBuf buf = null;
        try{
            buf = (ByteBuf) msg;
            byte[] bytes = new byte[buf.readableBytes()];
            buf.getBytes(buf.readerIndex(),bytes);
            System.out.println(new String(bytes));

            //该函数会自动释放资源
            //把消息群发给所有客户端
            Server.clients.writeAndFlush(buf);

            //把消息返回给发送消息的客户端
            //ctx.writeAndFlush(buf);

        }finally {
            if(buf!=null){
                //writeAndFlush会自动关闭资源，无需手动关闭
                //ReferenceCountUtil.release(msg);
            }
        }
    }

    //异常处理
    @Override
    public void exceptionCaught(ChannelHandlerContext ctx, Throwable cause) throws Exception {
        //打印异常
        cause.printStackTrace();
        //关闭资源
        ctx.close();
    }
}
```
## 要点
- netty 的读写都是使用的 ByteBuf 类，这是对 NIO 的 ByteBuffer 类的封装，更加方便使用。
- ByteBuf 需要关闭资源，但是 `writeAndFlush` 方法会自动关闭 ByteBuf
	- 只读数据不写数据时，要手动关闭 ByteBuf
	- 有写数据时，不需要手动关闭 ByteBuf
- Netty 中一切操作都是异步的，如果需要使用同步等待另一端的消息时，需要调用 `ChannelFuture.sync()` 方法
	- `ChannelFuture.channel().closeFuture().sync()` 方法会将程序一直运行，直到调用 `close()` 方法，用这个可以使客户端或服务器的长时间启动
- 一对一通信和转发的区别
	- 客户端没区别
	- 服务器
		- ```java
		  public static ChannelGroup clients = 
			  new DefaultChannelGroup(GlobalEventExecutor.INSTANCE);
		  ```
		  开辟一个通道组，存储所有客户端的通道
		- ```java
			//把通道加入通道组
			@Override
			public void channelActive(ChannelHandlerContext ctx) throws Exception {
				Server.clients.add(ctx.channel());
			}
		  ```
		  在客户端与服务器的通道初始化时，调用该方法，将该通道加入通道组
		- 把消息群发给所有客户端<br>
            `Server.clients.writeAndFlush(buf);`
		- 把消息返回给发送消息的客户端<br>
			`ctx.writeAndFlush(buf);`
- 如何启动多个客户端[https://kekaiyuan.github.io//wiki/intellij-idea/#%E5%B9%B6%E5%8F%91%E5%90%AF%E5%8A%A8%E4%BB%A3%E7%A0%81](https://kekaiyuan.github.io//wiki/intellij-idea/#%E5%B9%B6%E5%8F%91%E5%90%AF%E5%8A%A8%E4%BB%A3%E7%A0%81)

# 源码链接
该文章源码链接[https://github.com/kekaiyuan/javaquestion/tree/main/nettydemo](https://github.com/kekaiyuan/javaquestion/tree/main/nettydemo)