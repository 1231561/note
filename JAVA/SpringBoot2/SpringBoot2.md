# SpringBoot2

### SpringBoot2的环境要求

1、Java8及以上版本

SpringBoot2的内部源码设计基于Java8的新特性，如接口的方法使用默认实现，使得实现接口的类可以根据需求，实现部分方法即可。

适配器:Java8之前使用的中间类，空实现接口的所有抽象方法，让子类继承并重写。

2、Maven3.3及以上版本

#### 为什么要用SpringBoot

能快速创建生产级别的Spring应用，不用将精力花费在配置配置文件上。

#### SpringBoot的优点

1、可以创建独立的Spring应用

2、内嵌web服务器，不需要独立的使用其他服务器部署web工程

3、自动starter依赖，比如 一个web工程所需的所有依赖，都在starter依赖里。

4、无需编写xml配置文件，相关配置已被配置好了。

### SpringBoot的准备工作

#### HelloSpringBoot

当浏览器发送/hello请求，服务器就会响应HelloSpringBoot。

步骤：

**1、创建maven工程**

**2、在maven工程的config-settings文件加上阿里云镜像和jdk版本**

阿里云镜像：可以加快导入依赖的速度

```xml
<mirrors>
	<mirror>
  	<id>aliyunmaven</id>
  	<mirrorOf>*</mirrorOf>
  	<name>阿里云公共仓库</name>
 	 <url>https://maven.aliyun.com/repository/public</url>
	</mirror>
</mirrors>
```

jdk1.8：

```xml
<profile>
	<id>jdk-1.8</id>
	<activation>
		<activeByDefault>true</activeByDefault>
		<jdk>1.8</jdk>
	</activation>
	<properties>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
	</properties>
</profile>
```

在jar包target文件目录执行cmd，安装阿里云公共仓库：mvn install

![image-20220329150815718](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329150815718.png)

![image-20220329150824627](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329150824627.png)

安装成功：

![image-20220329150920494](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329150920494.png)

**3、在maven引入依赖**

引入SpringBoot的父项目

引入web的starter依赖，里面包含许多SpringMVC的必要组件，如ViewResolver，HttpMessageConverter等

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
</parent>
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```

**4、创建主程序**

因为SpringBoot内嵌服务器，所以主程序的运行就自动部署服务器，只需登录浏览器访问地址。

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
/**
 * 主程序类
 * @SpringBootApplication：该注解Boot应用，表示这是一个Spring标记的类上是主程序类
 */
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class,args);
    }
}
```

**5、编写响应HelloSpingboot的业务**

创建控制器类，编写业务

```java
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
//@RestController=@ResponseBody+@Controller，实现业务类和返回请求体类的注解功能
@RestController
public class HelloControllerTest {
    @RequestMapping("/hello")
    public String hello(){
        return "hello,springboot";//返回的"hello,springboot"就是请求体
    }
}
```

**6、测试**

点击主程序的运行将项目部署到服务器上，默认部署到8080端口：

![image-20220328143901956](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328143901956.png)

访问http://localhost:8080/hello：成功

![image-20220328143952896](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328143952896.png)

**7、简化配置**

在resource资源目录下，编写配置文件，可以配置服务器的端口号等。

![image-20220328144919638](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328144919638.png)

下面是查阅各种配置的官方文档:

https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties)

**8、简化部署**

​		SpringBoot可以使用插件，将项目打包成jar包，存放在target目录下，然后在target目录启动Windows 命令提示符cmd，然后直接启动jar包部署工程到服务器。

首先在maven导入将项目打包成jar包的插件：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

将项目打包成jar包后，在target目录运行cmd，启动jar包，部署工程。

![image-20220328145944113](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328145944113.png)

### SpringBoot依赖管理特性

maven中，spring-boot-starter-parent作为依赖的父项目，做依赖管理。

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
</parent>
```

spring-boot-starter-parent的父项目是：spring-boot-dependencies

```xml
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>2.3.4.RELEASE</version>
</parent>
```

​		spring-boot-dependencies帮我们声明了几乎所有开发场景所需要用到的依赖，及版本号。所以maven子项目导入依赖时，可以不用填写版本号，及默认使用spring-boot-dependencies声明的版本号，这叫做版本仲裁（不写版本号的以来引入也不会报红）。

![image-20220328153715397](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328153715397.png)

​		当我们想修改默认引入的依赖版本号，如mysql的版本号：

就通过spring-boot-dependencies的properties标签修改

```xml
<properties>
<mysql.version>8.0.16</mysql.version>
</properties>
```

#### starter场景启动器

​	如上图的spring-boot-starter-web就是web场景启动器，web开发场景所需要的所有依赖都可以引入spring-boot-starter-web来全部导入。

​	spring-boot-starter-*，就是表示某个开发场景的场景启动器，来导入该开发场景的所以依赖。

### SpringBoot自动配置原理

####	Tomcat和SpringMVC的自动配置

​		当我们引入spring-boot-starter-web，web的开发场景，spring-boot-starter-web内部就会自动引入spring-boot-starter-tomcat和spring-boot-starter-javamvc等场景，及SpringBoot帮我们自动引入tomcat和springmvc的所有组件，并配置好所有组件的功能。

例如我们在主程序类中查询SpringBoot的IOC容器中的所有组件名（Bean标签或注解生成的对象名），发现存在以配置好的tomcat和springmvc的组件名字，DispatcherServlet和multipartResolver（文件上传）等。

```java
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        //返回一个IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
        //IOC容器中所有被组件标记的对象的名字数组
        String[] beanDefinitionNames = run.getBeanDefinitionNames();
        for (String name:beanDefinitionNames){
            System.out.println(name);
        }
    }
}
```

![image-20220328164906617](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328164906617.png)

#### 默认扫描包的组件配置

​	SpringBoot自动配置了组件扫描，扫描的默认包路径是主程序类的同包下的组件或者主程序类所在包的子包的组件。

如：主程序类MainApplication所在包的子包controller下的组件就会默认被扫描。

![image-20220328165647087](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328165647087.png)

如果想修改扫描组件的包路径：

就使用主程序类的@SpringBootApplication的scanBasePackages属性修改

```java
@SpringBootApplication(scanBasePackages = "com")
```

#### Application配置

application.properties配置文件中，如果没有手动配置服务器端口等配置，将会有默认值，

不管是默认值还是手动的配置，这些配置都会映射到SpringBoot的一个类，并生成配置对象。

###  @Configuration注解

​	@Configuration注解的作用是告诉SpringBoot被@Configuration注解标记的类是配置类，该类的作用就像Springxml一样，可将组件（Bean实例对象）添加到IOC容器。

@Configuration注解有个proxyBeanMethods（代理对象方法）属性，proxyBeanMethods为true时（默认为true），配置类拥有代理功能，它可以解决属性的依赖问题。

实例：

有两个实体类user（人），和pet（宠物）,人可以养宠物（user组件依赖于pet组件）

user:

```java
public class User {
    private String name;
    private Pet pet;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Pet getPet() {
        return pet;
    }

    public void setPet(Pet pet) {
        this.pet = pet;
    }

    public User(String name) {
        this.name = name;
    }

    public User() {
    }

    @Override
    public String toString() {
        return "User{" +
                "name='" + name + '\'' +
                ", pet=" + pet +
                '}';
    }
}
```

pet:

```java
public class Pet {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Pet(String name) {
        this.name = name;
    }

    public Pet() {
    }

    @Override
    public String toString() {
        return "Pet{" +
                "name='" + name + '\'' +
                '}';
    }
}
```

配置类：

user组件依赖于pet组件

```java
@Configuration(proxyBeanMethods = true)
public class MyConfig {
    @Bean//将给IOC容器注入名字为 user01，类型为User，实例为user的组件
    public User user01(){
        User user = new User("张三");
        user.setPet(pet01());
        return user;
    }
    @Bean
    public Pet pet01(){
        return new Pet("旺财");
    }
}
```

主查询测试：

@Configuration(proxyBeanMethods = true)表示执行代理功能，false表示不执行代理功能，如果proxyBeanMethods = true代理类对象调用方法会优先到IOC容器中找到是否有该方法的返回的组件，如果有就直接用IOC容器获取，如果没有就新建一个组件

```java
@SpringBootApplication()
public class MainApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
        Object pet1 = run.getBean("pet01");
        Object pet2 = run.getBean("pet01");
        //配置类里的@Bean标记的方法给IOC容器添加的组件默认是单例的。
        System.out.println(pet1==pet2);//true
        //配置类自然也是IOC容器里的组件，而且它还是代理组件。
        MyConfig bean = run.getBean(MyConfig.class);
        System.out.println(bean);
        //因为pet组件已存在于IOC容器中，所以bean.user01().getPet()的pet组件直接从IOC中取出。
        Pet pet3 = bean.user01().getPet();
        Pet pet4 = bean.pet01();
        System.out.println(pet3==pet4);//true
    }
}
```

运行结果：

![image-20220328190918089](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328190918089.png)

#### Full模式与Lite模式

当配置类组件间没有依赖关系，就使用Lite模式，及@Configuration(proxyBeanMethods = false)

当配置类组件间有依赖关系，就用Full模式，及@Configuration(proxyBeanMethods =	true)

### @Import

@Import注解只能标注类，作用是给IOC容器创建组件。

例如：

@Import({Pet.class,User.class})，给IOC容器创建Pet和User类组件，组件类型是全类名。



```java
@Import({Pet.class,User.class})
@Configuration(proxyBeanMethods = true)
public class MyConfig {
    @Bean
    public Pet pet01(){
        return new Pet("旺财");
    }
    @Bean
    public User user01(){
        User user = new User("张三");
        user.setPet(pet01());
        return user;
    }
}
```

![image-20220328210254053](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328210254053.png)

### @Conditional

@Conditional注解是条件装配，@Conditional注解可以标记在方法或者类上，满足@Conditional的条件的方法或者类的其他注解才能生效，不满足则不生效。

例如：

关于Bean的条件装配

@ConditionalOnBean（name=xxx）：如果IOC容器存在名字为xxx的组件，则以下的@Bean就能给IOC注入组件。

@ConditionalOnMissingBean（name=xxx）：如果IOC容器不存在名字为xxx的组件，则以下的@Bean就能给IOC注入组件。

**注意：@Bean给IOC容器注入组件的顺序和@Bean标记方法的定义顺序有关，所以组件pet01先被注入IOC容器**

```java
@Configuration
public class MyConfig {
    @Bean
    public Pet pet01(){
        return new Pet("旺财");
    }
    @ConditionalOnBean(name = "pet01")
    @Bean
    public User user01(){
        User user = new User("张三");
        user.setPet(pet01());
        return user;
    }
}
```

测试：

结果为true，说明组件pet01存在于IOC容器中，所以user01能被@Bean注入IOC容器

```java
System.out.println(run.containsBean("user01"));
```

### **@ImportResource** 

@ImportResource的作用是导入原生配置文件（Spring配置文件）并解析。

例如：

创建一个Spring配置文件Bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="user" class="com.boot.bean.User">
            <property name="name" value="张三"></property>
        </bean>
        <bean id="pet" class="com.boot.bean.Pet">
            <property name="name" value="旺财"></property>
        </bean>
</beans>
```

如果没有ApplicationContext解析器解析配置文件，则Spring配置文件里的Bean不能将组件注入IOC容器。

配置类加上@ImportResource，指定Spring配置文件的路径

```java
@Configuration
@ImportResource("classpath:Bean.xml")
public class MyConfig {
    @Bean
    public Pet pet01(){
        return new Pet("旺财");
    }
    @ConditionalOnMissingBean(name = "pet01")
    @Bean
    public User user01(){
        User user = new User("张三");
        user.setPet(pet01());
        return user;
    }
}
```

测试结果：使用Spring配置文件创建的组件成功被注入IOC容器

![image-20220328213527587](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328213527587.png)

### @ConfigurationProperties

​		@ConfigurationProperties的作用是配置绑定，读取application.properties配置文件，可以获取精确的配置信息，注入标记的实体类属性，即将application.properties配置文件和实体类配置文件绑定。

应用场景：

application.properties配置文件中有大量的以键值对存在的配置信息，其中只有几条是关于数据库连接所需的数据，则可以通过@ConfigurationProperties精确地获取这几条数据并注入实体类属性，用于数据库连接池的配置。

**@ConfigurationProperties只能作用于存在于IOC容器里面的组件（对象），所以要配合@Component或@EnableConfigurationProperties使用**

实例：

application.properties配置文件有汽车的名字和价格信息。

```properties
car.brand=BMW
car.price=1000000
```

方式一：@ConfigurationProperties+@Component

@Component的作用是将实体类对象（组件）交给IOC,且默认将类的名字第一个字母小写，作为组件名字。

@ConfigurationProperties(prefix = "car")：prefix参数表示application.properties配置文件中前缀为car的key，然后**根据后缀名跟Car的brand，price的set方法绑定，并注入值**

```java
@Component
@ConfigurationProperties(prefix = "car")
public class Car {
    private String brand;
    private int price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public Car(String brand, int price) {
        this.brand = brand;
        this.price = price;
    }

    public Car() {
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}

```

测试:

```java
@SpringBootApplication()
public class MainApplication {
    public static void main(String[] args) {
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
        Car bean = run.getBean(Car.class);
        System.out.println("车品牌："+bean.getBrand()+"车价格："+bean.getPrice());

    }
}
```

结果：

![image-20220328224232588](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220328224232588.png)

方式二：@ConfigurationProperties+@EnableConfigurationProperties(Car.class)

@EnableConfigurationProperties(Car.class)的作用是

1、开启Car实体类和application.properties的绑定

2、将Car实体类的对象（组件）交给IOC容器，组件的名字是类的全路径名。

配置类：

```java
@Configuration
@EnableConfigurationProperties(Car.class)
public class MyConfig {

}
```

实体类：

```java
@ConfigurationProperties(prefix = "car")
public class Car {
    private String brand;
    private int price;

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }

    public Car(String brand, int price) {
        this.brand = brand;
        this.price = price;
    }

    public Car() {
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}
```

### @SpringBootApplication

@SpringBootApplication整合了下面三个注解的功能

**@SpringBootConfiguration**

告诉SpringBoot标记的类是配置类，而且是主配置类

**@EnableAutoConfiguration** 

@EnableAutoConfiguration的两个父注解

1、@AutoConfigurationPackage

将主配置类的包下的所有组件给导入进主配置类。

**2、@Import(AutoConfigurationImportSelector.class)** 

获取127个自动装配组件，并将其全部加载到IOC容器中，启动主程序时会全部加载

![image-20220329114604681](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329114604681.png)

**@ComponentScan**(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)

组件扫描，默认扫描主配置类所在包注解以及主配置类所在包子包下的注解

### 自动装配原理

​		虽然@SpringBootApplication在主配置类启动时会加载127个自动配置类，但是只有一些自动配置类能满足条件装配，然后生效，其他不满足条件装配的配置类不生效。

​		条件装配在底层会经常见到，SpringBoot默认会配置好所有组件，然后根据条件装配优先使用用户配置好的组件，没有再使用默认组件。

例如字符格式组件：用户设置优先
![image-20220329120105983](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329120105983.png)

**总结：**

1、SpringBoot会自动加载127个自动配置类，但满足条件装配的自动配置类才能发挥作用。

2、发挥作用的自动配置类会给IOC容器生成很多组件（如文件上传组件、字符格式组件、tomcat端口号），如果用户没在application.properties配置文件或者其他方式修改这些组件，这些组件会有默认值，如tomcat的的默认端口号是8080.

3、自动配置类的底层会有相关的properties，以及设置实体类与application.properties绑定的注解，可以根据这些来在application.properties配置文件修改自动配置类为IOC容器生成的组件（如将字符格式组件设置为UTF-8）。

### LomBok简化开发

LomBok是一个插件，它包含很多注解，可以帮助我们简化开发。

@Getter and @Setter
@FieldNameConstants
@ToString
@EqualsAndHashCode
, @RequiredArgsConstructor and @NoArgsConstructor
@Log, @Log4j, @Log4j2, @Slf4j, @XSlf4j, @CommonsLog, @JBossLog, @Flogger, @CustomLog
@Data
@Builder
@SuperBuilder
@Singular
@Delegate
@Value
@Accessors
@Wither
@With
@SneakyThrows
@val
@var
experimental @var
@UtilityClass
Lombok config system
Code inspections
Refactoring actions (lombok and delombok)

如@Getter and @Setter可以帮助我们自动生成实体类的get和set方法。

@Data：生成get、set、无参构造方法、toString方法

@AllArgsConstructor：全参构造方法

@NoArgsConstructor：无参构造方法

@Slf4j：快速注入日志功能

### Spring Initializr创建SpringBoot项目

使用Spring Initializr创建一个项目

![image-20220329143620116](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329143620116.png)

选择所需的依赖性

![image-20220329143940346](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329143940346.png)

创建完成：

​		1、自动帮我们生成主配置类和全局配置文件application.properties，并且帮我们创建static包和templates包。

​		2、自动引入我们刚才选择的依赖项

![image-20220329151157846](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329151157846.png)

![image-20220329151738541](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329151738541.png)

## SpringBoot2核心技术

### 配置文件

SpringBoot配置文件有两种类型

1.properties

之前用的

#### 2、yaml

1、基本语法：key： value

**注意，键和值之间要有空格**，**而且键不能是中文**

2、使用缩进表示层级关系

3、字符串元素无需加引号

#### 数据类型

字面量：单个的、不可再分的值。date、boolean、string、number、null 

```yaml
1 k: v
```

自定义对象和键值对集合（map、hash、object等）

```yaml
1 行内写法： k: {k1:v1,k2:v2,k3:v3} #或 
3 k: 
4 	k1: v1 
5 	k2: v2 
6 	k3: v3
```

数组：普通数组或array、list、queue、set等

```yaml
1 行内写法： k: [v1,v2,v3] 
2 #或者 
3 k: 
4 	- v1 
5 	- v2 
6 	- v3
```

实例：

Person实体类：

```java
@Data
@Component
@ConfigurationProperties(prefix = "person")
public class Person {
    private String userName;
    private Boolean boss;
    private Date birth;
    private Integer age;
    private Pet pet;
    private String[] interests;
    private List<String> animal;
    private Map<String, Object> score;
    private Set<Integer> salarys;
    private Map<String, List<Pet>> allPets;
}
```

Pet实体类：

```java
@Data
public class Pet {
    private String name;
    private Double weight;
}
```

application.yml：

```yaml
person:
  userName: 张三
  boss: true
  birth: 2000/5/31
  age: 18
  pet:
    name: 旺财
    weight: 50
  interests: [篮球,足球]
  animal: [猫,狗]
  score:
    english: 99
    math: 98
  salary: [123456]
  allPets:
    cat: [{name: 加菲猫,weight: 16}]
    dog: [{name: 柴犬,weight: 25}]
```

控制器方法：

```java
@RestController
public class Controller {
    @Autowired
    Person person;
    @RequestMapping("/person")
    public Person test(){
        return person;
    }
}
```

结果：

![image-20220329183347491](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329183347491.png)

## web开发

### 静态资源

在Spring中，如果静态资源放在以下目录下，即可根据以下路径直接访问资源.

**访问方式：http://localhost:8080/静态资源名**

1、/static

2、/public

3、/resources

4、/META-INF/resources

**静态资源映射：/**，即工程下所有资源**

​		**原理：当请求访问（http://localhost:8080/静态资源名） 时，DispartcherServlet会先去控制器方法查找有没有对应的请求映射，如果没有，则交给defaultServlet查找工程下的所有资源，如果以上目录下有该静态资源，则将静态资源响应给浏览器，没有则404.**



如图所示：

![image-20220329223904019](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329223904019.png)

![image-20220329223924485](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220329223924485.png)

#### 给静态资源路径加前缀

为了更好的区分不同的静态资源的访问，我们可以给静态资源访问路径加上前缀，相当于部署Tomcat时的上下文路径。

这样请求访问静态资源的地址变为：http://localhost:8080/res/静态资源名

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```

#### webjars

webjars是将Ajax和Jqury等静态资源打包成jar包，我们引入相关依赖就可以得到jar包。

例如，引入jquey依赖，就可以得到juery的jar包。

![image-20220330105234093](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220330105234093.png)

访问：http://localhost:8080/webjars/jquery/3.6.0/dist/jquery.js即可访问jquey静态资源。

#### 欢迎页面index.html

​		当我们将index.html放入静态资源目录下，则可直接通过访问根目录：http://localhost:8080访问index.html页面，如果给静态资源的访问路径（默认/**)加了前缀，则将访问不到ndex.html页面。

#### 禁用所有静态资源规则

在application.yml配置文件下配置以下语句,即可屏蔽掉所有访问静态资源的请求。

```yaml
resources: 
add-mappings: false
```

### Rest映射

Rest映射：表现层状态转移

作用：通过相同的请求地址，不同的请求方式来实现不同的功能。

例如：请求/user，请求方式为get、post、put、delete分别映射到通过id查询用户、添加用户、查询所有用户，删除用户的方法。

#### Rest映射的底层原理

当我们提交的表单带有隐藏域，且给隐藏域设置属性：name="_method",value="PUT"，mthod=POST后。

请求会被HiddenHttpMethodFilter拦截 。

​		1、判断表单的method是否为POST

​		2、如果是，将获取_method的值为PUT（PUT为合法的请求方式）。

​		3、将POST替换为PUT。

替换请求方式后，就找到对应的请求映射方法，执行方法。

**注意：HiddenHttpMethodFilter拦截只对POST请求有作用。** 

如果使用客户端方式（如PostMan）发送直接发送PUT请求，HiddenHttpMethodFilter不会处理这个（非POST）请求。

**SpringBoot实例：**
**控制器：**

```java
@RestController
public class Controller {
    @GetMapping("/user")
    public String getUser(){
        return "GET-张三";
    }

    @PostMapping("/user")
    public String saveUser(){
        return "POST-张三";
    }

    @PutMapping("/user")
    public String putUser(){
        return "PUT-张三";
    }

    @DeleteMapping("/user")
    public String deleteUser(){
        return "DELETE-张三";
    }
}
```

**表单：**

![image-20220330122814702](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220330122814702.png)

application.yml开启页面表单的Rest功能：

```yml
spring:
  mvc:
    hiddenmethod:
      filter:
        enabled: true
```

### 参数注解的使用

使用参数注解获取汽车信息的请求的参数：

请求地址：

![image-20220330161319887](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220330161319887.png)

```java
@GetMapping("/car/{id}/{name}")
public Map getCarInfo(@PathVariable("id") int id,//@PathVariable获取q
                      @PathVariable("name") String name,
                      @PathVariable Map<String,String> map1,//@PathVariable获取的所有值封装成键值对Map形式，注入map1
                      @RequestHeader("Host") String host,//获取单个请求头信息注入
                      @RequestHeader("Connection") String Connection,
                      @RequestParam("price") int price,//获取？后面的请求参数
                      @RequestParam("age") int age,
                      @RequestParam("interest") List<String> list,//将表单复选的值封装到List中，并注入list
                       @RequestParam MultiValueMap<String,String> map3)//MultiValueMap可以让一个key对应几个value，时候接收复选信息){
    HashMap<String, Object> map = new HashMap<>();
    map.put("id",id);
    map.put("name",name);
    map.put("map1",map1);
    map.put("Host",host);
    map.put("Connection",Connection);
    map.put("price",price);
    map.put("age",age);
    map.put("list",list);
    map.put("map3",map3);
    return map;
}
```

响应体：

![image-20220330162700588](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220330162700588.png)

### @RequestAttribute获取请求域的参数

实例：

```java
@Controller
public class RequestController {
    @GetMapping("goto")
    public String goTo(HttpServletRequest request){
        request.setAttribute("name","张三");
        return "forward:/target";//请求转发到/target
    }
    //有两种获取请求域参数的方法，1、@RequestAttribute 2、通过HttpServletRequest获取（因为请求转发是同一个请求域）
    @ResponseBody
    @GetMapping("/target")
    public Map target(@RequestAttribute("name") String name,HttpServletRequest request){
        HashMap<String, Object> map = new HashMap<>();
        Object name1 = request.getAttribute("name");
        map.put("name",name);
        map.put("name1",name1);
        return map;
    }
}
```

结果：@RequestAttribute获得的name和request获得的name一样。
![image-20220330165015794](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220330165015794.png)

### 矩阵变量

当Cookie被禁用，想要获取session时，可以使用矩阵变量的方式。

矩阵变量在请求地址的格式：

/类型名；属性1=属性值1；属性2=属性值2

例如:/abc:jsessionid=xxx 把Cookie的值使用矩阵变量来传递。

步骤：

1、重新configurePathMatch方法，将变量的;后面的内容保留

```java
@Configuration(proxyBeanMethods = false)
public class MyConfig implements WebMvcConfigurer {

        @Bean
        public WebMvcConfigurer webMvcConfigurer(){
            return new WebMvcConfigurer() {
                @Override
                public void configurePathMatch(PathMatchConfigurer configurer) {
                    UrlPathHelper urlPathHelper = new UrlPathHelper();
                    // 不移除；后面的内容。矩阵变量功能就可以生效
                    urlPathHelper.setRemoveSemicolonContent(false);
                    configurer.setUrlPathHelper(urlPathHelper);
                }
            };
        }

}
```

测试：

```java
@ResponseBody
@GetMapping("/pet/{petKind}")
public Map pet(@MatrixVariable("catName") String name,
               @MatrixVariable("age") Integer age,
               @PathVariable("petKind") String kind){
    HashMap<String, Object> map= new HashMap<>();
    map.put("catName",name);
    map.put("age",age);
    map.put("kind",kind);
    return map;
}
```

结果：

![image-20220330185114395](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220330185114395.png)

### Thymeleaf

#### Thymeleaf初体验

在SpringBoot中，我们只需要导入Thymeleaf的依赖。

Thymeleaf的视图解析器已经被自动配置好了。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

控制器方法：设置请求映射，将数据放到请求域中，返回视图名称。

```java
@org.springframework.stereotype.Controller
public class Controller {
    @RequestMapping("/target")
    //Model的功能和Request一样，将将数据以键值对的形式共享到请求域
    public String getTarget(Model model){
        model.addAttribute("head","SpringBoot你好");
        model.addAttribute("link", "https://www.baidu.com");
        return "target";
    }
}
```

html页面：

th是Thymeleaf提供的设置属性值的标签，需要改哪个属性，就在前面加th:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${head}">目标页面</h1><br>
<a href="www.hao123.com" th:href="${link}">百度</a>
<a href="www.hao123.com" th:href="@{link}">百度</a>
</body>
</html>
```

测试结果：

html页面的属性被成功改变。

![image-20220330213655512](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220330213655512.png)

### 抽取公共部分

抽取公共部分：

@{/js/jquery-1.10.2.min.js}设置绝对路径。

[[session.msg]]==th:text="${session.msg}"

方式一：使用div标签设置id

```html
<div id="commonscript">
<!-- Placed js at the end of the document so the pages load faster -->
<script th:src="@{/js/jquery-1.10.2.min.js}"></script>
<script th:src="@{/js/jquery-ui-1.9.2.custom.min.js}"></script>
<script th:src="@{/js/jquery-migrate-1.2.1.min.js}"></script>
<script th:src="@{/js/bootstrap.min.js}"></script>
<script th:src="@{/js/modernizr.min.js}"></script>
<script th:src="@{/js/jquery.nicescroll.js}"></script>
</div>
```

方式二：使用thymeleaf的fragment设置抽取部分的名字

```html
<div th:fragment="commonscript">
<!-- Placed js at the end of the document so the pages load faster -->
<script th:src="@{/js/jquery-1.10.2.min.js}"></script>
<script th:src="@{/js/jquery-ui-1.9.2.custom.min.js}"></script>
<script th:src="@{/js/jquery-migrate-1.2.1.min.js}"></script>
<script th:src="@{/js/bootstrap.min.js}"></script>
<script th:src="@{/js/modernizr.min.js}"></script>
<script th:src="@{/js/jquery.nicescroll.js}"></script>
</div>
```

原页面使用公共部分语法：<标签 th:replace(include/insert)="公共部分页面路径 :: 公共部分标签"></标签>

例如：将各个部分使用div标签包围起来，替换原页面的script。

#commonscript为根据id获取公共部分，不加#表示根据th:fragment获取公共部分。

```html
<div th:include="table/common :: #commonscript"></div>>
```

#### 遍历

\<th:each="遍历实体:${数组}">

例如:

```html
<tr class="gradeX" th:each="user,stats:${users}">
    <td th:text="${stats.count}">Trident</td>
    <td class="center hidden-phone" th:text="${user.userName}">4</td>
    <td class="center hidden-phone" th:text="${user.password}">X</td>
</tr>
```

### 拦截器

自定义拦截器要实现HandlerInterceptor接口，重新写preHandle，postHandle，afterCompletion

**管理登录的控制器：**

```java
@Controller
@Slf4j
public class IndexController {
    @RequestMapping(value = {"/","/login"})
    public String getLogin(){
        return "login";
    }
    @PostMapping("/login")
    public String testLogin( HttpSession session, User user, Model model){
        //如果用户名不为空，且密码等于123456，则将user对象保存到 session域中
        if (user.getUserName()!=null&&"123456".equals(user.getPassword())){
           session.setAttribute("user",user);
            return "redirect:main.html";
        }else {
            model.addAttribute("scm","账号或密码错误");
            return "login";
        }

    }
    @GetMapping("/main.html")
    public String testMain(){
        log.info("控制器的进入main页面的方法");
        return "main";
    }
}
```

**配置一个管理登录拦截器：**

```java
@Component
@Slf4j
public class LoginInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        log.info("preHandle");//使用日志形式输出，相当于printf
        HttpSession session = request.getSession();
        Object user = session.getAttribute("user");
        if (user != null) {//如果登陆成功，session域中会有登录的用户信息，preHandle放行
            return true;//放行
        } else{
            request.setAttribute("msc","请登陆后再访问");
            request.getRequestDispatcher("/login").forward(request,response);
            return false;//如果没有user信息，则没登录过，将拦截
        }
    }
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("postHandle");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        log.info("afterCompletion");
    }

}
```

**创建配置类配置拦截器**

配置类想要配置拦截器，需要实现WebMvcConfigurer接口，重写 addInterceptors方法。

```java
@Configuration
public class AdminConfig implements WebMvcConfigurer {
    @Autowired
    LoginInterceptor loginInterceptor;//自动装配拦截器
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(loginInterceptor)//添加自定义拦截器
                .addPathPatterns("/**")//拦截路径为所有路径
              .excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**");//设置放行登录页面，和所有静态资源

    }
}
```

测试：

除了登录页面和静态资源不被拦截处理，请求访问其他页面都被拦截处理，如果登录成功之后访问其他页面，拦截处理后放行。

且拦截器和控制器方法的执行顺序是：preHandle——》控制器方法——》postHandle——》afterCompletion

![image-20220331165119424](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331165119424.png)

如果除了登录页面其他资源都拦截拦截：

发现设置addPathPatterns("/**")，静态资源也会被拦截

```java
log.info(request.getRequestURI());//preHandle输出被拦截的所有请求
```

![](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331165936720.png)

### 文件上传

**上传文件表单：**

```html
<div class="row">
    <div class="col-lg-6">
        <section class="panel">
            <header class="panel-heading">
                Basic Forms
            </header>
            <div class="panel-body">
                <form role="form" action="/upLoad" method="post" enctype="multipart/form-data">
                    <div class="form-group">
                        <label for="exampleInputEmail1">邮箱</label>
                        <input type="email" name="email" class="form-control" id="exampleInputEmail1" placeholder="Enter email">
                    </div>
                    <div class="form-group">
                        <label for="exampleInputPassword1">名字</label>
                        <input type="text" name="userName" class="form-control" id="exampleInputPassword1" placeholder="Password">
                    </div>
                    <div class="form-group">
                        <label for="exampleInputFile">头像</label>
                        <input type="file" id="exampleInputFile" name="headerImg">
                        <p class="help-block">Example block-level help text here.</p>
                    </div>
                    <div class="form-group">
                        <label for="exampleInputFile">照片</label>
                        <input type="file" name="photos" multiple >
                        <p class="help-block">Example block-level help text here.</p>
                    </div>
                    <div class="checkbox">
                        <label>
                            <input type="checkbox"> Check me out
                        </label>
                    </div>
                    <button type="submit" class="btn btn-primary">Submit</button>
                </form>
```

**处理上传文件请求的控制器方法：**

```java
@Controller
public class UploadController {//跳转到上传表单页面
    @RequestMapping("/form_layouts.html")
    public String upLoad(){
        return "table/form_layouts";
    }
    //处理文件上传
    @PostMapping("/upLoad")
    public String getUpLoad( @RequestParam("userName")String userName,
                             @RequestParam("email")String email,
                            //获取单张图片
                            @RequestParam("headerImg") MultipartFile headerImg,
                            //获取多张图片
                            @RequestParam("photos") MultipartFile []  photos) throws IOException {
        System.out.println(userName);
        System.out.println(email);
        System.out.println(headerImg.getSize());
        System.out.println(photos.length);
        if (headerImg.getSize()>0){
            //获取上传文件完整文件名
            String name = headerImg.getOriginalFilename();
            headerImg.transferTo(new File("D://"+name));//上传文件存放位置磁盘加完整文件名
        }
        if (photos.length>0){
            for (MultipartFile photo : photos) {
                String name = photo.getOriginalFilename();
                photo.transferTo(new File("D://"+name));
            }
        }
        return "main";
    }
```

默认上传的文件大小为1MB，不够用，所以我们在application.yml里将文件上传大小设置为10MB

```yaml
spring:
  servlet:
    multipart:
      max-file-size: 10MB
```

上传测试：

![image-20220331212835114](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331212835114.png)

成功：

![image-20220331212908043](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331212908043.png)

### 异常处理

默认情况下，SpringBoot处理错误时，会到工程目录寻找templates目录下的error目录,并将error目录下的4xx.html和5xx.html作为404错误和500错误的跳转页面。

如工程存在error目录，里面有4xx.html和5xx.html

![image-20220331215846946](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331215846946.png)

测试500错误：

```java
@GetMapping("/main.html")
public String testMain(){
    int i=10/0;
    log.info("控制器的进入main页面的方法");
    return "main";
}
```

结果:![image-20220331220014915](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331220014915.png)

测试404错误：在地址栏输入一个不存在的资源路径：

![image-20220331220057047](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331220057047.png)

结果：

![image-20220331220111257](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220331220111257.png)

### 原生Servlet

当有多个Serlvet可以处理同一个请求，会优先选择最精确的请求处理方式。比如如果原生的Servlet能处理/user这个路径的请求，那就没必要通过DispatcherServlet来处理，就不会被Sprig的拦截器拦截。

### 数据库

在maven导入jdbc的场景，SpringBoot会自动帮我们导入Hikar数据源，和jdbc以及事务，但是数据库驱动要自己导入。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

![image-20220401115058905](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401115058905.png)

SpringBoot自动配置：

1、HikariDataSource连接池资源 ：只要我们不手动向IOC容器注册连接池

2、事务管理器:将数据库操作默认设置为事务

3、JdbcTemplate：一个可以完成CRUD操作的API，SpringBoot还自动的注册了JdbcTemplate的组件，可以直接自动装配使用。

 **我们只需在核心配置文件中配置数据库连接所需要的信息**

**application.yaml**

数据库原的修改都是通过**spring.datasource**这个配置文件绑定来修改。

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test
    username: root
    password: 123456Hh
#    数据库驱动
    driver-class-name: com.mysql.cj.jdbc.Driver
```

**相关源码：**

![image-20220401120824501](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401120824501.png)

测试：

```java
@SpringBootTest
class Boot06ApplicationTests {
    @Autowired
    JdbcTemplate template;//自动装配JdbcTemplate的组件
    @Test
    void contextLoads() {
        String sql="select count(*) from customers";
        Long aLong = template.queryForObject(sql, Long.class);
        System.out.println(aLong);

    }

}
```

### Druid数据源

**druid具有数据库连接池的功能，还有监控功能**

druid官方github地址

https://github.com/alibaba/druid <https://github.com/alibaba/druid> 

#### 使用官方starter方式整合第三方技术

**步骤**

**1、maven下引入druid环境**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.17</version>
</dependency>
```

**2、配置druid的数据库连接池，以及 druid的监控功能。**

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test?serverTimezone=UTC&useUnicode=true&zeroDateTimeBehavior=convertToNull&autoReconnect=true&characterEncoding=utf-8

    username: root
    password: 123456Hh
#    数据库驱动
    driver-class-name: com.mysql.cj.jdbc.Driver

    druid:
    #监控工程目录下的所以资源
      aop-patterns: com.springboot.boot06.*
      #开启stat(sql监控)和wall(防火墙)
      filters: stat,wall
#给访问监控页面设置用户名和密码
      stat-view-servlet:
        enabled: true
        login-username: admin
        login-password: admin
#设置web监控
      web-stat-filter:
        enabled: true
        #监控的请求地址为所有
        urlPattern: /*
        #不监控以下请求地址
        exclusions:  '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'
#设置拦截器的监控属性
      filter:
        stat:
          slow-sql-millis: 1000
          log-slow-sql: true
          enabled: true
        wall:
          enabled: true
          config:
            drop-table-allow: false
```

**3、测试：**

访问druid监控页：

http://localhost:8080/druid/

![image-20220401161423861](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401161423861.png)

### Mybatis

#### application.yaml+Mapper.xml

SpringBoot和Mybatis的sql映射文件配合使用。

**1、maven引入mabatis的环境**

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

SpringBoot帮我们自动配置了

1、SqlSessionFactory

2、SqlSession：是SqlSessionFactory的一个实例

3、Mapper：接口映射，在接口上标记@Mapper，通过组件扫描即可获取mapper实例，相当于sqlSession.getMapper（接口.class）.

**2、创建Bean实体类、Service、控制器方法、映射接口Mapper。**

实体类Order：

```java
import java.util.Date;
public class Order {
    private int orderId;
    public String orderName;
    public Date orderDate;
}
```

映射接口：OrderMapper

@Mapper的作用相当于sqlSession.getMapper（OrderMapper.class），创建出一个mapper实例，mapper.queryOrderById(int orderId)就是查询结果

```java
import com.springboot.boot06.bean.Order;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface OrderMapper {
    public Order queryOrderById(int orderId);
}
```

OrderService

创建查询order信息的业务方法

```java
import com.springboot.boot06.bean.Order;
import com.springboot.boot06.mapper.OrderMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class OrderService {
    @Autowired
    OrderMapper mapper;
    public Order getOrder(int id){
        Order order = mapper.queryOrderById(id);
        return order;
    }
}
```

控制器方法：通过请求传入的id来查询order信息，然后作为响应体相应到浏览器

```java
import com.springboot.boot06.bean.Order;
import com.springboot.boot06.service.OrderService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.ResponseBody;

@org.springframework.stereotype.Controller
public class OrderController {
    @Autowired
    OrderService orderService;
    @ResponseBody
    @GetMapping("/order/{id}")
    public Order getOrderById(@PathVariable("id") int id){
        Order order = orderService.getOrder(id);
        return order;
    }
}
```

**3、配置OrderMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.springboot.boot06.mapper.OrderMapper">
    <select id="queryOrderById" resultType="com.springboot.boot06.bean.Order">
        select * from `order` where order_id = #{orderId}
    </select>
</mapper>
```

**4、通过application.yaml配置mybatis**

mybatis.configuration相当于mabatis的核心配置文件，有了它就不用再编写mabatis的核心配置文件了。

```yaml
mybatis:
#sql映射配置文件的路径
  mapper-locations: classpath:mybatis/mapper/*.xml
  configuration:
  #设置驼峰命名规则，相当于mabatis的核心配置文件的set标签设置
    map-underscore-to-camel-case: true
```

测试:

数据库：

![image-20220401185850233](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401185850233.png)

相应浏览器结果：

![image-20220401185831358](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401185831358.png)

#### 注解+Mapper+application.yaml

简单的sql语句映射可以使用注解的方式，比较复杂的sql语句映射可以使用Mapper.xml配置。

如上面的例子：

根据id查询order可以直接使用注解的方式。效果和在Mapper.xml配置select方法一样。

```java
@Mapper
public interface OrderMapper {
    @Select("select * from `order` where order_id=#{orderId}")
    public Order queryOrderById(int orderId);

}
```

**使用Mapper.xml配置较复杂的insert语句。**

application.yaml配置文件不用变。

中加上接口方法，控制器方法，Mapper.xml配置以及Service方法即可。

**Mapper.xml配置**

以下两个属性设置，将数据库自增的id给返还给实体类对象

 keyProperty="orderId" 

useGeneratedKeys="true"

```xml
<insert id="addOrder" keyProperty="orderId" useGeneratedKeys="true">
    insert into `order`(order_id, order_name, order_date) values (#{orderId},#{orderName},#{orderDate})
</insert>
```

**接口方法：**

```java
@Mapper
public interface OrderMapper {
    @Select("select * from `order` where order_id=#{orderId}")
    public Order queryOrderById(int orderId);
    //添加order
    public int addOrder(Order order);
}
```

**控制器方法：**

```java
@ResponseBody
@GetMapping("/order/{id}/{name}/{date}")
public int addOrder(@PathVariable("id") int id,
                       @PathVariable("name") String name,
                       @PathVariable("date") Date date){
    Order order = new Order(id, name, date);
     return orderService.addOrder(order);

}
```

**Service方法：**

```java
@Service
public class OrderService {
    @Autowired
    OrderMapper mapper;
    public int addOrder(Order order){
        int count = mapper.addOrder(order);
        System.out.println(order);
        return count;
    }
}
```

测试：

![image-20220401214710520](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401214710520.png)

### Junit5单元测试

从SpringBoot2.20版本开始引入JUnit5作为单元测试默认库。

作为最新版本的JUnit框架，JUnit5与之前版本的Junit框架有很大的不同。由三个不同子项目的几个不同模块组成。 

**JUnit 5 = JUnit Platform + JUnit Jupiter + JUnit Vintage** 

**JUnit Platform**: Junit Platform是在JVM上启动测试框架的基础，不仅支持Junit自制的测试引擎，其他测试引擎也都可以接入。 

**JUnit Jupiter**: JUnit Jupiter提供了JUnit5的新的编程模型，是JUnit5新特性的核心。内部 包含了一个**测试引擎**，用于在Junit Platform上 

运行。 

**JUnit Vintage**: 由于JUint已经发展多年，为了照顾老的项目，JUnit Vintage提供了兼容JUnit4.x,Junit3.x的测试引擎。

![image-20220401230831097](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401230831097.png)

#### JUnit5常用注解

**@RepeatedTest :**表示方法可重复执行，下方会有详细介绍 

**@DisplayName :**为测试类或者测试方法设置展示名称 

**@BeforeEach :**表示在每个单元测试之前执行 

**@AfterEach :**表示在每个单元测试之后执行 

**@BeforeAll :**表示在所有单元测试之前执行 

**@AfterAll :**表示在所有单元测试之后执行 

**@Tag :**表示单元测试类别，类似于JUnit4中的@Categories 

**@Disabled :**表示测试类或测试方法不执行，类似于JUnit4中的@Ignore 

**@Timeout :**表示测试方法运行如果超过了指定时间将会返回错误 

**@ExtendWith :**为测试类或测试方法提供扩展类引用 

### 实例

```java
import org.junit.jupiter.api.*;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.jdbc.core.JdbcTemplate;

import java.util.concurrent.TimeUnit;
@SpringBootTest//此注解可以让测试类用于SpringBoot的功能，如自动装配等
@DisplayName("单元测试类")
public class JUnit5Test {
    @Autowired
    JdbcTemplate template;//可以成功自动装配
    @Test
    @DisplayName("单元测试方法1")
    public void test1(){
        System.out.println(1);
    }
    @RepeatedTest(3)//重复三次测试
    @DisplayName("单元测试方法2")
    public void test2(){
        System.out.println(1);
    }
    @BeforeEach
    public void beforeEach(){
        System.out.println("测试准备开始了");
    }
    @AfterEach
    public void afterEach(){
        System.out.println("测试准备结束了");
    }
    @BeforeAll
    static public void beforeAll(){
        System.out.println("所有测试准备开始了");
    }
    @AfterAll
    static public void afterAll(){
        System.out.println("所有测试准备结束了");
    }
    @Timeout(value = 5000,unit = TimeUnit.MILLISECONDS)//设定测试最长时间不能超过6000毫秒
    @Test
    public void testTime(){
        try {
            Thread.sleep(6000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
    @Test
    public void testSpringBoot(){
        System.out.println(template.getClass());
    }
}
```

![image-20220401233449896](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220401233449896.png)

### 断言

​		断言是测试方法的核心部分，Junit5的断言分为简单断言、数组断言、组合断言、异常断言、超时断言、快速失败等。**这些断言方法都是** **org.junit.jupiter.api.Assertions的静态方法**，得导入org.junit.jupiter.api.Assertions.*才能使用这些断言。

断言方法主要测试返回值和断言机制是否相等，如果相等，则通过，如不等则抛出异常，停止运行，并打印测试报告

```java
    @DisplayName("简单断言测试")
    @Test
    void simple(){
        assertEquals(1,2,"业务不相等");
    }
    @DisplayName("简单断言测试")
    @Test
    void same(){
        assertSame(new Object(),new Object(),"对象不相等");
    }

    @DisplayName("数组断言测试")
    @Test
    void Array(){
        assertSame(new int[]{1,2,3},new int[]{1,2,3},"数组不相等");
    }
    @DisplayName("组合断言测试")
    @Test
    void all(){//两个简单断言都通过，组合断言才通过
        assertAll("Math",
                ()->assertEquals(1,1),
                ()->assertTrue(1==1)
        );
    }
    @DisplayName("异常断言测试")
    @Test
    void exception(){
        ArithmeticException exception =assertThrows(
                //断言扔出数学异常，如果真的有数学异常则通过
              ArithmeticException.class,()-> System.out.println(1/0)
        );
        System.out.println(exception);//输出异常信息
    }
    @DisplayName("超时断言测试")
    @Test
    void timeOut(){//断言时间不超过500毫秒
        assertTimeout(Duration.ofMillis(500),()->Thread.sleep(100));
    }
    @DisplayName("快速测试")
    @Test
    void fault(){
        fail("快速失败");
    }
}
```

测试结果：

![image-20220402120122838](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402120122838.png)

### 前置条件

前置条件和断言类似，但是如果断言测试不满足断言方法逻辑，就会运行失败，前置条件测试不满足前置条件方法，则就不会运行，跳过测试。

例如：

```java
    @DisplayName("前置条件测试")
    @Test
    void pre(){
        assumeTrue(1==2);
    }
}
```

![image-20220402120612248](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402120612248.png)

### 嵌套测试

嵌套测试就是一个测试类里面还有测试类

```java
@DisplayName("嵌套测试类")
public  class StackDemoTest {


        Stack<Object> stack;

        @Test
        @DisplayName("is instantiated with new Stack()")
        void isInstantiatedWithNew() {
            new Stack<>();
            assertNull(stack);//外部类的测试不会启动内部类的@BeforeEach和@AfterEach，所以stack为空
        }

        @Nested
        @DisplayName("when new")
        class WhenNew {

            @BeforeEach
            void createNewStack() {
                stack = new Stack<>();
            }

            @Test
            @DisplayName("is empty")
            void isEmpty() {
                assertTrue(stack.isEmpty());
            }

            @Test
            @DisplayName("throws EmptyStackException when popped")
            void throwsExceptionWhenPopped() {
                assertThrows(EmptyStackException.class, stack::pop);
            }

            @Test
            @DisplayName("throws EmptyStackException when peeked")
            void throwsExceptionWhenPeeked() {
                assertThrows(EmptyStackException.class, stack::peek);
            }

            @Nested
            @DisplayName("after pushing an element")
            class AfterPushing {

                String anElement = "an element";

                @BeforeEach
                void pushAnElement() {
                    stack.push(anElement);
                }

                @Test
                @DisplayName("it is no longer empty")
                void isNotEmpty() {
                    assertFalse(stack.isEmpty());//内部部类的测试会自动启动外部类部类和本类的@BeforeEach和@AfterEach，所以外部类的@BeforeEach为其创建里一个stack，本类的@BeforeEach放进去一个元素，所以stack不为空。
                }

                @Test
                @DisplayName("returns the element when popped and is empty")
                void returnElementWhenPopped() {
                    assertEquals(anElement, stack.pop());
                    assertTrue(stack.isEmpty());
                }

                @Test
                @DisplayName("returns the element when peeked but remains not empty")
                void returnElementWhenPeeked() {
                    assertEquals(anElement, stack.peek());
                    assertFalse(stack.isEmpty());
                }
            }
        }
}
```

### 参数化测试

参数化测试是JUnit5的一个新特性,可以一次测试多个参数。配合@ValueSource和@MethodSource使用

```java
@ParameterizedTest
@ValueSource(ints = {1, 2, 3})//指定参数
@DisplayName("参数化测试")
public void paramTest(int i){
    System.out.println(i);
}

@ParameterizedTest
@MethodSource("stream")//指定参数来源方法
@DisplayName("参数化测试2")
public void paramTest2(int i){
    System.out.println(i);
}
static public Stream<String> stream(){
    return Stream.of("A","B","C");
}
```

### 指标监控

​		未来每一个微服务在云上部署以后，我们都需要对其进行监控、追踪、审计、控制等。SpringBoot就抽取了Actuator场景，使得我们每个微服务快速引用即可获得生产级别的应用监控、审计等功能。

**SpringBoot可以采用Actuator+endPoint（端点）的方式来监控指标**

#### 使用方法

**1、引入Actuator场景依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**2、在application.yaml加上相关配置**

将SpringBoot的相关信息暴露在浏览器上

```yaml
management:
  endpoints:
    enabled-by-default: true #暴露所有端点信息
    web:
      exposure:
        include: '*'  #以web方式暴露
```

**3、访问http://localhost:8080/actuator/端点**，**即Actuator+endPoint**

常用端点：

![image-20220402144235135](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402144235135.png)

### 监控指标的关闭和开启

```yaml
management:
  endpoints:	#设置所有端点
    enabled-by-default: false #禁用所有端点
    web:
      exposure:
        include: '*'  #以web方式暴露
  endpoint:	#设置端点的属性
    health:
      show-details: always	#展示health端点详细信息
      enabled: true #开启这个端点
```

### 高级特性-Profile环境切换

​		当我们的SpringBoot工程有多个application核心配置文件，其中一个默认配置文件，其他的是不同环境下的配置文件。根据加载的环境配置文件不同，能达到分工的效果。

​	例如：下面有三个核心配置文件，第一个是默认的，下面两个是生产环境和测试环境下的配置文件，默认配置文件在工程部署时会自动加载，其他的配置文件得由默认配置文件激活才能使用。

**Profile环境切换有两种方式**

1、默认配置文件激活环境

2、生成工程jar包，然后cmd命令激活环境，还可以修改配置环境的属性。

![image-20220402161641798](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402161641798.png)

**默认配置文件：application.properties**

激活生产环境，默认端口号8080

```properties
spring.profiles.active=product
server.port=8080
```

**生产环境：**

```yaml
person:
  name: product-张三
server:
  port: 8000
```

**测试环境：**

```yaml
person:
  name: test-张三
server:
  port: 7000
```

**控制器方法：**

```java
@RestController
public class HelloController {
    @Value("${person.name:李四}")//获取被激活的环境配置文件的name属性值，如果获取不到，就使用"李四"代替
    String name;
    @RequestMapping("/")
    public String hello(){
        return "hello,"+name;
    }
}
```

运行测试：

**结果可以看出默认配置文件在Tomcat部署时会自动加载，被激活的环境配置文件也会加载，而且被激活的环境配置文件的端口号会覆盖默认配置文件的默认端口号。**

![image-20220402162253689](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402162253689.png)

![image-20220402162305010](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402162305010.png)

#### Profile环境切换可以来处理分工

创建Worker和Manager实体类，都实现Person接口,创建DoWhat类，不同的身份做不同的工作，使用**@Profile条件装配功能**来进行分工。

person接口：

```java
public interface Person {
}
```

Worker类:

```java
@Component
@ConfigurationProperties("person")//绑定配置文件的person属性
@Profile("product")//条件装配，Worker实现生产功能
@Data
public class Worker implements Person{
    private String name;
    private int age;
}
```

Person类：

```java
@Component
@ConfigurationProperties("person")
@Profile("test")//条件装配，Manager实现测试功能
@Data
public class Manager {
    private String name;
    private int age;
}
```

Dowhat类：

```java
public class DoWhat {
    private String doWhat;

    public DoWhat(String doWhat) {
        this.doWhat = doWhat;//工作
    }

    @Override
    public String toString() {
        return "DoWhat{" +
                "doWhat='" + doWhat + '\'' +
                '}';
    }
}
```

配置类：给不同的身份配置不同工作

```java
@Configuration
public class PersonConfig {
    @Profile("product")
    @Bean
    public DoWhat product(){
        DoWhat doWhat = new DoWhat("生产");
        return doWhat;
    }
    @Profile("test")
    @Bean
    public DoWhat test(){
        DoWhat doWhat = new DoWhat("测试");
        return doWhat;
    }
}
```

#### Profiles分组

将product配置环境分组：

```properties
spring.profiles.active=product
server.port=8080
spring.profiles.group.product[0]=product
spring.profiles.group.product[1]=product2
```

product:

```yaml
person:
  name: product-张三
server:
  port: 8000
```

product2:

```yaml
person:
  age: 19
```

test：

```yaml
person:
  name: test-张三
  age: 25
server:
  port: 7000
```

**测试：默认配置文件绑定了product环境，看看Person是不是Woker，doWhat是不是生产。**

```java
@RestController
public class HelloController {
    @Autowired
    Person person;
    @Autowired
    DoWhat doWhat;
    @RequestMapping("/")
    public Person hello(){
        System.out.println(doWhat);
        return person;
    }
}
```

结果：

![image-20220402173703648](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402173703648.png)

![image-20220402173713905](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220402173713905.png)

### 自定义starter

下面来完成一个简单的starter环境启动器和自动配置。

步骤：

**1、创建一个环境启动器hello-springboot-boot-starter和hello-springboot-starter-autoconfig自动配置器。**

​	环境启动器是一个简单的maven工程，只在它的maven项目下引入自动配置器的依赖。自动配置器是一个springboot工程，定义了HelloService完成名字加前后缀的工作，完成对HelloService的自动装配,以及自定义实体类定义前后缀属性并绑定核心配置文件。

环境启动器引入自动配置器的依赖。

```xml
<groupId>groupId</groupId>
<artifactId>hello-springboot-boot-starter</artifactId>
<version>1.0-SNAPSHOT</version>
<dependencies>
    <dependency>
        <groupId>com.spring</groupId>
        <artifactId>hello-springboot-starter-autoconfig</artifactId>
        <version>0.0.1-SNAPSHOT</version>
    </dependency>
</dependencies>
```

### hello-springboot-starter-autoconfig

**2、自定义实体类HelloProperties：**与配置文件进行绑定，可以通过配置文件对prefix和suffix进行赋值。

```java
@ConfigurationProperties("com.hello")
public class HelloProperties {
    private String prefix;
    private String suffix;

    public String getPrefix() {
        return prefix;
    }

    public void setPrefix(String prefix) {
        this.prefix = prefix;
    }

    public String getSuffix() {
        return suffix;
    }

    public void setSuffix(String suffix) {
        this.suffix = suffix;
    }
}
```

**3、自定义HelloService**：自动装配HelloProperties，给userName加上前后缀。

```java
public class HelloService {
    @Autowired
    HelloProperties helloProperties;
    public String sayHello(String userName){
        return helloProperties.getPrefix()+":"+userName+":"+helloProperties.getSuffix();
    }
}
```

**4、自动配置类HelloServiceAutoConfig**

自动配置类将HelloProperties绑定配置文件，并将器注册进容器中，供HelloService自动装配，当用户没有给IOC容器注册HelloService组件，自动配置类就会自动生成HelloService组件

```java
import com.spring.hello.Service.HelloService;
import com.spring.hello.bean.HelloProperties;
import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;


@Configuration
@EnableConfigurationProperties(HelloProperties.class)
public class HelloServiceAutoConfig {
@ConditionalOnMissingBean(HelloService.class)
    @Bean
    public HelloService helloService(){
        return new HelloService();
    }
}
```

**5、添加目录META-INF/spring.factories（这是SpingBoot的约定）**

![image-20220403105559143](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403105559143.png)

**spring.factories配置文件里配置项目启动时要自动加载哪个自动配置类。**

```properties
# Auto Configure
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.spring.hello.auto.HelloServiceAutoConfig  #项目启动时自动加载HelloServiceAutoConfig自动配置类
```

**6、将环境启动器hello-springboot-boot-starter和hello-springboot-starter-autoconfig自动配置器安装到maven工程**

![image-20220403105949553](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403105949553.png)

**7、新建SpringBoot工程，导入自定义的环境启动器hello-springboot-boot-starter**

```xml
<dependency>
    <groupId>groupId</groupId>
    <artifactId>hello-springboot-boot-starter</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

**8、在配置文件中给prefix和suffix赋值**

![image-20220403110202181](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403110202181.png)

**9、测试**

自动配置类帮我们自动注册了HelloService，直接调用helloService.sayHello方法传入名字。

```java
@RestController
public class HelloController {
   @Autowired
    HelloService helloService;
    @RequestMapping("/")
    public String name(){
        return helloService.sayHello("张三");
    }


}
```

名字被成功加上前后缀：

![image-20220403110436739](C:\Users\鹤\AppData\Roaming\Typora\typora-user-images\image-20220403110436739.png)

### SpringBoot原理

