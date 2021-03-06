# JAVA基础

## 数据类型

| 关键字  | 占用内存   |
| ------- | ---------- |
| byte    | 1 字节     |
| short   | 2 字节     |
| char    | 2 字节     |
| int     | 4 字节     |
| float   | 4 字节     |
| long    | 8 字节     |
| double  | 8 字节     |
| boolean | 1 / 4 字节 |



## 面向对象

### 封装

指隐藏对象的属性和实现细节，仅对外提供公共访问方式。

### 继承

- 通过extends关键字可以实现类与类的继承
- 子类只能继承父类所有非私有的成员(成员方法和成员变量)
- 子类不能继承父类的构造方法，但是可以通过super(待会儿讲)关键字去访问父类构造方法
- 增加耦合性

### 多态

**定义**：一个引用变量倒底会指向哪个类的实例对象，该引用变量发出的方法调用到底是哪个类中实现的方法，必须在由程序运行期间才能决定。表现是不同的对象可以执行相同的行为，但是他们都需要通过自己的实现方式来执行（向上转型）

**条件**：继承、重写、向上转型。

- 继承：在多态中必须存在有继承关系的子类和父类。
- 重写：子类对父类中某些方法进行重新定义，在调用这些方法时就会调用子类的方法。
- 向上转型：在多态中需要将子类的引用赋给父类对象，只有这样该引用才能够具备技能调用父类的方法和子类的方法。

**实现**：

- 继承：父类和继承该父类的一个或多个子类对某些方法的重写
- 接口：多继承多实现，更灵活

### 接口

是抽象类型，是服务提供者和服务使用者之间的一个协议。

使用：不相关的类都实现一个方法；需要使用多重继承。

1. 接口中的“成员变量”会自动变为为`public static final`

### 抽象

**不能实例化**对象的类，需要继承抽象类才能实例化其子类。其目的主要是代码重用。大多用于抽取相关 Java 类的共用方法实现或者是共同成员变量，然后继承。

使用：在几个相关的类中共享代码。

1. 只要有抽象方法，就必须定义为抽象类
2. 不需要实现全部方法

### 区别

- 抽象类可以有自己的数据成员，可以有非abstarct的成员方法。接口只能够有静态的不能被修改的数据成员（static final）interface是一种特殊形式的abstract class
- 抽象类表示的是一种继承关系，一个类只能使用一次继承关系。但是一个类却可以实现多个接口
- 抽象类是对类的抽象，而接口是对行为的抽象



## 自动装箱

编译器调用`valueOf()`将原始类型值转换成对象



## 自动拆箱

编译器通过调用`xxxValue()`等方法将对象转换成原始类型值



## 异常

**Throwable** 是所有异常类型的基类，**Throwable** 下一层分为两个分支，**Error** 和 **Exception**。

- Error 描述了 JAVA 程序运行时系统的内部错误，通常比较严重，如虚拟机相关错误：系统崩溃，内存不足，堆栈溢出等，编译器不会对这类错误进行检测，JAVA 应用程序也不应对这类错误进行捕获。
    - java.lang.OutOfMemoryError
    - java.lang.StackOverflowError
- Exception 类型可以在应用程序中进行捕获并处理，下面又分为两个分支
    - 一个分支派生自 RuntimeException，这种异常通常为程序错误导致的异常。
        - ndexOutOfBoundsException (数组越界)
        - NullPointerException (空指针)
    - 另一个分支为非派生自 RuntimeException 的异常，这种异常通常是程序本身没有问题，由于像 I/O 错误等问题导致的异常
        - IOException、SQLException

