###### 第一章 初始 JAVA

```java
1.程序
2.计算机语言介绍
3.Java 语言介绍
4.Java 程序运行机制 
5.JDK 下载、安装和配置
6.开发 Java 程序步骤
7.开发 Java 第一个程序
8.常见注释和 API
9.常见 dos 命令
```

###### 介绍 😀

```java
1.程序:
	为了完成某个特定的功能而编写的一系列有序指令的集合

2.计算机语言介绍:
	第一代语言
		打孔机——纯机器语言
	第二代语言
		汇编语言
	第三代语言
		C、Pascal、Fortran面向过程的语言
		C++ 面向过程/面向对象的语言。
		Java 跨平台的纯面向对象的语言。
		.NET 跨语言的平台。

3.Java语言介绍:
	Java 语言背景和发展:
		91年 Java 语言的前身：oak
		Green “绿色计划”项目，需要一门编程语言，也就是oak（橡树），后来改名为 Java。
		95年 Java 语言正式问世，推出Java1.0版本，很快推出1.1版本，创始人；詹姆斯.高斯林（高司令）
		
	Java 语言版本:
		95年 Java 语言问世 😀 
		96年 jdk1.0 版 😀 
		98年 jdk1.2 版（集合等），J2SE、J2ME、J2EE 😀 

		04年 jdk1.5版（增强 for 循环，枚举、注释等），将jdk的发布版本更名为：jdk5.0  😀 
		06年 jdk1.6版（为了稳固 jkd1.5），JavaSE、JavaME、JavaEE  😀 

		09年 oracle 公司收购了 sun 公司  😀 
		11年 jdk1.7版（ Switch 结构变量中支持 String 类型）
		14年 jdk1.8版（多了 Lambda 表达式和 StreamAPI ） 😀 
		17年 jdk1.9版（模块化、shell 命令、接口中可以有私有的）
		18年 jdk1.10版（多了局部变量类型推断）
		
	Java 语言的技术平台:😀 
		JAVASE（Java Standard Edition）Java 标准版：桌面级应用
		JAVAME（Java Micro Edition）Java 微型版：手机端应用
		JAVAEE（Java Enterprise Edition）Java 企业版：企业级应用
		
	4.Java 程序运行机制 😀 
		JAVA运行机制的相关概念
		JVM：Java虚拟机，实现跨平台性，可以执行，存储、处理等。
		JRE：Java运行环境，JRE=JVM + 核心类库
		JDK：Java开发工具包，JDK=JRE + 开发工具
		
		如果只想运行程序，则安装JRE即可。
		如果想开发程序，则安装JDK即可。

5.JDK 的下载、安装和配置 😀 
	下载：
		www.oracle.com
			
	安装:
		傻瓜式安装
		建议：
			安装路径不要有中文或者特殊符号如空格等
			当提示安装JRE时，可以选择不安装
		配置: 😀 
			右击计算机——属性——高级系统配置——环境变量
			推荐使用：
				① 新建：JAVA_HOME：C:\Program Files\Java\jdk1.8.0_131
				② 在path中引用JAVA_HOME：“%JAVA_HOME\bin；”

6.开发 Java 程序的步骤
	1.编写Java程序（.java）
	2.编译Java程序（.class）
		将java文件 ——>字节码文件
		语法：
			javac 文件名.java
	3.运行
		JVM 中执行字节码文件
		语法：
			java 文件名(类名)
                    
7.开发Java第一个程序
	Java 程序框架：
		public class Test{
			public static void main(String[] args){
				System.out.println("Hello World!");
			}
		}
		
	Java语法特点：😀 
		每条语句的结尾使用分号；
		严格区分大小写
		所有的符号都是英文状态下的
		括号、引号都是成对出现的
		最好有缩进
		
	Java开发时的细节问题：
		文件名和类名可以不一致吗？
			可以，但要求类不能使用 public 修饰（使用 public 修饰的类名必须和文件名一致）
		一个文件可以有多个类吗？
			可以，但是只允许有一个类用 public 修饰，并且要和文件名一致
			
8.常见的注释和 API
	说明：辅助解释说明程序的文字；
	目的：提高代码的阅读性；
	建议：言简意赅的文字，
	特点：编译器忽略注释文字。
	写法：
		单行注释：//  可以放在类、方法或其他关键语句的上方或右方
		多行注释：/* 文字 */  可以放在类、方法或其他关键语句的上方
		文档注释：/** 文字*/  可以放在类或方法的上方
		备注：可以通过javadoc命令生成帮助文档，只有帮助文档注释才能在帮助文档显示。
			生成语法：
				javadoc -d mydoc 文件名.java

	转义字符：😀 
		\t：一个制表符，相当于一个 tab 键（大概4个空格）
		\n：一个换行符
		\\：一个反斜杠
		\"：一个双引号
		\'：一个单引号
		\r：一个回车，将光标定位到当前行的行首（有可能覆盖内容）

9.常见 dos 命令
	DOS介绍
	场景：
		打开E盘，新建一个目录 study，然后打开 study，新建三个目录 a  b  c 然后再打开 a目录，新建一个文件 news.txt,然后将 news.txt 复制到 c目录下，然后将a目录下的 news.txt 剪切到 study 下最后，将整个 study 删除
	切换目录：😀 
		“盘符号：“：切换到指定盘下
		cd 绝对路径/行对路径 ：切换到对应的目录下
		cd.. ：退回上一级
		cd\ :退回到根目录
		
	新建目录：😀 
		md 目录名 目录名 ：新建目录，支持新建多个文件夹
		
	查看目录子级：
		dir
		新建或编辑文件：
		”echo 文件内容 > 文件名.后缀名“
		
	查看文件内容：
		type 文件名.后缀名
		
	复制或移动文件:
		copy 源文件路径\文件名 目标路径\文件名
		D:\demo>copy D:\demo\a\a.txt D:\demo\b
		
		move 源文件路径\文件名 目标路径\ 文件名 
		D:\demo>move D:\demo\a\a.txt D:\demo\b
		
	删除目录:
		rd /s /q 目录名
		rd 只能删除空目录
		/s 可以删除目录树
		/q 不带询问的删除
		
	删除文件:
		del 文件名
		
	清屏:
		cls
		
	退出:
		exit

10、switch 用法：
https://www.cnblogs.com/fawei/p/6380383.html
```

