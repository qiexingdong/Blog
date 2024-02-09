---
title: Java笔记
date: 2023-03-15 23:04:00
tags: [java,笔记]
categories : Java
updated: 2023-03-22 00:00:00
index_img: /img/文章封面图/2.JPG
sticky: 99
---
<!--more-->
# JavaSE基础
- [第一阶段](#第一阶段)
- [第二阶段](#第二阶段)
## 第一阶段

### 一、类与对象

### 二、属性（字段）

### 三、成员方法（方法）
- 方法重载
- 可变参数
- 作用域

### 四、构造器（构造方法）
* this、super关键字

### 五、包、访问修饰符

### 六、封装继承多态
 #### &emsp;1.封装（set、get）
* 提供一个set方法用于对属性的赋值
* 提供一个get方法用于获取属性的值

#### &emsp;2.继承（extends）
* 子类构造器和父类的构造器的关系
* super关键字（注意和this关键字的区别）

#### &emsp;3.多态（体现在方法和对象）
* 方法的多态：重写和重载
* **对象的多态：向下转型、向上转型**
   **对象的编译类型和运行类型可以不一致，运行类型可以通过getclass()方法来查看**
* Java的动态绑定机制（和多态有关）
  调用方法时，**该方法会和该对象的运行类型绑定**；**属性没有动态绑定机制**，哪里声明哪里使用
* 多态数组
* 多态参数

### 七、Object类

* equals、toString、hashCode、finalize（很多方法都需要重写）

## 第二阶段

### 一、类变量和类方法（静态变量和静态方法）
* 非静态方法可以访问静态变量(方法)和普通变量(方法)
* 静态方法只能访问静态变量(方法)（因为先有静态后有普通变量）
* 静态方法适合做一些工具类

### 二、main方法语法
* 和静态方法一样，只能直接使用本类中的类变量和类方法
* 可以动态传值
* java虚拟机调用main方法（public），在调用时不用创建对象调用(static),没有返回值（void），args数组保存**执行java命令时传递给所运行的类的参数**（String[] args）

###  三、代码块(初始化块)
* 会在执行构造器前执行代码块，写在类内就可以，不用写在构造器或者方法中
* 语法：
   ``` java
    {
     语法
    }
    //或者
    static {
     语法
    }
   ```
* 静态代码块会在**类加载时**只执行一次，普通代码块会在**每一个对象创建时**执行，使用静态成员时不会执行普通代码块
* **注意：类何时被加载**：
  1.创建该类对象
  2.创建子类对象，父类也会被加载
  3.使用静态成员（方法、属性）
* 加载类信息->加载(父子类)静态代码块和静态属性初始化(按顺序执行，指谁写在前面就执行谁)->创建对象->加载(父子类)普通代码块和普通变量(顺序执行)->构造方法
* 构造器内语句的执行顺序：
    ``` java
    构造器(){
    //super();
    //加载普通代码块和普通变量
    语句;
   }
    ```
* (有继承时)父类静态代码块和静态属性初始化->子类静态代码块和静态属性初始化->父类加载普通代码块和普通变量->父类构造方法->子类加载普通代码块和普通变量->子类构造方法(**面试题**)
* 非静态代码块可以访问所有成员，静态代码块只能访问静态成员
  
### 四、单例模式
  1.饿汉式（没有使用对象时就创建对象）
  ``` java  
    class Single {
    private static Single instance = new Single();

    private Single() {
    }

    public static Single GetInstance() {
        return instance;
    }
}
  ```
 * 构造器私有化，禁止在外部创建对象
 * 创建私有静态对象（唯一），在类加载时对象就会被创建好
 * 创建公共获取唯一对象的方法
  2.懒汉式（用的时候再去创建，**但会有进程问题**）
  ``` java
  class Cat {

    private static Cat instance;

    private Cat() {

    }

    public static Cat GetInstance() {
        if (instance == null) {
            instance = new Cat();
        }
        return instance;
    }
  }
  ```

### 五、final
* 修饰类、成员、局部变量
* 作用：类不希望被继承；父类方法不希望被子类重写或重载；属性、局部变量不希望被改变 
* 修饰属性时，必须在定义时、构造器、代码块中赋值
* 修饰**静态**属性时，只能在定义和代码块中赋值
* final 和 static 往往搭配使用，效率更高，**不会导致类加载**.底层编译器做了优化处理
  ``` java
  class Demo {
     //打印Demo.num时不会显示“你好”，因为类没有被加载

    public static final int num = 100;

    static {
        System.out.println("你好");
    }
  }
  ```
* 包装类和String都是final修饰的

### 六、抽象类
* 父类的不确定的方法为抽象方法，该父类为抽象类
``` java
      public abstract class AbstractExercise {
        public abstract void eat();//没有方法体
    }
```
* abstract只能修饰类和方法，且类不能被实例化
* 抽象类可以没有抽象方法，但抽象方法不能没有抽象类
* **子类继承抽象类时，必须重写所有抽象方法**
* 抽象方法不能被private、final和static搭配使用
#### 重要的应用：模板设计模式(提高复用性)
``` java
    public abstract class AbstractExercise {
    public abstract void calculate();

    public void Num() {
        long start = System.currentTimeMillis();
        calculate();
        long end = System.currentTimeMillis();
        System.out.println("耗时：" + (start - end));
    }
}

class A extends AbstractExercise {
    @Override
    public void calculate() {
        int num = 0;
        for (int i = 0; i <= 100000; i++) {
            num += i;
        }
    }
}
class B extends AbstractExercise {
    @Override
    public void calculate() {
        int num = 0;
        for (int i = 0; i <= 800000; i++) {
            num += i;
        }
    }
}
```

### 七、接口（和继承很像）
*  接口里面可以有属性和方法(抽象方法、默认实现方法、静态方法)
*  接口不能被实例化
*  接口里面所有方法都是public，抽象方法不用加abstract
*  **抽象类**实现接口，可以不用实现接口的方法，但普通类要全部实现
*  一个类可以实现(implements)很多接口，接口不能继承类但能继承很多接口
*  所有属性都是public static final，调用方法：接口名.属性
*  接口在一定程度上实现了代码解耦(?)
*  接口具有多态：多态参数、多态数组
### 八、内部类
* 分类：局部内部类、匿名内部类;成员内部类、静态内部类
##### 局部内部类(相当于一个局部变量)：
  1.不能添加访问修饰符，但可以加final
  2.作用域：方法体内或者代码块中
  3.外部类想调用局部内部类，则需要在**包含内部类的方法**中创建内部类对象，再通过调用此方法调用内部类；其他外部类不能调用局部内部类
  4.外部类和局部内部类有重名属性，有如下解决方法
  ``` java
 public class Test{
    public static void main(String[] args){
        Outer outer = new Outer();
        outer.Printf();
    }
  } 
/**
 * 演示局内内部类的使用
 */
  class Outer {//外部类
   public int num = 100;
    public void Printf(){
      class Inner{//局部内部类
          public int num = 200;
            public void m1(){
                System.out.println(num);//200
                System.out.println(Outer.this.num);//100
            }
        }
        Inner inner = new Inner();
        inner.m1();
    }
  }
 class OtherOuter{//其他外部类
 }
  ```
##### 匿名内部类(相当于局部变量，很重要！！！)：
* 基本语法如下：
```java
   new 接口或类(参数列表){
    类体
    };
```
* 基于接口
```java
    class Test {
    public static void main(String[] args) {
        Outer outer = new Outer();
        outer.Printf();
    }
}

/**
 * 演示匿名内部类的使用
 */
public class Outer {//外部类

    public void Printf() {
/**
 * 如果某个类只使用一次，我可以用匿名内部类来简化开发
 * tiger的编译类型：IA
 * tiger的运行类型：匿名内部类
 * 匿名内部类只能使用一次，但实例tiger可以反复调用
 */
        /*
        我们看底层 ，系统会给 XXXXX 会分配 类名 Outer$1
        class XXXXX implements IA{
            @Override
            public void shout() {
                System.out.println("老虎汪汪叫");
            }
        }
         */
        IA tiger = new IA() {
            @Override
            public void shout() {
                System.out.println("老虎汪汪叫");
            }
        };
        System.out.println(tiger.getClass());//gerClass()是查看实例的运行类型
        tiger.shout();
        tiger.shout();
    }
}
输出：
class InnerC.Outer$1
老虎汪汪叫
老虎汪汪叫
```
* 基于类
```java
public class Outer {//外部类
    public void Printf() {
        /*
        我们看底层 ，系统会给 XXXXX 会分配 类名 Outer$1
        class XXXXX extends Mother{"111"}
         */
        Mother tiger = new Mother("111") {};
        new Mother("222"){
        }.Say();//基于接口的也可以这样使用，不用创建对象，匿名对象+匿名内部类
    }
}
class Mother {
    private String name;
    Mother(String name) {
        this.name = name;
    }
    public void Say(){
        System.out.println("你好！！！！");
    }
}
输出：
你好！！！！
```
* 注意事项和上面局部内部类差不多
* 可以当作实参直接传递，很实用
##### 成员内部类(相当于一个成员)
* 写在外部类中
* 可以被访问修饰符修饰
* 外部类想调用成员内部类，则需创建**成员方法**，在方法中创建成员内部类的对象，再调用
* 其他外部类也可以访问成员内部类(有两种方法)
##### 静态内部类(相当于一个成员)
* 相当于成员内部类被static修饰
* 和静态方法一样，只能访问外部类中的静态变量
* 其他外部类可以访问静态内部类(有两种方法)
```java
class NewTest {
    public static void main(String[] args) {
        OOuter oOuter = new OOuter();
        //第一种
        //Cat不是静态内部类，需要创建外部类对象来调用
        OOuter.Cat cat = oOuter.new Cat();
        //第二种
        OOuter.Cat cat1 = oOuter.t1();

        //第一种
        //Cat为静态，不需要创建外部类对象来调用，直接new就行
        OOuter.Lion lion = new OOuter.Lion();
        //第二种
        OOuter.Lion lion1 = OOuter.t2();
    }
}
public class OOuter {
    private int a = 100;
    private static int b = 100;
    public class Cat {
        public int a = 200;
        public void Read() {
            System.out.println(a);
            System.out.println(OOuter.this.a);//遇到重名的变量
        }
    }
    public Cat t1() {
        return new Cat();
    }

    public static class Lion {
    private int b = 400;
    public void Read(){
        System.out.println(b);
        System.out.println(OOuter.b);//遇到重名的变量
    }
    }
    public static Lion t2() {
        return new Lion();
    }
}
```
### 九、枚举类
##### 自定义类实现枚举
* 构造器私有化，删掉set方法，直接创建固定对象
```java
public class Season {
    private String name;
    private String decs;
    public static final Season SPRING = new Season("春天","温暖");//对象名要大写，最好配合staic final来优化性能
    private Season(String name, String decs) {
        this.name = name;
        this.decs = decs;
    }
    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", decs='" + decs + '\'' +
                '}';
    }
}
```
##### enum关键字实现枚举
* 会隐式的继承Enum类
* 对象一定要放在最前面；若对象使用无参构造器，括号都可以省略
```java
public enum Season {
    SPRING("春天","温暖"),
    WINTER("冬天","寒冷");
    private String name;
    private String decs;

    private Season(String name, String decs) {
        this.name = name;
        this.decs = decs;
    }
    @Override
    public String toString() {
        return "Season{" +
                "name='" + name + '\'' +
                ", decs='" + decs + '\'' +
                '}';
    }
}
```
##### Enum类的成员方法(这里推荐看源码！！)
* ordinal():当前枚举对象的序列号
* values():返回含有枚举对象的数组
```java
        for(Season i : Season.values()){
            System.out.println(i);
        }//新知识点：增强for循环
```
* value():将字符串转化为枚举对象
* compareTo():对比枚举对象的序列号