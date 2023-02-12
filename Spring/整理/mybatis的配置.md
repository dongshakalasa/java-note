# mybatis的配置

### 1、mybatis快速入门

- 在pom.xml文件中引入 mybatis依赖

  ```
  <dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.6</version>
  </dependency>
  ```

- resources文件夹下创建 mybatis-config.xml文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
  <environments default="development">
    <environment id="development">
      <transactionManager type="JDBC"/>
      <dataSource type="POOLED">
      <!-- 数据库连接信息 -->
       <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
      </dataSource>
    </environment>
  </environments>
  <!-- 加载sql的映射文件 -->
  <mappers>
    <mapper resource="org/mybatis/example/BlogMapper.xml"/>
  </mappers>
</configuration>
```

- resources文件夹下创建sql语句的映射文件 **表名**+Mapper.xml文件

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  
    <!-- 
    	namespace:名称空间
    	resultType:返回结果的类型
    	id:当前sql的唯一标识
    -->
<mapper namespace="test">
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{id}
  </select>
</mapper>
```

- 创建测试类

```
// 1、加载mybatis的核心配置文件，获取SqlSessionFactory
String resource = "org/mybatis/example/mybatis-config.xml";
InputStream inputStream = Resources.getResourceAsStream(resource);
SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

// 2、获取SqlSession对象，用来执行sql
SqlSession sqlSession = sqlSessionFactory.openSession();

// 3、执行sql
List<Object> objects =  sqlSession.selectLIst("test.selectAll");

// 4、释放资源
sqlSession.close();
```

### 2、Mapper代理

- 定义SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下
  - 创建一个Mapper接口
  - 一般在resources下创建一个和测试类同名的文件例如：com/iteheima/mapper

![](图\mybatis\M~_4T04MQQZKOE]09$C6P9N.png)

- 设置SQL映射文件namespace属性为Mapper接口全限定名
- 在Mapper接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致
- 编码
  - 通过SqlSession的getMapper方法获取Mapper接口的代理对象
  - 调用对应方法完成sql的执行
