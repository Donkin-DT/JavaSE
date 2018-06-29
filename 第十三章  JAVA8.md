###### 第十三章  JAVA8

```java
1.JAVA8 新特性
2.Lambda 表达式 😀
3.函数式接口 😀
4.方法引用 😀
5.构造器引用
6.数组引用 😀
7.Stream API 😀
```

###### 新特性

```java
引入：
	14年    推出：java8 jdk8 （jdk5 之后改革最大一个版本）😀
	14年3月 推出：jdk8 LTS版本（长期版本）
	17年9月 推出：jdk9 （六个月版本升级计划）
	18年3月 推出：jdk10
	18年9月 推出: jdk11 LTS版本

新特性:
	1.Lambda 表达式
	2.StreamAPI
	3.Optional 类(为了减少空指针异常）
	4.Nashone 引擎（jvm 上运行 js）
	5.新日期 API
	6.接口中可以定义默认方法和静态方法
```

###### Lambda表达式😀

```java
1.Lambda 表达式
	理解：
		Lambda 表达式实际上是一个匿名函数，理解成一段可以传递的代码传递给方法
		Lambda 表达式其实就是匿名内部类的代替版

		匿名内部类应用场景：
			场景1：当做接口实例赋值给一个接口
				接口A a = new 接口A(){};

			场景2：当做接口实例传递给方法
				method(new 接口A(){});	
				public void method(接口A a){
					//调用a的方法
				}
		
		Lambda 表达式的应用场景同匿名内部类

  	好处：
		语句更加简洁，更加紧凑，使java的表达能力得到提升！

	语法：(参数类型 参数名,参数类型 参数名)->{Lambda体}

	注意：
		①参数类型可以省略
		②如果仅仅只有一个参数，小括号可以省略
		③如果Lambda体中仅仅只有一句话，则大括号可以省略
		④如果Lambda体中仅有的一句话为return语句，则return关键字可以省略。
            
	案例：
		public class TestLambda1 {
			//匿名内部类
			@Test
			public void test1() {
				//匿名内部类
				TreeSet<String> set = new TreeSet<>(new Comparator<String>(){
					@Override
					public int compare(String o1, String o2) {
						return o1.compareTo(o2);
					}
				});
			}
	
			//测试Lambda
			@Test
			public void test3() {
				//匿名内部类
				TreeSet<String> set = new TreeSet<>((o1,o2)->o1.compareTo(o2));
			}
		}


2.方法引用😀
	理解:
		方法引用，本质是 Lambda 表达式,可以作为函数式接口实例出现
		必须满足一定的要求，才能使用方法引用

	特点:
		能用方法引用的，肯定能用 Lambda 表达式
		能用 Lambda 表达式的，不一定能用方法引用，必须满足以下要求
	
	要求:😀
		1.Lambda 体中仅仅有一条语句
		2.仅有一条语句为方法调用
		3.调用方法与抽象方法参数列表和返回类型一致
		
		注意：如果抽象方法第一个参数和调用方法调用者一致，可以使用：类名::普通方法😀
	
	语法:
		1.对象::普通方法
		2.类名::静态方法
		3.类名::普通方法
	
	案例：😀
			1.对象::普通方法
			@Test
			public void testCase1() {
				//匿名内部类
				Consumer<Character> c = new Consumer<Character>(){
					@Override
					public void accept(Character t) {
						System.out.println(t);
					}};
                
				//方法引用😀
				Consumer<Character> c2 = System.out::println;
			}
            
			2.类名::静态方法
			@Test
			public void testCase2() {
				//匿名内部类
				Comparator<Integer> com1 = new Comparator<Integer>(){
					@Override
					public int compare(Integer o1, Integer o2) {
						return Integer.compare(o1, o2);
					}
				};
				
				//方法引用😀
				Comparator<Integer> com2 = Integer::compare;
			}
	
			3.类名::普通方法
			@Test
			public void testCase3() {
				//匿名内部类
				Function<Employee,String> fun1 = new Function<Employee,String>(){
					@Override
					public String apply(Employee t) {
						return t.getName();
					}
				};
				
				//方法引用😀
				Function<Employee,String> fun2= Employee::getName;
			}
            
			4.类名::普通方法
			@Test
			public void testCase3_2() {
				//匿名内部类
				Comparator<String> com1 = new Comparator<String>(){
					@Override
					public int compare(String o1, String o2) {
						return o1.length();
					}
				};
		
				//方法引用😀
				Comparator<String> com2 = String::compareTo;
			}
	
			5.类名::普通方法
			@Test
			public void testCase3_3() {
				//匿名内部类
				BiPredicate<String, String> b1 = new BiPredicate<String, String>() {
					@Override
					public boolean test(String t, String u) {
						return t.equals(u);
					}
				};
				
				//方法引用😀
				BiPredicate<String, String> b2 = String::equals;
			}
			
3.造器引用
	理解:
		构造器引用，本质上是 Lambda 表达式，作为函数式接口实例出现
		能用构造器引用的，肯定能用 Lambda 表达式
		但能用 Lambda 表达式的，不一定能用构造器引用，必须满足一定的要求

	要求：😀
		1.Lambda 体中仅仅只有一句话
		2.仅有的一句话为构造器调用，即：return new XX();
		3.构造器参数列表和抽象方法参数列表一致
		
 	语法： 
 		类名::new
           
    案例：
		1.返回一个String对象  new String();
		@Test
		public void test1() {
			//匿名内部类
			Supplier<String> sup1 = new Supplier<String>(){
				@Override
				public String get() {
					return new String();
				}
			};
		
			//构造器引用
			Supplier<String> sup2 = String::new;
			System.out.println(sup2.get());
		}

		2.返回一个File对象   new File(String pathName)
		@Test
		public void test2() {
           	 //匿名内部类
			Function<String,File> fun1 = new Function<String,File>(){
				@Override
				public File apply(String t) {
					return new File(t);
				}
			};
		
			//构造器引用
			Function<String,File> fun2 = File::new;
		}
	
		3.返回一个Employee对象   new Employee("范瑶",34,'男',8000)
         interface A<T,U,R,M,N>{
			N method(T t,U u,R r,M m);
		}

		@Test
		public void test3() {
			//匿名内部类
			A<String,Integer,Character,Double,Employee> a1 = new  	A<String,Integer,Character,Double,Employee>(){
				@Override
				public Employee method(String t, Integer u, Character r, Double m) {
					return new Employee(t,u,r,m);
				}	
			};

			//构造器引用
			A<String,Integer,Character,Double,Employee> a2 = Employee::new;
		}

4.数组引用😀
	理解：
		数组引用 本质上就是 Lambda 表达式的简化。作为函数式接口的实例出现的。
		能用数组引用的地方，肯定能用 Lambda 表达式
		能用 Lambda 表达式的地方，不一定能用数组引用，必须满足以下要求
		
	要求：😀
		1.Lambda 体中仅仅只有一句话
		2.仅有的一句话为返回一个数组对象，形式：return new XX[长度];
		3.抽象方法的参数为数组的长度类型，返回类型为数组类型
		
	语法：
		数组类型[]::new
            
	总结：
		1.匿名内部类
		2.Lambda 表达式
		3.方法引用|构造器引用|数组引用
		
	案例：
		//返回一个长度为5的 String 数组
		@Test
		public void test1() {
			//匿名内部类
			Function<Integer,String[]> fun1 = new Function<Integer,String[]>(){
				@Override
				public String[] apply(Integer t) {
					return new String[t];
				}	
			};
			
			//数组引用
			Function<Integer,String[]> fun2 = String[]::new;
			String[] apply = fun2.apply(5);
			
			for (String string : apply) {
				System.out.println(string);
			}
		}
		

```

###### StreamAPI😀

```java
理解：	
	1.Stream 讲究的是“计算”，可以处理数据，但不能更新源数据
	2.Stream 属于“惰性操作”，必须等待终止操作执行后，前面的中间操作或开始操作才会处理
	3.Stream 只能消费一次，一旦消费，就不能再次使用，除非重新创建 Stream 对象
	4.Stream 中间操作可以有0个或多个，每个操作都会返回一个新 Stream
	5.Stream 相当于一个更强大的 Iterator，可以处理更加复杂的数据，并且实现并行化，效率更高
	
1.StreamAPI开始操作
	创建对象：😀
			方式1：通过集合获取 StreamI 😀
			方式2：通过数组获取 Stream
			方式3：通过一系列值获取 Stream
			方式4：生成无限流
	案例：
		方式1：通过集合获取Stream 
			@Test
			public void test1() {
				//创建集合对象
				List<Employee> list = EmployeeData.getEmployees();
				//开始操作：获取Stream对象
					//获取串行流
						Stream<Employee> stream = list.stream();
					//获取并行流
						Stream<Employee> parallelStream = list.parallelStream();		
				//终止操作
					stream.forEach(System.out::println);
			}
	
		方式2：通过数组获取Stream
			@Test
			public void test2() {
				String[] arr = {"john","lucy","lily"};
				//开始操作：获取Stream对象
				Stream<String> stream = Arrays.stream(arr);
				//终止操作
				stream.forEach(System.out::println);
			}
			
		方式3：通过一系列值获取Stream
			@Test
			public void test3() {
				//开始操作：获取Stream对象
				Stream<Integer> of = Stream.of(1,2,3,4,5,6);
				//终止操作
				of.forEach(System.out::println);
			}
			
		方式4：生成无限流
			@Test
			public void test4() {
				//匿名内部类
				Stream<Double> generate = Stream.generate(new Supplier<Double>(){
					@Override
					public Double get() {
						return Math.random();
					}
				});
				//开始操作：获取Stream对象
				Stream<Double> generate = Stream.generate(Math::random);
				//终止操作
				generate.forEach(System.out::println);
			}

2.StreamAPI中间操作😀
	分类：
		1.filter(Predicate):接收 Lambda,从流中排除某些元素
			Stream<T> filter(Predicate<? super T> predicate)
    			Returns a stream consisting of the elements of this stream that match the given predicate. 
    
 		2.skip（n）：跳过元素，返回一个扔掉了前 n 个元素的流。若流中元素不足 n 个，则返回一个空流。与 limit(n) 互补
 			Stream<T> skip(long n)
    			Returns a stream consisting of the remaining elements of this stream after discarding the first n elements of the stream.
 		
		3.limit(n):截断流，使其元素不超过给定数量
			Stream<T> limit(long maxSize)
    			Returns a stream consisting of the elements of this stream, truncated to be no longer than maxSize in length. 
    
		4.distinct():筛选，通过流所生成元素的 hashCode() 和 equals() 去除重复元素
			Stream<T> distinct()
    			Returns a stream consisting of the distinct elements (according to Object.equals(Object)) of this stream. 
    
		5.map(Function):接收一个函数作为参数，将元素转换成其他形式或提取信息，该函数会被应用到每个元素上，并将其映射成一个新的元素
			<R> Stream<R> map(Function<? super T,? extends R> mapper)
    			Returns a stream consisting of the results of applying the given function to the elements of this stream. 
    
		6.flatMap(Function):接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流
			<R> Stream<R> flatMap(Function<? super T,? extends Stream<? extends R>> mapper)
    			Returns a stream consisting of the results of replacing each element of this stream with the contents of a mapped stream produced by applying the provided mapping function to each element. 
    
		7.sorted():自然排序，要求：元素类型实现 Comparable 接口
			Stream<T> sorted()
    			Returns a stream consisting of the elements of this stream, sorted according to natural order. 
    
 		8.sorted(Comparator):定制排序
 			Stream<T> sorted(Comparator<? super T> comparator)
    			Returns a stream consisting of the elements of this stream, sorted according to the provided Comparator. 

	特点：😀
		1.中间操作可以多次拼接
		2.中间操作是一个延迟操作，也就是必须等待最后的终止操作后，才能处理
		3.Stream 只能消费一次
		
	案例：
		List<Employee> list = EmployeeData.getEmployees();
		@Test
		public void test1() {
			//1.获取Stream
				Stream<Employee> stream = list.stream();
			//2.中间操作
				//2-1 .filter
					Stream<Employee> filter = stream.filter(t->t.getAge()>40);
				//2-2. limit 限制元素个数<=指定的n
            		Stream<Employee> limit = stream.limit(5);
				//2-3 .skip 跳过n项
					Stream<Employee> skip = stream.skip(3);
				//2-4 .distinct 去重
					Stream<Employee> distinct = stream.distinct();
			//3.终止操作
				distinct.forEach(System.out::println);
		}
	
		@Test
		public void test2() {
			//1.开始操作：获取Stream对象
				Stream<Employee> stream = list.stream();
			//2.中间操作
			//2-1.map 
				Stream<String> map = stream.map(Employee::getName);
			//2-2.flatMap
			//匿名内部类
				stream.flatMap(new Function<Employee,Stream<Object>>(){
					@Override
					public Stream<Object> apply(Employee t) {
						return TestStreamMiddle.empToStream(t);
					}
				});
			//方法引用
				Stream<Object> flatMap = stream.map(TestStreamMiddle::empToStream);
            
			//3.终止操作
				flatMap.forEach(System.out::println);
		}
	
		@Test
		public void test3() {
			//1.获取Stream对象
				Stream<Employee> stream = list.stream();
			//2.中间操作
			//2-1. sorted
				Stream<Employee> sorted = stream.sorted();
			//2-2. sorted定制排序
				Stream<Employee> sorted = stream.sorted((o1,o2)->Integer.compare(o1.getAge(),o2.getAge()));
			//3.终止操作
				sorted.forEach(System.out::println);
		}

		@Test
		public void exec1() {
			list
                .stream()
                .filter(t->t.getAge()>40)
                .forEach(System.out::println);
		}
	
		@Test
		public void exec2() {
			list
                .stream()
                .limit(8)
                .skip(3)
                .forEach(System.out::println);
		}

		@Test
		public void exec3() {
			list
                .stream()
                .map(Employee::getName)
                .filter(t->t.length()>2)
                .forEach(System.out::println);
		}

		@Test
		public void exec4() {
		list
            .stream()
            .map(Employee::getSalary)
            .limit(6)
            .skip(2)
            .forEach(System.out::println);
		}

		@Test
		public void exec5() {
			list
             	.stream()
				.map(Employee::getAge)
				.distinct()
				.sorted(Integer::compare)
				.limit(6)
				.skip(1)
				.forEach(System.out::println);
		}
```

