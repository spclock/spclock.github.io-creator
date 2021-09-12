---
title: Mybatis
date: 2020-02-19T09:26:31+08:00
lastmod: 2020-02-19T09:26:31+08:00
author: spc
cover: /img/landscape15.jpg
categories: ["java"]
tags: ["数据库", "ORM"]
# showcase: true
draft: false
---

ORM：Object Relational Mapping

<!--more-->

对自己梳理清晰逻辑前言：
对于复杂的需求，Mybatis 比较复杂 
1. 方法要传入什么参数（sql需要的 或者 形成sql参数所需要参数 ）
2. 我觉得先什么都不写，先返回方法参数类型
3. 要分清 mapper.xml中sql 的parameterType resultType

# Mybatis
在调用时候你会觉得有点不对用的是mybatis包名是ibatis：  
版本1和2是包名 ibatis   
版本3是mybatis 因为向后兼容所以没改

xml的属性要顺序的不然报错（config.xml 中的settings、typeAliases、environments、mappers）

* ⾸先配置⽇志框架，可以极⼤地提⾼排查问题的效率
* 然后配置数据源
* Mapper：接⼝由MyBatis动态代理
  * 优点：⽅便
  * 缺点：SQL复杂的时候不够⽅便
* Mapper：⽤XML编写复杂SQL
  * 优点：可以⽅便地使⽤MyBatis的强⼤功能
  * 缺点：SQL和代码分离

## Object Relationship Mapping
* 对象关系映射
  * ⾃动完成对象到数据库的映射
* Association(对象套对象)
  * ⾃动装配对象

```xml
    <select id="getInnerJoinOrders" resultMap="Order">
        select  `ORDER`.ID as ORDER_ID ,USER.NAME as USER_NAME ,
        GOODS.NAME as GOODS_NAME ,
        (`ORDER`.GOODS_PRICE * `ORDER`.GOODS_NUM) as TOTAL_PRICE
        from `ORDER`
            inner join USER on `ORDER`.USER_ID=USER.id
            inner join GOODS on `ORDER`.GOODS_ID=GOODS.id
        where GOODS.NAME is not null  and  USER.NAME is not null
    </select>

    <resultMap type="Order" id="Order">
        <result property="id" column="ORDER_ID"/>
        <result property="totalPrice" column="TOTAL_PRICE"/>
        <association property="user" javaType="User">
            <result property="name" column="USER_NAME"/>
        </association>
        <association property="goods" javaType="Goods">
            <result property="name" column="GOODS_NAME"/>
        </association>
    </resultMap>
```
```java
    public void batchInsertUsers(List<User> users) {
        try (SqlSession sqlSession = sqlSessionFactory.openSession(true)) {
            sqlSession.selectOne("Mapper.batchInsertUsers", users);
        }
    }
```


## Mapper
* parameterType
  * 参数的#{}和${}  （防不防sql注入）
  * 参数是按照Java Bean约定读取的:getter/setter
* resultType
  * typeAlias
  * 写参数是按照Java Bean约定的:getter/setter
* Association(对象套对象)

typeAliases 别名
```xml
<typeAliases>
  <typeAliase aliase="User" type="全限定类名">
</typeAliases>
```

## 动态SQL——MyBatis的灵魂
* if
* choose
* foreach
* script

```xml
<!-- 建议官方文档下面代码只是方便自己回归 -->
<!-- java代码省略 -->
<!--  <if> -->
<select id="getUserByPage" parameterType="Map" resultType="User">
        select id,name,tel,address from USER
        <if test="username != null">
            where name = #{username}
        </if>
        limit #{offset} , #{limit} ;
    </select>
<!-- <foreach> -->
    <insert id="batchInsertUsers" parameterType="java.util.List">
        insert into USER
        (name,tel,address) values
        <foreach collection="list" item="user" separator=",">
            (#{user.name},#{user.tel},#{user.address})
        </foreach>
    </insert>
```

## MyBatis的缓存
* 缓存是如何⼯作的？
* MyBatis缓存源代码深⼊解读
* Decorator模式

## MyBatis MapperProxy
动态字节码增强?
```java
    public User selectUserById(Integer id) {
        try (SqlSession sqlSession = sqlSessionFactory.openSession()) {
            final SelectMap mapper = sqlSession.getMapper(SelectMap.class);
            return mapper.SelectIdGetUser(id);
        }
    }
    interface SelectMap {
        @Select("Select * from USER where id =#{id}")
        User SelectIdGetUser(@Param("id") Integer id);
    }
```
还有在config.xml 配置mapper class="....."
返回的类型User 是通过JavaBean约定  （调试器会调到get/set方法里面去）

## MyBatis字符
Utf8 utf8mb4

数据库 在mybatis中有一个驼峰形式 xx_xx to xxXx  
```propertiesd
mybatis.configuration.mapUnderscoreToCamelCase=ture
```


```java
Session.commit() 等等 if catch(){  session.rollback(); }
Session.flushStatements();
```