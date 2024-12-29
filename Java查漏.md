# Java查漏

## 方法过载和方法覆盖的区别

### 方法覆盖（方法重写）：

1、重写方法与被重写方法参数列表类型必须完全一致。

2、重写方法的访问修饰符一定要大于或等于被重写方法的访问修饰符（public>protected>default>private）。

3、重写的方法的返回值和被重写的方法的返回值必须保持一致；

4、重写的方法所抛出的异常必须和被重写方法的所抛出的异常一致，或者是其子类；

5、被重写的方法不能为private，否则在其子类中只是新定义了一个方法，并没有对其进行重写。

6、静态方法不能被重写为非静态的方法（会编译出错）。

用一句话总结重写规则：重写方法与被重写方法访问控制可能不一样，方法内容不一样，其余的均与被重写方法一样。

注意：overload是重载，一般是用于在一个类内实现若干重载的方法，这些方法的名称相同而参数形式不同。

### 方法过载（方法重载）

在java中含义：同一样东西在不同的地方具有多种含义,同一个类同名方法的不同表现形式。

重载（过载）的规则：

1.  方法名相同，参数类型不同或参数列表形式不同；
2.  不能通过访问权限、返回类型、抛出的异常进行重载；
3.  方法的异常类型和数目不会对重载造成影响；



## Object类

**顶级父类**，**仅有空参构造**

### Object中的方法

#### toString

返回对象的字符串表示形式：包名+类名+@+地址值

想获取对象中的属性值可以对toString方法进行重写

#### equals

比较两个对象（的地址值）是否相等

可以对方法进行重写以比较内部的属性值

##### **字符串中的equals方法**:

先判断参数是否为字符串，如果是，再比较内部的属性

如果不是，会直接返回false

#### clone

把A对象的属性值完全拷贝给B对象，也叫对象拷贝，对象复制

##### 细节

1.重写Object中的clone方法

2.让javabean类实现Cloneable接口

3.创建原对象并调用clone

##### 浅克隆（浅拷贝）

对于基本数据类型会直接拷贝数据值

对于引用数据类型会拷贝相应的地址值

Object类中的clone属于浅克隆

##### 深克隆（深拷贝）

对于基本数据类型会直接拷贝数据值

对于引用数据类型，字符串复用，而其他类型会重新创建新的 

### Objects工具类

#### equals

```java
boolean result = Objects.equals(s1,s2);
```

1.方法的底层会判断s1是否为null，如果是，则直接返回false

2.如果不是，那么就利用s1再次调用equals方法

3.如果此时s1是Student类型，所以最终调用的还是Student的equals方法

如果没有重写，比较地址值；如果重写了，就比较属性值

#### isNull

判断对象是否为null，是则返回true

```java
Student s1 = new Student();
Student s2 = null;
sout(Objects.isNull(s1));//false
sout(Objects.isNull(s2));//true
```



## 接口的类型



## 异常处理

### 概念

### 作用

### 异常类的层次

### 嵌套的异常处理

### throw语句

### 自定义异常类的方法和使用



## 集合

### 数据结构

#### 队列

数据从后端进，从前端出

先进先出，后进后出

#### 数组

查询快(索引)，增删慢

#### 链表

查询慢，无论查询哪个数据都要从头开始找

但增删相对数组更快





## 线程

### 并发和并行

#### 并发

在同一时刻，有多个指令在单个CPU上**交替**执行

#### 并行

在同一时刻，有多个指令在多个CPU上**同时**执行

### 继承Thread类

#### 实现步骤

1）创建Thread类的子类

2）重写run方法

3）创建线程对象

4）启动线程

```java
package com.bobo.thread;
 
public class ThreadDemo02 {
 
    /**
     * 线程的第一种实现方式
     *     通过创建Thread类的子类来实现
     * @param args
     */
    public static void main(String[] args) {
        System.out.println("main方法执行了1...");
        // Java中的线程 本质上就是一个Thread对象
        Thread t1 = new ThreadTest01();
        // 启动一个新的线程
        t1.start();
        for(int i = 0 ; i< 100 ; i++){
            System.out.println("main方法的循环..."+i);
        }
        System.out.println("main方法执行结束了3...");
    }
}
```

```java
/**
 * 第一个自定义的线程类
 *    继承Thread父类
 *    重写run方法
 */
class ThreadTest01 extends Thread{
 
    @Override
    public void run() {
        System.out.println("我们的第一个线程执行了2....");
        for(int i = 0 ; i < 10 ; i ++){
            System.out.println("子线程："+i);
        }
    }
}
```

### 实现Runnable接口

#### 实现步骤

1）创建Runable的实现类

2）重写run方法

3）创建Runable实例对象（通过实现类来实现）

4）创建Thread对象，并把第三步创建的Runable实例作为Thread构造方法的参数

5）启动线程

```java
package com.bobo.runable;
 
public class RunableDemo01 {
 
    /**
     * 线程的第二种方式
     *     本质是创建Thread对象的时候传递了一个Runable接口实现
     * @param args
     */
    public static void main(String[] args) {
        System.out.println("main执行了...");
        // 创建一个新的线程  Thread对象
        Runnable r1 = new RunableTest();
        Thread t1 = new Thread(r1);
        // 启动线程
        t1.start();
        System.out.println("main结束了...");
    }
}
 
/**
 * 线程的第二种创建方式
 *   创建一个Runable接口的实现类
 */
class RunableTest implements Runnable{
 
    @Override
    public void run() {
        System.out.println("子线程执行了...");
        
    //此时若想调用thread类中的方法，需要先创建对象
    Thread.currentThread().getName();
    }
}
```

#### 好处

1）可以避免Java单继承带来的局限性

2）适合多个相同的程序代码处理同一个资源的情况，把线程同程序的代码和数据有效的分离，较好的体现了面向对象的设计思想

### 线程同步

CPU在执行代码时，执行权随时都会被不同的线程 争抢，导致线程执行时具有随机性

####  同步代码块

把操作共享数据的代码锁起来

```java
synchronized(锁){
    操作共享数据的代码
}
```

注意：锁对象可以是任意的，但必须唯一（static修饰）

常用MyThread.class作锁对象

##### **特点**

1.锁默认为打开状态，有一个线程进去了，锁会自动关闭

2.里面的代码全部执行完毕，线程出来，锁自动打开

####  同步方法

将synchronized关键字加到方法上

```java
修饰符 synchronized 返回值类型 方法名（方法参数）{}
```

##### **特点**

1.同步方法是锁住方法里面所有的代码

2.锁对象不能自己指定:

对于非静态：this

对于静态:当前类的字节码文件对象

#### 等待唤醒机制

用**notify()**或**notifyAll()**方法可以唤醒其它一个或所有线程，而使用**wait()**方法来使该线程处于阻塞状态，等待其它的线程用notify()唤醒。

#### 多线程run()方法书写套路

1.循环

2.同步代码块

3.判断共享数据是否到了末尾（到了末尾）

4.判断共享数据是否到了末尾（没到，则执行核心逻辑）

## 流

### 流的基本概念和分类； 

### File 类、文件及文件 I/O；

#### File

对文件本身进行操作，但不能处理文件中的数据

#### IO流

存储和读取数据的解决方案

##### 分类

**按流的方向分为输入流和输出流:**

输入流负责读取，输出流负责写出

**按操作文件类型分为字节流和字符流:**

字节流可以操作所有类型的文件，字符流只能操作纯文本文件

纯文本文件：Windows自带的记事本打开能读懂的文件

### 字节流，字符流，管道流的基本概念和作用

#### 字节流

输入流：InputStream

输出流：OutputStream

#### 字符流

输入流 ：Reader

输出流：Writer

#### 管道流?

### 最常见的输入输出流的分类使用方法