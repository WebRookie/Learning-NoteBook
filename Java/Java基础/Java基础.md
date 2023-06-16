### Java基础

#### 三大特性



##### 封装

利用抽象的数据和基于数据的操作封装在一起，使其构成一个不可分割的独立实体。数据被保护在抽象数据类型的内部，尽可能的隐藏内部的细节，只保留一些对外接口使之与外部发生联系，用户无需知道内部的细节，但可以通过对象对外提供的接口来访问该对象。

优点

* 减少耦合：可以独立开发、测试、优化、使用、理解和修改
* 减轻维护的负担：更容易被程序员理解，并且在调试的时候，可以不影响其他模块
* 有效的调节性能：可以通过剖析确定哪些模块影响了系统的性能
* 提高软件的可重用性
* 降低了构建大型系统的风险：即使整个系统不可用，但是这些独立的模块却可能是有用的



##### 继承

继承实现了IS-A关系，例如Cat和Animal就是一种IS-A关系，因此Cat可以继承自Animal，从而获得Animal非privated的属性和方法。

继承应该遵循里氏替换原则，子对象必须能够替换所有的父类对象



##### 多态

多态分为编译时多态和运行时多态

* 编译时多态主要指方法的重载
* 运行时多态指程序中定义的对象引用所指向的具体类型在运行期间才能确定

运行时多态有三个条件

* 继承
* 覆盖（重写）
* 向上转型



#### 基本数据类型

##### 包装类型

八个基本类型：

* Boolean / 1
* byte / 8
* char / 16
* short / 16
* Int / 32 
* float / 32
* long / 64 
* Double / 64

基本类型都有对应的包装类型，基本类型与其对应的包装类型之间的赋值使用自动装箱与拆箱完成。

因为基本类型int, double 等 不是对象，无法通过向上转型获取Object提供的方法，比如无法参与转型，泛型，反射等过程。

```
Integer x = 2; // 装箱
int y = x; // 拆箱
```



#####  String 

String 被声明为final，因此它不可以被继承

内部使用char数组存储数据，该数据被声明为final，这意味着value数组初始化之后就不能再引用其他数组，并且String内部没有改变value数组的方法，因此保证了String的不可变。

**不可变的好处**

1、可以缓存hash值

因为String的hash值经常被使用，例如String用作HashMap的key，不可变的特性可以使得hash值也不可变，因此只需要一次计算。

2、String Pool的需要

如果一个String对象已经被创建过了，那么就会从String Prool 中取得引用，只有String 是不可变的，才可能使用String Pool

3、安全性

String常用来做参数，String的不可变性可以保证参数不变。比如在网络连接过程中， String被修改。改变String对象那一方以为现在连接的是其他主机，而实际情况不一样。

4、线程安全

String 不可变性天生具备线程安全，可以在多个线程种安全地使用。



##### 运算

**参数传递**

Java的参数是以值传递的形式传入方法中的，而不是引用传递。

但是。如果在方法中改变对象的字段值会改变原对象该字段值，因为改变的是同一个地址指向的内容



##### float与double

1.1字面量属于double类型，不能直接将1.1赋值给float变量，因为这是向下转型。Java不能向下转型，这会使得精度降低。

1.1f字面量是float类型



#### 继承

##### 访问权限

Java中有三个访问修饰符: private、protected 以及 public,如果不加访问修饰符，表示包级可见。



可以对类或者类中的成员（字段以及方法）加上访问修饰符。

* 类可见表示其他类用这个类创建对象；
* 成员可见表示其他类可以用这个类的实例对象访问到该成员；



protected用于修饰成员，表示在继承体系中，成员对于子类可见，但是这个访问修饰符对于类没有意义



字段是不允许共有的，因为这么做就是去了 对这个字段修改行为的控制，客户端可以对其进行随意修改。

但是也有例外，如果包级私有的类或者私有的嵌套类，那么直接暴露成员不会有太大的影响

```java
public class AccessWithInnerClassExample 「
  private class InnerClass {
    int x;
  }
private InnerClass innerClass;
public AccessWithInnerClassExample() {
  innderClass = new InnerClass();
}
public int getValue() {
  return innerClass.x; // 直接访问
}
```



#### 抽象类与接口

1、抽象类

抽象类和抽象方法都用abstract关键字进行声明。抽象类一般包含抽象方法，抽象方法一定位于抽象类中。

抽象类和普通类最大的区别是，抽象类不能被实例化，需要继承抽象类才能实例化其子类。

```java
public abstract	class AbstractClassExample {
  protected int x;
  protected int y;
  
  public abstract void func1();
  
  public void fun2() {
    System.out.println("func2");
  }
}
```

```java
public class AbstractExtendClassExample extends AbstractClassExample {
  @Overrid
  public void func1() {
    System.out.println("func1")
  }
}
```

```java
// AbstractClassExample ac1 = new AbstractClassExample(); // 'AbstractClassExample' is abstract; cannot be instantiated
AbstractClassExample ac2 = new AbstractExtendClassExample();
ac2.func1();

```

2、接口

接口是抽象类的延伸，可以看成一个完全抽象的类，不包含任何的方法实现

背景： 如果需要增加一个方法，那么就需要修改所有实现了该接口的类。

接口的成员（字段+方法）默认都是public的，且不允许定义为private 或者protected

接口的字段默认都是static和final的



3、比较

* 一个类可以实现多个接口，但是不能继承多个抽象类。
* 接口的字段只能是static或final类，而抽象类的字段没有这样的限制。
  * 如果需要有要变化的东西，就需要放在实现中，不能放在接口中，接口只是对一类事物的属性和行为更改层次的抽象，对修改关闭、对扩展（不同的实现implements）实现，接口是对开闭原则的一种体现。
* 接口的成员只能是public的，而抽象类的成员可以有多种访问权限



4、使用选择

使用接口：

* 需要让不相关的类都实现一个方法，例如不相关都可以实现Compareable接口中的compareTo() 方法；
* 需要使用多重继承。

使用抽象类

* 需要在几个相关的类中共享代码
* 需要能控制继承来的成员的访问权限，而不是都是public
* 需要继承非静态和非常量字段



warning： 很多情况，接口优先于抽象类，因为接口没有抽象类严格的类层次结构要求，可以灵活地为一个类添加行为。



#### 重写重载

1.重写(Override)

存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

限制：

* 子类方法的访问权限必须大于等于父类方法；
* 子类方法的返回类型必须是父类方法返回类型或者为其子类型

2.重载(Overload)

存在于同一个类，指一个方法与已经存在的方法名称相同，但是参数类型、个数、顺序至少有一个不同。

应该注意的是⚠️：返回值不同，其他都相同的不算是重载。



#### Object 通用方法

概览

```java
public final native Class<?> getClass()

public native int hashCode()

public boolean equals(Object obj)

protected native Object clone() throws CloneNotSupportedException

public String toString()

public final native void notify()

public final native void notifyAll()

public final native void wait(long timeout) throws InterruptedException

public final void wait(long timeout, int nanos) throws InterruptedException

public final void wait() throws InterruptedException

protected void finalize() throws Throwable {}

```



equals 与 == 的区别？

* 对于基本类型， == 判断两个值是否相等，基本类型没有equals方法
* 对于饮用类型， == 判断两个变量是否引用同一个对象，而equals() 判断引用的对象是否等价



##### hashCode()

hashCode()返回散列值，而 equals 是用来判断两个对象是否等价。等价的两个对象散列值一定相同，但是散列值相同的两个对象不一定等价



##### toString()

@后面的数值为散列码的无符号十六进制表示



##### clone()

clone() 是Object 的protected 方法，它不是public， 一个类不显示去重写 clone ，其他类就不能直接去调用该实例的clone 方法



使用clone() 方法来拷贝一个对象既复杂又有风险，还会抛出异常，可以使用拷贝构造函数或者拷贝工厂来拷贝一个对象

```java
public class CloneConstructorExample {
    private int[] arr;

    public CloneConstructorExample() {
        arr = new int[10];
        for (int i = 0; i < arr.length; i++) {
            arr[i] = i;
        }
    }

    public CloneConstructorExample(CloneConstructorExample original) {
        arr = new int[original.arr.length];
        for (int i = 0; i < original.arr.length; i++) {
            arr[i] = original.arr[i];
        }
    }

    public void set(int index, int value) {
        arr[index] = value;
    }

    public int get(int index) {
        return arr[index];
    }
}

```



#### 关键字

**final**

1.数据

声明数据为常量，可以是编译时常量，也可以是运行时被初始化后不能被改变的常量

* 对于基本数据类型，final使数值不变
* 对于引用类型，final使引用不变，也就不能引用其他对象，但是被引用的对象本事是可以修改的

```
final int x = 1;
// x = 2 // cannot assign value to final variable 'x'
final A y = new A();
y.a = 1;
```



2.方法

声明方法不能被子类重写。

private方法隐式地被指定为final，如果在子类定义的方法和基类中的一个private方法签名相同，此时子类的方法不是重写基类方法，而是在子类中定义了一个新的方法。



3.类

声明类不允许被继承



##### static

1.静态变量

* 静态变量又称为类变量，也就是说这个变量是属于这个类的，类所有的实例都是共享静态变量，可以直接通过类名来访问它，静态变量在内存中只存在一份。
* 实例变量：每创建一个实例就会产生一个实例变量，它与该实例共同生死



2.静态方法

静态方法在类加载的时候就存在了，它不依赖于任何实例。所以静态方法必须有实现，也就是说它不能是抽象方法（abstract）。

只能访问所属类的静态字段和静态方法，方法中不能有this和 super关键字



3.静态语句块

静态语句块在类初始化的时候运行一次



4.静态内部类

非静态内部类依赖外部类的实例，而静态内部类不需要

```Java
public class OuterClass {
    class InnerClass {
    }

    static class StaticInnerClass {
    }

    public static void main(String[] args) {
        // InnerClass innerClass = new InnerClass(); // 'OuterClass.this' cannot be referenced from a static context
        OuterClass outerClass = new OuterClass();
        InnerClass innerClass = outerClass.new InnerClass();
        StaticInnerClass staticInnerClass = new StaticInnerClass();
    }
}

```

静态内部类不能访问外部类的非静态的变量和方法



5.静态导包

在使用静态变量和方法时不用再指明ClassName,从而简化代码，

```java
import static com.xx.ClassName.*		
```



6.初始化顺序

静态变量和静态语句块优先于实例变量和普通语句块，静态变量和静态语句块的初始化顺序取决于它们在代码中的顺序。

最后才是构造函数的初始化



存在继承的情况下，初始化顺序为：

* 父类（静态变量、静态语句块）
* 子类 （静态变量、静态语句块）
* 父类（实例变量、普通语句块）
* 父类（构造函数）
* 子类（实例变量、普通语句块）
* 子类（构造函数）



#### 反射

用法： 类加载，相当于Class对象的加载，类在第一次使用时才动态的加载到JVM中，可以使用``` Class.forName("com.mysql.jdbc.Driver")``` 这种方式来控制类的加载，该方法会返回一个Class对象

反射可以提供运行时的类信息，并且或者类可以在运行时才加载进来，甚至子啊编译时期，该类的.class不存在也可以加载进来。

Class 和java.lang.reflect一起对反射提供类支持，java.lang.reflect主要包含了以下三个类

* **Field** : 可以使用get() 和 set() 方法读取和修改Field 对象关联的字段
* **Method**： 可以使用invoke() 方法调用与 Method 对象关联的方法；
* **Constructor** 可以用Constructor创建新的对象



#### 泛型

详见 泛型章节



#### 注解

Java注解是附加在代码中的一些元信息，用于一些工具在编译、运行时进行解析和使用，起到说明、配置的功能。注解不会也不能影响代码的实际逻辑，仅仅起到辅助作用。

详见 注解 章节







#### 常见Q&A



**1、Java中应该使用什么数据类型来代表价格？**

如果不是特别关心内存和性能的话，使用BigDecimal，否则使用预定义精度的double类型。

**2、怎么将byte转换为String?**

可以使用String接收byte[]参数的构造器来进行转换，需要注意的是使用正确的编码，否则会使用平台默认编码，这个编码可能跟原来的编码相同，也许不同。

**2、Java中怎样将bytes转换为long类型？**

String接受bytes的构造器转为String，再Long.parseLong

**3、如果强行将int转换为byte类型的变量吗？如果该值大于byte类型的范围，会发生什么现象？**

可以强制转换，但是Java中int是32位，byte是8位，其中高出来的24位会被丢弃。byte类型的范围是-128 ～127

**4、Java中++操作符是线程安全的吗？**

不是线程安全操作。它涉及到多个指令，比如读取变量值，增加，然后存储会内存，这个过程中可能会出现线程交叉。还会存在竞态条件（读取-修改-写入）

**5、a = a + b 与 a += b 的区别**

+= 隐式的将加操作的结果类型强制转换成为持有结果的类型，如果两个整型相加，如byte、short或者int，首先会将它们提升到int类型，然后再执行加分操作。

```
byte a = 127;
byte b = 127;
b = a + b; // error cannot convert from int to byte
b += a; // ok
```

(因为 a + b 操作会将a、 b 提升为int类型，所以将int类型赋值给byte就会编译出错)

**6、3 * 0.1 == 0.3 将会返回什么 true 还是false？**

false，因为有些浮点数不能完全精准的表示出来。

**7、int 和integer哪个会占用更多的内存？**

integer会占用更多的内存。因为integer是一个对象，需要存储对象的元数据，但是int是一个原始类型的数据，所以占用的空间更小。

**8、为什么Java中的String是不可变的（Immutable）？**

Java中的String不可变，是因为Java

**9、什么是不可变对象（immutable object）？ Java如何创建一个不可变对象？**

不可变状态对象一旦被创建，状态就不能再改变，任何修改都会创建一个新的对象，如String，Integer及其他包装类。

需要遵循以下几个原则

* immutable对象的状态在创建后就不能发生改变，任何对它的改变都应该产生一个新的对象。
* immutable类的所有属性都是应该是final的
* 对象必须被正确创建，比如： 对象引用在对象创建过程中不能被泄露（leak）
* 对象应该是final的。以此来限制子类继承父类，以避免子类改变父类的immutable特性
* 如果类中包含mutable类对象，那么返回给客户端的时候，返回该对象的一个拷贝，而不是该对象本身

**10、Java中，Comparator与Comparable有什么不同？**

Comparable接口用于定义对象的自然顺序，而Comparator通常用于定义用户定制的顺序，Comparable总是只有一个，但是可以有很多个comparator来定义对象的顺序。

**11、为什么重写equals方法的时候，需要重写hashCode方法？**

因为有强制的规范制定需要同时修改hashcode与equals是方法，许多容器类，如hashMap、HashSet都依赖于hashcode于equals的规定

**12、”a==b" 和 “a.equals(b)" 有什么区别？**

如果a和b都是对象，则a==b是比较两个对象的引用，只有当a和b指向的是堆中的同一个对象才会返回true，而 a.equals(b) 是进行逻辑比较，所以通常都需要重写该方法来提供逻辑一致性的比较。例如 String类重写equals方法，可以用于两个不同对象，但是包含字母相同的比较

**13、接口是什么？为什么要使用接口而不是直接使用具体类？**

接口用于定义API，它定义了类必须得遵循的规则。同时，它提供了一种抽象，因为客户端只使用接口这样可以有多重实现，如List接口，你可以使用可随机访问的ArrayList，也可以使用方便插入和删除的LinkedList。接口中不允许普通方法，以此来保证抽象，但是Java8 可以在接口声明静态方法和默认普通方法。

**14、Java中抽象类和接口有什么不同？**

Java中一个类只能继承一个类，但是可以实现多个接口。抽象类可以很好的定义一个家族类的默认行为，而接口能更好的定义类型，有助于实现后续的多态。

**15、抽象类和最终类**

抽象类可以没有抽象方法，最终类可以没有最终方法

最终类不能被继承，最终方法不能被重写（可以重载）

**16、Java移位运算符**

* ``<<``: 左移运算符 ``` x << 1``` 相当于x乘以2（不溢出的情况下） 低位补0



## 泛型机制详解

#### 为什么会引入泛型

> 泛型的本质是为了参数化类型（在不创建新的类型的情况下，通过泛型制定的不同类型来控制型参具体限制的类型）。也就是说在泛型使用过程中，操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口、和方法中，分别被称为泛型类、泛型接口、泛型方法。

引入反省的意义在于：

* 适用于多种数据类型执行相同的代码（代码复用）

```java
private static int add (int a, int b) {
  System.out.println("a")
    retun a + b;
}
private static float add(float a, float b) {
  return a + b;
}
```

通过泛型，可以服用一个方法

```java
private static <T extands Number> double add(T a, T b) {
  return a.doubleValue() + b.doubleValue();
}
```



* 泛型中的类型在使用时指定，不需要强制类型转换（类型安全，编译器会检查类型）

```java
List list = new ArrayList();
list add("XXString");
list.add(100d);
list.add(new Person());
```

以上元素都是Object类型，所以在取出元素时，需要人为的强制类型转化到具体的目标类型，且很容易出现``` java.lang.ClassCastException``` 异常

引入泛型，它将提供类型约束，提供编译前的检查：

```java
List<String> list = new ArrayList<String>();
```





### 泛型类

简单泛型类

```java
class Poiont<T> { // 此处可以随便写标识符，T是type的简称
  private T var;  // var的类型由T指定，（外部决定）
  public T getVar() { // 返回值的类型由外部决定
    return var;
  }
  public void setVar(T var) { // 设置类型也有外部决定
    this.var = var;
  }
}


public class GenericsDemo{
  public static void main(Sting args[]) {
    Point<String> p = new Point<String>(); // 里面的var类型为String类型
    p.setVar("it");  
  }
}
```

多元泛型

```java
class Notepad<K,V> { // 此处指定了两个泛型类型
  private K key;
  private V value;
      public K getKey(){  
        return this.key ;  
    }  
    public V getValue(){  
        return this.value ;  
    }  
    public void setKey(K key){  
        this.key = key ;  
    }  
    public void setValue(V value){  
        this.value = value ;  
    }  
}

public class GenericsDemo{
  public static void main(String args[]) {
    Notepad<String, Integer> t = null; // 定义了两个泛型类型的对象
    t = new Notepad<String, Integer>(); // 里面的Key为String，value为Integer
    
  }
}
```



### 泛型接口

* 简单的泛型接口

```java
interface Info<T> { // 在接口上定义泛型
  public T getVar(); //定义抽象方法，抽象方法的返回值就是泛型类型
  
}
class InfoImp<T> implements Info <T> { // 定义泛型接口的子类
  private T var; // 定义属性
  public InfoImpl(T var) {  // 通过构造方法设置属性内容
    this.setVar(var);
  }
  public void setVar (T var) {
    this.var = var;
  }
  public T getVar(){
    return this.var;
  }
}

public class GenericsDemo{
  public static void main(String args[]) {
    Info<String> i = null; // 声明接口对象
    i = new InfoImpl<String>("Tom"); // 通过子类实例化对象
  }
}
```



### 泛型方法

泛型方法，是在调用方法的时候指明泛型的具体类型，重点看下泛型的方法

![img](https://www.pdai.tech/images/java/java-basic-generic-4.png)



#### 泛型的上下限

为了解决泛型中隐含的转换问题，Java泛型加入了类型参数的上下边界机制。`<? extends A>`表示该类型参数可以是A(上边界)或者A的子类类型。编译时擦除到类型A，即用A类型代替类型参数。这种方法可以解决开始遇到的问题，编译器知道类型参数的范围，如果传入的实例类型B是在这个范围内的话允许转换，这时只要一次类型转换就可以了，运行时会把对象当做A的实例看待。

```

```

* 泛型的上下限引入

上限

```java
class Info<T extend Number> { // 此处泛型只能是数字类型
	private T var; // 定义泛型变量
	public void setVar(T var) {
    this.var = var;
  }
}

public class Demo {
  public static void main(String args[]) {
    Info<Integer> l1 = new Info<Integer>(); // 声明Integer的泛型对象
  }
}
```



下限

```java
class Info<T> {
  private T var; // 定义泛型变量
  public void setVar(T var) {
    this.var = var 
  }
  
}

public class GenericsDemo {
  public static void main(String args[]) {
    Info<String> i1 = new Info<String>(); // 声明String类型泛型对象
    Info<Object> i2 = new Info<Object>(); // 声明Object的泛型对象
    i1.setVar("hello");
    i2.setVar(new Object());
  }
  
  public static void fun(Info< ? super String> temp) { // 只能接收String或者Object类型的泛型， String类的父类只有Object类
    System.out.print(temp+ ",")
  }
}
```



小结

```java
<?> 无限制通配符
<? extends E> extends 关键字声明了类型的上界，表示参数化的类型可能是所指定的类型，或者是此类型的子类
<? super E> super 关键字声明了类型的下界，表示参数化的类型可能是指定的类型，或者是此类型的父类

// 使用原则《Effictive Java》
// 为了获得最大限度的灵活性，要在表示 生产者或者消费者 的输入参数上使用通配符，使用的规则就是：生产者有上限、消费者有下限
1. 如果参数化类型表示一个 T 的生产者，使用 < ? extends T>;
2. 如果它表示一个 T 的消费者，就使用 < ? super T>；
3. 如果既是生产又是消费，那使用通配符就没什么意义了，因为你需要的是精确的参数类型。

```



* 多个限制

使用&符号

```java
public class Client {
  // 工资低于2500的上班族并且站票的乘客车票打8折
  public static <T extends Staff & Passenger> void discount(T t) {
    if(t.getSalary() < 2500 && t.isStanding()) {
      System.out.println("需要打八折");
    }
  }
  public static void main(String[] args) {
    discount(new Me())
  }
}
```



#### 如何理解Java中的泛型是伪泛型？泛型中类型擦除

> Java泛型是通过JDK1.5才开始加入的，为了兼容以前的版本，Java泛型实现采用了“伪泛型”的策略，但是在编译阶段会进行所谓的“**类型擦除**”（Type Erasure）将所有的泛型表示（尖括号中的内容）都替换为具体的类型（其对应的原生生态类型），就像完全没有泛型一样。

##### 泛型的类型擦除原则是：

* 消除类型参数声明，即删除``` <>``` 及其包围的部分
* 根据类型参数的上下界推断并替换所有类型参数为原生态类型：如果类型参数是无限制通配符或没有上下界限定，则替换为Object，如果存在上下界限定，则根据子类替换原则，取类型参数的最左边限定类型(即父类)
* 为了保证类型安全，必要时插入强制类型转换代码
* 自动产生"桥接方法"已保证擦除类型后的代码仍然具有泛型的"多态性"



### Java注解机制详解

注解：可以对包、类、接口、字段、方法参数、局部变量等进行注解。它主要有以下四个方面

* 生成文档，通过代码里标识的元数据生成javadoc文档。
* 编译检查，通过代码里的标识元数据让编译器在编译期间进行检查验证。
* 编译时动态处理，编译时通过代码里标识的元数据动态处理，例如动态生成代码。
* 运行时动态处理，运行时通过代码里标识元数据动态处理，例如使用反射注入实例。



以下是常见分类

* **Java自带的标准注解**， 包括``@Override ``、 `@Deprecated `和` @SuppressWarnings`， 分别用于标明重写某个方法，标明某个方类或方法过时、标明要忽略的警告，用这些注解标明后编译器就会进行检查。
* **元注解**，元注解时用于定义注解的注解，包括`@Retention`、`@Target`、`@Inherited`、`@Documented`， `@Retention` 用于标明注解被保留的阶段，`@Target`用于标明注解使用的范围，`@Inherited`用于标明注解可继承，`@Documented`用于标明是是否生成javadoc文档。
* **自定义注解**，可以根据自己的需求定义注解，并可以用元注解对自定义注解进行注解.





#### 元注解

JDK1.5中提供了4个准确的元注解：`@Target`、`@Retention`, `@Documented`,`@Inherited`，在JDK1.8中提供了两个元注解`@Repeatable`和`@Native`



#### 元注解-@Target

> Target注解作用是，描述注解的使用范围（即：被修饰的注解可以什么地方）

Target注解用来说明那些被它所注解的注解类可修饰的对象范围：注解可以用于修饰packages、types（类、接口、枚举、注解类）、类成员（方法、构造方法、成员变量、枚举值）、方法参数和本地变量（如循环变量、catch参数），在定义注解类时使用了@Target能够更加清晰的知道它能够被用来装饰哪些对象，它的取值范围定义在ElementType枚举中。

```java
public enum ElementType {
 
    TYPE, // 类、接口、枚举类
 
    FIELD, // 成员变量（包括：枚举常量）
 
    METHOD, // 成员方法
 
    PARAMETER, // 方法参数
 
    CONSTRUCTOR, // 构造方法
 
    LOCAL_VARIABLE, // 局部变量
 
    ANNOTATION_TYPE, // 注解类
 
    PACKAGE, // 可用于修饰：包
 
    TYPE_PARAMETER, // 类型参数，JDK 1.8 新增
 
    TYPE_USE // 使用类型的任何地方，JDK 1.8 新增
 
}

```

示例：

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(value={CONSTRUCTOR, FIELD, LOCAL_VARIABLE, METHOD, PACKAGE, PARAMETER, TYPE})
public @interface Deprecated {
}

```



#### 元注解 - @Retention & @RetentionTarget

> Retention注解的作用是：描述注解保留的时间范围（即：被描述的注解在它所修饰的类中可以被保留到何时）。

Retention注解用来限定那些被它所注解的注解类在注解到其他类上以后，可以被保留到何时，一共有三种策略，定义在RetentionPolicy枚举。

```java
public enum RetentionPolicy {
 
    SOURCE,    // 源文件保留
    CLASS,       // 编译期保留，默认值
    RUNTIME   // 运行期保留，可通过反射去获取注解信息
}

```

以下测试用于区分三种注解类有何区别?

```java
@Retention(RetentionPolicy.SOURCE)
public @interface SourcePolicy{
  
}
@Retention(RetentionPolicy.CLASS)
public @interface ClassPolicy {
  
}
@Retention(RetentionPolicy.RUNTIME)
public @interface RuntimePolicy{}


// 用定义好的三个注解类分别去注解一个方法

public class RetentionTest {
  @SourcePolicy
  public void sourcePolicy(){
    
  }
  @ClassPolicy
  public void classPolicy(){}
  
  @RuntimePolicy
  public void runtimePolicy() {}
}

// 通过执行 javap -verbose RetentionTest 命令获取的RetentionTest的class字节码如下
// 略

```

最终结果看到

* 编译器没有记录下source Policy（）方法的注解信息；
* 编译器分别使用了` RuntimeInvisibleAnnotations`和` RuntimeVisibleAnnotations`属性去记录了`classPolicy()`方法和`runtimePolicy()`方法的注解信息；





#### 元注解- @Documented

> Documented注解的作用是： 描述在使用javadoc工具为类生成帮助文档时是否要保留其注解信息。



使用实例: 以下会是使用Javadoc工具可i

```java
@Documented
@Target({ElementType.TYPE, ElementType.METHOD})
public @interface TestDocAnnotation {
  public String value() default "default"；
}

@TestDocAnnotation("myMethodDoc")
public void testDoc() {

}
```



#### 元注解- @Inherited

> Inherited注解的作用： 被它修饰的Annotation将具有继承性。如果某个类使用了被@Inherited修饰的Annotation，则其子类将自动具有该注解



测试：

定义`@Inherited`注解:

```java
@Inherited
@Retention(RetentionPolicy.RUNTIME)
@Target({ELementType.TYPE,ElementType.METHOD})
public @interface TestInheritedAnnotation {
  String [] values();
  int number();
}
// 使用注解

@TestInheritedAnnotation(values = {"value"}, number = 10)
public class Persion {}
class Student extends Persion {
  @Test
  public void test() {
    Class clazz = Student.class;
    Annotation[] annotations = clazz.getAnnotations();
    for( Annotation annotation: annotations ) {
      System.out.println(annotation.toString());
    }
  }
}

// 输出

xxxx.TestInheritedAnnotation(value=[value], number = 10)

```



即使Student类没有显示地被注解`@TestInheritedAnnotation`，但是它的父类Person被注解，而且`@TestInheritedAnnotattion`被`@Inherited`注解，因此Student类自动有了该注解。



#### 元注解 - @Repeatable



> 重复注解？ 允许在同一申明类型(类、属性、或方法)的多次使用同一个注解





#### 注解与反射接口

> 定义注解后，如何获取注解中的内容呢？发射包java.lang.reflect下的AnnotatedElement接口提供这些方法。这里需要注意：只有注解被定义为RUNTIME后，该注解才能是运行时可见，当class文件被装载时，被保存在class文件中的Annotation才会被虚拟机读取。

AnnotatedElement接口时所有 的程序元素(Class、Method和Constructor)的父接口，所以程序通过反射获取了某个类的AnnotatedElement对象之后，程序就可i调用该对象的方法来访问的Annotation信息。

以下是Annotation信息的相关接口。

判断该程序元素上是否包含指定类型的注解，存在则返回true，否则就返回false。注意：此方法会忽略注解对应的注解容器

* `boolean isAnnotationPresent(Class<? extends Annotation> annotationClass)`



返回该程序元素上存在的、指定类型的注解，如果该类型不存在，则返回null

* `<T extends Annotation > T getAnnotation(Class<T> annotationClass)`



返回该程序元素上存在所有注解，若没有注解，则返回长度为0的数组

* `Annotation[] getAnnotations()`



#### 自定义注解

* 定义自己的注解

```java

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyMethodAnnotation {
  public String title() default "";
  public String descrition() default "";
}


// 使用注解
public class TestMethodAnnotation {
  @Override
  @MyMethodAnnotation(title = "toStringMethod", description="ovverride toString method")
  public String toString() {
    return "Override toString method"
  }
  
  @Desprecated
  @MyMethodAnnotation(title = "old static method", description="deprecated old static method")
  public static void oldMethod(){
    System.out.println("old method don't use it");
  }
  
  @SuppressWarnings({"unchecked", "deprecation"})
  @MyMethodAnnotation(title = "test method", description="suppres warning static method")
  public static void genericsTest() throws FileNotFoundException 「
    List l = new ArrayList();
  	l.add("abc");
    oldMethod();
}
```



* 用反射接口获取注解信息

在TestMethodAnnotation中添加main方法进行测试：

```java
public static void main(String[] args) {
  try {
    // 获取所有method
    Method[] methods = TestMethodAnnotation.class.getClassLoader()
      .loadClass(("com.rookie.java.annotation.TestMethodAnnotation"))
      .getMethods();
    
    // 遍历
    for(Method method : methods) {
      // 方法上是否有MyMethodAnnotation注解
      if(method.isAnnotationPresent(MyMethodAnnotation.class)) {
       	try {
          // 获取遍历方法上所有的注解
          for(Annotation anno : method.getDeclareAnnotations()) {
            System.out.println("Annotation in method" + method + "" + anno);
          }
          
          // 获取MyMethodAnnotation对象信息
          MyMethodAnnotation methodAnno = method.getAnnotation(MyMethodAnnotation.class);
          
          System.out.println(methodAnno.title());
          
        } catch(Throwable ex) {
          ex.printStackTrace();
        }
        
      }
    }
  } catch (SecurityException | ClassNotFoundException e) {
    e.printStackTrace();
  }
}
```



#### 深入理解注解

#### Java8 提供了哪些心注解

* `ElementType.TYPE_PARAMETER`

`ElementType.TYPE_USE`(此类型包括类型声明和类型参数声明，是为了方便设计者进行检查)包含了`ElementType.TYPE`（类、接口（包括注解类型）和枚举的声明）和`ElementType.TYPE_PARAMETER`

```java
// 自定义ElementType.TYPE_PARAMETER注解
@Retention(RetentionPolicy.RUNTIME)
@Target(ELementType.TYPE_PARAMETER)
public @interface MyNotEmpty {
  
}
// 自定义ElementType.TYPE_USE注解
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE_USE)
public @interface MyNotNull {}

// 测试类
public class TypeParameterAndTypeUseAnnotation<@MyNotEmpty T> {
  
  /*
  * 使用TYPE_PARAMER类型，会编译不通过
  * public @MyNotEmpty T test(@MyNotEmpty T a) {
  * new ArrayList<@MyNotEmpty String>();
  * return a;
  * }
  **/
  
  // 使用TYPE_USE类型，编译通过
  
  public @MyNotNull T test2(@MyNotNull T a) {
    new ArrayList<@MyNotNull String>();
    return a;
  }
} 

```





####  自定义注解和AOP-通过切面实现解耦

> 最为常见的就是Spring AOP切面实现统一的操作日志管理

* 自定义Log注解

```java
@Target({ELementType.PARAMER, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Log {
  /**
   * 模块
   */
  public String title() default "";
  
  /**
   * 功能
   */
  public BusinessType businessType() default BusinessType.OTHER;
  
  /**
   * 操作人类别
   */
  public OperateorType operatorType() default OperatorType.MANAGE;
  
  /**
   * 是否保存请求的参数
   */
  public boolean isSaveRequestData() default true;
}

```



* 实现日志的切面，对自定义注解Log做切点进行拦截

即对注解了@Log的方法进行切点拦截

```java
@Aspect
@Component
public class LogAspect {
  private static final Logger log = LoggerFactory.getLogger(LogAspect.class);
  
  /**
   * 配置切入点 - 自定义注解的包路径
   *
   */
  @Pointcut("@annotation(com.xxx.aspectj.lang.annotation.Log)")
  public void logPointCut() {}
  
  /*
   * 处理完请求后执行
   * 
   * @param joinPoint 切点
   */
  @AfterReturning(pointcut = "logPointCut()", returning= "jsonResult")
  public void doAfterRetruning(JoinPoint joinPointm Object jsonResult) {
    handleLog(joinPoint, null, jsonResult);
  }
  
  /**
   * 拦截异常操作
   * 
   * @param joinPoint 切点
   * @param e 异常
   */
  @AfterThrowing(value = "logPointCut()", throwing = "e")
  public void doAfterThrowing(JoinPoint joinPoint, Exception e) {
    handleLog(joinPoint, e, null);
  }
  
  protected void handleLog(final JoinPoint joinPoint, final Exception e, Object jsonResult) {
    try {
      // 获得注解
      Log controllerLog = getAnnotationLog(joinPoint);
      if(controllerLog == null) {
        return;
      }
      
      // 获取当前的用户
      User currentUser = ShiroUtils.getSysUser();
      
      // 数据库日志
      OperLog operLog = new OperLog();
      OperLog.setStatus(BusinessStatus.SUCCESS.ordinal());
      // 请求的地址
      String ip = ShiroUtils.getIp();
      operLog.setOperIp(ip);
      // 返回参数
      operLog.setJsonResult(JSONObject.toJSONString(jsonResult));
      
      operLog.setOperUlr(ServletUtils.getRequest().getRequestURI());
      if(currentUser != null) {
        operLog.setOperName(currentUser.getLoginName())
          if(StringUtils.isNotNull(currentUser.getDept())
             && StringUtils.isNotEmpty(currentUser.getDetpt().getDeptName())) {
            operLog.setDeptName(currentUser.getDept().getDeptName());
          }
      }
      if ( e != null) {
        operLog.setStatus(BusinessStatus.FAIL.ordinal());
        operLog.setErrorMsg(StringUtils.substring(e.getMessage(),0, 2000));
      }
      // 设置方法名称
      String className = joinPoint.getTarget().getClass().getName();
      String methodName = joinPoint.getSignature().getName();
      operLog.setMethod(className + "." + methodName + "()");
      // 设置请求方法
      operLog.setRequestMethod(ServeletUtils.getReqeust().getMethod());
      // 处理设置注解上的参数
     	getControllerMethodDescription(controllerLog, operLog);
      // 保存数据库
      AsyncManager.me().execute(AsyncFactory.recordOper(operLog));
    }catch( Exception exp) {
      // 记录本地异常日志
      log.error("----前置通知异常------");
      log.error("异常信息:{}", exp.getMessage());
      exp.printStackTrace();
    }
  }
  
  
  
  /**
   * 获取注解中对方法的描述信息，用于Controller层注解
   * 
   * @param log 日志
   * @param operLog 操作日志
   * @throws Exception
    */
  public void getControllerMethodDescription(Log log, OperLog operLog) throws Exception {
    // 设置action动作
    operLog.setBusinessType(log.businessType().ordinal());
    // 设置标题
    operLog.setTitle(log.title());
    // 设置操作人类别
    operLog.setOperatorType(log.operatorType().ordinal());
    // 是否需要保存request, 参数和值
    if(log.isSaveReuqestData()) {
      // 获取参数信息，传入到数据库中
      setRequestValue(operLog);
    }
  }
  
  /**
   * 获取请求的参数，放到log中
   * @param operLog
   * @param request
   */
  private void setRequestValue(OperLog operLog) {
    Map<String, String>[] map = ServletUtils.getRequest().getParameterMap();
    String params = JSONObject.toJSONString(map);
    operLog.setOperParam(StringUtils.substring(params, 0, 2000));
  }
  
  
  /**
   * 是否存在注解，如果存在就获取
   */
  private Log getAnnotationsLog(JoinPoint joinPoint) throws Exception {
    Signature signature = joinPoint.getSignature();
    MehtodSignature methodSignature = (MethodSignature) signature;
    Method method = methodSignature.getMethod();
    
    if(method != null) {
      return methodgetAnnotation(Log.class);
    }
    return null;
  }
}
```



* 使用@Log注解



示例：每对“部门”进行操作就会产生一条操作日志存入数据库。

```java
@Controller
@RequestMapping("/system/dept")
public class DeptController extends BashController {
  private String preFix = "system/dept";
  
  @Autowired
  private IDeptService deptService;
  
  /**
   * 新增保存部门
   */
  @Log(title = "部门管理", businessType = BusinessType.INSERT)
  @RequirePermission("system:dept:add")
  @PostMapping("/add")
  @ResponseBody
  public AjaxResult addSave(@Validated Dept dept) {
    if(UserConstants.DEPT_NAME_NOT_UNIQUE.equals(deptService.checkDeptNameUnique(dept))) {
      return error("新增部门" + dept.getDeptName() + "失败，部门名称已存在");
    }
    return toAjax(deptService.insertDept(dept));
  }
  
  
  /**
   * 保存
   */
  @Log(title = "部门管理", businessType = BusinessType.UPDATE)
  @RequiresPermissions("system:dept:edit")
  @PostMapping("/edit")
  @ResponseBody
  public AjaxResult editSave(@Validate Dept dept) {
    if(UserConstants.DEPT_NAME_NOT_UNIQUE.equals(deptService.checkDeptNameUnique(dept))) {
      return error("修改部门" + dept.getDeptName() + "失败，部门名称已存在");
      
    }else if(detp.getParentId().equals(dept.getDeptId())) {
      return error("修改部门" + dept.getDeptName() + "上级部门不能是自己")
    }
    return toAjax(deptServivce.updateDept(dept));
  }
  
  /**
   * 删除
   */
  @Log(title = "部门管理", businessType = BusinessType.DELETE)
  @RequiresPermission("system:dept:remove")
  @GetMapping("/remove/{deptId}")
  @ResponseBody
  public AjaxResult remove(@PathVariable("deptId") Long deptId) {
    if(deptService.selectDeptCount(deptId) > 0) {
      return AjaxResult.warn("存在下级部门，不允许删除");
    }
    if(deptService.checkDeptExistUser(deptId)) {
      return AjaxResult.warn("部门存在用户，不允许删除");
    }
    return toAjax(deptService.deleteDeptById(deptId));
  }
}
```



----------------------------------------



#### Java反射机制详解

#### 反射基础

RTTI（Run-Time Type Identification） 运行时类别识别.作用：在运行时识别一个对象的类型和类的信息。主要有两种方式：一种是传统的RTTI。它假定我们在编译时已经知道了 所有的类型；另一种是“反射”机制，它允许我们在运行时发现和使用类的信息。

反射就是把Java类中的各个成分映射成一个个的Java对象

例如：一个类有： 成员变量、方法、构造方法、包等等信息。利用发射技术可以对一个类进行剖析，把各个组成部分映射成一个个对象。

> 理解Class类，以及类的加载机制；然后基于此通过反射获取Class类以及类中的成员变量、方法、构造方法等。



#### Class类

*  Class类也是类的一种，与class关键字是不一样的。
* 手动编写的类编译后会产生一个Class对象，其表示的是创建的类的类型信息，而这个Class对象保存在同名.class的文件中（字节码文件）
* 每个通过关键字class标识的类，在内存有且只有一个与之对应的Class对象来描述其类型信息，无论创建多少个实例对象，其依据的都是一个Class对象。
* Class类只存私有构造函数，因此对应Class对象只能有JVM创建和加载
* Class类的对象作用是运行时提供或获得某个对象的类型信息，



#### 类加载

1.类加载机制的流程

![img](https://www.pdai.tech/images/jvm/java_jvm_classload_2.png)

2，类的加载

![img](https://www.pdai.tech/images/java/java-basic-reflection-3.png)

#### 反射的使用

> 基于此可以通过反射获取Class类对象以及类中的成员变量、方法、构造方法等





#### Class类对象的获取

在类加载的时候，jvm会创建一个class对象

class对象是可以说反射中最常用，获取class对象的方式主要有三种

* 根据类名： 类名.class
* 根据对象： 对象.getClass()
* 根据全限定类名： Class.forName(全限定类名)

```java
    public void classTest() throws Exception {
        // 获取Class对象的三种方式
        logger.info("根据类名:  \t" + User.class);
        logger.info("根据对象:  \t" + new User().getClass());
        logger.info("根据全限定类名:\t" + Class.forName("com.test.User"));
        // 常用的方法
        logger.info("获取全限定类名:\t" + userClass.getName());
        logger.info("获取类名:\t" + userClass.getSimpleName());
        logger.info("实例化:\t" + userClass.newInstance());
    }

```



#### Constructor类及其用法

> Constructor类存在于反射包中，反映的是Class对象所表示的类的构造方法。

获取Constructor对象是通过Class类中的方法获取的，Class类与Constructor相关方法如下：

* forName(String className) 
  * 返回带有给定字符串名的类或接口相关联的Class对象。 `static Class<?>`
* getConstructor(Class<?>...parameterTypes) 
  * 返回指定参数类型、具有public访问权限的构造函数对象` Constructor`
* getConstructors()
  * 返回所有具有public访问权限的构造函数的Constructore对象数组`Constructor<?>[]`
* getDeclaredConstructor(Class<?> ... parameterTypes)
  * 返回指定参数类型、所有声明的（包括private）构造函数对象`Constructor`
* getDeclaredConstructors()
  * 返回所有声明的(包括private)构造函数对象`Constructor<?>[]`
* newInstance()
  * 调用无参构造器创建此Class对象所表示一个类的一个新实例 `T`



以下是Constructor对象的使用

```java


public class ConstructionTest implements Serializable {
  public static void main(String[] args) throws Exception {
    Class<?> clazz = null;
    
    // 获取Class对象的引用
    clazz = CLass.forName("com.example.javabase.User");
    
    // 第一种，实例化默认构造方法，User必须无参构造，否则将抛异常
    User user = (User) clazz.newInstance();
    user.setAge(20);
    user.setName("Jack");
    System.out.prinln(user);
    
    // 第二种，获取带String参数的public构造函数
    Constructor cs1 = claszz.getConstructor(String.class);
    // 创建User
    User user1 = (User) cs1.newInstance("hi");
    user1.setAge(22);
    System.out.println(user1.toString());
    
    // 第三种， 取得指定带int和String参数构造函数，该方法是私有构造private
    Constructor cs2 = clazz.getDeclaredConstructor(int.class, String.class);
    // 由于是private需要设置才能访问；
    cs2.setAccessible(true);
    // 创建User对象
    User user2 = (User) cs2.newInstance(25, "Tom");
    System.out.println(user2.toString());
    
    
    // 第三种 获取所有构造包含private
    Constructor<?> cons[] = clazz.getDeclaredConstructors();
    // 查看每个构造方法需要的参数
    for(int i = 0; i< cons.lenth; i++) {
      // 获取构造函数参数类型
      Class<?> classs[] = cons[i].getParameterTypes();
      // 构造函数
      // cons[i].toString()
      // 参数类型 
      // i
      for(int j = 0; j < clazzs.length; j++) {
        if(j == clazzs.length - 1)
          System.out.println(clazzs[j].getName())
         else
           System.out.println(clazzs[j].getName() + ",")
      }
      System.out.println("")
    }
  }
}



class User {
  private int age;
  private String name;
  public User() {
    super();
  }
  public User(String name) {
    super();
    this.name = name;
  }
  
  /**
   * 私有构造
   * @param age
   * @param name
   */
  private User(int age, String name) {
    super();
    this.age = age;
    this.name = name;
  }
  public int getAge() {
    return age;
  }
  public void setAge(int age) {
    this.age = age;
  }
  public String getName() {
    return name;
  }

  public void setName(String name) {
    this.name = name;
  }
  @Override
  public String toString() {
    return "User{" +
      "age=" + age +
      ", name='" + name + '\'' +
      '}';
  }
}
```



##### 关于Constructor类本身一些常用的方法

* getDeclaringClass() 
  * 返回Class对象，该对象表示声明由此Constructor对象表示的构造方法的类，其实就是返回真实类型（不包含参数） `Class`
* getGenericParameterTypes()
  * 按照声明顺序返回一组Type对象，返回的就是Constructor对象构造函数的形参类型。`Type[]`
* getName() 
  * 以字符串形式返回有次构造方法的名称`String`
* getParameterTypes()
  * 按照声明顺序返回一组Class对象，即返回Constructor对象所表示构造方法的形参类型`Class<?>[]`
* newInstance(Object initargs) 
  * 使用此Constructor对象表示的构造函数来创建新实例`T`
* toGenericsString（）
  * 返回描述此Constructor的字符串，其中包括类型参数 `String`



#### Filed类及其用法

> Filed提供有关类或接口的单个字段的信息，以及对它的动态访问权限。反射的字段可能是一个类（静态）字段或实例字段

可以通过Class类提供的方法来获取代表字段信息的Field对象，Class类与Filed对象方法如下：

* getDeclaredField(String name) 
  * 获取指定name名称的（包含private修饰的）字段，不包括继承的字段`Field`
* getDeclaredFields()
  * 获取Class对象所表示的类或接口的所有（包含private修饰的）字段，不包括继承的字段`Field[]`
* getField(String name) 
  * 获取指定name名称，具有public修饰的字段，包含继承字段。`Filed`
* getFields()
  * 获取修饰符为public的字段，包含继承字段。`Field[]`

```java
public class ReflectField {
  public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
    Class<?> clazz = Class.forName("reflect.Student");
    // 第一种： 获取指定字段名称的Filed类，注意字段修饰符必须为public而且存在该字段
    // 否则抛出NoSuchFieldException
    Field field = clazz.getField("age");
    System.out.println(field)
      
      // 第二种 获取所有修饰符为public的字段，包含父类字段，注意修饰符为public才会获取
     Field fields[] = clazz.getFields();
    for(Field f: fields) {
     	System.out.println(f.getDeclaringClass());
    }
    
    // 第三种 获取当前类所字段（包含private）注意不包含父类字段
    Field fields2[] = clazz.getDeclaredFields();
    for(Field f: fields) {
      System.out.println(f.getDeclaringClass());
    }
    
    // 第四种 获取指定字段名称的Field类，可以是任意修饰符的自动，注意不包含父类的字段
    Field field2 = clazz.getDeclaredField("desc");
    System.out.println(field2)
  }
  /**
      输出结果: 
     field:public int reflect.Person.age
     f:public java.lang.String reflect.Student.desc
     f:public int reflect.Person.age
     f:public java.lang.String reflect.Person.name

     ================getDeclaredFields====================
     f2:public java.lang.String reflect.Student.desc
     f2:private int reflect.Student.score
     field2:public java.lang.String reflect.Student.desc
     */

} 
```





#### Method类及其用法

> Method提供了关于类或接口上单独某个方法（以及如何访问该方法）的信息，所反映的方法可能是类方法或实例方法（包括抽象方法）



* getDeclaredMethod(String name, Class<?> ... parameterTypes)
  * 返回一个指定参数的Method对象，该对象反映此Class对象所表示的类或接口的指定已声明方法`Method`
* getDeclaredMethods()
  * 返回Method对象的一个数组，这些对象反映此Class对象表示的类或接口声明的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。`Method[]`
* getMethod(String name, Class<?> ...parameterTypes)
  * 返回一个Method对象，它反映此Class对象所表示的类或接口的指定公共成员方法。`Method`
* getMethods()
  * 返回一个包含某些Method对象的数组，这些对象反映此Class对象所表示的类或接口(包括那些由该类或接口声明以及从超接口即成的那些类或接口)的公共成员方法

```java
public class ReflectMethod {
  public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException {
    Class clazz = Class.forName("reflect.Circle");
    
    // 第一种：根据参数获取public的Method，包含继承自父类的方法
    Method method = clazz.getMethod("draw", int.class, String.class)
      
    System.out.println(methods);
    
    // 第二种：获取所有public的方法
    Method[] methods = clazz.getMethods();
    for(Method m: methods) {
      System.out.println(m)
    }
    
    // 第三种： 获取当前类的方法包含private, 该方法无法获取继承父类的method
    Method method1 = clazz.getDeclaredMethod("drawCircle")
    System.out.println(methods1)
      
      // 第四种
      Method[] methods1 = clazz.getDeclaredMethods();
    for(Method m: methods1) {
      System.out.println(m)
    }
  }
}
```





#### 反射机制执行的流程

#### 反射获取类实例

首先调用java.lang.Class的静态方法，获取类信息

```java
@CallerSensitive
public static Class<?> forName(String className) throws ClassNotFoundException {
  // 先通过反射，获取调用进来的类信息，从而获取当前的classLoader
  
  Class<?> caller = Reflection.getCallerClass();
  // 调用native方法进行获取class信息
  return forName(className, true, ClassLoader.getClassLoader(caller), caller);
}
```



forName()反射获取类信息，并没有将实现留给java,而是交给了jvm去加载。

主要是先获取了ClassLoader,然后调用native方法，获取信息，加载类则是回调java.lang.ClassLoader

最后，jvm又会回调ClassLoader进行类加载。



后续 略。。。。。



#### Java常用机制-SPI机制详解

#### 什么是SPI机制

SPI（Service Provider Interface），是JDK内置的一种，服务提供发现机制，可以用来启用框架扩展，替换组件，主要是被框架的开发人员使用。Java中SPI机制主要思想是将装配的控制权移到程序之外，在模块化设计中这个机制尤其重要，其中核心思想就是 **解耦**

调用方，调用标准服务接口，然后本地服务发现加载服务。就可以使用服务方提供的各个实现类。

详细过程是:

* 当服务的提供者提供了一种接口的实现后，需要在classpath下，`META-INF/services/`，目录里创建了一个服务接口命名的文件，这个文件里的内容就是这个接口具体的实现类，当其他的程序需要这个服务的时候，就可以通过查找这个jar包，的`META-INF/services/`中的配置文件，配置文件中有接口的具体实现类名，可以根据这个类名进行加载实例化，就可以使用该服务了。JDK中查找服务的实现的工具类是:`java.util.SerivceLoader`



#### SPI机制简单示例

需求：需要一个内容搜索接口，搜索的实现可能是基于文件系统的搜索，也可能是基于数据库的搜索

* 先定义好接口

```java
public interface Search {
  public List<String> searchDoc(String keyword);
}
```

* 文件搜索实现

```java
public class FileSearch implements Search {
  @Override
  public List<String> searchDoc(String keyword) {
    System.out.println("文件搜索" + keyword);
    return null;
  }
}
```

* 数据库搜索实现

```java
public class DatabaseSearch implements Search
  @Overrid
  public List<String> searchDoc(String keyword) {
  	System.out.println("数据搜索" + keyword);
    return null;
}
```

* resources接下来可以在resources下新建META-INF/services/目录，然后新建接口全限定名的文件： `com.cainiao.learn.search`,里面加上需要用到的实现类

* 测试方法

```java
public class TestCase {
  public static void main(String[] args) {
    ServiceLoader<Search> s = ServiceLoader.load(Search.class);
    Iterator<Search> iterator = s.iterator();
    while(iterator.hasNext()) {
      Search search = iterator.next();
      search.serchDoc("hello world")
    }
  }
}
```

