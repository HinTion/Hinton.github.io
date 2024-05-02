![](.Java_images/8f2439a8.png)

[目录](http://www.javathinker.net/kecheng/javaguide/mulu.pdf)

[孙卫琴讲解版](http://www.javathinker.net/lesson.jsp?type=1&number=1)

[杜聚宾讲解版](https://i.bjpowernode.com/course/video/10095.html)

# 与C++不同

## 精准控制格式化输出

```java
%(+|-)nd  // 格式化整数输出
%(+|-)m.nf  // 格式化浮点数输出
%(+|-)nx  // 格式化十六进制数输出
%(+|-)no  // 格式化八进制数输出
%(-)ns  // 格式化字符串输出
```

## 数据转换

### 整数转换为字符串

```java
public static void main(String[] args) {
    int x = 30;
    System.out.println(Integer.toBinaryString(x)); // 二进制11110
    System.out.println(Integer.toOctalString(x));  // 八进制36 
    System.out.println(Integer.toHexString(x)); // 十六进制1e
}
```


# 类

## 静态变量(static修饰) 和实例变量

在内存中只有一个, 被类的所有实例共享, 可以通过直接访问类名来访问静态变量

静态变量属于类级别的方法, 不依赖特定的实例

类的每个对象都有相应的实例变量, 每创建一个对象, JVM就会为他的实例变量分配内存

## 静态方法和实例方法

静态方法定义需添加static修饰, 不需要new对象, 直接通过 类名. 访问, 不能使用this, 因为静态方法中没有当前对象, 而this只能表示当前对象

大部分的工具类中的工具方法都是静态的, 方便使用

```java
int f = 100;
public static void m{
//以下三行都是报错的
//System.out.println(this);
//System.out.println(f);
//System.out.println(this.f);
var t = new t();
     System.out.

println(t.f);
}
```

## 生命周期

1. 类:从类的字节码被加载到内存中开始, 到被卸载结束
2. 对象: 从被创建开始, 倒不被任何变量引用结束

## 重载 & 继承

### 构造方法的重载

不同条件下, 对象会有不同的初始化行为 假如使用了this语句, 一定位于第一行

```java
public class Employee {
    String name;
    int age;

    //员工的姓名和年龄都已知
    public Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // 员工的姓名已知, 年龄位置
    public Employee(String name) {
        // 调用 Employee(String name, int age)构造方法
        this(name, -1);
    }

    // 姓名年龄都未知
    public Employee() {
        // 调用 Employee(String  name) 构造方法
        this("无名氏");
    }
}
```

### 成员方法的覆盖

子类的方法覆盖掉父类 父类的构造方法不会被子类继承, 也不会被子类的构造方法覆盖

```java
class base {
    public void peel(Apple apple) {
        手工削苹果
    }
}

class Sub extends base {
    public void peel(Apple apple) {
        削皮机削苹果皮
    }
}
```

Sub子类覆盖了base父类的peel方法, 为该方法提供了不同的实现方式, 但子类和父类方法的功能是一样的

#### 覆盖方法必须满足多种约束

1. 子类方法的名字, 参数签名和 返回类型必须与父类的一致

```java
class base {
    public void methos() {...}
}

//非法, 返回类型不一致
class sub extends base {
    public int method() {
        return 0;
    }
}
```

java ide首先判断sub类的method方法与base类的method方法的参数签名, 由于两者一致,ide认为sub类的method试图覆盖父类的方法 ==>
子类的方法必须和被覆盖的方法有相同的返回类型

```java
// 合法,  这是重载方法
class base {
    public void method() {...}
}

class sub extends basse {
    public int method(int a, int b) {
        return a + b;
    }
}
```

2. 子类不能缩小父类方法的访问级别 Eg. 父类开了个修车铺, 父亲宣布对外免费充气, 就不允许儿子对着干, 取消对外服务
3. 子类不能抛出比父类更多的异常 Eg. 父亲开诊所, 说个别顾客会出现肌肉疼痛异常, 儿子就不能够说顾客会出现晕厥,嘎异常

## super

Sub子类通过super关键字访问Base父类

```java
/*Base.java*/
package mypack1;

public class Base {
    String var = "Base";

    void method() {
        System.out.println("Base method");
    }
}

/* Sub.java */
package mypack1;

public class Sub extends Base {
    String var = "Sub";

    void method() {
        System.out.println("Sub method");
    }

    void test() {
        // 访问sub类的实例变量var, 则打印Sub
        System.out.println(var);

        // 访问Base类的实例变量var, 则打印Base
        System.out.println(super.var);

        // 调用Sub可以的method, 打印Sub method
        method();

        // 调用Base类的method, 打印 Base method
        super.method();

    }

    public static void main(String[] args) {
        Sub sub = new Sub();
        sub.test();
    }
}

/* Sub.java */
package mypack1;

public class Sub extends Base {
    String var = "Sub";

    void method() {
        System.out.println("Sub method");
    }

    void test() {
        // 访问sub类的实例变量var, 则打印Sub
        System.out.println(var);

        // 访问Base类的实例变量var, 则打印Base
        System.out.println(super.var);

        // 调用Sub可以的method, 打印Sub method
        method();

        // 调用Base类的method, 打印 Base method
        super.method();

    }

    public static void main(String[] args) {
        Sub sub = new Sub();
        sub.test();
    }
}
```

## final

1. 修饰的类 不能被继承, 即没有子类
2. 修饰的成员方法不允许被子类的方法覆盖
3. 修饰的变量表示常量, 只允许被赋一次值

## abstract

```java
public abstract class Vehicle {
    public abstract void run(); // 模拟驾驶的行为
}
```

交通工具是一个抽象的概念, Vehicle用abstract修饰

Vehicle的run也用abstract修饰, 意味着它是一个没有被实现的抽象方法, 仅仅声明了交通工具应该有的功能

```java
public class car extends Vehicle {
    public void run() {
        发动引擎, 带动车轮滚动
    }
}

public class bike extends Vehicle {
    public void run() {
        蹬踏板, 车轮滚动
    }
}
```

### 修饰内容

1. 修饰的类表示抽象类, 不能被实例化, 即不能通过new语句创造实例本身
2. 修饰的成员方法表示抽象方法, 抽象方法没有方法体, 用来声明类具有什么功能, 但不提供具体的实现

```java
public abstract class base {
    public abstract void method();

    public void method2() {
        System.out.println("method2");
    }

}
// base就像一个产品的半成品, 有的功能已经实现, 有的功能还没实现, 需要子类去实现
```

### 语法规则

1. 如果一个类有抽象方法, 这个类必须定义为抽象类
2. 如果子类没有实现父类中所有的抽象方法, 子类也必须被定义为抽象类
3. 没有静态抽象方法, abstract和static不能一起用

## 重载toString()方法

```java
public class Employee {
    String id;
    String name;

    public Employee() {
    }

    public Employee(String id, String name) {
        this.id = id;
        this.name = name;
    }

    //覆盖Object类的equals()方法
    public boolean equals(Object o) {...}

    // 覆盖Object类的toString()方法
    public String toString() {
        return "id=" + id + ",name=" + name;
    }

    // 重载Object类的toString()方法
    public static String toString(String info) {
        String[] infos = info.split(",");
        return "id = " + infos[0] + ", name = " + infos[1];
    }

    public static void main(String[] args) {
        Employee m1 = new Employee("001", "张三");
        Employee m2 = new Employee("001", "张三");

        System.out.println(m1.equals(m2));

        // 打印: id = 001, name = 张三
        System.out.println(m1);

        //打印: id = 001, name = 张三
        System.out.println(Employee.toString("001, 张三"));
    }
}
```

## 多态

本质: 父类型引用指向子类型对象

```java
/* Performer.java */
public class Performer {
    // 表示所有Performer对象(包括子类对象)的数目
    static int count;

    String grade;
    String name;

    public Performer() {
        this("无名氏", "未知级别");
    }

    public Performer(String name, String grade) {
        count++;
        this.name = name;
        this.grade = grade;
    }

    public void perform() {
        System.out.println("报幕");
    }

    public static void printCount() {
        System.out.println("所有演员的数目: " + count);
    }
}

/* Dancer.java */
public class Dancer extends Performer {
    // 表示所有Dancer对象的数目
    static int count;

    // 舞蹈演员的级别
    String grade;

    public Dancer() {
        this("无名氏", "未知级别", "未知级别");
    }

    public Dancer(String name, String dancerGrade, String performerGrade) {
        super(name, performerGrade);
        this.grade = dancerGrade;
        count++;
    }

    public void perform() {
        System.out.println("跳舞");
    }

    public static void printCount() {
        System.out.println("所有舞蹈演员的数目: " + count);
    }

    public static void main(String[] args) {
        Performer p1 = new Performer("Tom", " 一级演员");
        Performer p2 = new Dancer("Mary", "一级舞蹈演员", "一级演员");

        p1.perform(); // 报幕
        p1.printCount(); // 所有演员的数目: 2
        System.out.println(p1.count); // 2
        System.out.println(p1.grade); // 一级演员

        p2.perform(); // 跳舞
        p2.printCount(); // 所有演员的数目: 2
        System.out.println(p2.count); // 2
        System.out.println(p2.grade); // 一级演员

        Dancer p3 = (Dancer) p2;
        p3.perform();  // 跳舞
        p3.printCount();  // 所有舞蹈演员的数目: 1
        System.out.println(p3.count); // 1
        System.out.println(p3.grade); // 一级演员
    }

}
```

通过变量p2访问实例方法perform(), 实际上调用的是Dancer实例的perform方法 通过变量p2访问静态方法printCount(), 静态变量count, 实例变量grade, 都是调用的performer类的静态方法和成员变量

### 引用变量的类型转换

![](.Java_images/945e5ba2.png)

### 继承 & 多态

1. 如果类B扩展类A => 类B是类A
2. 编译器检查的是引用变量的类, 而不是引用指向的具体对象的类
3. 如果没有显示扩展另一个类, 所有这样的类都隐含扩展了Object

```java
class Object {
    boolean equals(); // 确认两个对象是否相等

    Class getClass(); // 返回那个对象的类类型, 即他是从哪个类实例化得来的

    int hashCode(); //  打印对象的一个散列码

    String toString(); // 打印一个字符串, 包含这个类名+数字
}
```


## 内部类

一个类中可以嵌套另外一个类，语法格式如下：

```java
class OuterClass {   // 外部类
    // ...
    class NestedClass { // 嵌套类，或称为内部类
        // ...
    }
}
```

要访问内部类，可以通过创建外部类的对象，然后创建内部类的对象来实现。

### 成员内部类


```java
/* Outer.java */
public class Outer {
    public class Inner {
        public int max(int a, int b) {
            return a >= b ? a : b;
        }
    }

    public void test() {
        Inner inner = new Inner();
        System.out.println(inner.max(1, 2));
    }
}

/* Client.java*/
public class Client {
    public static void main(String[] args) {
        Outer.Inner inner = new Outer().new Inner();
        System.out.println(inner.max(1, 2));
    }
}
```

在outer类中, 可以直接访问inner类

```java
Inner inner = new inner();
```

Inner类的完整类名是Outer.Inner

Client类是Outer类及其内部类的客户类, 如果要在Client类中使用Inner类, 必须引用完整类名

```java
Outer.Inner inner;
```

#### 实例内部类 (妈妈和宝宝的关系)

实例内部类的实例依赖于外部类的特定实例, 能直接访问外部类的所有成员

```java
public class Mother {
    private int a1;
    public int a2;
    static int a3;

    public Mother(int a1, int a2) {
        this.a1 = a1;
        this.a2 = a2;
    }

    protected int method() {
        return a1 + a2;
    }

    class Baby {
        int b1 = a1, b2 = a2, b3 = a3, b4 = method();
    }

    public static void main(String[] args) {
        Mother.Baby baby = new Mother(1, 2).new Baby();
        System.out.println(baby.b1); // 1 
        System.out.println(baby.b2); // 2
        System.out.println(baby.b3); // 0
        System.out.println(baby.b4); // 3
    }
}
```

#### 静态内部类 (static 修饰)

1. 静态内部类的实例不和外部类的特定实例关联, 在创建静态内部类的实例时, 不必创建外部类的实例

```java
/*Outer.java*/
public class Outer {
    public static class Inner {
        int v;   // 静态内部类
    }
}

/* Client.java */
public class Client {
    public static void main(String[] args) {
        Outer.Inner inner = new Outer.Inner();
        System.out.println(inner.v); // 打印  0
    }
}
```

2. 静态内部类可以直接访问外部类的静态成员, 如果访问外部类打的实例成员, 必须通过外部类的实例去访问

```java
public class Outer {
    private int a1;
    private static int a2;

    public static class Inner {
        int b1 = a1; // 非法, 不能直接访问Outer类的实例变量a1
        int b2 = new Outer().a1; // 合法, 访问Outer实例
        init b3 = a2; // 合法
    }
}   
```

### 局部内部类(方法中定义)

```java
public class Outer {
    Inner b = new Inner(); // 非法, 不允许访问方法的局部内部类

    public void method() {
        class Inner {        // 局部内部类
            int v1, v2;
        }
    }

    Inner b = new Inner();   // 合法
}
```

```java
public class Outer {
    int a;

    public void main(final int p1, int p2) {
        int localv1 = 1;
        final int localv2 = 2;
        localv1 = 3;

        class Inner {  // 局部内部类
            int b1 = a;
            int b2 = p1;
            int b3 = p2;
            int b4 = localv1;  //  非法, 不允许访问方法的非最终变量
            int b5 = localv2;
        }
    }

}
```

最终参数和变量, 虽然没有被final修饰, 但是取值不会改变

以上代码中p2是实际上的最终参数, 因为它的取值没有发生改变, 因此, 内部类Inner可以访问p2

### 匿名类(没有名字)

```java
public class MotorDriver {
    MotorDriver(String vehicle) {
        System.out.println(vehicle);
    }

    MotorDriver() {
        this("未知车型");
    }

-

    public static void main(String[] args) {
        MotorDriver driver = new MotorDriver() {   // 匿名类
            public void drive() {
                System.out.println("开车闯红灯");
            }
        };

        driver.drive();
    }
}
// 运行结果
// 未知车型
// 开车闯红灯
```

以上 new MotorDriver(){...} 语句定义了一个继承MotorDriver类的匿名类, 并且会创建这个匿名类的实例

1. 匿名类本身没有显式定义构造方法, 但是会调用父类的构造方法

```java
public static void main(String[] args) {
    MotorDriver driver = new MotorDriver("雅马哈摩托车") {
        public void drive

        {
            System.out.println("开车闯红灯");
        }
    };

    driver.drive();
}
/*
 运行结果:
 雅马哈摩托车
 开车闯红灯
*/
```

2. 匿名类可以实现接口

```java
public static void main(String[] args) {
    // Thread类的构造方法的参数是实现Runnable接口的匿名类的实例
    Thread t = new Thread(new Runnable() {
        public void run() {
            System.out.println("Hello");
        }
    });

    t.start();
}
```

上述匿名类实现了java.lang.Runnable接口, 这个匿名类的实例的引用作为参数, 传给java.lang.Thread类的构造方法.

main的第一句相当于

```java
// 定义一个实现Runnable接口的匿名类并创建它的实例
Runnable r = new Runnable() {
    public void run() {
        System.out.println("Hello");
    }

    ;
    Thread t = new Thread(r);
}
```

3. 匿名类可以访问外部类的所有成员, 而且当匿名类位于一个方法中时, 就和局部类一样, 还可以访问所在方法的final类型变量和参数, 以及实际上的最终参数和变量

## 构造器 & 垃圾回收

1. 构造器: 调用new时所运行的代码
2. 构造器没有返回值
   ```java
   public Duck{
      System.out.println("Quack");
   }
   ```
3. 构造器不能继承
4. 两个构造器不能有相同的参数列表(包括类型和顺序)
5. super()语句必须放在第一行
6. 每个构造器可以有一个super()或者this()调用

```java
import java.awt.color;

class Mini extends Car {
    private Color color;

    // 无参数构造器提供一个默认的color, 并调用重载的实际构造器(调用super()的那个构造器)
    public Mini() {
        this(Color.RED);
    }

    // 完成初始化对象具体工作(包括调用super())的实际构造器
    public Mini(Color c) {
        super("Mini");
    }

    // ERROR  不能同时有super()和this()因为他们都必须位于第一条语句
    public Miini(int size) {
        this(Color.RED);
        super(size);
    }
}
```


# 反射

包的名称

```java
java.lang.reflect .*;
```

## 相关类

必须先获得Class才能获取Method、Constructor、Field

| 类                             | 含义                              |
|-------------------------------|---------------------------------|
| java.lang.Class               | 整个字节码。一个类型，整个类                  |
| java.lang.reflect.Method      | 字节码中的方法字节码。 类中的方法               |
| java.lang.reflect.Constructor | 字节码中的构造方法字节码。代表类中的构造方法          |
| java.lang.reflect.Field       | 字节码中的属性字节码。代表类中的成员变量（静态变量+实例变量） |


所有Java类都有一个静态变量class, 该变量引用这个类的class对象

> Q: 为什么JVM为每个类创建一个class对象
>
>A: 类文件中包含二进制的字节码, java程序无法直接读懂字节码. JVM会解析类的字节码, 把他们包装成class对象, Constructor对象(
> 类的构造方法), Method对象(类的成员方法), Java就能按照访问对象的方式, 来方便地获取类的类型信息

给出一个待测试的Employee类

```java
public class Employee {
    int id;
    String name;
    int age;

    public employee() {
    }

    public Employee(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }

    public int getId() {
        returnid;
    }

    public void setId
}
```

## 动态创建对象

```java
import java.lang.reflect.*;

public class ConstructorTest {
    public static void main(String[] args) throws Exception {
        Class<Employee> classType = Employee.class;

        // 不带参数的默认构造方法
        Constructor constructor = classType.newInstance(new Class[]{});

        //默认构造方法创建Employee对象
        Object m1 = constructor.newInstance(new Object[]{});

        // 带参数的构造方法
        constructor = classType.getConstructor(new Class[]{int.class, String.class, int.class});

        // 带参数的构造方法创建Employee对象
        Object m2 = constructor.newInstance(new Object[]{Integer.valueOf(1), "Tom", 21});

        // 打印=> id: 0, name: null, age: 0
        System.out.println(m1);

        // 打印=> id: 1, name: Tom, age: 21
        System.out.println(m2);
    }
}
}
```

Class类的getConstructor(Class<?... parameterTypes)方法返回表示构造方法的Constructor对象, parameterTypes指定构造方法的参数类型 newInstance(Object...initargs)方法会根据当前构造方法创建对象,参数initargs指定传给构造方法的参数

## 动态访问对象的属性

```java
import java.lang.reflect.Field;

public class FieldTest {
    public static void main(String[] args) throws Exception {
        Employee m = new Employee(1, "Tom", 21);

        // 表示Employee类的Class对象
        Class<?> classType = m.getClass();

        // 表示Employee类的name属性
        Field nameField = classType.getDeclaredField("name");

        // 设置Employee对象的name属性
        nameField.set(m, "Mike");

        // 获得Employee对象的name属性值
        String name = (String) nameField.get(m);

        // 打印=> Mike
        System.out.println(name);
    }
}
```

getDeclaredField(String name)方法返回表示类的特定属性的Field对象, 参数name指定属性的名字

```java
// 返回表示Employee类的name属性的Field对象
Field nameField = classType.getDeclaredField("name");
```


## 动态访问对象的方法

```java
import java.lang.reflect.Method;

public class MethodTest {
    public static void printMethods(Class<?> classType) throws Exception {
        // 获得类的所有方法
        Method methods[] = classType.getDeclaredMethods();
        for (Method method : methods)
            System.out.println(method.toString());
    }

    public static void invokeMethod(Employee m) throws Exception {
        // 表示Employee类的setAge方法
        Method method = m.getclass().getMethod("setAge", new Class[]{int.class});
        // 调用Employee对象的setAge方法
        method.invoke(m, new Object[]{Integer.valueOf(18)});

        // 表示Employee类的getAge方法
        method = m.getClass().getMethod("getAge");
        // 调用Employee对象的getAge方法 返回Integer对象
        Object returnValue = method.invoke(m);
        //打印=> 18
        System.out.println(returnValue);
    }

    public static void main(String[] args) throws Exception {
        printMethods(Employee.class);
        invokeMethod(new Employee(1, "Tom", 21));
    }
}
```

invoke(Object obj, Object... args) 方法调用参数obj引用对象的方法, args参数指定传入方法的参数

## 动态创建和访问数组

java.lang.Array包下

1. newInstance() 创建数组对象
2. set() 给数组的特定元素赋值
3. get() 读取数组的特定元素的值

```java
import java.lang.reflect.Array;

public class Array1 {
    public static void main(String[] args) throws Exception {
        // 创建一个长度20的字符串数组
        Object array = Array.newInstance(String.class, 20);

        // 把索引5的位置设置为hello
        Array.set(array, 5, "hello");

        //读取索引5的元素
        String s = (String) Array.get(array, 5);
        System.out.println(s);

        //获取数组元素的类型
        Class<?> elementType = array.getClass().getComponentType();
        //打印=> java.lang.String
        System.out.println(elementType.getName());
    }
}
```

```java
import java.lang.reflect.Array;

public class Array2 {
    public static void main(String[] args) {
        int dims[] = new int[]{20, 20};
        Object array = Array.newInstance(double.class, dims);

        //返回array[3]
        Object array3 = Array.get(array, 3);

        //把元素array[3][5]设为66.22
        Array.set(array3, 5, 66.22);
        // 打印=> 66.22
        System.out.println(Array.get(array3, 5));
    }
}
```

# 接口

统一两个类之间的交互方式, 用于声明现类具备的功能, 或对外提供的服务

```java
public interface BladeIFC {
    // 接口中抽象方法的abstract可以省略
    public void rotate();

    public void stop();
}

public class Blade implements BladeIFC {
    // 终止扇叶旋转的信号标记
    private boolean isStop = true;

    // 扇叶旋转
    public void rotate() {...}

    // 终止旋转
    public void stop() {....}

}
// 如果Blade类实现了BladeIFC窗口, 但没有实现rotate()或stop()方法, 编译会出错, 需要把Blade类声明为抽象类
```

1. 如果不希望实例化一个类, 就用abstract标记抽象类
2. 所有抽象方法必须在继承树种的第一个具体子类中实现
3. 抽象方法没有方法体, 而且声明以一个分号结尾
4. Java中的每个类要么是类Object的直接子类, 要么是他的间接子类
5. 如果不做强制类型转换, Object类型的引用变量不能赋给任何其他引用类型

```java
Dog d = (Dog) x.getObject(aDog);
```

6. 不能多重继承, 会带来"致命的死亡菱形", 一个类只能有一个直接超类


## 成员变量

接口中只能包含public, static, final类型的成员变量, 必须被显式初始化

```java
public interface CommonIFC {
    public static final int MAX_CAPACITY = 1000;
    int MIN_CAPACITY = 100;
}
```

接口只能包含静态常量, 通过接口名字来访问

```java
System.out.println(CommonIFC.MAX_CAPACITY);
```

> Q: 为什么只能包含静态常量, 不能包含实例变量
>
> A: 接口侧重声明对外提供的公开服务, 这样能提高接口的独立性和通用性

## 方法

接口没有构造方法, 只有成员方法

方法默认情况都是public访问级别, 只是显式不显示的问题

接口中可以包含三种对外公开的方法

* 抽象方法: 没有提供具体的实现
* 默认方法: default修饰, 提供默认的具体实现
* 静态方法: 提供具体的实现, 允许直接通过接口的名字来访问

```java
public interface ServiceIFC {

    // 公开静态方法
    public static void method1() {
        forMethod1();
    }

    // 私有的静态方法
    public static void forMethod1() {
        System.out.println("私有静态方法");
    }

    // 公开默认方法
    public default void method2() {
        forMethod2();
    }

    // 私有的默认方法
    private void forMethod2() {
        System.out.println("私有默认方法");
    }
}
```

## 抽象类与接口的区别

1. 接口的成员变量只能是public, static, final类型, 静态方法和默认方法只能是public, private访问级别
2. 一个类只能继承一个直接的父类, 但可以实现多个接口

```java
abstract class Flyable {
    public abstract void fly();
}

class Bird extends Animal, Flyable {...
} // 不允许

class Kite extends HandCraft, Flyable {...
} // 不允许
```

```java
// 定义为接口, 就会很方便
interface Flyable {
    void fly();
}

class Bird extends Animal, Flyable {...
}

class Kite extends HandCraft, Flyable {...
} 
```

3. 接口是对多个类的共同行为, 抽象类是根据多个类的共性进行的类型抽象
4. 接口是软件系统中最高层次的抽象, 声明了软件系统对外提供的服务
5. 接口本身必须十分稳定的, 一旦被制定, 就不允许随意修改
6. 通常用抽象类定制程序中的软件系统中的扩展点. 把抽象类看作介于"抽象"和"实现"之间的半成品,抽象类力所能及地完成了部分实现细节, 还有一些功能等待它的子类去实现和扩展


"我怎么知道API里有什么?" -- 成功的关键

1. 对一个项目或库的整体组织有帮助, 不再是一大堆杂乱的类, 可以分组到特定功能的包中
2. 包会提供一个命名作用域, 如果和其他人都创建了一个同名的类, 需要告诉JVM想使用的是哪个包的类
3. 提供一层安全性, 使得只有同一个包中的其他类能访问那些代码
4. import 可以让少敲代码, 只是为java提供类全名的方法 运行速度不会因此变慢,也不会膨胀

## ~~STL~~

### ArrayList

#### add(E, e)

将指定元素增加到列表末尾

#### remove(int index)

删除指定位置上的元素

#### remove(Object o)

删除指定元素的第一次出现

#### contains(Object o)

如果列表中还有指定元素, 返回true

#### indexOf(Object o)

返回元素的第一个索引 或者 -1

#### get(int index)

返回指定位置的元素

```java
import java.util.ArrayList;

ArrayList<Egg> myList = new ArrayList<Egg>();

// 增
Egg egg1 = new Egg();
myList.

add(egg1);

// 查
boolean isIn = myList.contains(egg1);

// 找
int idx = myList.indexOf(egg1);

// 删
myList.

remove(egg1);
```


# 异常处理

1. 异常 是一个Exception类型的对象
2. 抛出异常的方法必须声明它可能抛出异常
3. 如果要准备处理异常, 把调用包在一个try/catch中, 并把异常处理/恢复代码放在catch块中
4. 如果无法从异常恢复, 至少使用printStackTrace()得到一个栈轨迹
5. 编译器不该耐心RuntimeException类型的异常
6. try成功 => catch块中的代码就不会运行, 转到finally块, 然后再运行方法的其余部分 try失败, 则try中其余代码不会运行, 转到catch块, catch块完成时, finally块开始运行, 然后再继续运行方法的其余部分 如果try/catch块中有一个return语句 => finally块还是会运行

![1](./Images/Java-1714043905041.png)

```java
public class ExcepTest {
    public static void main(String args[]) {
        int a[] = new int[2];
        try {
            System.out.println("Access element three :" + a[3]);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Exception thrown  :" + e);
        } finally {
            a[0] = 6;
            System.out.println("First element value: " + a[0]);
            System.out.println("The finally statement is executed");
        }
    }
}
```

```java
// InsufficientFundsException.java

import java.io.*;

//自定义异常类，继承Exception类
public class InsufficientFundsException extends Exception {
    //此处的amount用来储存当出现异常（取出钱多于余额时）所缺乏的钱
    private double amount;

    public InsufficientFundsException(double amount) {
        this.amount = amount;
    }

    public double getAmount() {
        return amount;
    }
}

// CheckingAccount.java
import java.io .*;

//此类模拟银行账户
public class CheckingAccount {
    //balance为余额，number为卡号
    private double balance;
    private int number;

    public CheckingAccount(int number) {
        this.number = number;
    }

    //方法：存钱
    public void deposit(double amount) {
        balance += amount;
    }

    //方法：取钱
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount <= balance) {
            balance -= amount;
        } else {
            double needs = amount - balance;
            throw new InsufficientFundsException(needs);
        }
    }

    //方法：返回余额
    public double getBalance() {
        return balance;
    }

    //方法：返回卡号
    public int getNumber() {
        return number;
    }
}

// BankDemo.java
public class BankDemo {
    public static void main(String[] args) {
        CheckingAccount c = new CheckingAccount(101);
        System.out.println("Depositing $500...");
        c.deposit(500.00);
        try {
            System.out.println("\nWithdrawing $100...");
            c.withdraw(100.00);
            System.out.println("\nWithdrawing $600...");
            c.withdraw(600.00);
        } catch (InsufficientFundsException e) {
            System.out.println("Sorry, but you are short $" + e.getAmount());
            e.printStackTrace();
        }
    }
}
/*
 打印
 Depositing $500...

Withdrawing $100...

Withdrawing $600...
Sorry, but you are short $200.0
InsufficientFundsException
        at CheckingAccount.withdraw(CheckingAccount.java:25)
        at BankDemo.main(BankDemo.java:13)

*/
```




































































































































