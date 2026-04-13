# MybatisPlus

#### 常见注解

![image-20240911215908975](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240911215908975.png)

![image-20240911220059688](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240911220059688.png)

####  常见配置

![image-20240911220144493](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240911220144493.png)

id-type:全局配置 << 字段上配置

#### 条件构造器

原来都是基于ID

构造器：Wrapper

![image-20240912085752192](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912085752192.png)

  

```java```

QueryWarpper<User> wrapper = new QueryWrapper<>()

​	.select("id", "username")

​	.like("username", "0")

​	.ge("balance", 1000)



userMapper.SelectList(wapper)



![image-20240912090833971](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912090833971.png![image-20240912090844672](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912090844672.png)



![image-20240912090913406](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912090913406.png)

![image-20240912090928633](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912090928633.png)

#### 自定义sql

  .......

![image-20240912092354508](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912092354508.png![image-20240912092533712](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912092533712.png)

![image-20240912092413228](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912092413228.png)



![image-20240912092441807](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912092441807.png)

#### Service接口

Iservice: 基础增删改查

![image-20240912092227115](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912092227115.png)

![image-20240912092655003](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912092655003.png)

![image-20240912092843179](C:\Users\jzhou\AppData\Roaming\Typora\typora-user-images\image-20240912092843179.png)

