###### 第九章 IO流

```java
1.File类
2.IO流相关概念
3.体系图
4.四个基础流
5.四个缓冲流
6.两个对象流
7.其他小流：
	转换流
	打印流
	标准输入输出流
	
永久保存数据的介质：文件 数据库 网络
```

###### File类

```java
理解：
	java.io 包
	Class File 
	public class File
					extends Object
					implements Serializable, Comparable<File>
					
说明：
	一个 File 对象表示一个磁盘中文件或目录。
	每一个文件或目录,可以通过一个路径字符串来构建。（路径字符串 + 分隔符）
	File 可以实现对文件或目录的操作,如:创建、重命名、删除、获取大小,但不能进行文件的读写
	
创建对象：😀
	new File(String pathName):
		根据路径字符串指向的文件或磁盘创建一个 File 对象
	new File(File parent,String childName):
		根据父目录和路径字符串指向的文件或磁盘创建一个 File 对象
	new File(String parent,String childName):
		根据父目录和路径字符串指向的文件或磁盘创建一个 File 对象
		
常见方法：😃
	1.exists：判断文件或目录是否存在
		public boolean exists()
            Tests whether the file or directory denoted by this abstract pathname exists.
                 
	2.mkdir:创建一级目录
		public boolean mkdir()
        	Creates the directory named by this abstract pathname.
        
	3.mkdirs:创建多级目录
		public boolean mkdirs()
        	Creates the directory named by this abstract pathname, including any necessary but nonexistent parent directories.
        
	4.delete：删除文件或空目录
		public boolean delete()
        	Deletes the file or directory denoted by this abstract pathname.
        
	5.deleteOnExit:删除文件或空目录
		public void deleteOnExit()
        	Requests that the file or directory denoted by this abstract pathname be deleted when the virtual machine terminates. 
        
	6.createNewFile:创建新文件，如果文件目录不存在，会抛异常 IOException
		public boolean createNewFile() throws IOException
			Atomically creates a new, empty file named by this abstract pathname if and only if a file with this name does not yet exist.
                
	7.getName:获取文件名
		public String getName()
        	Returns the name of the file or directory denoted by this abstract pathname.
        
	8.getAbsolutePath:获取文件路径
		public String getAbsolutePath()
        	Returns the absolute pathname string of this abstract pathname.
        
	9.getParent:获取父级，类型 String
		public String getParent()
        	Returns the pathname string of this abstract pathname's parent, or null if this pathname does not name a parent directory.
        
	10.getParentFile:获取父级，类型 File
		public File getParentFile()
        	Returns the abstract pathname of this abstract pathname's parent, or null if this pathname does not name a parent directory.
        
	11.isFile:判断是否是文件
		public boolean isFile()
        	Tests whether the file denoted by this abstract pathname is a normal file.
        
	12.isDirectory:判断是否是目录
		public boolean isDirectory()
			Tests whether the file denoted by this abstract pathname is a directory.
        
	13.isHidden:是否隐藏
		public boolean isHidden()
        	Tests whether the file named by this abstract pathname is a hidden file.
        
	14.list:获取当前一级子级，类型 String[]
		public String[] list()
        	Returns an array of strings naming the files and directories in the directory denoted by this abstract pathname.
        
	15.listFiles:获取当前的一级子级，类型 File[]
		public File[] listFiles()
        	Returns an array of abstract pathnames denoting the files in the directory denoted by this abstract pathname. 
		
delete 与 deleteOnExit对比：
					返回类型		删除时机
	delete			 boolean		立即
	deleteOnExit	 void			程序结束时
```
###### 递归😃

```java
理解：
	1、定义在方法中
	2、自己调用自己
	3、必须有出口条件，反之报栈溢出异常

案例：
	//1.打印目录
	public static void printFiles(File filePath) {
		File[] files = filePath.listFiles();
		
		for (File file : files) {
			System.out.println(file.getName());//递归调用实际操作
			
			if(file.isDirectory()) {
				printFiles(file);
			}
		}
	}
	
	//2.获取目录大小
	public class TestFile {
		public static void main(String[] args) {
			File file = new File("D:\\DonkinFiles\\a");
			System.out.println("文件大小为：" + sizeOfFiles(file));
		}
	
		public static long sizeOfFiles(File file) {
			if(file.isFile()) {
				return file.length();
			}
            
			long size = 0L;
			File[] files = file.listFiles();
            
			for (File file2 : files) {
				size += sizeOfFiles(file2);
			}
            
			return 	size;
		}
	}
```
###### IO流😀

```java
概念：
	通过 IO 体系中的类，实现数据在两个设备之间像“水流”一样进行传输

特点：
	1、IO 体系中，数据传输时其中一个节点必须是程序
	2、都以程序为基准的，进入程序叫做输入（读取），从程序出去的叫做输出（写入）

分类:😀
	按流向不同：
		输入流：
			从其他节点（文件、网络、内存、键盘）流向程序
		输出流：
			从程序流向其他节点（文件、网络、内存、显示器）
	
	按传输单位不同：
		字节流：
			一个字节一个字节传输（8位），比较适合处理二进制文件，比如图片、音频、视频等，效率较低
		字符流：
			一个字符一个字符传输，比较适合处理纯文本文件，效率较高
	
	按功能不同：
		节点流（基础流）：
			主要做读写，用于连接两个节点
		处理流（包装流）：
			在节点流基础上，提高一些其他功能，比如提高效率
		
体系图：😀
	字节输入流InputStream:抽象类
		文件字节输入流 FileInputStream：文件——>程序
		缓冲字节输入流 BufferedInputStream:提高效率
		对象输入流 ObjectInputStream:序列化
	
	字节输出流 OutputStream:抽象类
		文件字节输出流 FileOutputStream:程序——>文件
		缓冲字节输出流 BufferedOutputStream :提高效率
		对象输出流 ObjectOutputStream:反序列化
		
	字符输入流 Reader:抽象类
		文件字符读取流 FileReader:文件——>程序
		缓冲字符读取流 BufferedReader:提高效率
	
	字符输出流 Writer:抽象类
		文件字符写入流 FileWriter:程序——>文件
		缓冲值写入流 BufferedWriter:提高效率
```

###### 基础流😀

```java
FileInputStream😀
理解：
	java.io 包
	Class FileInputStream 
	public class FileInputStream
							  extends InputStream
							  
功能：
	按字节从文件到程序
	
创建对象：
	new FileInputStream(String pathName);
	new FileInputStream(File file);
	要求：
		指向的文件必须存在，否则报异常
		
常见方法:
	1.read():读取单个字节，如果读到文件末尾，返回 -1
        public int read() throws IOException
        	Reads a byte of data from this input stream. This method blocks if no input is yet available.
                
	2.read(byte[]):读取多个字节到byte数组，默认索引从0开始，如果读到文件末尾，返回-1
		public int read(byte[] b) throws IOException
			Reads up to b.length bytes of data from this input stream into an array of bytes. 
                
	3.read(byte[]，off,len):读取多个字节到 byte 数组的指定部分，默认索引从 off 开始，如果读到文件末尾，返回-1
	public int read(byte[] b,int off,int len)throws IOException
		Reads up to len bytes of data from this input stream into an array of bytes.

	4.close()：关闭
		public void close() throws IOException
			Closes this file input stream and releases any system resources associated with the stream. 

FileOutputStream😀
理解：
	java.io包
	Class FileOutputStream
	public class FileOutputStream
							   extends OutputStream

功能：
	按字节从程序到文件
	
创建对象：
	new FileOutputStream(String pathName);
        构建一个 FileOutputStream 对象，默认指针指向文件的首端 【覆盖】
	new FileOutputStream(File file);
		构建一个 FileOutputStream 对象，默认指针指向文件的首端 【覆盖】
	new FileOutputStream(String pathName,true);
		构建一个 FileOutputStream 对象，默认指针指向文件的尾端 【追加】
	new FileOutputStream(File file,true);
		构建一个 FileOutputStream 对象，默认指针指向文件的尾端 【追加】
		
注意：
	如果文件不存在，会自动创建；如果文件存在，则编写
	
常见方法：
	1.write(int)：
		public void write(int b) throws IOException 
			Writes the specified byte to this file output stream.
            
	2.write(byte[]):
		public void write(byte[] b) throws IOException
			Writes b.length bytes from the specified byte array to this file output stream.

	3.write(byte[],off,len)：
		public void write(byte[] b,int off,int len)throws IOException
			Writes len bytes from the specified byte array starting at offset off to this file output stream.
            
    4.close()：
    	public void close() throws IOException
    		Closes this file output stream and releases any system resources associated with this stream.
 
FileReader😀
理解：
	java.io 包
	Class FileReader 
	public class FileReader 
        				extends InputStream
        				
功能：
	按字符从文件到程序
	
创建对象：
	new FileReader(String pathName);
	new FileReader(File file);
	要求：
		指向的文件必须存在，否则报异常
		
常见方法：
	1.read():读取单个字符，如果读到文件末尾，返回 -1
		public int read() throws IOException
			Reads a single character.
        
	2.read(char[]):读取多个字符到byte数组，默认索引从0开始，如果读到文件末尾，返回 -1
		public int read(char[] cbuf) throws IOException
			Reads characters into an array.
        
	3.read(char[]，off,len):读取多个字符到char数组的指定部分，默认索引从off开始，如果读到文件末尾，返回-1
		public abstract int read(char[] cbuf,int off,int len) throws IOException
			Reads characters into a portion of an array. 

	4.close()：关闭
 		public void close() throws IOException
			Closes the stream and releases any system resources associated with it.
        
FileWriter😀
理解：
	java.io 包
	Class FileWriter
	public class FileWriter
						extends OutputStreamWriter
						
功能：
	按字符从程序到文件
	
创建对象：
	new FileWriter(String pathName);
        构建一个FileWriter，默认指针指向文件的首端 【覆盖】
	new FileWriter(File file);
		构建一个FileWriter对象，默认指针指向文件的首端 【覆盖】
	new FileWriter(String pathName,true);
		构建一个FileWriter对象，默认指针指向文件的尾端 【追加】
	new FileWriter(File file,true);
		构建一个FileWriter对象，默认指针指向文件的尾端 【追加】
	注意：
		如果文件不存在，会自动创建；如果文件存在，则编写
		
常见方法：😀
	1.write(int)：
		public void write(int c) throws IOException
			Writes a single character.
            
	2.write(char[])：
		public void write(char[] cbuf) throws IOException
			Writes an array of characters.
            
	3.write(char[],off,len)：
		public void write(char[] cbuf,int off,int len) throws IOException
			Writes a portion of an array of characters.

	4.write(string)：
		public void write(String str) throws IOException
			Writes a string.
            
	5.write(string,off,len)：
		public void write(String str,int off,int len) throws IOException
			Writes a portion of a string.

	6.flush：😀
		public void flush() throws IOException
			Flushes the stream.

	7.close() 😀：必须关闭或刷新，否则写入不到文件，只是写到缓存, close 方法内部先调用了 flush
		public void close() throws IOException
			Closes the stream, flushing it first. Once the stream has been closed, further write() or flush() invocations will cause an IOException to be thrown.
            
相关API：😀
	new String(byte[]):
		将整个字节数组转换成字符串
	new String(byte[],off,len)：
		将字节数组中 len 个字节（索引从off开始）转换成字符串
	string.getBytes():
		将字符串转换成byte[]
	string.toCharArray():
		将字符串转换成char[]
```

###### 处理流😀

```java
缓冲流😀
	理解：
		1.处理流
		2.提高效率
  		3.多了一些新方法
  	
 	分类：😀
 		1.BufferedInputStream
  		2.BufferdOutputStream
  		3.BufferedReader
   		4.BufferedWriter
    
	特点：
  		①必须套接在对应节点流（基础流）之上
  		②只需关闭外层流即可
    	③BufferedReader多了一个readLine😀
    	④BufferedWriter多了一个newLine😀
    
	使用步骤：
		1.创建流对象，套接在对应基础流上
		2.读写
		3.关闭
		
对象流
	功能：
		提供了一系列读写基本类型和对象类型数据方法，而且可以实现将存储的对象再还原回来
		
	分类：😀
		1.ObjectOutputStream:
			对象字节输出流（序列化流） 将内存中的对象数据持久化到本地或网络
  		2.ObjectInputStream:
			对象字节输入流（反序列化流）将本地或网络中的对象数据再还原回来
```

###### 其他小流

  ```java
分类:
  	PrintStream
  	PrintWriter
  
特点:
	1.提供了一系列比较方便的方法，用于打印，如:print、println、printf
	2.只有输出，没有输入
	3.除了构造器之外，调用打印方法，都不会抛异常
	4.具备自动刷新功能
		PrintStream：
			如果自动刷新功能被开启，也仅仅当调用 println 或调用 write + newLine 或调用 write + write('\n') 时，才有效
  		PrintWriter:
			如果自动刷新功能被开启，也仅仅当调用 println 或调用 printf 或调用 format 方法时，才有效
	5、可以指定编码格式
  ```

###### 转换流😀

```java
分类：
 	1.InputStreamReader
 	2.OutputStreamWriter

功能:
  	1、更高效
  	2、很好处理中文问题
    3、提供了较方便读写方法
    
使用步骤:
  	1、创建转换流对象，指向一个字节流对象
  	2、调用读写方法
  	3、关闭
  	
特点:😀
	可以在创建转换流对象同时指定字符编码格式
```

###### 标准输入输出流

```java
分类:
	System.in: 标准输入流		 类型：InputStream		 默认设备：键盘
	System.out：标准输出流		类型：PrintStream		默认设备：显示器
	System.err：标准错误流		类型：PrintStream		默认设备：显示器

可重定向:😀
	System.setIn(InputStream)
	System.setOut(PrintStream)
	System.setErr(PrintStream)
```
