---
layout: post
title: template page
categories: [cate1, cate2]
description: some word here
keywords: keyword1, keyword2
---

# socket通信的read方法阻塞问题
socket通信用的是InputStream和OutputStream这两个Io流，其中InputStream的read方法会有阻塞的风险
- 原因
	- read方法在读完所有的数据，检测到流的末尾时，会返回值-1。这往往作为我们结束循环的标志。
	   ```java
		while (read() != -1) {
			read();
		}
	   ```
	- 在读取文件时，文件的长度是可知的，流的末尾是可见的。
	- 但在socket通信时，流的末尾却是不可见的。接受方并不知道发送方是否还会继续发送数据，这时read方法就会阻塞。
		- 这很容易理解，假设现在有两个人通过socket传输一份文件，在一份文件传输完毕后，人脑知道已经没有下一份文件了，可以结束通信。但是电脑并不知道，它觉得可能还会有第二份文件、第三份文件...所以它就会阻塞地等待下一个数据的到来。
- 解决方法
	- 接收方
		- 设置超时，会在设置的超时时间到达后抛出SocketTimeoutException异常而不再阻塞。
		- 在头部约定好数据的长度。当读取到的长度等于这个长度时就不再继续调用read方法。
		- 约定结束标志，当读到该结束标志时退出不再read。
	- 发送方
		- 调用socket的shutdownOutput方法关闭输出流，该方法的文档说明为，将此套接字的输出流置于“流的末尾”，这样另一端的输入流上的read操作就会返回-1。不能调用socket.getInputStream().close()。这样会导致socket被关闭。
		- 调用socket.close()