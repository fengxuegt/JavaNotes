[[面试技能树]]

直接看面经吧。主要还是实战中使用，在做项目的过程中使用。

# 配置信息

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver  
spring.datasource.url=jdbc:mysql://localhost:3306/itheima  
spring.datasource.username=root  
spring.datasource.password=root

# 配置Mybatis的日志输出到控制台
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

```
![[Pasted image 20231122231136.png]]

但是我们发现输出的SQL语句:delete from emp where id = ?，我们输入的参数16并没 有在后面拼接，id的值是使用?进行占位。那这种SQL语句我们称为预编译SQL。

# 预编译SQL
## 性能更高
预编译一次之后会将SQL缓存起来，后面执行语句时不会再次编译；

## 更安全
防止SQL注入


## 参数占位符
/#
表示执行sql时，会将这个标识符替换为？使用时机为参数传递时都用这个；

/$
用于拼接sql，直接将参数拼接到sql语句中，存在sql注入问题
项目开发中，建议使用#

## sql语句中#是写的属性名
而不是字段名，都是驼峰命名

## 获取返回的主键值
`//会自动将生成的主键值，赋值给emp对象的id属性 @Options(useGeneratedKeys = true,keyProperty = "id")`

## 更新操作
直接写update语句


## 查询操作
**Mybatis的数据封装；**
- 如果实体类属性名称和数据库表查询返回的字段名一致，那么会自动封装；
- 如果实体类属性名称和数据库表查询返回的字段名不一致，那么不能自动封装；
**解决办法：**
- 给字段起别名
```java
@Select("select id, username, password, name, gender, image, job,
entrydate, " +

        "dept_id AS deptId, create_time AS createTime, update_time AS
updateTime " +

"from emp " +

        "where id=#{id}")
```

- 手动映射

```java
@Results({@Result(column = "dept_id", property = "deptId"),
          @Result(column = "create_time", property = "createTime"),
          @Result(column = "update_time", property = "updateTime")})

@Select("select id, username, password, name, gender, image, job,
entrydate, dept_id, create_time, update_time from emp where id=#{id}")
public Emp getById(Integer id);
```

- 自动映射
Mybatis自动映射开关
```xml
# 在application.properties中添加: mybatis.configuration.map-underscore-to-camel-case=true
```


## Mybatis 配置文件用法
- xml配置文件的namespace属性要与mapper接口的全类名保持一致
- xml配置文件中sql语句的id与mapper接口中的方法名要保持一致
- xml配置文件的名称需要与mapper接口的名称保持一致，并且放置在同名的包路径下；只是一个在Java路径下，一个在resource路径下，但是要声明名字相同的包

# Mybatis动态SQL
## 条件查询
不同条件，SQL语句不一样；

动态SQL的标签

## if标签
判断条件是否成立，使用test属性进行条件判断，如果条件为true，则拼接SQL;
使用test属性，test属性就是用来指定条件的；
![[Pasted image 20231123230800.png]]
![[Pasted image 20231123235214.png]]
![[Pasted image 20231123235333.png]]


# 文件上传

## 本地存储


## 阿里云OSS










