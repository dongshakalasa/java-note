## 1、IOC入门案例

1、创建maven的java项目

2、添加spring的依赖jar包

```
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.2.10.RELEASE</version>
    </dependency>
```

3、添加案例所需的类

4、添加spring配置文件

<img src="图\spring\1629734336440.png" alt="1629734336440" style="zoom:50%;" />

5、在配置文件中完成bean的配置

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <!--bean标签标示配置bean
    	id属性标示给bean起名字
    	class属性表示给bean定义类型
	-->
	<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl"/>

</beans>

注意事项：bean定义时id属性在同一个上下文中(配置文件)不能重复
```

6、获取IOC容器

​	使用spring提供的接口完成IOC容器的创建

```
ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); 
```

7、从容器中获取对象进行方法调用

```
 BookService bookService = (BookService) ctx.getBean("bookService");
        bookService.save();
```

## 2、DI入门案例

需求:基于IOC入门案例，在BookServiceImpl类中删除new对象的方式，使用Spring的DI完成Dao层的注入

1.删除业务层中使用new的方式创建的dao对象

![img](图\spring\ZT}KS_}SHM~_[`7[7]55W2Y.png)

2.在业务层提供BookDao的setter方法

3.在配置文件中添加依赖注入的配置

![img](图\spring\LPGOJV[1`@K1L2Q%NH]`B4L.png)

4.运行程序调用方法

## 3、IOC相关内容

1、bean基础配置

​	1.1、bean基础配置(id与class)

```
<bean id="" class=""/>	
```

![img](图\spring\image-20210729183500978.png)