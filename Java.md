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
1.1 List<Object>和ArrayList<Long>没有继承关系
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

