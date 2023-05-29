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

