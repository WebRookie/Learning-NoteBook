## Java8特性知识体系详解

#### 函数编程

> 面向对象编程是对数据进行抽象；函数式编程是对行为进行抽象

*  Lambda表达式的特点
* Lambda表达式使用和Stream下的接口
* 函数接口定义和使用，四大内置函数接口Consumer, Function, Supplier, Predicate
* Comparator排序为例。



#### Optional类

> 这是一个可以为null的容器对象，如果值存在，则isPresent()方法会返回true, 调用get() 方法会返回该对象。

* Optional类的意义
* Optional类有哪些常用的方法
* 多重类嵌套Null值判断



#### default方法

> 默认方法给予我们修改接口而不破坏原来的是实现类的结构提供了便利，

* 为什么会出现默认方法？
* 接口中出现默认方法，且类可以多接口的，那和抽象类有什么区别？
* 多重实现的默认方法冲突怎么办？



#### 类型注解

* 什么是类型注解？
* 类型注解的作用是什么？
* 为什么会出现类型注解（JSR308）



#### 类型推断

* 什么是泛型？



#### JRE精简

* 为什么精简Java8 JRE， 及好处是啥？
* 紧凑的JRE分3种，分别是compact1、compact2、compact3，他们的关系是？
* 在不同平台上如何编译？



#### LocalDate / LocalDateTime

> Date / Calendar 槽点， Java8对其进行了重写

* Java 8 之前的Date有哪些槽点？（Calender 的所有属性都不可变，SimpleDateFormat线程不安全等）
* Java8之前使用哪些常用的 第三方时间库
* Java8关于时间和日期有哪些类和方法，相比Java8之前它的特点是什么？
* 其他语言时间库？



#### StampedLock

* 为什么会引入StampedLock
* 用Lock写悲观锁和乐观锁举例
* 用StampedLock 写悲观锁和乐观锁举例
* 性能对比





### Java8 - 函数编程（Lambda表达式）

面向对象编程是对数据进行抽象；函数式编程是对行为进行抽象。

核心思想: 使用不可变值和函数，函数对一个值进行处理，映射成另一个值。



#### lambdab表达式

* lambda表达式仅能放下如下代码：预定义使用了@Functional注释的函数式接口，自带一个抽象函数的方法，或者SAM（Single Abstract Method 单个抽象方法）类型。这些称为lambda表达式的目标类型，可以用作返回类型，或lambda目标代码的参数.
* lambda表达式内可以使用`方法引用`,仅当该方法不修改lambda表达式提供的参数。

```java
list.forEach(n -> System.out.println(n));
list.forEach(System.out::println);
```

如果对参数有任何修改，则不能使用方法引用，而需键入完整地lambda表达式;

```java
list.forEach((String s) -> System.out.prinln("*" + s + "*"));
```



事实上，可以省略这里的lambda参数的类型声明，编译器可以从列表的类属性推测出来

* lambda内部可以使用静态、非静态和局部变量，这称为lambda内的变量捕获。
* Lambda表达式在Java中又称为闭包或匿名函数，
* Lambda表达式有个限制，那就是只能引用final或final局部变量，就是说不能在lambda内部修改定义在作用域外的变量，另外，只是访问它而不作修改是可以的

```java
List<Integer> primes = Arrays.asList(new Integer[] {2, 3, 5, 7});
int factor = 2;
prime.forEach(element -> {System.out.println(factor * element); })
```





#### stream & parallelStream

每个Stream都有两种模式： 顺序执行和并行执行

顺序流：

```
List<Person> people = list.getStream.collect(Collectors.toList());
```

并行流

```
List<Person> people = list.getStream.parallel().collect(Collectors.toList());
```

顾名思义，当使用顺序方式去遍历时，每个item读完后再读下一个item，而使用并行去遍历时，数组会被分成多个段，其中每一个都在不同的线程中处理，然后将结果一起输出。



#### parallelStream 原理

```java
List originalList = someData;
split1 = originalList(0, mid); // 将数据分小部分
split2 = originalList(mid, end);
new Runnable(split1.process()); // 小部分执行操作
new Runnable(split2.process());
List revisedList = split1 + split2; // 将结果合并
```

其实处理大数据的核心思想就是大而化小，分配到不同的机器去运行map，最终通过reduce将所有机器的结果结合起来得到一个最终结果，与MapReduce不同，Streamn则是利用多核技术可将大数据通过多核并行处理，而MapReduce则可以分布式的。



#### stream与parallelStream性能测试对比

```java
long t0 = System.nanoTime();

//初始化一个范围100万整数流,求能被2整除的数字，toArray()是终点方法
int a[] = IntStream.range(0, 1_000_000).filter(p -> p % 2 ==0).toArray();

long t1 = System.nanoTime();

//和上面功能一样，这里是用并行流来计算
int b[] = IntStream.range(0, 1_000_000).parallel().filter(p -> p % 2 == 0).toArray();

long t2 = System.nanoTime();

System.out.printf("serial: %.2fs, parallel %.2fs%n", (t1 - t0) * 1e-9, (t2 - t1) * 1e-9);
// 本机结果serial: 0.05s, parallel 0.03s， 证明并行流确实比顺序流快。
```



#### Stream中常用方法如下：

* `stream()`, `parallelStream()`
* `filter()`
* `findAny()`, `findFirst()`
* `sort`
* `forEach()` void
* `map()`,`reduce()`
* `flatMap()` - 将多个Stream连接成一个Stream
* `collect(Collectors.toList())`
* `distinct`, `limit`
* `count`
* `min`, `max`, `summaryStatistics`





#### 常用例子

#### 匿名类简写

```java
new Thread( () -> System.out.println("In Java8, Lambda expression rocks !!")).start();

(params) -> expression
  (params) -> statement
  (params) -> { statements }
```



#### forEach

````java
// forEach
List features = Arrays.asList("lambda", "Default Method", "Stream Api", "Date and Time API");
features.forEach(n -> System.out.println(n));


// 使用Java 8的方法引用更方便，方法引用由:: 双冒号操作符表示，
features.forEach(System.out::println);

````



#### 方法引用

构造引用

```java
// Supplier<Student> s = () -> new Student();
Supplier<Student> s = Student::new;
```

对象:: 实例方法Lambda表达式的（形参列表）与实例方法的(实参列表)类型，个数是对应

```java
// set.forEach(t -> System.out.println(2));
set.forEach(System.out::println);

```

类名:: 静态方法

```java
// Stream<Double> stream = Stream.generate(() -> Math.random());
Stream<Double> stream = Stream.generate(Math::random);

```

类名::实例方法

```java
// TreeSet<String> set = new TreeSet<>((s1,s2) -> s1.compareTo(s2));
/* 编译器会提示： Can be replaced with Comparator.naturalOrder, 这句话说明，String已经重写了compareTo() 方法，这里写是多此一举。
这编译器就提示： Lambda can be replaced with method reference。
*/
TreeSet<String> set = new TreeSet<>(String::compareTo);
```



#### Filter & Predicate

常规用法

```java
public static void main(args []) {
  List languages = Arrays.asList("Java", "Scala", "C++", "Haskell", "Lisp");
  
  System.out.println("Languages which start with J :");
  filter(languages, (str) -> str.startsWidth("J"));
  
  System.out.println("Languages which ends with a ");
  filter(languages, (str) -> str.endsWith("a"));
  
      System.out.println("Print all languages :");
    filter(languages, (str)->true);
 
    System.out.println("Print no language : ");
    filter(languages, (str)->false);
 
    System.out.println("Print language whose length greater than 4:");
    filter(languages, (str)->str.length() > 4);

  
}


public static void filter(List<String> names, Predicate<String> condition) {
  names.stream().filter((name) -> (condition.test(name))).forEach((name) -> {
    System.out.println(name + "");
  })
}
```



#### Predicate

基本用法，

1. test(T t) 方法

   test方法主要用于参数符不符合规则，返回boolean

   ```java
   Predicate<String> forLetterLong = new Predicate<String>() {
     @Override
     public boolean test(String s) {
       return s.length() > 4 ? true : false;
     }
   }
   
   // 搭配lambda表达式 filter使用如下
   public static void main(String[] args) {
    List names = Arrays.asList("Java", "Scala", "C++", "Haskell", "Lisp","Hell","opt");
     Predicate<String> fourLetterLong1 = new Predicate<String>() {
       @Override
       public boolean test(String s) {
         return s.length() > 4 ? true : false;
       }
     };
     names.stream()
       .filter(fourLetterLong1)
       .forEach((n) -> System.out.println("this is:" + n));
   }
   ```

2. and(Predicate < ? super T > other)

```java
default Predicate<T> and (Predicate<? super T> other) {
	Objects.requireNonNull(other);
	return (t) -> test(t) && other.test(t);
}
```

and 方法等同于我们的逻辑与 && ，存在短路特性，需要所有条件满足

两边都是Predicate 类型

```java
Predicate<String> startswith = new Predicate<String>{
  @Override
  public boolean test(String s) {
    return s.equals("Haskell") ? true : false;
  }
}

names.stream()
  .filter(fourLetterLong.and(startsWidth))
  .forEach((n) -> System.out.println("this is:" + n));
```

3. or(Predicate< ? Super T> other)

```java
default Predicate<T> or (Predicate<? super T> other) {
	Objects.requireNonNull(other);
  return (t) -> test(t) || other.test(t);
}
```

or 等同于逻辑与 || 多个条件只要一个条件满足即可。

4. negate()

negate 等同于逻辑非。



5. isEqual(Object targetRef)方法

```java
static <T> Predicate<T> isEqual(Object targetRef) {
	return (null == targetRef) ? Objects::isNull : object -> targetRef.equals(object);
}
```

isEqual类似于equals（） 区别在于先判断对象是否是null，不为null的话 再使用equals方法进行比较。





#### Map & Reduce

map将集合类（例如列表）元素进行转换的，还有一个reduce()函数可以将所有值合并成一个

```java
List<Integer> costBeforeTax = Arrays.asList(100, 200, 300, 400, 500);
double bill = costBeforeTax.stream().map((cost) -> cost + .12* cost).reduce((sum, cost) -> sum + cost).get();
System.out.println("Total : " + bill)

```

#### Collectors

```java
// 将字符串换成大写并用逗号连接起来
List<String> G7 = Arrays.asList("USA", "Japan", "France", "Germany", "Italy", "U.K", "Canada");
String G7Countries = G7.stream().map(x -> x.toUpperCase()).collect(Collectors.joining("，"));
System.out.println(G7Countries);
```

* Colloctors.joining(",")
* Collectors.toList()
* Collectors.toSet() // 生成set集合
* Collectors.toMap(MemberModal::getUid， Function.identity())
* Collectors.toMap(IImageModel::getAid, o -> IMAGE_ADDRESS_PREFIX+ o.getUrl())



#### flatMap

将多个Stream连接成一个Stream

```java
List<Integer> result = Stream.of(Arrays.asList(1,3),  Arrays.asList(5,6)).flatMap(a -> a.stream()).collect(Collectors.toList());
```



#### distinct

去重

```java
List<likeDo> likeDOs = new ArrayList<LikeDO>();
List<Long> likeTidList = likeDOs.stream().map(LikeDO::getTid)
  .distinct().collect(Collectors.toList());
```



#### count

计总数

```java
int countOfAdult = persons.stream().filter(p -> p.getAge() > 18)
  .map(person -> new Adult(person))
  .count();
```



#### Match

```java
boolean anyStartsWithA = stringCollection
  .stream().anyMatch((s) -> s.startWidth("a"));

System.out.println(anyStartsWithA); // true

boolean allStartsWithA = stringCollection
  .stream().allMatch((s) -> s.startsWith());

System.println(allStartsWithA); // false

boolean noneStartsWithZ = stringCollection
  .stream().noneMatch((s) -> s.startsWith("z"));

System.out.println(noneStartsWithZ); // true
```



#### min, max, summaryStatistics

最小值，最大值

```java
List<Person> lists = new ArrayList<Person>();
lists.add(new Person(1L, "p1"));
lists.add(new Person(2L, "p2"));
lists.add(new Person(3L, "p3"));
lists.add(new Person(4L, "p4"));
Person a = lists.stream().max(Comparator.comparing(t -> t.getId())).get();
System.out.println(a.getId());

```

如果比较器涉及多个条件，比较复杂，可以定制

```java
Person a = lists.stream().min(new Comparator<Person>(){
  @Override
  public int compare(Person o1, Person o2) {
    if(o1.getId() > o2.getId()) return -1;
    if(o1.getId() < o2.getId()) return 1;
    return 0;
  }
}).get();
```

summaryStatistics

```java
//获取数字的个数、最小值、最大值、总和以及平均值
List<Integer> primes = Arrays.asList(2, 3, 5, 7, 11, 13, 17, 19, 23, 29);
IntSummaryStatistics stats = primes.stream().mapToInt((x) -> x).summaryStatistics();
System.out.println("Highest prime number in List : " + stats.getMax());
System.out.println("Lowest prime number in List : " + stats.getMin());
System.out.println("Sum of all prime numbers : " + stats.getSum());
System.out.println("Average of all prime numbers : " + stats.getAverage());

```



#### FunctionalInterface

#### 理解注解 @FunctionInterface

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.Type)
public @interface FunctionalInterface{}
```

* interface做注解的注解类型，被定义为Java语言规范
* 一个被它注解的接口只能有一个抽象方法，有两种例外
  * 第一是接口允许有实现的方法，这种实现的方法是用default关键字来标记的(Java反射中， Java.lang.reflect.Method## isDefault())方法是用来判断是否default方法)
  * 第二如果声明的方法和java.lang.Object种的某个方法一样，它可以不当作未实现的方法，不违背这个原则：一个被它注解的接口只能有一个抽象方法，比如`java public iinterface Comparator<T> { int compare(T o1, T o2); boolean equals(Object obj);}`
* 如果一个类型被这个注解修饰，那么编译器会要求这个类型必须满足一下条件
  * 这个类型必须是一个interface,而不是其他注解类型，枚举enum或者类class
  * 这个类型必须满足function interface的所有要求，如果包含两个抽象方法的接口增加这个注释，会有编译错误
* 编译器会自动满足function interface要求的接口自动识别function interface



#### 自定义函数接口

```java
@FunctionalInterface
public interface IMyInterface {
	void study();
}

package com.learn.java;
public class TestMyInterface {
  public static void main(String[] args) {
    IMyInterface iMyInterface = () -> System.out.println("I like study");
    iMyIterface.study();
  }
}
```





### 内置四大函数接口

* 消费型接口：Consumer< T > void accept( T t )有参数，无返回值的抽象方法；

> 比如 map.forEach( BigConsumer<A , T > )

```java
Consumer<Person> greeter = (p) -> System.out.println("hello," + p.firstName);
greeter.accept(new Person("Luke", "Skywaller"));
```

* 供给型接口：Supplier < T > T get ( ) 无参有返回值的抽象方法；

> 以stream().collect(Collector < ? super T, A , R > collector)为例;

比如

```java
Supplier<Person> personSupplier = Person::new;
personSupplier.get();  // new Person
```

* 断定型接口：Predicate< T > boolean test ( T t )： 有参，但是返回值类型是固定的**boolean**

> 比如：steam().filter()中参数就是Predicate

```java
Predicate<String> predicate = (s) -> s.length() > 0;

predicate.test("foo");			// true
predicate.negate().test("foo"); // false

Predicate<Boolean> nonNull = Objects::nonNull;
Predicate<Boolean> isNull = Objects::isNull;

Predicate<String> isEmpty = String::isEmpty;
Predicate<String> isNotEmpty = isEmpty.negate();
```

* 函数型接口：Funtion<T , R > R apply (T t )有参有返回值的抽象方法；

> 比如： steam( ).map( ) 中参数就是Function< ? Super T , ? Extends R> ; reduce（ ）中参数BinaryOperator< T >

```java
Function<String, Integer> toInteger = Integer::valueOf;
Function<String, String> backtoString = toInteger.andThen(String::valueOf);

backToString.apply("123");  // "123"
```





## Java8 - Optional类深度解析

> 调用一个方法得到了返回值去不能直接将返回值作为参数去调用别的方法。首先要判断这个返回值是否等于null，只有在非空的情况下才能将其作为其他方法的参数。



Java8 引入了一个新的Optional类。

> 这是一个可以为Null 的容器对象，如果值存在则isPresent（）方法会返回True，调用get（）方法会返回该对象。



### Optional类包含的方法

#### Of

> 为非null的值创建一个Optional

of方法通过工厂方法创建Optional类。需要注意的是，创建对象是传入的参数不能是null，如果传入的是Null，则会抛出NulPointException。

```java
// 调用工厂方法创建Optional实例
Optional<String> name = Optional.of("Sanaulla");
// 传入参数为null，抛出NullPointerException
Optional<String> someNull = Optional.of(null);
```

#### ofNullable

> 为指定的一个值创建Optional，如果指定的值为null，则返回一个空的Optional；

ofNullable与of方法相似，唯一的区别就是可以接受参数为null的情况。

```java
// 如下创建了一个不包含任何值的Optional实例
Optional empty = Optional.ofNullable(null);
```



#### isPresent

> 如果值存在返回true,如果不存在则返回false。

```java
//isPresent方法用来检查Optional实例中是否包含值
if (name.isPresent()) {
  //在Optional实例内调用get()返回已存在的值
  System.out.println(name.get());//输出Sanaulla
}

```

#### get

> 如果Optional有值则将其返回，否则返回NoSuchElementException

```java
//执行下面的代码会输出: No value present 
try {
  //在空的Optional实例上调用get()，抛出NoSuchElementException
  System.out.println(empty.get());
} catch (NoSuchElementException ex) {
  System.out.println(ex.getMessage());
}
```

#### ifPresent

> 如果Optional实例有值则为其调用consumer, 否则不做处理

要理解ifPresent方法，需要了解Consumer类。简单说，Consumer类包含一个抽象方法。该抽象方法对传入的值进行处理，但没有返回值。Java8支持不用接口直接通过lambda表达式传入参数

如果Optional实例有值，调用ifPresent()可以接受接口段或lambda表达式，

```java
//ifPresent方法接受lambda表达式作为参数。
//lambda表达式对Optional的值调用consumer进行处理。
name.ifPresent((value) -> {
  System.out.println("The length of the value is: " + value.length());
});
```

#### orElse

> 如果有值则将其返回，否则返回指定的其他值

如果Optional实例有值则将其返回，否则返回orElse方法传入的参数。

```java
// 如果值不为null，orElse方法返回Optional实例的值。
// 如果为null， 返回传入的消息
// 输出： There is no value present!
System.out.println(empty.orElse("Thereis no value present!"));
// 输出： Sanaulla
System.out.println(name.orElse("There is some value!"));
```

#### orElseGet

> orElseGet与orElse方法类似，区别在于得到的默认值，orElse方法将传入的字符串作为默认值，orElseGet方法可以接受Suppliler接口的实现用来生成默认值，

```java
// orElseGet与orElse方法类似，区别在于orElse传入的默认值，
// orElseGet可以接受一个lambda表达式生成默认值
// 输出： Default Value
System.out.println(empty.orElseGet(() -> "Default Value"));
// 输出： Sanaulla
System.out.println(name.orElseGet(() -> "Default Value"));
```

#### map

> 如果有值，则对其执行调用mapping函数得到返回值，如果返回值不为null，则创建包含mapping返回值的Optional作为map方法返回值，否则返回返回空Optional。

map方法用来对Optional实例的值执行一系列操作。

```java
// map方法执行传入的lambda表达式参数对Optional实例的值进行修改
// 为lambda表达式的返回值创建新的Optional实例作为map方法的返回值。
Optional<String> upperName = name.map((value) -> value.toUpplerCase());
System.out.println(uppperName.orElse("no Value Found"));
```

#### filter

filter方法通过传入限定条件对Optional实例的值进行过滤。

> 如果有值并且满足断言条件返回包含该值的Optional，否则返回空Optional



### Java - 8 默认方法

> * 为什么会出现默认方法？
> * 接口中出现默认方法，且类可以实现多接口的，那和抽象类有啥区别？
> * 多重实现的默认方法冲突怎么办？



#### 什么是默认方法，为什么要有默认方法？

```java
 public interface A {
   defaullt void foo() {
     System.println("Calling A.foo()");
   }
 }

public class Clazz implements A {
  public static void main(String[] args) {
    Class clazz = new Clazz();
    clazz.foo(); // 调用A.foo()
  }
}
```



#### 什么是默认方法？

简单的说，就是接口可以有实现方法，而且不需要实现其方法，只需要在方法名前价格default关键字即可。



#### 为什么需要默认方法？

首先，之前的接口是一个双刃剑，好处是面向抽象而不是面向具体编程，缺陷是，当需要修改接口的时候，需要修改全部实现该接口的类，目前的Java8之前的集合框架没有foreach方法，通常能想到的解决办法是在JDK里给相关的接口添加新的方法及实现，然而对于已发布的版本，是没法给接口添加新方法的同时不影响已有的实现，所以引进的默认方法，是为了解决接口的修改与现有实现的不兼容问题。



#### Java8抽象类与接口对比


|                            相同点                            |                            不同点                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
|                         都是抽象类型                         | 抽象类不可以多重继承，接口可以（无论是多重类型继承还是多重行为继承） |
|               都可以有实现方法（以前接口不行）               | 抽象类和接口所反应出的设计理念不同。其实抽象类表示的是“is- a”关系，接口表示的是“like - a”关系 |
| 都可以不需要实现类或继承者去实现所有方法，（现在接口中默认方法不需要实现者实现） | 接口中定义的变量默认是public static final型，且必须给其初值，所以实现类中不能改变其值；抽象类中变量默认是friendly型，其值可以在子类中重新定义，也可以重新赋值 |

* friendly型： 假设一个类、类属变量及方法不以这三种（public, protected, private)修饰符来修饰，它就是friendly类型。那么包内不论什么类都能访问它。



#### 多重继承的冲突

由于同一个方法可以从不同接口引入，自然而然就会有冲突现象，默认方法判断冲突的规则如下：

1. 一个声明在类里面的方法优先于任何默认方法（classess always win）
2. 否则，则会优先选取路径最短的。

##### 举例

* Case1

```java
public interface A{
	default void aa() {
		System.out.println("A's aa");
	}
}
public interface B{
	default void aa() {
		System.out.println("B's aa");
	}
}
public static class D implements A,B{
	
}
```

报错 Duplicate default methods named aa with the parameters() and () are inherited from the types DocApplication B and DocApplication.A

如果一定要这么写呢。同时实现A,B并且使用A中的aa？

```java
public static class D implements A,B {
  @Overrid
  public void aa() {
    A.super.aa();
  }
}
```



* Case 2

```java
public interface A{
	default void aa() {
		System.out.println("A's aa");
	}
}
public interface B{
	default void aa() {
		System.out.println("B's aa");
	}
}
public interface C extends A, B{
	default void aa() {
		System.out.println("C's aa");
	}
}
public static class D implements A,B,C{
	
}

```

输出 C's aa



* Case 3

```java
public interface A{
	default void aa() {
		System.out.println("A's aa");
	}
}
public interface C extends A{
	default void aa() {
		System.out.println("C's aa");
	}
}
public static class D implements C{
	
}

```

输出C's aa



## Java 8 - 类型注解



> * 可以在哪些地方使用注解？
> * 什么是类型注解？
> * 类型注解有什么作用？
> * 为什么会出现类型注解（JSR 308）?



#### 什么是类型注解

在Java8之前 注解只能在声明的地方使用，例如：类、方法，属性；

在Java8，注解可以放在任何地方。



**创建类实例**

```java
new @Interned MyObjcet()
  // @Interned 使用此注解修饰类，其子类将默认继承这个元注解
```

**类型映射**

```java
myString = (@NonNull String) str;
// @NonNull 用于注解方法，参数以及变量，指示目标对象不能为null
// @NonNullApi 包级别注释， 指定参数和方法返回值默认不能为null；
// @NonNullFields 包级别注释， 用于变量不能为null;
// @Nullable 可用于注解方法，参数以及变量，指定目标对象可以为null，若是与@NonNullApi和@NonNullFields公用时，则会覆盖。
```

**implements语句中**

```java
class unmodifiableList<T> implements @Readonly List<@Readonly T> {...}

```

**Throw exception 声明**

```java
void monitorTemperature() throws @Critical TemperatureException{ ...}
```



需要注意的是，**类型注解只是语法而不是语义，并不会影响Java的编译时间，加载时间，以及运行时间，也就是说，编译成class文件的时候并不包含类型注解**



## Java 8 - 重复注解

>* jdk8之前对重复注解是怎么做的？
>* jdk8对重复注解添加了什么支持



### 什么是重复注解

允许在同一申明类型( 类， 属性， 或方法 ) 的多次使用同一个注解



####  JDK8之前

java8之前也有重复使用注解的解决方案，但可读性不好。

```java
public @interface Authority {
  String role();
}

public @interface Authorities {
  Authority[] value();
}

public class RepeatAnnotationUseOldOversion {
  @Authorities({@Authority(role="Admin"),@Authority(role="Manager")})
  public void doSomeThing() {
    
  }
}
```

用另一个注解来存储重复注解，在使用的时候，用存储注解Authorities来扩展重复注解。



#### JDk8重复注解

```java
@Repeatable(Authorities.class)
public @interface Authority {
  String role();
}

public @interface Authorities {
  Authority[] value();
}

public class RepeatAnnotationUseNewVersion {
  
  @Authority(role="Admin")
  @Authority(role="Manager")
  public void doSomeThing(){}
}
```

不同的是，创建重复注解Authority时，加上@Repeatable,指向存储注解Authorities，在使用时候，直接可以重复使用Authority注解。





## Java 8 - LocalDate / LocalDateTime

### Java8时间和日期

java8仍然沿用了ISO的日历体系，并且与它的前辈不同，java.time包中的类是不可变且线程安全的，新的时间及日期API位于java.time包中，

* Instant - 它代表的是时间戳
* LocalDate - 不包含具体的日期， 比如 2014-01-14， 可用来存储生日。
* LocalTime - 它代表的是不含日期的时间。
* LocalDateTime - 它包含了日期及时间，不过还没有偏移信息或者说明时区。
* ZonedDateTime - 这是一个包含时区的完整的日期时间，偏移量是UTC/格林威治时间为基准的。





