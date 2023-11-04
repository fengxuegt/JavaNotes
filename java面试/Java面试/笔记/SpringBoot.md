[[面试技能树]]
这个主要看黑马的web2023，看完做项目，做完了最后刷面经。

## 传递日期类型的参数
![[Pasted image 20231026231250.png]]



## 传递json数据

使用@RequestBody注解
传递json数据，将形参数据映射到实体类pojo中



## 路径请求参数
![[Pasted image 20231026232647.png]]
使用注解获取路径参数


## 返回响应数据

![[Pasted image 20231026232833.png]]


## 扫描类包

扫描之后才能够进行依赖注入。
Springboot中默认扫描的包是启动类所在包及其子包。
![[Pasted image 20231028001254.png]]
## Autowired

自动装配注解。这是按照类型进行自动装配的。如果有多个类型的话，就会报错。
如何解决？
### Primary注解
![[Pasted image 20231028001640.png]]

### Qualifier注解
![[Pasted image 20231028001720.png]]

### Resource注解
![[Pasted image 20231028001828.png]]














