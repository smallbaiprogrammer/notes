Java中的常用类

Object类

其中包含的方法

equals 、hashcode 、nodify、nodifyall、clone、getclass、toString、wait 、finalize （垃圾回收方法），一个对象在回收的时候直接调用这个方法

垃圾对象： 没有有效引用指向一个对象时，该对象被视为垃圾对象

垃圾回收：由GC销毁垃圾对象，释放数据存储空间

自动回收机制，JVM内存耗尽一次性回收所有垃圾对象

手动回收机制，调用方法System。gc



包装类

基本类型数据都存在栈中，引用类型的数据都存储在堆中的，基本数据转为对象时，数据从栈中移到堆中存储

```java
// 基本类型转为字符串
 // 1.+“”
 String s1 = 1+"";
 // 2. 采用每个基本类型的包装类的toString方法
 // 这里转为二进制形式的字符串形式
 String s2 = Integer.toString(1,2);
 // 反过来转换 采用包装类的parseXXX方法
 String s3 = "333";
 int x = Integer.parseInt(s3);
 // boolean 类型和字符串类型转换
 // 方法相同，只有“true” = true 其他的都不可以
```



整数缓冲区

```java
Integer integer1 = new Integer(100);
       Integer integer2 = new Integer(100);
       System.out.println(integer1==integer2);
       // 返回false 的具体原因
        
        
        Integer integer3 = 100;
        Integer integer4 = 100;
        System.out.println(integer3==integer4);
        // 这里为什么变成了true
        
        Integer integer5 = 200;
        Integer integer6 = 200;
        System.out.println(integer5 == integer6);
        // 这里为什么变成了 false
```

自动装箱其实调用的是 Integer.valueOf 方法

这个方法的源码的原理是在堆中存在着一个缓存数组，这个数组中存着 -128 到 127 中的每一个数的 包装对象，采用valueOf将 int基本类型的数 转为对象，如果该基本类型的数在-128 到 127 之间，那么就会之间在这个缓存数组中取出这个对象赋给这个包装对象，如果不在这个范围内，就会重新调用关键字创建一个新的对象

所以数字范围 在 -128 到127 之间的数 自动装箱 ，其实他们获得了相同的包装类对象的引用，在大于这个范围才会创建新对象，所以 上述代码，integer3 和 integer4 是自动装箱之后其实是同一个引用，所以返回true，而200超过了这个范围，integer5和integer6 自动装箱的时候，执行了new Integer() 方法，所以才会这样，这样处理的原因是，在我们日常编程的时候，-128 到127 这个段数字的包装类对象用的比较多，所以采用了整数缓冲区的形式，这样会节省内存的使用





字符串 String 类 

字符串是常量，创建之后不可改变，字符串字面值存储在字符串池中，可以共享

以下属于 字面值创建字符串，string 是唯一一个可以采用字面值创建的对象

```java
// 
// hello 存储在字符串池中，也叫做字符串池中
String name = "hello";
// 栈中的局部变量name 指向 字符串池中的 hello
name = "张三";
// 赋值过程 
// 在这个过程 在字符串池中重新开辟一块空间，然后name指向 字符出池中 张三 
// hello 就变成了 垃圾 此时如果再创建一个字符串对象
String name2 = "张三";
// 那么此时一样 栈中的name2 在字符串池中查找张三，找到之后，地址指向张三，所以 字符串可以共享
```

如果采用new关键字创建字符串对象

这里的话String会创建两个对象，首先在常量池中寻找是否存在这个字符串对象，如果存在，则指向这个地址，然后编译器发现new 关键字 调用String 构造方法，此时会在堆中创建一个对象，此时内存中出现了两个对象，堆中存在一个 字符串池中有一个，但其实事实上堆中是指向栈中的，每次String创建方法的时候，都会产生这个方法。

StringBuffer 的实现原理

新建一个缓存区，在缓存区上对字符串进行操作，这样速度会快很多，StringBuilder 同样原理也是这样的，二者区别是前者是线程安全的，后者不是，但是后者稍微快一点

十万个数据相加，运行时间 ，我们发现这三者的运行速度的差别

String 运行时间26419
StringBuffer 运行时间4
StringBuilder运行时间2



大数类 和 精度 小数类

BigInteger类和BigDecimal类

Date类 Unix 元年  1970年1月1日 8点

目前常用的时间类

Calendar类

System类

中的主要方法，arraycopy复制数组  这个方法效率比较高，所以不要采用一般不要采用数组的类

gc  垃圾回收  告诉垃圾回收器，检查能否回收垃圾

exit（0） 正常突出JVM 其他数字，异常退出