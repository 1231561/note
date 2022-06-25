# 牛客错题

#### 引用

引用作为形参时，传进实参，会赋值实参的地址，不会产出新的对象。

当对象值作为形参时，传进实参，会生成一个新的对象，然后传值。

![image-20220403134647743](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403134647743.png)

#### 循环

do-while循环结束的条件时关键字while后的条件不成立

![image-20220403140147391](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403140147391.png)

#### 运算符

D是无符号右移赋值，左边空出的位置以0填充

![image-20220403144623474](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403144623474.png)

#### 抽象类

A：

抽象类只能被继承，但是final是最终类，又不能被继承，两者矛盾。

B：
抽象方法，只能被子类重写，但是定义private又不能被子类访问，两者矛盾。

C:

修饰符重复



![image-20220403152857784](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403152857784.png)

#### 正则表达式

![image-20220403153634688](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403153634688.png)

#### Java包

lang包是语言包，由java解释器自动引入，而不用import语句引入。

![image-20220403153805744](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403153805744.png)

#### IO流

![image-20220403154312485](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403154312485.png)

#### Servlet和cgi

![image-20220403154356681](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403154356681.png)

#### 排序时间复杂度

![image-20220403154504645](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403154504645.png)

#### 正则表达式

![image-20220403154542577](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403154542577.png)

#### Java并发

![image-20220403155358478](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403155358478.png)

### Java

javac.exe是编译.java文件

java.exe是执行编译好的.class文件

javadoc.exe是生成Java说明文档

jdb.exe是Java调试器

javaprof.exe是剖析工具

![image-20220403155444355](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403155444355.png)

#### java集合API

List和Set继承于Collection接口是单列集合，Map和Collection同等级，Map是双列集合。

![image-20220403155956596](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403155956596.png)

#### 接口

C:接口可以多继承，类只能单继承。

B:接口的父接口必是接口

A:子类默认调用父类的无参构造方法,如果父类没有构造方法，则需要显示地调用父类的哪个有参构造方法，子类的有参构造方法与父类的有参构造方法是否调用无关。

![image-20220403160352931](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403160352931.png)

#### 构造方法

构造方法没有返回值类型。

![image-20220405212712115](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220405212712115.png)

#### 修饰符及访问权限

B:定义无修饰符（default）的类或变量或方法只能被同包中的类访问。

D：定义protected修饰符的类或变量或方法能被同包的访问，以及被不同包的子类访问。

![image-20220405212819998](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220405212819998.png)

#### 变量++

count=count++先将count用临时变量temp保存，然后count+1，然后将temp的值0赋给count，所以count为0；

![image-20220405220023477](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220405220023477.png)

#### Cookie的获取

Cookie值可以通过request获取请求头或者request获取Cookie来获取

![image-20220405220233433](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220405220233433.png)

#### Map

Map和SorteMap是接口，不能直接实例化，只能通过它的子类HashMap或者TreeMap来实例化。

![image-20220405220358064](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220405220358064.png)

#### 多态

父类无法通过多态调用子类特有的方法。

![image-20220406214708319](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406214708319.png)

#### Object

四个方法都是Object的方法。

![image-20220406215102368](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406215102368.png)

![image-20220406215049932](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406215049932.png)

#### 局部变量

局部变量必须赋值了才能使用，默认没有赋值，得手动赋值。

成员属性默认赋null值。

![image-20220406215224778](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406215224778.png)

#### 动态字符串数组和引用

StringBuffer是动态字符串数组，append方法的作用是将字符追加到初始定义的字符串上，而且保持原字符串地址不变，相当于"+"。

所以字符串a被追加了字符"B"，且地址不变。

x=y只是将y的引用变为x，并没有改变b的引用，所以字符串b不变。

![image-20220406215923946](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406215923946.png)

#### 抽象类

JDK 1.8以前，抽象类的方法默认访问权限为protected

JDK1.8以后，抽象类的默认访问权限是default。

![image-20220406220544709](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406220544709.png)

#### import导包

import导包只能导当前路径层下的包的所有类，不能导入当前路径层的包里的包的类（下一层路径的包的类）。

![image-20220406220745399](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406220745399.png)

#### java关键字

除了sizeof，其三个都是java的关键字。

![image-20220406221117740](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406221117740.png)

#### 请求转发和重定向

![image-20220406221211633](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220406221211633.png)

#### 继承

实例化子类时，会先实例化父类，先调用父类的无参构造方法，因为子类重写了test方法，所以父类无参构造方法调用的是子类的test方法，实例化完父类后，实例化子类，调用子类的有参构造方法，再次调用子类的test方法。

![image-20220408210204402](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408210204402.png)

#### 创建了几个对象

由于常量池没有改拼接成的字符串，就会在常量池里生成一个对象，然后将字符串交给这个对象保存。

![image-20220408211618688](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408211618688.png)

#### 三元运算符

三元运算符的两个表达式的数据类型如果能相互转换，则等级低的类型会向等级高的类型转换。

![image-20220408213342773](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408213342773.png)

#### 接口和修饰符

接口的字段可用的修饰符：final（默认）,public,static

接口的方法可用的修饰符：abstract（默认）,public

![image-20220408214122208](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408214122208.png)

#### 线程

D：实现多线程的三种方式，一种是继承Thread类使用此方式就不能继承其他的类了，还有两种是实现Runnable接口或者实现Callable接口

![image-20220408214601136](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408214601136.png)

#### Json

json语句的键要有""双引号

![image-20220408214838617](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408214838617.png)

#### 多态的作用

隐藏细节是封装的作用，多态的作用是提高可重用性和扩展代码模块。

![image-20220408214934957](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408214934957.png)

#### java语言函数

lang包就是language，语言包。

![image-20220408215233166](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408215233166.png)

#### 运算符

~运输符是取反操作

![image-20220408215338824](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408215338824.png)

#### java、javac、jar

![image-20220408215500595](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408215500595.png)

#### 多线程与多进程

同进程的线程可以数据共享，所以执行开销比进程小，B错，一个线程只能属于一个进程D错

![image-20220408215704885](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408215704885.png)

#### 异常处理

多个catch的参数类型，父类要放在子类后面，因为父类包含的错误类型更多，会覆盖子类。

![image-20220408215946769](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220408215946769.png)

#### 引用（重要）

change方法的参数str得到了实参ex.str保存的地址值，即和ex.str一起引用了在堆区的字符串"good"。

change方法体中str改变了引用，引用了在常量池的"test ok"字符串,所以ex.str没变。

change方法的参数 ch[]得到了实参ex.ch保存的字符数组{'a','b','c'}的栈区地址，即和ex.ch一起引用了字符数组的地址，而在change方法体中，ch[]改变了引用的内容，而不是改变了引用，所以引用了字符数组{'a','b','c'}的变量都发生了变化。

![image-20220409210553619](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409210553619.png)

#### 包（package）

定义在同一个包下的类可以不用import导包，可以直接使用。

![image-20220409211730194](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409211730194.png)

#### 异常处理

C：throw语句是肯定会抛出异常的

D：throws语句在捕获到异常的时候才会抛出异常

![image-20220409212049039](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409212049039.png)

#### JSP内置对象

A：session对象在客户和服务器断开连接时，即关闭浏览器时，客户端失去获取session对象的唯一标识符JSESSIONID,但是服务器中的session对象还在，直到规定的session对象的声明周期到了，服务器中的session对象才会被销毁。

C：一个服务器只有一个application对象，一个application对象可以供多个用户使用。

![image-20220409212743935](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409212743935.png)

#### static

因为静态代码块中的x是局部变量（因为是自己int出来的，如果里面是x=5，会给下面的静态变量x赋值），不影响下面静态变量x的值，所以静态变量x和y都是0.

所以main方法中调用myMethod方法后，x=1，y=0，所以结果是3。

![image-20220409214330497](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409214330497.png)

#### java数组

java new数组要规定数组的大小,二维数组行必须要规定大小，列可以省略。

![image-20220409215134618](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409215134618.png)

#### 抽象类声明

C:抽象方法没有方法体

![image-20220409215354893](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409215354893.png)

#### 常量池与堆区

如果常量池中有"xyz"，则只会在堆区new一个对象。

如果常量池没有"xyz"，则会在常量池中生成一个对象在堆区new一个对象。

![image-20220409215456941](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409215456941.png)

#### 字符串与字符

String.valueOf和Character.toString方法是将字符转变为字符串，char只有遇到int类型时才会转化为ascii码值。

![image-20220409220430347](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220409220430347.png)

#### 单继承和多实现

​	Java不支持多继承（指不支持类的多继承），但是支持多实现。

​	Java只支持子类继承一个父类，但是可以实现多个接口。

​	但是Java接口可以多继承。

![image-20220420194957124](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420194957124.png)

#### 包装类和引用

​		Byte,Short,Integer,Long这4种整型的包装类也只是在对应值小于等于127并且大于等于-128时才可使用常量池，超过这个范围就会new一个对象，Character常量池的范围是0~127。

即下面的Integer如果定义的数值不大于127，不小于-128，则引用常量池(内存)的数。

所以i01==i03为true；

Interger为int的包装类，所以i02==i04比较时，Interger会拆除包装转换为int类型，比较的实质就是比较int型的数值是否相等，只要int变量和Integer变量的数相等，比较结果都为true。

当Integer和Integer用==比较时，比较的是引用的地址，因为i01和i03都是引用常量池的59，而i04是引用堆里的59，所以i01==i04和i03==i04都为false。

![image-20220420201359968](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420201359968.png)

#### 异常处理

C:error才表示无法处理的严重问题，不是runtimeExeption

![image-20220420203900187](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420203900187.png)

#### Java概念

Java异常和错误的基类是Throwable

Java的基本类型不是对象

高版本的JDK编写的程序可能无法在低版本的JDK环境正常运行。

![image-20220420204230941](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420204230941.png)

#### 方法重写与覆盖

方法重写也叫覆盖，只有子类才能重写父类的同方法，在一个类中两个方法只有返回值不同，也算同方法，会报错。

![image-20220420204642044](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420204642044.png)

字符编码集GBK

​	GBK的一个中文字符占两字节，java中的一个char字符也占两个字符，所以没问题。但是C语言的char只占一个字节，会报错。

![image-20220420204928434](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420204928434.png)

#### byte与数字

![image-20220420222913795](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420222913795.png)

![image-20220420205219605](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420205219605.png)

#### HttpServlet

当客户端将请求发送给servlet处理时，会调用HttpServlet的service方法，然后再通过service方法来判断请求类型，再来调用对应的do方法。

![image-20220420223004482](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420223004482.png)

#### this和this()

this()方法必须在构造方法的第一句，表示调用本类的另一个构造方法。

this调用属性不必放在构造方法第一句

![image-20220420223618289](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420223618289.png)

#### final和private

因为父类的run()方法是私有的，所以子类的无法重写父类的run()方法，所以父类的带final的run()方法没有被覆盖，所以合法。

![image-20220420223839935](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420223839935.png)

#### 修饰符

B：缺省修饰符的方法，其不在同包的子类无法访问，更无法继承覆盖。

![image-20220420224410844](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420224410844.png)

#### JVM

jvm包括程序计数器、虚拟机栈、本地方法栈、堆、方法区（包括常量池）。

![image-20220420224717697](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220420224717697.png)

### final

final不可修饰抽象类和接口，因为抽象类和接口只能被用来继承，但是final又不允许继承，矛盾

![image-20220421214940039](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220421214940039.png)

#### IO流和flush()方法

flush()方法是输出流的方法，作用是将写入缓存的数据清理，一次性写出。

![image-20220421215127241](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220421215127241.png)

#### 两同两小一大

方法名相同，参数类型相同

子类返回类型小于等于父类方法返回类型，
子类抛出异常小于等于父类方法抛出异常，
子类访问权限大于等于父类方法访问权限。

所以接口的实现类方法的返回值范围可以小于接口方法的返回值，不一定要相等、

![image-20220421215301547](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220421215301547.png)

#### final与引用

final修饰引用变量后，该引用变量不能再去引用其他对象地址，但是引用的对象的内容可改变。

![image-20220421215618465](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220421215618465.png)

#### this()和super()

this()指当前对象，super()指父类对象，但是static环境没有对象的说法，都是共用的，所以this()和super()不能在static环境使用

![image-20220421215918426](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220421215918426.png)

#### 构造方法

构造方法不能被继承，所以final关键字对构造方法没用。

构造方法是用来创建对象的，static没有对象的说法。

Java也不支持在构造方法加synchronized、native修饰。

![image-20220425170130705](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425170130705.png)

#### Java常识

接口的方法可以用public、abstract、default、static修饰，后面两个需要写方法体。

能出现在import语句前面的不只可以有注释，还可以有package语句。

![image-20220425171257454](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425171257454.png)

#### 哈希冲突

threadlocalmap使用开放定址法解决hash冲突，hashmap使用链地址法解决hash冲突

![image-20220425171749064](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425171749064.png)

#### 集合

Map和Collection和Iterator是集合界的三大祖宗接口，地位是一样的。

Map是双列（键值对）的集合接口，Collection是单列(一个值)的集合接口，Iterator是迭代器接口。

![image-20220425171924307](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425171924307.png)

#### 静态变量

静态变量是属于整个类的，不能在方法里定义，不管是静态方法还是动态方法都不行。

![image-20220425172652423](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425172652423.png)

#### 字符集ASCII码

标准的ASCII码使用7bit

扩展的ASCII码使用8bit

ASCII码包含一些不可打印的特殊空字符

![image-20220425173833692](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425173833692.png)

#### try-catch-finally

finally语句是在return执行完成之前执行，不是在return执行之前执行。

![image-20220425174243405](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425174243405.png)

#### 修饰符访问权限

![image-20220425175936997](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425175936997.png)

如果在外包new car对象，无法访问Car类中protect、private、default修饰的属性

![image-20220425175957453](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425175957453.png)

#### JDK版本与JVM

JDK1.8后将，常量池归于JVM堆，之前的版本归于方法区。

![image-20220425180202317](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425180202317.png)

#### 垃圾回收

垃圾回收线程优先级很低

垃圾回收的时间。对象，程序员无法指定。

程序逻辑出现问题也会导致内存溢出

DEAD的线程，它还可以恢复

![image-20220425180346120](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425180346120.png)

#### 重写与重载

子类方法名、方法参数（参数类型，个数，顺序都要相同）和父类方法相同，为重写方法，要求子类方法的返回值类型要与父类的相同。

![image-20220425180947067](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425180947067.png)



#### Byte包装类

Byte是byte的包装类，默认值是null

![image-20220425181632737](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425181632737.png)

#### 静态方法和静态变量

静态方法只能使用静态变量

![image-20220425181939204](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425181939204.png)

##### final关键字

final修饰的方法不能被重写，但是可以重载。

final修饰的变量不允许在被赋值，但是final的对象类型变量引用的对象内容可以改

![image-20220425182119571](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425182119571.png)

![image-20220425182100919](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220425182100919.png)

#### protect修饰符

![image-20220426183018280](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426183018280.png)

protect修饰的成员方法和成员变量可以被本类、同包的所有类、其他包的子类访问。

![image-20220426183002333](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426183002333.png)

#### Java跨平台特性

Java的跨平台特性表现在编译器将。java文件转化为字节码文件，然后可以在JVm允许

![image-20220426183434463](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426183434463.png)

#### String的substring方法

substring截取字符串是左闭右开。

![image-20220426195557641](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426195557641.png)

#### switch

当case语句没有break，还会继续执行下面的case语句

![image-20220426195635166](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426195635166.png)

#### 子类和父类的初始化

初始化过程是这样的： 

1.首先，初始化父类中的静态成员变量和静态代码块，按照在程序中出现的顺序初始化； 

2.然后，初始化子类中的静态成员变量和静态代码块，按照在程序中出现的顺序初始化； 

3.其次，初始化父类的普通成员变量和代码块，在执行父类的构造方法；

4.最后，初始化子类的普通成员变量和代码块，在执行子类的构造方法； 

 

（1）初始化父类的普通成员变量和代码块，执行 C c = new C(); 输出C 

（2）super("B"); 表示调用父类的构造方法，不调用父类的无参构造函数，输出B 

（3） System.out.print("B"); 

 所以输出CBB

![image-20220426195835158](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426195835158.png)

#### switch的判断条件

byte short char 在switch的判断条件都会转换成int类型

![image-20220426195943868](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426195943868.png)

#### byte和int的转换

byte类型的变量在做运算时被会转换为int类型的值，故A、B左为byte，右为int，会报错；而C、D语句中用的是a+=b的语句，此语句会将被赋值的变量自动强制转化为相对应的类型。

![image-20220426200443638](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426200443638.png)

#### 隔离级别和数据库系统

A，我们写java程序的时候只是设定事物的隔离级别，而不是去实现它

B，Hibernate是一个java的数据持久化框架，方便数据库的访问

C，事物隔离级别由数据库系统实现，是数据库系统本身的一个功能

D，JDBC是java database connector，也就是java访问数据库的驱动

![image-20220426200714740](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426200714740.png)

#### 父类和子类的初始化

不会初始化子类的几种

1. 调用的是父类的static方法或者字段

​	2.调用的是父类的final方法或者字段

3. 通过数组来引用

![image-20220426200938332](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426200938332.png)

#### Java和C++的char

java中只有byte, boolean是一个字节, char是两个字节, 所以对于java来说127不会发生溢出, 输出328

但是对于c/c++语言来说, char是一个字节, 会发生溢出, 对127加一发生溢出, 0111 1111 --> 1000 0000, 1000 0000为补码-128, 所以结果为200-128=72

![image-20220426201039259](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220426201039259.png)

####	不需要同步的原子操作

​		同步是害怕在操作过程的时候被其他线程也进行读取操作，一旦是原子性的操作就不会发生这种情况。

因为一步到位的操作，其他线程不可能在中间干涉。另外三项都有读取、操作两个步骤，而X=1则是原子性操作。

####	![image-20220427140545724](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427140545724.png)

#### 强制类型转换

因为强制转换的优先级比加减乘除高，所以只有10被转换成了short型，shrot型的10和浮点数相乘，结果会升级成等级更高的double型。

![image-20220427140719612](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427140719612.png)

#### try-catch-finally

fun(1)没有执行过程没有抛出异常，结果为"12"

fun(2)抛出了异常，因为catch中有return，所以字符串先加上一个0，然后再return执行结束之前执行finally加上"1".

所以最后是"1201"

![image-20220427141011715](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427141011715.png)

#### throw和throws的区别

throw是生成异常对象然后抛出异常对象的关键字。

throws是声明在方法中的，告诉上层的方法需要处理本方法的什么类型的异常。

![image-20220427141356156](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427141356156.png)

#### 类型能否转化

A：return;  没有返回值，**错误**

B：short → float 无须强制转换，**正确**

C：long → float  无须强制转换（**这个最选项容易出错**），**正确。**

float占4个字节为什么比long占8个字节大呢，因为底层的实现方式不同。

**浮点数的32位并不是简单直接表示大小，而是按照一定标准分配的**。

第1位，符号位，即S

接下来8位，指数域，即E。

剩下23位，小数域，即M，取值范围为[1 ,2 ) 或[0 , 1)

然后按照公式： **V=(-1)^s \* M \* 2^E**

**也就是说浮点数在内存中的32位不是简单地转换为十进制，而是通过公式来计算而来，通过这个公式虽然，只有4个字节，但浮点数最大值要比长整型的范围要大**。

D：double → float 没有强制转换，**错误**。

![image-20220427141646977](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427141646977.png)

#### 继承

子类能继承父类的所有成员，包括私有成员，但是不可以访问私有成员。

![image-20220427141758860](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427141758860.png)

#### 类加载器

JVM在判定两个class是否相同时，不仅要判断两个类名是否相同，而且要判断是否由同一个类加载器实例加载的。

![image-20220427215634988](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427215634988.png)

#### 静态变量和静态代码块

执行顺序：

静态变量x被初始化为10

静态代码块1改变静态变量的值x+=5 //15

静态代码块2改变静态变量的值x/=3 //5

![image-20220427220520114](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427220520114.png)

#### ServletContext

getParameter()是获取POST/GET传递的参数值；
getInitParameter获取Tomcat的server.xml中设置Context的初始化参数

getAttribute()是获取对象容器中的数据值；
getRequestDispatcher是请求转发。

![image-20220427220916871](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427220916871.png)

#### final修饰

final可以修饰：
变量（即修饰为常量）

类(修饰后不可被继承)

方法(修饰后无法被重写)

不可修饰不可变类

即Integer和String等系统库类

![image-20220427221747457](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427221747457.png)

#### 调用构造方法的方式

1、new

2、反射:newInstance方法

![image-20220427222459812](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427222459812.png)

#### Ceil和floor

Math.ceil(a)：返回小于a，最接近a的整数

Math.floor(a)：返回大于a,	最接近a的整数

![image-20220427222641580](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427222641580.png)

#### 类加载器

**A.Java系统提供3种类加载器：*启动类加载器（Bootstrap ClassLoader） 扩展类加载器（Extension ClassLoader） 应用程序类加载器（Application ClassLoader）. A正确**

**B：接口是特殊的类，不同的类加载器生成的接口不同。**

**  u**



![image-20220427223232637](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220427223232637.png)

#### Statement

​		Statement对象的作用就是执行SQL语句，但是Statement没有预编译功能(没有占位符)，可能会存在SQL注入问题，所以一般我们使用PreparedStatement对象来执行执行SQL语句，可以避免sql注入问题。

![image-20220428134640411](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428134640411.png)

#### 枚举类

enum是枚举类，所以enum不是一个基本数据类型。

![image-20220428140007571](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428140007571.png)

#### StringBuffer的length()和capacity()方法

length()方法返回的都是StringBuffer对象调用append方法里的字符串长度。

capacity()方法返回值分两种情况：

例如下图：

情况1： sb.append("1");

字符串实际长度1小于初始化容量长度2，capacity()方法返回初始化容量长度2.

情况2： sb.append("123")：初始化容量长度2<字符串实际长度3<扩展后的容量：初始化容量长度2*2+2，capacity()方法返回扩展后的容量：6.

情况3：sb.append("1234567")：字符串实际长度7(新扩展之后的容量)>之前扩展后的容量6,capacity()方法返回新扩展之后的容量7，之后再实际的字符串长度和扩展之后的容量相同。

情况4：前面的情况都是只使用一次sb.append()方法，当使用多个sb.append()方法时。

如：

sb.append("1234567");
sb.append("1234567");
sb.append("1234567");

会不断更新初始化容量长度，当执行第次sb.append("1234567");时，初始化容量长度被更新为7。所以执行两次sb.append("1234567");时，capacity()方法返回16.执行三次sb.append("1234567");时，初始化容量长度被更新为16，，capacity()方法返回34.

```java
class Solution432 {
    public static void main(String[] args) {
        StringBuffer sb = new StringBuffer(2);//初始化容量长度
        sb.append("1234567");
        System.out.println(sb.length());
        System.out.println(sb.capacity());
    }
}
```

![image-20220428141042081](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428141042081.png)

#### JVM内存的组成成分

![image-20220428145200721](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428145200721.png)

![image-20220428145108224](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428145108224.png)

#### 抽象类

抽象类只能用来给子类继承，不能实例化抽象类对象。

final类不能被继承，但是可以实例化final类的对象。

抽象类可以没有抽象方法

![image-20220428145336089](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428145336089.png)

#### hashCode

“==”：作用是判断两个对象的地址是否相等，即，判断两个对象是不是同一个对象，如果是基本数据类型，则比较的是值是否相等。

"equal"：作用是判断两个对象是否相等，但一般有两种使用情况

​       1.类没有覆盖equals()方法,则相当于通过“==”比较

​       2.类覆盖equals()方法，一般，我们都通过equals()方法来比较两个对象的内容是否相等，相等则返回true,如String

地址比较是通过计算对象的哈希值来比较的，hashcode属于Object的本地方法，对象相等（地址相等），hashcode相等，对象不相等，hashcode()可能相等，哈希冲突

![image-20220428150004340](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428150004340.png)

#### String

String变量指向的内存里的内容不可改变。

String变量可以改变指向的内存，即引用可以改变，String变量赋新值本质就是改变引用。

![image-20220428150347332](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428150347332.png)

#### try-catch-finally

```
    System.out.print(getNumber(0));//进入catch，返回的0被finally的-1代替
    System.out.print(getNumber(1));//进入finally，返回-1
    System.out.print(getNumber(2));//进入finally，返回1
    System.out.print(getNumber(4));//进入finally，finally没有return，return result 0
```

![image-20220428150848049](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428150848049.png)

#### ~运算符

公式-n=~n+1可推出~n=-n-1，所以~10=-11再加5结果为-6

![image-20220428151322049](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220428151322049.png)

#### 计算机系统和Java编程语言

计算机由硬件和软件(系统软件。应用软件)组成，操作系统属于系统软件，操作系统也不是必须的。

集成开发环境不是必须的,JDK才是

![image-20220429135150160](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429135150160.png)

### switch

JDK1.7版本，switch支持 int及以下（char， short， byte），String， Enum

![image-20220429135657532](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429135657532.png)

#### jvm内存配置参数

D

-Xmx：最大堆大小

-Xms：初始堆大小

-Xmn:年轻代大小

-XXSurvivorRatio：年轻代中Eden区与Survivor区的大小比值

年轻代5120m， Eden：Survivor=3，Survivor区大小=1024m（Survivor区有两个，即将年轻代分为5份，每个Survivor区占一份），总大小为2048m。

-Xms初始堆大小即最小内存值为10240m

![image-20220429140125636](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429140125636.png)

#### Collection接口的常用方法

1. size():返回集合中元素的个数
2. add(Object obj):向集合中添加一个元素
3. addAll(Colletion coll):将形参coll包含的所有元素添加到当前集合中
4. isEmpty():判断这个集合是否为空
5. clear():清空集合元素
6. contains(Object obj):判断集合中是否包含指定的obj元素
   ① 判断的依据：根据元素所在类的equals()方法进行判断
   ②明确：如果存入集合中的元素是自定义的类对象，要去：自定义类要重写equals()方法
7. constainsAll(Collection coll):判断当前集合中是否包含coll的所有元素
8. rentainAll(Collection coll):求当前集合与coll的共有集合，返回给当前集合
9. remove(Object obj):删除集合中obj元素，若删除成功，返回ture否则
10. removeAll(Collection coll):从当前集合中删除包含coll的元素
11. equals(Object obj):判断集合中的所有元素 是否相同
12. hashCode():返回集合的哈希值
13. toArray(T[] a):将集合转化为数组
    ①如有参数，返回数组的运行时类型与指定数组的运行时类型相同。
14. iterator():返回一个Iterator接口实现类的对象,进而实现集合的遍历。
15. 数组转换为集合：Arrays.asList(数组)

![image-20220429140238979](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429140238979.png)

#### 垃圾回收

![image-20220429140614815](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429140614815.png)

#### 垃圾回收

![image-20220429141132265](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429141132265.png)

![image-20220429141121155](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429141121155.png)

#### Java的while的参数

Java中while()只能放boolean类型的值，不能放int型的值。

C语言中while()不仅能放boolean类型的值，还能放int型的值，负整型为false，正整型为true

![image-20220429141242627](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429141242627.png)

#### URL类的实例化

​	实例化URL类需要捕获检测异常，这个异常是字符串不符合URL格式时才会抛出，如果字符串符合URL格式，但网址不存在也会返回该字符串。

如下才会抛出异常：

```java
    public static void main(String[] args) {
        try {
            URL w = new URL("wwsaasds");//不是网址格式的字符串
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
    }
}
```

![image-20220429141634023](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429141634023.png)

#### ThreadLocal

ThreadLocal继承Object

ThreadLocal的重要作用是保证了线程之间的独立，所以保证了线程的安全。

```
ThreadLocal是采用哈希表的方式来为每个线程都提供一个变量的副本
```

![image-20220429142128778](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220429142128778.png)

#### 字节码文件

java源文件的后缀是.java。源文件通过jvm虚拟机编译后会生成后缀为.class的二进制文件。

![image-20220501200920656](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501200920656.png)

#### 编译时异常和运行时异常

ClassCastException是类型转化异常，是运行时异常。

FileNotFoundException是找不到文件异常，是编译时异常

![image-20220501201221161](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501201221161.png)

#### replaceAll

replaceAll方法参数是正则表达式，"."表示任何字符，即把字符串的所有字符都换成了"/"。

如果想替换的只是"."，那么久要写成"\\."

![image-20220501201825580](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501201825580.png)

#### static变量的调用

main方法和静态变量在同一个类里面，所以main方法可以直接使用静态变量x。

使用类对象调用静态变量和直接使用类名调用静态变量的效果一样，静态变量只属于类。

![image-20220501202421829](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501202421829.png)

#### volatile

​		出于运行速率的考虑，java编译器会把经常经常访问的变量放到缓存（严格讲应该是工作内存）中，读取变量则从缓存中读。但是在多线程编程中,内存中的值和缓存中的值可能会出现不一致。volatile用于限定变量只能从内存中读取，保证对所有线程而言，值都是一致的。但是volatile不能保证原子性，也就不能保证线程安全。

![image-20220501202914159](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501202914159.png)

#### split()方法

split()方法如果找不到指定的分隔符，会把原字符串当成长度为1的数组。

```java
public static void main(String args[ ]){
    String a = "12/3";
    String b = "123";
    String[] a1 = a.split("/");
    String[] b1 = b.split("/");
    System.out.println(Arrays.toString(a1));
    System.out.println(Arrays.toString(b1));
}
```

![image-20220501203302935](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501203302935.png)

#### 线程同步和锁

线程对象t没有启动，只是单纯的调用run方法，只有一个主线程所以会按顺序执行：输出"SougouHello"。

如果t.run()改为t.start()，则主线程创建分线程t后，占用同步方锁HelloSougou.class(main方法和Sougou方法使用同一把锁HelloSougou.class)。所以main方法执行完毕后，主线程才会把锁交给t线程去执行Sougou方法，所以j结果是:"HelloSougou"。



![image-20220501203909094](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501203909094.png)

#### volatile和synchronized

synchronized能保证数据的原子性，即单线程，volatile存在同步安全问题，所以volatile不能保证数据的原子性。

多线程访问volatile不会发生阻塞，而synchronized会出现阻塞

![image-20220501205302441](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501205302441.png)

#### 多行注释和单行注释

​		在Java中规定，多行注释可以嵌套单行注释，但是不能嵌套多行注释。 不太能理解原因的朋友不妨这样想，如果多行注释/*A//B*/完全可以将内部的A//B作为普通字符串来理解，没有所谓单行不单行之说，所以可以嵌套单行注释。 但是如果是嵌套多行注释呢可以看看/*A/*B*/C*/，到编译器遇到多行注释的时候会以’/*A/*B*/’来作为匹配的字符串，从而C*/会被理解为程序语句，而C*/显然不符合程序语法，因此这样想就能理解为什么不能多行注释嵌套多行注释了

![image-20220501205635299](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220501205635299.png)

#### 静态变量、静态代码块、非静态代码块、构造方法

​	一个static对象它在内存中仅有一个，且JVM只会为它分配一次内存，首先进行类的加载，在这过程中已经为Test的static对象分配好了内存，执行顺序为: 

  1.public static Test t1 = new Test(); 

  2.static

​     {  

​       System.out.println("blockB");  

​     }  

  所以它在执行第一步实例化中不会再去执行static代码块，因为已经分配过一次内存，只会进行实例化，也就是输出A，然后按照顺序执行static语句块，输出B，最后实例化t2，输出A

![image-20220502164654210](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502164654210.png)

#### 成员变量和局部变量

成员变量是属于对象的，因为对象在堆区，所以成员变量也在堆区。

局部变量在栈区。

![image-20220502170646514](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502170646514.png)

#### 静态代码块

静态代码块无法直接调用

![image-20220502170911420](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502170911420.png)

#### Java内存区域

B：类信息保存在方法区中

C:Java堆是线程共享的

![image-20220502171255961](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502171255961.png)

#### 值类型和引用类型

D:局部的引用变量保存在栈中

![image-20220502171802607](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502171802607.png)

#### Java的数据类型占用的内存

Object在没有实例化的时候占用的内存是1bit(位)，0.128字节

int 占32bit，4字节

long占64位，8字节

byte占8位，一个字节，byte就是字节。

![image-20220502172243126](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502172243126.png)

#### GBK编码转换为UTF-8

选B，先通过GBK编码还原字符串，在该字符串正确的基础上得到“UTF-8”所对应的字节串。

![image-20220502172920507](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502172920507.png)

#### 匿名内部类

匿名内部类没有名字，构造方法名要和类名相同，所以匿名内部类没有构造方法。

![image-20220502172954885](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502172954885.png)

#### 接口的属性

​	接口的属性修饰符必须是public static final，这三个修饰符可以部分省略，也可以全部省略。不能使用其他修饰符，因为接口的成员属性有final修饰，所以成员属性必须有初始值。

![image-20220502173756227](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502173756227.png)

#### ArrayList的扩充

创建ArrayList对象时，直接分配10的数组大小，没有扩充。调用add方法添加才有可能扩充。

![image-20220502173830409](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502173830409.png)

#### 抽象类和接口

抽象类和接口都可以有静态方法

因为接口的成员属性默认修饰符public static final，所以接口的变量都常量

![image-20220502174241572](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220502174241572.png)

#### javac-d命令

![image-20220503185632996](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503185632996.png)

#### super关键字的作用

1、调用父类的构造方法

2、调用父类被重写的方法

3、调用父类非私有的成员方法和成员变量

![image-20220503190312560](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503190312560.png)

![image-20220503190324771](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503190324771.png)

#### 数组的创建方式

java的数组规范定义方式是 ：数据类型[] 数组名 = new 数据类型[]；

如果是二维数组，行的长度不能省,列长可以省。

![image-20220503190945607](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503190945607.png)

#### JDK1.8接口

JDK1.8之后的接口带default或static的方法可以有方法体。

定义接口的修饰符是：interface

![image-20220503191840687](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503191840687.png)

#### 线程安全

A：ConcurrentHashMap使用的是Segement（继承自   ReentrantLock  ）分段锁的技术来保证同步的，  使用synchronized关键字保证线程安全的是HashTable；

B：  HashMap实现的是Map接口

 C:  Arrays.asList方法返回List列表，  public static <T>   [List](http://www.yq1012.com/api/java/util/List.html)  <T>   **asList**  (T... a)

  D:  SimpleDateFormat查看Java源码可以看到，它的方法都不是Synchronized的，也没有采用其他的同步措施

![image-20220503192034083](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503192034083.png)

#### JAVA编译和运行

**java的运行命令:java+java程序名，不加后缀**

![image-20220503192321584](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503192321584.png)

#### service()方法的定义和doGet、doPost方法的实现

service方法在javax.servlet.GenericServlet接口实现

doGet/doPost 是在 javax.servlet.http.HttpServlet 中实现的

![image-20220503192657498](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503192657498.png)

#### API的继承

答案：ABE

A，Thread可以被继承，用于创建新的线程

B，Number类可以被继承，Integer，Float，Double等都继承自Number类

C，Double类的声明为

```
public` `final` `class` `Doubleextends Numberimplements Comparable<Double>
```

 final生明的类不能被继承

D，Math类的声明为

```
public` `final` `class` `Mathextends Object
```

  不能被继承

E，ClassLoader可以被继承，用户可以自定义类加载器

![image-20220503192933953](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220503192933953.png)

#### 抽象类abstract

抽象类不能被实例化

抽象类不仅可以被继承，还可以被用来调用实现类里的静态方法。

![image-20220504163707570](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504163707570.png)

#### instanceof关键字

  A

|    |

B   C

|

D

D属于B,D属于A,D属于D,D不属于C

所以选C

![image-20220504164144452](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504164144452.png)

#### 使用锁提高访问性能

答案：C

A，只对写操作加锁，不对读操作加锁，会造成读到脏数据

B，CopyOnWrite的核心思想是利用高并发往往是读多写少的特性，对读操作不加锁，对写操作，先复制一份新的集合，在新的集合上面修改，然后将新集合赋值给旧的引用。这里读写平均，不适用

C，分段加锁，只在影响读写的地方加锁，锁可以用读写锁，可以提高效率

![image-20220504164337562](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504164337562.png)

#### 方法或代码块中的局部内部类

方法或代码块中的局部内部类以及局部变量等，都不能使用访问权限修饰符来修饰)(public、private、protect等)

![image-20220504165049881](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504165049881.png)

![image-20220504165245327](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504165245327.png)

#### Set的实现类的内部排序

LinkedHashSet按照元素的添加顺序排序

TreeSet会调用集合元素的compareTo(Object obj)方法来比较元素之间大小关系，然后将集合元素按升序排列，这种方式就是自然排序。

![image-20220504165505229](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504165505229.png)

#### final修饰的成员变量的初始化

final修饰的成员变量必须要初始化，可以直接赋值，也可以在构造方法、静态代码块、非静态代码块初始化。

![image-20220504170142311](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504170142311.png)



![image-20220504165935170](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504165935170.png)



#### hashCode()和equals()方法

hashCode()方法和equals()方法的作用其实是一样的，在Java里都是用来对比两个对象是否相等一致。

***那么equals()既然已\******经\******能\******实现\******对\******比的功能了，为什么还要hashCode()呢？\***因为重写的equals()里一般比较的比较全面比较复杂，这样效率就比较低，而利用hashCode()进行对比，则只要生成一个hash值进行比较就可以了，效率很高。*
*

***那么hashCode()既然效率这么高为什么还要equals()呢******？*** 因为hashCode()并不是完全可靠，有时候不同的对象他们生成的hashcode也会一样（生成hash值得公式可能存在的问题），所以hashCode()只能说是大部分时候可靠，并不是绝对可靠，

**所以我们可以得出：**

**1.equals()相等的两个对象他们的hashCode()肯定相等，也就是用equals()对比是绝对可靠的。**

**2.hashCode()相等的两个对象他们的equal()不一定相等，也就是hashCode()不是绝对可靠的。**

所有对于需要大量并且快速的对比的话如果都用equals()去做显然效率太低，所以解决方式是，每当需要对比的时候，首先用hashCode()去对比，如果hashCode()不一样，则表示这两个对象肯定不相等（也就是不必再用equal()去再对比了）,如果hashCode()相同，此时再对比他们的equals()，如果equals()也相同，则表示这两个对象是真的相同了，这样既能大大提高了效率也保证了对比的绝对正确性！

所以选D

![image-20220504170546415](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504170546415.png)

#### java构造函数和公共类

​		公共类是指java源文件中，带public修饰符的类，一个源文件只能有一个公共类。公共类可以被所有的包访问并实例化，非公共类不能被其他包访问。

一个源文件有多个类，公共类和非公共类都拥有和自己同名的构造方法，所以构造器只和本类同名。

![image-20220504170927869](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504170927869.png)

#### Servlet的生命周期即准备工作

![image-20220504171545542](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504171545542.png)

![image-20220504171635125](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504171635125.png)

#### Integer类型和int类型的数值比较

- **选项A**，a1、a2赋值给Integer类型，自动装箱。对于–128到127（默认是127）之间的值，Integer.valueOf(int i) 返回的是缓存的Integer对象（并不是新建对象），变量所指向的是同一个对象，**所以a1==a2返回true**。
- **选项B**，Integer和int比较会进行自动拆箱，比较的是数值大小，**所以d1==d2返回true**。
- **选项C**，由于超出自动装箱的范围，return返回的是新建的对象，所以**对象内存地址不同**，**b1==b2返回false。**
- **选项D**，普通new创建对象，两个new创建两个地址不同的对像，**所以c1==c2返回false**。

![img](https://uploadfiles.nowcoder.com/images/20190524/300975041_1558682067274_06DBFDA8D65634ECA793E9422DC7FC0D)

![image-20220504172522094](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504172522094.png)

#### 使用泛型的好处

**使用泛型的好处**

**1，类型安全。** 泛型的主要目标是提高 Java 程序的类型安全。通过知道使用泛型定义的变量的类型限制，编译器可以在一个高得多的程度上验证类型假设。没有泛型，这些假设就只存在于程序员的头脑中（或者如果幸运的话，还存在于代码注释中）。

 

**2，消除强制类型转换。** 泛型的一个附带好处是，消除源代码中的许多强制类型转换。这使得代码更加可读，并且减少了出错机会。

 

**3，潜在的性能收益。** 泛型为较大的优化带来可能。在泛型的初始实现中，编译器将强制类型转换（没有泛型的话，程序员会指定这些强制类型转换）插入生成的字节码中。但是更多类型信息可用于编译器这一事实，为未来版本的 JVM 的优化带来可能。由于泛型的实现方式，支持泛型（几乎）不需要 JVM 或类文件更改。所有工作都在编译器中完成，编译器生成类似于没有泛型（和强制类型转换）时所写的代码，只是更能确保类型安全而已。

**所以泛型只是提高了数据传输安全性，并没有改变程序运行的性能**

![image-20220504172614025](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220504172614025.png)

#### IO流的继承关系

BufferedReader的父类是Reader

![image-20220507203904703](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507203904703.png)

![image-20220507203839875](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507203839875.png)

#### 内部类和静态方法

A:静态内部类才可以声明静态方法

C/D:静态方法不可以使用非静态变量

B:抽象方法不可以有函数体

![image-20220507204802981](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507204802981.png)

#### ISO8859-1字符串转成GB2312编码

![image-20220507205100436](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507205100436.png)

#### Integer的parseInt()、valueOf()、intValue()方法。

intValue()是把Integer对象类型变成int的基础数据类型；
parseInt()是把String 变成int的基础数据类型；
Valueof()是把String 转化成Integer对象类型；（现在JDK版本支持自动装箱拆箱了。）
本题：parseInt得到的是基础数据类型int，valueof得到的是装箱数据类型Integer，然后再通过valueInt转换成int，所以选择D

![image-20220507205743030](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507205743030.png)

#### substring()方法创建对象

String对象调用substring()方法实际上会创建一个新的String对象

所以str2、str3、str4都是引用了堆区的内存。

![image-20220507210000430](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507210000430.png)

#### 线程安全的Map的实现类

线程安全的map实现类有:HashTable,\SynchronizedMap,ConcurrentHashMap

![image-20220507210401636](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507210401636.png)

#### 编译时异常和运行时异常



![image-20220507210733782](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507210733782.png)

#### 匿名内部类和方法的重写

equals方法重写后，无论参数是什么，都返回true

![image-20220507211939738](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507211939738.png)

#### Object类的方法

**protected** **Object** **clone** **()**               //创建并返回此对象的一个副本。 

**boolean** **equals** **(Object obj)**            //指示某个其他对象是否与此对象“相等”。 

**protected** **void** **finalize** **()**               //当垃圾回收器确定不存在对该对象的更多引用时，由对象的垃圾回收器调用此方法。 

**Class<? extends Object>** **getClass** **()**     //返回一个对象的运行时类。 

**int** **hashCode** **()**                       //返回该对象的哈希码值。 

**void** **notify** **()**                          //唤醒在此对象监视器上等待的单个线程。 

**void** **notifyAll** **()**                       //唤醒在此对象监视器上等待的所有线程。 

**String** **toString** **()**                      //返回该对象的字符串表示。 

**void** **wait** **()**                           //导致当前的线程等待，直到其他线程调用此对象的 notify () 方法或 notifyAll () 方法。 

**void** **wait** **(** **long** **timeout)**                //导致当前的线程等待，直到其他线程调用此对象的 notify () 方法或 notifyAll () 方法，或者超过指定的时间量。 

**void** **wait** **(** **long** **timeout,** **int** **nanos)**       //导致当前的线程等待，直到其他线程调用此对象的 notify () 方法或 notifyAll () 方法，或者其他某个线程中断当前线程，或者已超过某个实际时间量。

**没有copy()方法**

![image-20220507212102072](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507212102072.png)

#### 静态成员

private修饰的静态成员在外部类不能使用类名调用。

![image-20220507212339280](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507212339280.png)

#### 抽象类和接口的区别

**抽象类**

特点:

1.抽象类中可以构造方法

2.抽象类中可以存在普通属性，方法，静态属性和方法。

3.抽象类中可以存在抽象方法。

4.如果一个类中有一个抽象方法，那么当前类一定是抽象类；抽象类中不一定有抽象方法。

5.抽象类中的抽象方法，需要有子类实现，如果子类不实现，则子类也需要定义为抽象的。

**接口**

1.在接口中只有方法的声明，没有方法体。

2.在接口中只有常量，因为定义的变量，在编译的时候都会默认加上

public static final 

3.在接口中的方法，永远都被public来修饰。

4.接口中没有构造方法，也不能实例化接口的对象。

5.接口可以实现多继承

6.接口中定义的方法都需要有实现类来实现，如果实现类不能实现接口中的所有方法

7.则实现类定义为抽象类。

![image-20220507212634383](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507212634383.png)

#### 循环优化

常见的代码优化技术有：复写传播，删除死代码, 强度削弱，归纳变量删除

![image-20220507215437043](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507215437043.png)

#### 默认类型

整数类型 默认为 int
带小数的默认为 double

![image-20220507215556149](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507215556149.png)

#### 保证线程安全的变量声明方式

A B选项，免程序在多线程竞争情况下读到不正确的值需要保证内存可见性，即当一个线程修改了volatile修饰的变量的值，volatile会保证新值立即同步到主内存，以及每次使用前立即从主内存读取。

C选项，synchronized可以修饰方法、代码块或对象，并不修饰变量。

D选项，static修饰的变量属于类，线程在使用这个属性的时候是从类中复制拷贝一份到线程工作内存中的，如果修改线程内存中的值之后再写回到原先的位置，就会有线程安全问题。用static修饰的变量可见性是无法确保的。

![image-20220507215740770](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507215740770.png)

#### LinkedBlockingQueue和PriorityQueue

LinkedBlockingQueue根据优先级从大到小进行出队。

![image-20220507220158353](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507220158353.png)

#### 反射

A Class类在java.lang包

C 反射访问私有成员时，Field调用setAccessible可解除访问符限制

D CGLIB实现了字节码修改，反射不行

E 反射会动态创建额外的对象，比如每个成员方法只有一个Method对象作为root，他不胡直接暴露给用户。调用时会返回一个Method的包装类

F 反射带来的效率问题主要是动态解析类，JVM没法对反射代码优化。

![image-20220507220514225](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507220514225.png)

#### 类型自动转换

int类型会自动转换为精度更高的float型，所以结果为0.5

![image-20220507220706886](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507220706886.png)

#### 接口的修饰

接口也不能使用protected修饰，因为接口可以让所有的类去实现，使用protected修饰则不能让外包的非子类的类取实例

![image-20220507220853764](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220507220853764.png)

#### 接口和抽象类的关键字

![image-20220508214544443](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508214544443.png)

#### 异常处理

A：运行时异常JVM帮我们处理，无需我们处理。

B:NumberFormatException 是数字类型异常

![image-20220508214734147](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508214734147.png)

#### java类之间的关系有哪些

**1、 泛化(Generalization) IS-A
** 表示类与类之间的继承关系，接口与接口之间的继承关系，或类对接口的实现关系。  

  **2、关联(Association) 
**    对于两个相对独立的对象，当一个对象的实例与另一个对象的一些特定实例存在固定的对应关系时，这两个对象之间为关联关系  

   **2.1聚合(Aggregation) HAS-A**  

​      聚合关系(Aggregation)：是关联关系的一种，是强的关联关系。聚合是整体与个体之间的关系。如汽车类与引挚类，轮胎类之间的关系就是整体与个体的关系。  

​    **2.2组合(Composition)**   

​        是关联关系的一种，是比聚合关系强的关系。它要求普通的聚合关系中代表的对象负责代表部分的对象的生命周期，合成关系是不能共享的。  

   **3、 依赖(Dependency) USES-A**  

   **对于两个相对独立的对象，当一个对象负责构造另一个对象的实例，或者依赖另一个对象的服务时，这两个对象之间主要体现为依赖关系。**   

![image-20220508215407821](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508215407821.png)

##### CallableStatement、PreparedStatement、Statement接口的关系

![image-20220508215854969](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508215854969.png)

![image-20220508215545734](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508215545734.png)

#### 重载方法的选择

因为无论是s还是o都可以选择public static void sayHello(Object to)，但是编译器会选择最合适的，即参数不用类型转换的调用方式，即s选择public static void sayHello(String to) ，o选择public static void sayHello(Object to)。

![image-20220508220250311](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508220250311.png)

#### 类和类成员的比较

Test类没有重写Object的equals方法，所以test.equals(testB)比较对象的地址，并没有深入比较对象的内容。

test.name.equals(testB.name)比较name字符串是否相等，二者都是"abc"，结果返回true。

​	在test对象初始化时，JVM会先检查字符串常量池中是否存在该字符串的对象，此时字符串常量池中并不存在"abc"字符串对象，JVM会在常量池中创建这一对象。当testB被创建时，同样会执行private String name = "abc";，那么此时JVM会在字符串常量池中找到test对象初始化时创建的"abc"字符串对象，那么JVM就会直接返回该字符串在常量池中的内存地址，而不会新建对象。也就是说test.name和testB.name指向的内存是同一个，那么test.name == testB.name返回true

![image-20220508220912360](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508220912360.png)

#### 自定义异常和接口的成员声明

B选项说的情况就是我们**自定义异常的情况**，请仔细读：**我们可以用违例（Exception）来抛出一些并非错误的消息**，可以，并非错误的消息。比如我自定义一个异常，若一个变量大于10就抛出一个异常，这样就对应了B选项说的情况，我用抛出异常说明这个变量大于10，而不是用一个函数体（函数体内判断是否大于10，然后返回true或false）判断，**因为函数调用是入栈出栈，栈是在寄存器之下的速度最快，且占的空间少，而自定义异常是存在堆中，肯定异常的内存开销大！**所以B对。
C选项说的是接口包含方法声明和变量声明。因为**接口中方法默认是 abstract public,所以在接口只写函数声明是符合语法规则**。但是**变量默认是用public final static 修饰的，意思它是静态常量，常量不管在接口中还是类中必须在声明时初始化！**所以C的后半句是错的，必须在声明时并给出初始化！

![image-20220508221422791](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508221422791.png)

#### 线程Tread类的方法

​		yield()让当前正在运行的线程回到就绪状态，以允许具有相同优先级的其他线程获得运行的机会。因此，使用yield()的目的是让具有相同优先级的线程之间能够适当的轮换执行。但是，实际中无法保证yield()达到让步的目的，因为，让步的线程可能被线程调度程序再次选中。

![image-20220508221530737](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508221530737.png)

#### Tread类中会释放锁资源的方法

1.sleep会使当前线程睡眠指定时间，不释放锁

2.yield会使当前线程重回到可执行状态，等待cpu的调度，不释放锁

3.wait会使当前线程回到线程池中等待，释放锁，当被其他线程使用notify，notifyAll唤醒时进入可执行状态

4.当前线程调用 某线程.join（）时会使当前线程等待某线程执行完毕再结束，底层调用了wait，释放锁

![image-20220508221738611](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220508221738611.png)

## Java字符集编码

A 显然是错误的，Java一律采用Unicode编码方式，每个字符无论中文还是英文字符都占用2个字节。

B 也是不正确的，不同的编码之间是可以转换的，通常流程如下：

将字符串S以其自身编码方式分解为字节数组，再将字节数组以你想要输出的编码方式重新编码为字符串。

例：String newUTF8Str = new String(oldGBKStr.getBytes("GBK"), "UTF8");

C 是正确的。Java虚拟机中通常使用UTF-16的方式保存一个字符

D 也是正确的。ResourceBundle能够依据Local的不同，选择性的读取与Local对应后缀的properties文件，以达到国际化的目的。

![image-20220510194026219](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220510194026219.png)

#### java监视器

java监视器就是锁，实现线程之间的同步执行。

![image-20220510194226263](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220510194226263.png)

#### 中间件的特点

中间件是一种独立的系统软件或服务程序，分布式应用软件借助这种软件在不同的技术之间共享资源。中间件位于客户机/ 服务器的操作系统之上，管理计算机资源和网络通讯。是连接两个独立应用程序或独立系统的软件。相连接的系统，即使它们具有不同的接口，但通过中间件相互之间仍能交换信息。执行中间件的一个关键途径是信息传递。通过中间件，应用程序可以工作于多平台或OS环境。

（简单来说，中间件并不能提高内核的效率，一般只是负责网络信息的分发处理）

![image-20220510194334971](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220510194334971.png)

#### Java Application 源程序的主类

![image-20220510203258339](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220510203258339.png)

#### 静态方法

静态方法中不能使用this关键字

![image-20220510203348613](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220510203348613.png)

#### 线程开始执行

start方法是启动线程，此时线程处于就绪状态。线程开始执行是CPU调度时调用run()。

![image-20220510203452815](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220510203452815.png)

#### 不同类型的赋值

B：Java浮点型默认为double类型，double类型的11.1 转成 float，或者在浮点数据后面加f,是需要强制转换的

C: double类型的0.0 转成 int，也是需要强制转换的

D:int 转为 封装类型Double，是无法编译的

![image-20220510203834045](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220510203834045.png)

### 数组

E:数组继承了Object的equal方法，比较的是数组的地址值，想要比较数组里的每个元素，需要调用Arrays.equals()方法。

F：多维数组在Java中是合法的

![image-20220511135417981](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511135417981.png)

#### String、StringBuffer、StringBuilder

String的值是堆区数组的引用，类型是final，不可以改变，但是String的值的内容(字符串数据)可以改变。

在运行速度上StringBuffer因为兼顾了线程安全，效率不及StringBuilder

StringBuffer是线程安全的

运行速度StringBuilder>StringBuffer>String

![image-20220511140142317](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511140142317.png)

#### 线程抛出InterruptedException异常的情况

当线程在活动之前或活动期间处于正在等待、休眠或占用状态且该线程被中断时，抛出该异常。

抛InterruptedException的代表方法有：

- java.lang.Object 类的 wait 方法
- java.lang.Thread 类的 sleep 方法
- java.lang.Thread 类的 join 方法

![image-20220511140436681](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511140436681.png)

#### 常量池

![image-20220511162719915](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511162719915.png)

### mvc设计模式的优点

​		MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。
MVC只是将分管不同功能的逻辑代码进行了隔离，增强了可维护和可扩展性，增强代码复用性，因此可以减少代码重复。但是不保证减少代码量，多层次的调用模式还有可能增加代码量

![image-20220511163246176](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511163246176.png)

#### Float类

1. Float是类，float不是类.
2. 查看JDK源码就可以发现Byte，Character，Short，Integer，Long，Float，Double，Boolean都在java.lang包中.
3. Float正确复制方式是Float f=1.0f,若不加f会被识别成double型,double无法向float隐式转换.
4. Float a= new Float(1.0)是正确的赋值方法，但是在1.5及以上版本引入自动装箱拆箱后，会提示这是不必要的装箱的警告，通常直接使用Float f=1.0f.

![image-20220511163553457](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511163553457.png)

#### JVM参数配置

![image-20220511163822678](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511163822678.png)

#### Socket通信编程

```
客户端通过new Socket()方法创建通信的Socket对象
服务器端通过new ServerSocket()创建TCP连接对象  accept接纳客户端请求
```

![image-20220511164025178](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511164025178.png)

#### 线程安全的类

在集合框架中，有些类是线程安全的，这些都是jdk1.1中的出现的。在jdk1.2之后，就出现许许多多非线程安全的类。 下面是这些线程安全的同步的类：

vector：就比arraylist多了个同步化机制（线程安全），因为效率较低，现在已经不太建议使用。在web应用中，特别是前台页面，往往效率（页面响应速度）是优先考虑的。

statck：堆栈类，先进后出

hashtable：就比hashmap多了个线程安全

enumeration：枚举，相当于迭代器

除了这些之外，其他的都是非线程安全的类和接口。

![image-20220511164357125](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511164357125.png)

#### String和常量池

new String会在堆区生成String对象。

String s ="字符串",会在常量池找有没有该字符串，如果有，直接引用在常量池的地址，如果没有则new一块堆区

![image-20220511165121114](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511165121114.png)

#### 线程销毁的结束标志

![image-20220511165433101](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511165433101.png)

![image-20220511165422503](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511165422503.png)

#### 进制计算

八进制13转十进制：1*8+3=11
八进制14转十进制：1*8+4=12
11*12=132
八进制204转十进制：2*8*8+0*8+4=132 

所以：（1*x¹+3*x°）* (1*x¹+4*x°) = 2*x²+0*x¹+4*x°

​       (x+3)*(x+4)=2x²+4

​       x²+7x+12=2x²+4

​       x²-7x=8

​       x*(x-7)=8

​       x₁=8  x₂=-1

 解二元一次方程组 得到  8  【x代表进制】

![image-20220511170134661](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511170134661.png)

#### 通信方式：管道

**对于管道，有下面这几种类型：**

**①普通管道（PIPE）：通常有两种限制，一是单工，即只能单向传输；二是血缘，即常用于父子进程间（或有血缘关系的进程间）。**

**②流管道（s_pipe）：去除了上述的第一种限制，实现了双向传输。**

**③命名管道（name_pipe）：去除了上述的第二种限制，实现了无血缘关系的不同进程间通信。**

**显然，要求是对于不同的服务器之间的通信，是要要求全双工形式的，而管道只能是半双工*，虽然可以双向，*但是同一时间只能有一个方向传输，全双工和半双工的区别可以如下图示理解：![img](https://uploadfiles.nowcoder.com/images/20180322/4846014_1521723853172_FF523AF3E7DA7B365BEA995386A30039)**

![image-20220511170341593](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511170341593.png)

#### 枚举类

枚举类有三个实例，故调用三次构造方法，打印三次It is a account type

![image-20220511170802853](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511170802853.png)



![image-20220511170632413](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511170632413.png)

#### Java包装类

Java包装类是相对于基本数据类型的。

因为没有string这个数据类型，所以String不是包装类，String就是一个类，一个API。

| 基本数据类型 | 包装类    |
| ------------ | --------- |
| byte         | Byte      |
| boolean      | Boolean   |
| short        | Short     |
| char         | Character |
| int          | Integer   |
| long         | Long      |
| float        | Float     |
| double       | Double    |

![image-20220511171555396](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511171555396.png)

#### 面向对象的理解

![image-20220511171826064](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511171826064.png)

#### java7以后的switch

在Java7之前，switch只能支持 byte、short、char、int或者其对应的封装类以及Enum类型。在Java7中，呼吁很久的String支持也终于被加上了。

![image-20220511171944230](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511171944230.png)

#### 重写方法的权限修饰符

接口实现类和抽象类子类的重写方法的的权限修饰符的权限应该要大于原方法的修饰符权限。

而且，接口的只能使用default和public修饰方法。

![image-20220511172345511](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511172345511.png)

#### 编译器的优化

​		这题是在考编译器的优化，hotspot中 编译时"tao"+"bao"将直接变成"taobao"，b+c则不会优化，因为不知道在之前的步骤中bc会不会发生改变，而针对b+c则是用语法糖，新建一个StringBuilder来处理

![image-20220511174326276](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511174326276.png)

#### 子类和父类的加载顺序

1.首先，需要明白**类的加载顺序**。

(1) 父类静态代码块(包括静态初始化块，静态属性，但不包括静态方法)

(2) 子类静态代码块(包括静态初始化块，静态属性，但不包括静态方法 )

(3) 父类非静态代码块( 包括非静态初始化块，非静态属性 )

(4) 父类构造函数

(5) 子类非静态代码块 ( 包括非静态初始化块，非静态属性 )

(6) 子类构造函数

其中：类中静态块按照声明顺序执行，并且(1)和(2)不需要调用new类实例的时候就执行了(意思就是在类加载到方法区的时候执行的)

2.其次，需要理解子类覆盖父类方法的问题，也就是**方法重写实现多态**问题。

Base b = new Sub();**它为多态的一种表现形式，声明是Base,实现是Sub类，** **理解为** **b** **编译时表现为Base类特性，运行时表现为Sub类特性。**

当子类覆盖了父类的方法后，意思是父类的方法已经被重写，**题中** **父类初始化调用的方法为子类实现的方法，子类实现的方法中调用的baseName为子类中的私有属性。**

由1.可知，此时只执行到步骤4.,子类非静态代码块和初始化步骤还没有到，子类中的baseName还没有被初始化。所以此时 baseName为空。 所以为null。

**如果子类的baseName是静态变量。则会输出sub**

![image-20220511174850254](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511174850254.png)

#### 实例变量和类变量

实例变量就是类的实例(对象)用的变量，即非静态变量。类变量是使用类名调用的变量，即静态变量

**A.类的成员变量包括实例变量和类变量（静态变量）,成员方法包括实例方法和类方法（静态方法）。** **A正确**

**B.类变量（静态变量）用关键字static声明，B错误**

**C.方法中的局部变量在方法被调用加载时开始入栈时创建，方法入栈创建栈帧包括局部变量表操作数栈，局部变量表存放局部变量，并非在执行该方法时被创建，C错误**

**D.局部变量被使用前必须初始化，否则程序报错。D正确**

![image-20220511175221849](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220511175221849.png)

#### 静态变量和静态块和构造块

t1和t2都是静态变量，需要第一时间初始化，初始化需要都调用构造块，因为t1和t2初始化都占用了static资源，所以两次初始化都没有调用静态代码块，只调用了构造块。直到t1和t2初始化完成，才调用t对象的静态代码块，最后调用t对象的构造块和构造方法。

![image-20220512195325334](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512195325334.png)

#### 重写方法

只要是被子类重写的方法，不被super调用都是调用子类方法

![image-20220512200246598](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512200246598.png)

#### 字符流和字节流

以stream结尾的都是字节流，字符流才能编码集(Unicode)

![image-20220512200501791](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512200501791.png)

#### 反射获取对象的属性

getDeclaredField：查找该Class所有声明属性(静态/非静态)，但是他不会去找实现的接口/父类的属性

getField：只查找该类public类型的属性，如果找不到则往上找他的接口、父类，依次往上，直到找到或者已经没有接口/父类

因为是static属性，get方法可以取null，不是则需要传对象

![image-20220512200747834](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512200747834.png)

#### 实体类继承抽象类

实体类继承抽象类必须实现抽象方法，不然编译不通过

![image-20220512201224521](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512201224521.png)

#### Math类的方法

floor: 求小于参数的最大整数。返回double类型-----n. 地板，地面

​     例如：Math.floor(-4.2) = -5.0

\-----------------------------------------------------------

ceil:  求大于参数的最小整数。返回double类型-----vt. 装天花板；

​     例如：Math.ceil(5.6) = 6.0

\-----------------------------------------------------------

round: 对小数进行四舍五入后的结果。返回int类型

​     例如：Math.round(-4.6) = -5

![image-20220512201412639](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512201412639.png)

#### JSP的内置对象

这些都是JSP内置对象

JSP内置对象有：
1.request对象
   客户端的请求信息被封装在request对象中，通过它才能了解到客户的需求，然后做出响应。它是HttpServletRequest类的实例。
2.response对象
   response对象包含了响应客户请求的有关信息，但在JSP中很少直接用到它。它是HttpServletResponse类的实例。
3.session对象
   session对象指的是客户端与服务器的一次会话，从客户连到服务器的一个WebApplication开始，直到客户端与服务器断开连接为止。它是HttpSession类的实例.
4.out对象
   out对象是JspWriter类的实例,是向客户端输出内容常用的对象
5.page对象
   page对象就是指向当前JSP页面本身，有点象类中的this指针，它是java.lang.Object类的实例
6.application对象
   application对象实现了用户间数据的共享，可存放全局变量。它开始于服务器的启动，直到服务器的关闭，在此期间，此对象将一直存在；这样在用户的前后连接或不同用户之间的连接中，可以对此对象的同一属性进行操作；在任何地方对此对象属性的操作，都将影响到其他用户对此的访问。服务器的启动和关闭决定了application对象的生命。它是ServletContext类的实例。
7.exception对象
  exception对象是一个例外对象，当一个页面在运行过程中发生了例外，就产生这个对象。如果一个JSP页面要应用此对象，就必须把isErrorPage设为true，否则无法编译。他实际上是java.lang.Throwable的对象
8.pageContext对象
pageContext对象提供了对JSP页面内所有的对象及名字空间的访问，也就是说他可以访问到本页所在的SESSION，也可以取本页面所在的application的某一属性值，他相当于页面中所有功能的集大成者，它的本 类名也叫pageContext。
9.config对象
config对象是在一个Servlet初始化时，JSP引擎向它传递信息用的，此信息包括Servlet初始化时所要用到的参数（通过属性名和属性值构成）以及服务器的有关信息（通过传递一个ServletContext对象）

![image-20220512201512013](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512201512013.png)

#### 子类和父类

子类不能继承父类的构造方法，但是能调用父类的构造方法

子类能继承父类的private的成员，不能调用父类的private成员

![image-20220512201559922](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512201559922.png)

#### 斐波那契数列

​		当两级台阶时，可以走一步，也可以一次走两步，有两种走法。 当三级台阶时，可以走一步，走三次。可以先走一步，再两步。也可以，先两步，再一步。总共三种方法。 可以看到当台阶是3时，第1级台阶选中之后(一种)，剩下可以走的方式恰好是台阶为2时的总数，总的数量为前两个之和，所以是f(3)=f(1)+f(2)。即，f(n)=f(n-2)+f(n-1)，总结起来刚好是一个斐波那契数列。

![image-20220512202549516](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512202549516.png)

#### final

final修饰变量，则等同于常量

![image-20220512202727139](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512202727139.png)

#### 类型为Final的类

String，StringBuilder，StringBuffer都是final修饰的，HashMap和Hashtable不是final修饰的

![image-20220512203016996](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512203016996.png)

#### Cookie和Session

​	1.session用来表示用户会话，session对象在服务端维护，一般tomcat设定session生命周期为30分钟，超时将失效，也可以主动设置无效； 

​	2.cookie存放在客户端，可以分为内存cookie和磁盘cookie。内存cookie在浏览器关闭后消失，磁盘cookie超时后消失。当浏览器发送请求时，将自动发送对应cookie信息，前提是请求url满足cookie路径；

​	 3.可以将sessionId存放在cookie中，也可以通过重写url将sessionId拼接在url。因此可以查看浏览器cookie或地址栏url看到sessionId； 

​	4.请求到服务端时，将根据请求中的sessionId查找session，如果可以获取到则返回，否则返回null或者返回新构建的session，老的session依旧存在，请参考API。

![image-20220512204214818](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512204214818.png)

#### Java并发同步器

​	A，Java 并发库 的Semaphore 可以很轻松完成信号量控制，Semaphore可以控制某个资源可被同时访问的个数，通过 acquire() 获取一个许可，如果没有就等待，而 release() 释放一个许可。
 	B，CyclicBarrier 主要的方法就是一个：await()。await() 方法没被调用一次，计数便会减少1，并阻塞住当前线程。当计数减至0时，阻塞解除，所有在此 CyclicBarrier 上面阻塞的线程开始运行。
​	 C，直译过来就是倒计数(CountDown)门闩(Latch)。倒计数不用说，门闩的意思顾名思义就是阻止前进。在这里就是指 CountDownLatch.await() 方法在倒计数为0之前会阻塞当前线程。
​	 D，Counter不是并发编程的同步器

![image-20220512210833198](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220512210833198.png)

#### 将文件数据显示到控制台的步骤

​		这是输入输出流的一个概念，我们要将文件里的内容输出到控制台，事实上是分为两步。第一将文件内容读入内存（in），第二步将内容从内存里写出到控制台。这道题的答案就是第一步的内容，文件输入流。

![image-20220513163705938](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220513163705938.png)

#### 接口的默认修饰符

1、接口的修饰符只能为(且默认为) public abstract。

2、接口的变量的修饰符只能为(且默认为) public static final。

![image-20220513164605855](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220513164605855.png)

#### java.exe的运行条件、J2SDK、Appletviewer.exe

A：正确,main方法是入口 ,有main方法才能运行java.exe

B：J2SDK不仅仅包含java API 

C：jar利用java.exe 选项 运行,jar文件

D：Appletviewer是运行applet的， applet 不用main方法，继承applet类即可。

#### JDBC的设计模式

​	桥接模式是结构型模式，关注点在依赖关系的维护。对于jdbc技术来说，它解耦了业务与数据库通信协议这两个纬度之间的关系，所以这两个纬度之间的关系就需要一个桥，即Driver，至于DriverManager把这个关系接到哪里就是运行时的事情了。
微观上，从connection的创建来看，它更像一个抽象工厂模式，特定的Driver创建对应的connection。
宏观上，从业务代码与connection的关系来看，关键点在于一个sql怎么转化为对应的通信协议，就属于桥接。

![image-20220513170654718](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220513170654718.png)

#### java数组的声明

在java 中，声明一个数组时，不能直接限定数组长度，只有在创建实例化对象时，才能对给定数组长度.。

如下，1，2,3可以通过编译，4，5不行。而String是Object的子类，所以上述BCF均可定义一个存放50个String类型对象的数组。

1. String a[]=new String[50];

2. String b[];

3. char c[];

4. String d[50];

5. char e[50];

![image-20220513170938923](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220513170938923.png)

#### JDK1.7的接口和抽象类

B:抽象类和接口都可以有静态成员

D：JDK1.7版本即之前版本的接口的方法不能有方法体，所以方法必须是抽象的。JDK1.8之后的版本，接口的方法default修饰（默认）的方法可以有方法体

![image-20220514163137427](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514163137427.png)

#### Servlet的层级结构

![image-20220514163655428](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514163655428.png)

![image-20220514163626735](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514163626735.png)

#### 子类访问父类成员的方法

1.super可以访问父类中public、default、protected修饰的成员变量，不能访问private修饰的成员变量。格式为super.成员名称。

2.super可以访问父类中public、default、protected修饰的实例方法，不能访问private修饰的实例方法。格式为super.实例方法。

3.super可以访问父类中public、default、protected修饰的构造方法，不能访问private修饰的构造方法，格式为super(参数).

**且父类中只有有参构造函数，则子类构造函数必须调用**

![image-20220514164212669](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514164212669.png)

#### web服务器和虚拟服务器

```
LVS ： 是linux虚拟服务器  负载均衡
Nginx : 代理服务器  web服务器
Lighttpd：web服务软件
Apache：是一个web 服务器
```

![image-20220514164550771](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514164550771.png)

#### 基本数据类型

String是一个类，但也不是包装类，更不是基本数据类型

![image-20220514164611010](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514164611010.png)

#### 导致线程中断或停止运行的方式

​	一、InterruptedException异常被捕获； 

  二、线程调用了wait方法。 

  以下方法不会中断线程或停止运行： 

  当前线程创建了一个新的线程（主线程中的子线程在运行，则主线程可能也在持续运行啊，如果关了主线程那子线程也无法继续执行了吧）； 

  高优先级线程进入就绪状态（高优先级只是在分配时间片的时候有优先级吧，当线程已经在执行时，高优先级的也不能挤占啊）。

![image-20220514164958077](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514164958077.png)

#### 抽象类和接口

抽象类有构造方法但是不能实例化，抽象类的构造方法是给子类实例化或调用抽象父类成员用的。

![image-20220514165350754](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220514165350754.png)

#### 子类一定为抽象类的情况

D:一个类实现接口时，如果不全部实现接口的抽象方法，则该类一定为抽象类

![image-20220515215506904](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220515215506904.png)

#### has-a和is-a关系

has-a一般指类中的成员和类的从属关系，一个属性或方法属于一个类，可以用类has-a属性或方法表示

is-a表示类与类的继承关系，可以用：子类is-a父类来表示。

Widget has-a Sprocket ，表示Widget类有一个Sprocket属性，因为Widget是父类，所以Sprocket属性必须是Widget的属性，因为Gadget是子类，没必要声明Sprocket属性。父类有的子类一定有，所以选A、C

![image-20220515215630170](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220515215630170.png)

#### java concurrent包下的类

A、Semaphore：类，控制某个资源可被同时访问的个数;

B、ReentrantLock：类，具有与使用synchronized方法和语句所访问的隐式监视器锁相同的一些基本行为和语义，但功能更强大；

C、 Future：接口，表示异步计算的结果；

D、 CountDownLatch： 类，可以用来在一个线程中等待多个线程完成任务的类。

![image-20220515220156880](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220515220156880.png)

#### 类的修饰符

这题只说定义一个类，但是没有说这个类是普通外部类或者内部类。

因为**普通类也就是外部类，**只能用 **public, abstract 和 final 修饰。**

**内部类**则可以用 **修饰成员变量的修饰符修饰内部类，**比如 **public** **private, static,** **protected 修饰。**

![image-20220515220418745](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220515220418745.png)

#### 程序文件名和公共类名

公共类名T1和文件名T1要保持一致

![image-20220515220646788](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220515220646788.png)

![image-20220515220537229](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220515220537229.png)

#### String拼串和常量池

```java
public static void main(String[] args) {
    String a = new String("myString");
    String b = "myString";
    String c = "my" + "String";
    String d = c;
    String e = "my";
    String f = e+"String";
    System.out.print(a == b);
    System.out.print(a == c);
    System.out.print(b == c);//c字符串是常量"my"+"String",经过java优化，b和c都指向常量池的同一个地址
    System.out.print(b == f);//f字符串是变量e+常量"String"，java优化不会进行优化
}
```

所以下图b==d为true

![image-20220515221510897](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220515221510897.png)
