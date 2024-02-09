---
title: NIO与Socket编程技术
date: 2023-03-22 20:53:00
tags: [Java]
categories : Java
updated: 2023-03-22 20:53:00
index_img: /img/文章封面图/5.JPG
sticky: 
---
## NIO与Socket编程技术指南

### 多线程
* 让某一个类实现多线程操作，有两种方法：
  1.继承Thread类(Thread类已经实现Runnable接口了)
  2.**实现Runnable接口**
* **补充：**
  1.start():启动线程的方法，不能重复调用！  
  2.run()：一个类中的普通方法，可以重复调用！
* 开启线程的具体代码如下：
``` java
    package learnthread;

    public class Learn {
        public static void main(String[] args) {
        TestThread testThread = new TestThread("1");
        TestThread testThread1 = new TestThread("2");
        testThread.run();
        testThread1.run();
        }
    }
    
    /**
     * 继承Thread类
     */
    class TestThread extends Thread {
        private String name;
    
        public TestThread(String name) {
            this.name = name;
        }

        public void run() {
            for (int i = 0; i < 3; i++) {
                System.out.println(name + "运行  :  " + i);
                try {
                    sleep((int) Math.random() * 10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
输出结果：
1运行  :  0
1运行  :  1
1运行  :  2
2运行  :  0
2运行  :  1
2运行  :  2
```
```  java
 package learnthread;

public class Learn {
    public static void main(String[] args) {
    TestRunnable testRunnable = new TestRunnable("1");
    TestRunnable testRunnable1 = new TestRunnable("2");
    testRunnable.run();
    testRunnable1.run();
    }
}

/**
 * 实现Runnable接口
 */
class TestRunnable implements Runnable {
    private String name;

    public TestRunnable(String name){
        this.name = name;
    }
    /**
     * 注意：实现Runnable接口必须必须必须要重写接口里面的抽象函数run()
     */
    public void run() {
        for (int i = 0; i < 3; i++) {
            System.out.println(name + "运行  :  " + i);
            try {
                Thread.sleep((int) Math.random() * 10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
输出结果：
1运行  :  0
1运行  :  1
1运行  :  2
2运行  :  0
2运行  :  1
2运行  :  2
跟上面一样的！
```
### 多线程的安全问题
* 补：同步在**一定程度上**可以看做是单线程，异步在**一定程度上**可以看做是多线程

##### 1.实例锁(sychorinzed)

* 在方法前加上sychroinzed。但是只对类的一个对象、一个线程起保护作用

##### 2.全局锁、类锁(static sychorinzed) 
* 在方法前加上static synchroinzed。这个锁的范围是针对类的所有对象，就是这个类的所有对象，都能保证线程同步
* **这个文章写得很好**:https://blog.csdn.net/zcg_741454897/article/details/108540717?spm=1001.2014.3001.5506

##### 3.实例方法中同步块
* 和实例锁很相似，但同步块不需要同步整个方法，而是同步方法中的一部分
``` java
  public class MyClass {
   public synchronized void log1(String msg1, String msg2){
      log.writeln(msg1);
      log.writeln(msg2);
   }

   public void log2(String msg1, String msg2){
      synchronized(this){
         log.writeln(msg1);
         log.writeln(msg2);
      }
   }
}
```

##### 4.静态方法中同步块
* 和类锁相似，但同步块不需要同步整个方法，而是同步方法中的一部分
``` java
public class MyClass {
    public static synchronized void log1(String msg1, String msg2){
       log.writeln(msg1);
       log.writeln(msg2);
    }
    public static void log2(String msg1, String msg2){
       synchronized(MyClass.class){
          log.writeln(msg1);
          log.writeln(msg2);
       }
    }
  }
```
* **推荐文章，写的也很好**：https://blog.csdn.net/weixin_34221036/article/details/92793161?spm=1001.2014.3001.5506

### string常量池问题
  **常量池**是为了避免频繁的创建和销毁对象而影响系统性能，其实现了**对象的共享**。**字符串常量池**，在编译阶段就把所有的字符串文字放到**一个常量池**中
* String的两个值都是"nantong",两个线程持有相同的锁，造成B线程不能执行同步synchronized代码块不使用String作为锁对象
```java
public class Service {
	public static void print(String stringParam) {
		try {
			synchronized (stringParam) {//同步块
				while (true) {
					System.out.println(Thread.currentThread().getName());
					Thread.sleep(1000);
				}
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}
```
以下是改正方法:
```java
class Service {
	public static void print1(String stringParam) {
		try {
			synchronized (stringParam) {//同步块
				while (true) {
					System.out.println(Thread.currentThread().getName());
					Thread.sleep(1000);
				}
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}

	public static void print2(String stringParam) {
		try {
			synchronized (stringParam.getClass()) {//类锁
				while (true) {
					System.out.println(Thread.currentThread().getName());
					Thread.sleep(1000);
				}
			}
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
}

```
