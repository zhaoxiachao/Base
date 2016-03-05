
### 1.以下代码有问题吗，是什么问题？
1.1
```java
List<Object> obj = new ArrayList<Long>();
obj.add("I love Android!");
```

1.2
```java
Object[] objArray = new Long[1];
objArray[0] = "I love Android!";
```

结果:   
1.1 编译出错   
1.2 编译可以，运行出错

原因：   
1.1 List\<Object\>和ArrayList\<Long\>没有继承关系
1.2 数组是在运行时类型检查

考察点：泛型、数组

### 2.以下代码运行结果？
2.1
```java
Long l1 = 128L;
Long l2 = 128L;
System.out.print(l1 == l2);
System.out.print(l1 == 128L);
```

2.2
```java
Long l1 = 127L;
Long l2 = 127L;
System.out.print(l1 == l2);
System.out.print(l1 == 127L);
```

结果：   
2.1 false  true   
2.2 true  true

原因：   
2.1 l1和l2是两个对象，==比较的是对象的地址，所以false。Long l1== 128L中l1转值比较，所以true。   
2.2 -128 - 127之间的值，Java维护在一个常量池中，所以l1和l2引用同一个对象。

考察点：常量池、==、自动装箱拆箱

### 3.程序可以编译运行吗，结果是神马？
```java
List<String>[] list = new List<String>[3];
```

结果：
不可以运行，编译报错。

原因：
创建泛型、参数化类型或者类型参数的数组是非法的，都会导致编译错误。

考察点：Java默认是没有泛型数组的。

### 4.是否存在使 i>j || i<=j 结果为false的数吗？
结果：  
存在  
原因：  
数值NaN代表not a number，无法用于比较，例如即使是 i = Double.NaN; j = i; 最后i == j的结果依旧为false。  
考察点：Java基础知识点NaN  

### 5.程序运行结果？
```java
public class Main {

    public static void main(String[] args) {
        TxtThread txtThread = new TxtThread();

        new Thread(txtThread).start();
        new Thread(txtThread).start();
        new Thread(txtThread).start();

    }

    static class TxtThread implements Runnable {

        int num = 100;
        String str = new String();

        @Override
        public void run() {
            synchronized (str) {
                while (num > 0) {
                    try {
                        Thread.sleep(1);
                    } catch (InterruptedException ex) {
                        Logger.getLogger(TxtThread.class.getName()).log(Level.SEVERE, null, ex);
                    }
                    System.out.println(Thread.currentThread().getName() + " this is " + num--);
                }
            }
        }
    }

}
```
结果：
Thread-0 this is 100
......
......
Thread-0 this is 1

原因：
三个线程共享对一个Runnable对象，所以同步锁中其他两个线程没有执行机会

考察点：Thread,Runnable,同步。

### 6.垃圾回收面试题
####面试题一
```java
 1．fobj = new Object ( ) ;   
 2．fobj. Method ( ) ;   
 3．fobj = new Object ( ) ;   
 4．fobj. Method ( ) ;   
```
问：这段代码中，第几行的fobj 符合垃圾收集器的收集标准？ 
答：第3行。因为第3行的fobj被赋了新值，产生了一个新的对象，即换了一块新的内存空间，也相当于为第1行中的fobj赋了null值。这种类型的题是最简单的。 
####面试题二
```java
1．Object sobj = new Object ( ) ;   
2．Object sobj = null ;   
3．Object sobj = new Object ( ) ;   
4．sobj = new Object ( ) ;   
```
问：这段代码中，第几行的内存空间符合垃圾收集器的收集标准？ 
答：第2行和第4行。因为第2行为sobj赋值为null，所以在此第1行的sobj符合垃圾收集器的收集标准。而第4行相当于为sobj赋值为null，所以在此第3行的sobj也符合垃圾收集器的收集标准。

####面试题三
```java
1．Object aobj = new Object ( ) ;   
2．Object bobj = new Object ( ) ;   
3．Object cobj = new Object ( ) ;   
4．aobj = bobj;   
5．aobj = cobj;   
6．cobj = null;   
7．aobj = null; 
```
问：这段代码中，第几行的内存空间符合垃圾收集器的收集标准？ 
答：第4，7行。注意这类题型是认证考试中可能遇到的最难题型了。 
行1-3：分别创建了Object类的三个对象：aobj，bobj，cobj
行4：此时对象aobj的句柄指向bobj，原来aojb指向的对象已经没有任何引用或变量指向，这时，就符合回收标准。
行5：此时对象aobj的句柄指向cobj，所以该行的执行不能使aobj符合垃圾收集器的收集标准。 
行6：此时仍没有任何一个对象符合垃圾收集器的收集标准。 
行7：对象cobj符合了垃圾收集器的收集标准，因为cobj的句柄指向单一的地址空间。在第6行的时候，cobj已经被赋值为null，但由cobj同时还指向了aobj（第5行），所以此时cobj并不符合垃圾收集器的收集标准。而在第7行，aobj所指向的地址空间也被赋予了空值null，这就说明了，由cobj所指向的地址空间已经被完全地赋予了空值。所以此时cobj最终符合了垃圾收集器的收集标准。 但对于aobj和bobj，仍然无法判断其是否符合收集标准。 

总之，在Java语言中，判断一块内存空间是否符合垃圾收集器收集的标准只有两个： 
1．给对象赋予了空值null，以下再没有调用过。 
2．给对象赋予了新值，既重新分配了内存空间。 

### 7.以下程序运行的结果是神马？
String s1 = "abc";
StringBuffer s2 = new StringBuffer(s1);
System.out.println(s1.equals(s2));

结果：   
false

原因:考String的toString方式，toString方法进行了instance of判断。
>>>>>>> b081475e54b424700dc34c3f632652c03d4803aa
