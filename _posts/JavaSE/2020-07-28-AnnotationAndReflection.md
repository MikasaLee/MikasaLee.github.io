---
layout: post
title: Java 注解和反射
categories: JavaSE
description: 
keywords: Java,Reflection,Annotation
---

# 注解
Annotation 是从JDK5.0开始引入的新技术

+ Annotation 的作用：
	+ 不是程序本身，但可以对程序作出解释。(这一点跟注释(comment)没什么区别)
	+ **可以被其他程序（eg：编译器等）读取**
+ Annotation 的格式
	+ 注解是以 `@注释名` 在代码中存在的，还可以添加一些参数值（eg：`@SuppressWarnings(value="unchecked")`）
+ Annotation 在哪里可以使用？
	+ 可以附加在 package，class，method，field 等上面，相当于给他们添加了额外的辅助信息，我们可以通过反射机制编程实现对这些元数据的访问
+ 注解可以分为 内置注解，元注解，自定义注解

## 1、 内置注解
这里只介绍三个比较常见的。
+ @Override ： 定义在 java.lang.Override 中，此注解只适用于修饰方法，表示一个方法打算重写父类中的另一个方法
+ @Deprecated ：定义在 java.lang.Deprecated 中，此注解可以用于修饰方法，属性，类。表示不鼓励程序员使用这样的元素，通常是因为它很危险或者存在更好的选择。
+ @SuppressWarnings ：定义在 java.lang.SuppressWarnings 中，用来抑制编译时的警告信息。
	+ 与前两个不同，需要添加一个参数才能正确使用，这些参数已经定义好了，我们选择性的使用就行
	+ "参数：all","unchecked",value={"unchecked","deprecation"} 等

## 2、 元注解
+ 元注解(meta-annotation)的作用就是负责注解其他注解，java定义了4个标准的 meta-annotation 类型，他们被用来提供对其他 annotation 类型作说明
+ 这些类型和它们所支持的类在 java.lang.annotation 包中可以找到
	+ @Target注解：描述注解的使用范围（即：被修饰的注解可以用在什么地方） 
	+ @Retention注解：用于描述注解的生命周期，一共有三种策略（范围：RUNTIME > CLASS > SOURCE）
		+ SOURCE,    // 源文件保留
    	+ CLASS,       // 编译期保留，默认值
    	+ RUNTIME   // 运行期保留，可通过反射去获取注解信息
    + @Documented注解：描述在使用 javadoc 工具为类生成帮助文档时是否要保留其注解信息。
    + @Inherited注解：子类自动继承父类的该注解

### 2.1、 Demo
```java
@MyAnnotation
public class Test02 {
    public static void main(String[] args) {

    }
    @MyAnnotation
    public void test(){}
}

@Target(value={ElementType.TYPE,ElementType.METHOD})
@Retention(value = RetentionPolicy.RUNTIME)
@Documented
@Inherited
@interface MyAnnotation{}
```

## 3、 自定义注解 
+ 使用 @interface 自定义注解时，自动继承了 java.lang.annotation.Annotation 接口
+ 格式: @interface 注解名{定义内容}
+ 其中的每一个方法实际上是声明了一个配置参数
+ 方法名就是参数名，返回值类型就是参数的类型(只能为基本类型,Class,String,enum)。
+ 可以通过default来声明参数的默认值
+ 如果只有一个参数，推荐使用 value，这样我们在使用的时候可以简写
+ 注解元素必须要有值，我们定义注解元素时，经常使用空字符串，0作为默认值

### 3.1、 Demo
```java
public class Test03 {
    //没有默认值，则使用时必须显示赋值
    @MyAnnotation2(age=18,schools = {"asdf","safd"})
    @MyAnnotation3("可以省略value")
    public void test01(){}
}

@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation2{
    //参数写法有点像普通类中的方法声明：参数类型 + 参数名();
    String name() default "";
    int age();
    int id() default -1;
    String[] schools();
}

@Target({ElementType.METHOD,ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation3{
    String value();
}
```

# 反射
+ Reflection(反射)是java被视为准动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。
	eg ：`Class stringClass = Class.forName("java.lang.String"); `
+ 加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象(一个类只有一个Class对象)，这个对象就**包含了完整的类的结构信息。我们可以通过这个对象看到类的结构**。
+ 正常方式：引入需要的"包类"名称 ——> 通过new实例化 ——> 取得实例化对象
+ 反射方式：实例化对象 ——> getClass()方法 ——> 得到完整的"包类"名称
+ 反射的优缺点：
	+ 优点：可以实现动态创建对象和编译，体现出很大的灵活性
	+ 缺点：对性能有影响。使用反射基本上是一种解释操作，我们可以告诉JVM，我们希望做什么并且它满足我们的要求。这类操作总是慢于 直接执行(通过new执行)相同的操作。

## 1、Class 类
+ 在Object类中定义了一个getClass方法：`public final Class getClass()`
+ 该方法返回值的类型是一个 Class 类，这是Java反射的源头.

### 1.1、 入门Demo
```java
public class Test01 {

    public static void main(String[] args) throws ClassNotFoundException 	{
        //通过反射获取类的Class对象
        Class c1 = Class.forName("com.lrr.reflection.User");

        System.out.println(c1);
        Class c2 = Class.forName("com.lrr.reflection.User");
        Class c3 = Class.forName("com.lrr.reflection.User");
        Class c4 = Class.forName("com.lrr.reflection.User");

        //一个类在内存中只有一个Class对象
        //一个类被加载后，类的整个结构都会被封装在Class对象中
        System.out.println(c2.hashCode());
        System.out.println(c3.hashCode());
        System.out.println(c4.hashCode());
    }
}


//一个普通的pojo类
class User{
    private String name;
    private int id;
    private int age;
    public User() {
    }
    public User(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }
   public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getId() {
        return id;
    }
    public void setId(int id) {
        this.id = id;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}
```
### 1.2、 得到Class类的几种方式
1. 若已知具体的类，通过类的 class 属性获取，该方法最为安全可靠，程序性能最高
	`Class c1 = Person.class;`
2. 已知某个类的实例，调用该实例的getClass() 方法获取 Class 对象
	`Class c2 = person.getClass();`
3. 已知一个类的全类名，且该类在类路径下，可通过 Class 类的静态方法 forName() 获取，可能抛出 ClassNotFoundException 异常
	`Class c3 = Class.forName("com.lrr.reflection.Person");`
4. 内置基本数据类型可以直接用类名.Type
5. 还可以利用ClassLoader (之后介绍)

Demo：
```java
package com.lrr.reflection;

public class Test02 {
    public static void main(String[] args) throws ClassNotFoundException {
        //1. 通过类名.class获得
        Class c1 = Person.class;
        Person person = new Person();
        //2. 通过对象获得
        Class c2 = person.getClass();
        //3. forName获得
        Class c3 = Class.forName("com.lrr.reflection.Person");

        //基本类型的包装类都有一个type
        Class c4 = Integer.TYPE;

        //获得父类类型
        Class c5 = c1.getSuperclass();
        
        System.out.println(c1.hashCode());
        System.out.println(c2.hashCode());
        System.out.println(c3.hashCode());
        System.out.println(c4);
        System.out.println(c5);
    }
}

class Person{
    String name;
}
```
输出：

```
664740647
664740647
664740647
int
class java.lang.Object
```

### 1.3、 哪些类型可以有Class对象
+ class：外部类，成员(成员内部类，静态内部类)，局部内部类，匿名内部类
+ interface：接口
+ []：数组(所有维度)
+ enum：枚举
+ annotation：注解@interface
+ primitive type：基本数据类型
+ void

Demo:
```java
package com.lrr.reflection;

import java.lang.annotation.ElementType;

public class Test03 {
    public static void main(String[] args) {
        Class c1 = Object.class;        //类
        Class c2 = Comparable.class;    //接口
        Class c3 = String[].class;      //一维数组
        Class c4 = int[][].class;       //二维数组
        Class c5 = Override.class;      //注解
        Class c6 = ElementType.class;   //枚举
        Class c7 = Integer.class;       //包装类
        Class c8 = int.class;           //基本数据类型
        Class c9 = void.class;          //void
        Class c10 = Class.class;        //Class

        System.out.println(c1 +" "+c1.hashCode());
        System.out.println(c2 +" "+c2.hashCode());
        System.out.println(c3 +" "+c3.hashCode());
        System.out.println(c4 +" "+c4.hashCode());
        System.out.println(c5 +" "+c5.hashCode());
        System.out.println(c6 +" "+c6.hashCode());
        System.out.println(c7 +" "+c7.hashCode());
        System.out.println(c8 +" "+c8.hashCode());
        System.out.println(c9 +" "+c9.hashCode());
        System.out.println(c10+" "+c10.hashCode());

        //只要类型一致，就是同一个Class
        int[] a= new int[10];
        int[] b= new int[100];
        System.out.println(a.getClass().hashCode());
        System.out.println(b.getClass().hashCode());
    }
}

```

输出：
```
class java.lang.Object 664740647
interface java.lang.Comparable 804564176
class [Ljava.lang.String; 1421795058
class [[I 1555009629
interface java.lang.Override 41359092
class java.lang.annotation.ElementType 149928006
class java.lang.Integer 713338599
int 168423058
void 821270929
class java.lang.Class 1610427175
1160460865
1160460865
```

## 2、 简单从JVM角度分析类初始化
这部分看不懂可以跳过，完整学习完JVM(学完周志明的《深入理解Java虚拟机》就能理解了)

### 2.1、 Java的内存布局
当一个java程序运行时，jvm会划分三块内存区：堆，栈，方法区。
+ 堆：
   +  存放 new 的对象和数组
   +  被所有的线程共享，不会存放别的对象的引用
+ 栈：
   + 存放基本类型
   + 存放对象在堆中的具体地址(就是指针)
+ 方法区：
   + 被所有线程共享
   + 包含了所有的静态变量，静态方法，常量池，方法
   
### 2.2、 类的加载过程
当程序主动使用某个类时，如果该类还未被加载到内存中，则系统会通过以下三个步骤来对类进行初始化
+ 类的加载(Load)：将类的class文件字节码读入内存，并将这些静态数据(**代码，常量池，静态方法，静态变量**)转换成方法区的运行时数据结构，然后为之创建一个java.lang.Class对象来表示该类(**注意这时的Class对象并没有被初始化，只是在堆中创建了一个指向方法区的对象，所以此阶段为 Load 而不是 Initialize **)。此过程由类加载器完成。
+ 类的链接(Link)：将类的二进制数据合并到 JRE 中
   + 验证：确保加载的类信息符合 JVM 规范，没有安全方面的问题
   + 准备：正式为类变量(static)分配内存并设置类变量默认初始值的阶段（**注意static变量在类初始化之前的链接阶段创建的，也就是先有static变量，再有Class类**），这些内存都将在方法区中进行分配。
   + 解析：虚拟机常量池内的符号引用(常量名)替换为直接引用(地址)的过程
+ 类的初始化(Initialize)：JVM负责对类进行初始化
   + 执行类构造器 `<clinit>()` 方法的过程。类构造器 `<clinit>()` 方法是由 JVM 自动收集类中所有类变量的赋值动作和静态代码块中的语句合并而成。（类构造器是构造类信息的，不是构造该类对象的构造器）
   + 当初始化一个类的时候，如果发现其父类还没有初始化，则需要先触发其父类的初始化
   + 虚拟机会保证一个类的`<clinit>()`方法在多线程环境中被正确加锁和同步(确保类加载安全) 

Demo：
```java
public class Test04 {
    public static void main(String[] args) {
        A a;        //声明不会加载该类
        System.out.println(A.m);    //创建对象或者使用static方法时才会加载该类
    /*
        对m的值变换进行分析
        1. 加载A类到内存
        2. 链接，m=0
        3. 初始化,依据先后顺序合并static，所以m由0 -> 300 -> 100 -> 500
        <clinit>(){
            static {
                System.out.println("A静态代码块初始化-1");
                m = 300;
            }
            public static int m = 100;
            static {
                System.out.println("A静态代码块初始化-2");
                m = 500;
            }
        }
     */
    }
}

class A{
    static {
        System.out.println("A静态代码块初始化-1");
        m = 300;
    }
    public static int m = 100;
    static {
        System.out.println("A静态代码块初始化-2");
        m = 500;
    }
    public A(){
        System.out.println("A类的无参构造初始化");
    }
}
```

### 2.3、 什么时候会发生类初始化
+ 类的主动引用(一定会发生类的初始化)
	+ 当虚拟机启动时，先初始化 main 方法所在的类
	+ new 一个类的对象
	+ 调用类的静态成员(除了 final 常量) 和 静态方法
	+ 使用java.lang.reflect 包的方法对类进行反射调用
	+ 当初始化一个类，如果其父类没有被初始化，则先会初始化它的父类
+ 类的被动引用(不会发生类的初始化)
	+ 当访问一个静态域时，只有真正声明这个域的类才会被初始化。如：当通过子类引用父类的静态变量，不会导致子类初始化
	+ 只声明了一个类
	+ 通过数组定义类引用，不会触发此类的初始化
	+ 引用常量不会触发此类的初始化(常量在链接阶段就存入调入类的常量池中了)

Demo：
```java
public class Test05 {
    static {
        System.out.println("Main类被加载");
    }

    public static void main(String[] args) throws ClassNotFoundException {
        //1 主动引用
//        Son son = new Son();
//         Class.forName("com.lrr.reflection.Son");

        //2 被动引用
        Son son;
        Son[] sons = new Son[4];
        System.out.println(Son.b);
        System.out.println(Son.M);
    }
}
class Father{
    static int b = 2;
    static {
        System.out.println("父类被加载");
    }
}
class Son extends Father{
    static{
        System.out.println("子类被加载");
        m = 300;
    }
    static int m = 100;
    static final int M = 1;
}
```

### 2.4、 类加载器
+ 类加载器的作用（前文有）：将 class 文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据接口，然后在堆中生成一个代表这个类的 java.lang.Class 对象，作为方法区中类数据的访问入口。
+ 类缓存：标准的JavaSE类加载器可以按照要求查找类，但一旦某个类被加载到类加载器中，它将维持加载（缓存）一段时间。不过JVM垃圾回收机制可以回收这些Class对象
+ JVM 规定了如下类型的类加载器
	+ 引导类加载器(Bootstrap ClassLoader)：扩展类加载器的父类，用C++编写的，是JVM自带的类加载器，负责Java平台核心库(rt.jar包)，用来装载核心类库。该加载器无法直接获取
	+ 扩展类加载器(Extension ClassLoader)：系统类加载器的父类，负责 jre/lib/ext 目录下的jar包或 -D java.ext.dirs 指定目录下的jar包装入工作库
	+ 系统类加载器/用户类加载器(System ClassLoader/Application ClassLoader 这俩是一个东西两种叫法)：负责 java -classpath 或 -D java.class.path所指的目录下的类与jar包装入工作，是最常用的加载器

```java
public class Test06 {
    public static void main(String[] args) throws ClassNotFoundException {
        //获取系统类的加载器
        ClassLoader systemClassLoader = ClassLoader.getSystemClassLoader();
        System.out.println(systemClassLoader);

        //获取系统类的加载器的父类 -> 扩展类加载器
        ClassLoader extensionClassLoader = systemClassLoader.getParent();
        System.out.println(extensionClassLoader);

        //获取扩展类加载器的父类 -> 引导类加载器/根加载器 （c/c++ 编写，不能直接获取，所以得到的是null）
        ClassLoader bootstrapClassLoader = extensionClassLoader.getParent();
        System.out.println(bootstrapClassLoader);

        //测试当前类是哪个加载器加载的
 System.out.println(Class.forName("com.lrr.reflection.Test06").getClassLoader());

        //测试JDK内置类是哪个加载器加载的
 System.out.println(Class.forName("java.lang.Object").getClassLoader());

        //得到类加载器可以加载的路径
        System.out.println(System.getProperty("java.class.path"));
        /*
        E:\JDK\jdk1.8.0_131\jre\lib\annotations-api.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\catalina-ant.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\catalina-ha.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\catalina-storeconfig.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\catalina-tribes.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\catalina.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\charsets.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\deploy.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ecj-4.6.3.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\el-api.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\access-bridge-64.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\cldrdata.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\dnsns.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\jaccess.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\jfxrt.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\localedata.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\nashorn.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\sqljdbc42.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\sunec.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\sunjce_provider.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\sunmscapi.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\sunpkcs11.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\ext\zipfs.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jasper-el.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jasper.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jaspic-api.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\javaws.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jce.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jfr.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jfxswt.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jsp-api.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\jsse.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\management-agent.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\plugin.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\resources.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\rt.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\servlet-api.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\sqljdbc42.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-api.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-coyote.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-dbcp.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-i18n-es.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-i18n-fr.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-i18n-ja.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-jdbc.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-jni.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-util-scan.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-util.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\tomcat-websocket.jar;
        E:\JDK\jdk1.8.0_131\jre\lib\websocket-api.jar;
        E:\IdeaProject\out\production\kuangshenshuo;
        E:\IdeaProject\kuangshenshuo\src\lib\commons-io-2.7.jar;
        E:\IDEA\IntelliJ IDEA 2020.1\lib\idea_rt.jar
         */
    }
}

```

## 3、 创建运行时类的对象

### 3.1、 Class 类的常用方法，得到该类的属性，方法等
IDEA 点进去/或者文档 自己看

### 3.2、 通过反射动态创建对象以及使用对象

**创建对象**：
+ 调用无参构造器创建类的对象：调用Class对象的newInstance()方法，需要满足以下两个要求
   1. 类必须有一个无参数的构造器(因为本质就是调用无参的构造器)
   2. 类的构造器的访问权限需要足够
+ 调用有参构造器创建类的对象：
   1. 通过Class类的getDeclaredConstructor(Class .. parameterType) 得到本类指定形参类型的构造器
   2. 向构造器的形参传递一个对象数组进去，里面包含了构造器所需的各个参数
   3. 通过Constructor实例化对象

**使用对象**：

 1. 通过Class类的getMethod(String name,Class ...parameterTypes)方法取得一个Method对象，并设置此方法操作时所需要的参数类型
 2. 之后使用invoke方法进行调用，并想方法中传递要设置的obj对象的参数信息
 3. public Object invoke(Object obj, Object... args)
    + Object 对应原方法的返回值，若原方法无返回值，此时返回null
    + 若原方法为静态方法，此时形参obj可为null
    + 若原方法参数列表为空，则args为null
    + 若原方法声明为private,则需要在调用invoke()前，显示调用方法对象的setAccessible(true)方法，才能访问private的方法
 4. setAccessible()：启动和禁用访问安全检查的开关
    + Method和Field、Constructor对象都有 setAccessible() 方法
    + 参数为true表示使用时取消该反射对象的Java语言访问检查
      + 可以提高访问效率。如果代码中必须使用反射，并且频繁被调用，那么请设置为true
      + 使原本无法访问的私有成员也可以访问
    + 参数为false表示使用时进行该反射对象的Java语言访问检查

```java
public class Test07 {
    public static void main(String[] args) throws ClassNotFoundException, IllegalAccessException, InstantiationException, NoSuchMethodException, InvocationTargetException, NoSuchFieldException {
        Class c1 = Class.forName("com.lrr.reflection.User");
        //无参构造
        User user = (User) c1.newInstance();
        System.out.println(user);
        //有参构造
        Constructor constructor = c1.getConstructor(String.class,int.class,int.class);
        user = (User)constructor.newInstance("Lee",100,200);
        System.out.println(user);
        //通过反射调用方法
        Method method = c1.getDeclaredMethod("setName",String.class);
        method.invoke(user,"lee");
        System.out.println(user);
        //通过反射调用属性
        Field field = c1.getDeclaredField("name");
        field.setAccessible(true);  //访问私有属性和方法需要禁用访问安全检查
        field.set(user,"Lee");
        System.out.println(user);
    }
}
```


## 4、 通过反射操作注解
好好理解这一节，反射+注解是当前大部分框架的基础

### 4.1、 练习：ORM
ORM：Object relationship Mapping（对象关系映射）：将数据库中的表和javaBean对应起来
+ 类和表结构对应
+ 属性和字段对应
+ 对象和记录对应

```java
public class Test09 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException {
        Class c = Class.forName("com.lrr.reflection.User2");
        Annotation[] annotations =  c.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);
        }
        TableInterface annotation = (TableInterface)c.getAnnotation(TableInterface.class);
        System.out.println(annotation.value());

        Field field = c.getDeclaredField("name");
        TableField tableFields = (TableField) field.getAnnotation(TableField.class);
        System.out.println(tableFields.name());
    }
}

@TableInterface("db_student")
class User2{
    @TableField(name = "db_name",type = "varchar",length = 10)
    private String name;
    @TableField(name = "db_id",type = "int",length = 10)
    private int id;
    @TableField(name = "db_age",type = "int",length = 10)
    private int age;

    public User2() {
    }

    public User2(String name, int id, int age) {
        this.name = name;
        this.id = id;
        this.age = age;
    }



    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", id=" + id +
                ", age=" + age +
                '}';
    }
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface TableInterface{
    String value();
}

@Target(ElementType.FIELD)
@Retention(RetentionPolicy.RUNTIME)
@interface TableField{
    String name();
    String type();
    int length();
}
```