> IO常用工具类
```xml
<dependency>
	<groupId>commons-io</groupId>
	<artifactId>commons-io</artifactId>
	<version>2.5</version>
</dependency>
```
##### 详解
```txt
1.IOUtils 包含一些工具类,用于处理读,写和拷贝,这些方法基于 InputStream, OutputStream, Reader 和 Writer工作.
2.FileUtils 包含一些工具类,它们基于File对象工作，包括读，写，拷贝和比较文件
3.FilenameUtils包含一些工具类,它们基于文件名工作而不是File对象。这个类旨在 在Unix和Windows环境下保持一致,帮助在两个环境下过渡(如从开发环境到生成环境)
4.FileSystemUtils包含一些工具类，基于文件系统访问功能不被JDK支持。目前，只有一个方法就是得到驱动器空余空间。注意这使用命令行而不是 native code。
5.EndianUtils 包含静态方法来交换Java基本类型和流的字节序
6.SwappedDataInputStream实现了DataInput接口。可以从文件中读取非本地字节序
```
https://blog.csdn.net/backbug/article/details/99572931
