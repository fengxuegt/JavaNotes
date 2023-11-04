动力节点老杜

## 第一个Spring 程序的十个注意点

### bean的ID可以重复吗？
bean的id不能重复

### 底层是怎么创建对象的？是通过反射调用无参数构造方法吗？
Spring通过调用对象的无参数构造方法来创建对象。因此必须保证类是有无参数构造方法的。

### 把创建好的对象储存到一个什么数据结构中去了呢？
放到了一个map当中。
key是String，是bean的id，value是创建出来的对象。

### Spring的配置文件必须叫做Spring.xml吗？
Spring.xml文件的名字是随意的，没有什么要求和标准

### Spring.xml的配置文件可以有多个吗？
这样的文件可以有多个，创建对象的时候会传递文件路径，传递哪个路径就使用哪个xml文件。

### 在配置文件中使用的类必须是自定义的吗，可以使用Java类库中的类吗？
可以。在这个xml文件中可以配置任意类，只要不是抽象类并且有无参构造方法。


### getBean方法如果传入的id值不存在会怎样？
会报异常。
![[Pasted image 20231021201320.png]]

### getBean方法返回的是Object，如果想要调用特殊方法还需要向下转型，有没有其他办法可以代替？

User user1 = context.getBean("userBean", User.class);

### 如果类路径中没有配置文件，配置文件放到了其他地方，如果调用？
ApplicationContext applicationContext2 = new FileSystemXmlApplicationContext("d:/spring6.xml");
Vip vip = applicationContext2.getBean("vipBean2", Vip.class);

可以使用FileSystemXmlApplicationContext

### ApplicationContext的超级⽗接⼝BeanFactory
也就是说BeanFactory是父亲接口。
BeanFactory是Spring容器的超级接⼝。ApplicationContext是BeanFactory的⼦接⼝。



## 依赖注入

### set注入
#### 实现原理

![[Pasted image 20231021202907.png]]

总结：set注⼊的核⼼实现原理：通过反射机制调⽤set⽅法来给属性赋值，让两个对象之间产⽣关系。




### 构造方法注入
```xml
<bean id="orderDaoBean" class="com.powernode.spring6.dao.OrderDao"/>
<bean id="orderServiceBean" class="com.powernode.spring6.service.OrderService">

<!--index="0"表示构造⽅法的第⼀个参数，将orderDaoBean对象传递给构造⽅法的第⼀个参数。-->
<constructor-arg index="0" ref="orderDaoBean"/>
</bean>
```

如果构造方法有两个参数：
![[Pasted image 20231021203217.png]]

![[Pasted image 20231021203314.png]]

### set注入专题

#### 注入外部bean

#### 注入内部bean

#### 注入简单类型

#### p空间

#### c空间

#### 使用properties文件
需要引入context空间
![[Pasted image 20231021205815.png]]

# Bean的作用域

## 单例
默认情况下，Spring创建的对象是单例的。
对象创建的时机是在Spring上下文初始化的时候完成的。

## 多例

可以在配置文件bean标签中配置scope属性为prototype
![[Pasted image 20231021210219.png]]



## 简单工厂模式
Spring中的BeanFactory就使⽤了简单⼯⼚模式。


## 工厂方法模式


# Bean的实例化方式
## 通过构造方法实例化


## 通过简单工厂模式实例化

## 通过factory-bean实例化


## 通过FactoryBean实例化
FactoryBean在Spring中是⼀个接⼝。被称为“⼯⼚Bean”。“⼯⼚Bean”是⼀种特殊的Bean。所有的“⼯⼚Bean”都是⽤来协助Spring框架来创建其他Bean对象的。


## BeanFactory 和 FactoryBean的区别
前者是Spring的顶级对象，是工厂，用于生产对象。
后者是接口，是一个Bean，用于帮助产生对象的bean。

# Bean的生命周期
## 五步的说法


## 七步的说法


## 十步的说法


## 自定义对象交给Spring管理
![[Pasted image 20231021213442.png]]


# Bean的循环依赖问题

## 单例+set注入
通过测试得知：在singleton + set注⼊的情况下，循环依赖是没有问题的。Spring可以解决这个问题。

## 多例+set注入
因为每次都需要新建对象。
![[Pasted image 20231021214009.png]]

## 单例+构造注入


## Sring解决循环依赖的机制
实例化bean的时候，可以调用无参构造方法先实例化，将对象曝光给外界，然后再进行set注入将属性赋值。

总结：
**Spring只能解决setter⽅法注⼊的单例bean之间的循环依赖。ClassA依赖ClassB，ClassB⼜依赖ClassA，形成依赖闭环。Spring在创建ClassA对象后，不需要等给属性赋值，直接将其曝光到bean缓存当中。在解析ClassA的属性时，⼜发现依赖于ClassB，再次去获取ClassB，当解析ClassB的属性时，⼜发现需要ClassA的属性，但此时的ClassA已经被提前曝光加⼊了正在创建的bean的缓存中，则⽆需创建新的的ClassA的实例，直接从缓存中获取即可。从⽽解决循环依赖问题。**


# 回顾反射机制

## 调用方法的四要素
调用哪个对象的
哪个方法
传什么参数
返回什么值

## 反射调用方法
![[Pasted image 20231021223111.png]]


## 调用set方法赋值
![[Pasted image 20231021224600.png]]

# 手写Spring框架

主要就是dom4j解析xml文件，利用反射机制创建对象并保存到对象map当中，注意set注入时要跟对象的创建分开，这是为了解决bean的循环依赖问题；

# 常用注解

## autowired
![[Pasted image 20231022225508.png]]


