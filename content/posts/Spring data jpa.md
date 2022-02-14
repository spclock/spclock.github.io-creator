---
title: "Spring Data Jpa"
date: 2022-02-10T21:22:09+08:00
author: Spc
# cover: /img/设计模式.png
categories: ["ORM"] 
tags: ["jpa","hibernate","ORM"]
# showcase: true
draft: false
---

spring data jpa 中的hibernate也有他的优点，那jpa和mybaits谁更胜一筹？


<!--more-->

# Spring data jpa

**spring data jpa的官网：**[spring data Jpa](https://docs.spring.io/spring-data/jpa/docs/2.4.7/reference/html/#jpa.repositories)



## 简介

spring默认使用是hibernate orm框架，全自动orm能帮你不用写任何一条sql语句，在服务端和数据库之前增加一层新的。从而使人可以更加关注代码逻辑。



**优点**是直接关注对象，创建表和查询语句都能jpa帮你自动生成

**缺点**也是很明显，复杂的sql，更新要传递整个实体对象



怎么用？

高级的用法（扩展包Fenix https://github.com/blinkfox/fenix）？

比较常见的错误？



读写锁

@Lock(LockModeType.PESSIMISTIC_WRITE) 读写锁sql方法上

只能在select上用？？



如果是@NameQuery，则可以

```javascript
@NamedQuery(name="lockArticle",query="select a from Article a where a.id = :id",lockMode = PESSIMISTIC_READ)
public class Article
```

如果用entityManager的方式，则可以设置LocakMode：

```javascript
Query query = entityManager.createQuery("from Article where articleId = :id");
  query.setParameter("id", id);
  query.setLockMode(LockModeType.PESSIMISTIC_WRITE);
  query.getResultList();
```

```java
entityManager.find(Department.class, 1, LockModeType.PESSIMISTIC_READ);
```





@Transient

```
Specifies that the property or field is not persistent. It is used
* to annotate a property or field of an entity class, mapped
* superclass, or embeddable class.
```

@Modifying



@Repository





HQL

原生sql  `@Query(nativeQuery = true,value="sql 语句"`

您可以使用构造函数表达式JPQL查询，您的查询如下所示：

```
select new StatsDTO(count(u),u.typeId,u.modifiedAt) from UserCampaignObjective u where campId = ? group by objectiveTypeId,modifiedAt
```



用原生的话  属性名要改对应的  

```java
public interface StatsBo {
    Integer getUserCount();
    Byte getTypeId();
    Instant getModifiedAt();
}

//CrudRepository 不是StatsBo 返回值要StatsBo可以通过这种方式
public interface UserRepository extends CrudRepository<StatsBean, Long> {
    @Query(value = "select count(type_id) userCount, type_id as typeId, modified_at as modifiedAt from ...", nativeQuery = true)

        List<StatsDTO> getStatsDTO(Long camp_id);
}

```





jpa问题

sql 用别名 分页出现问题

因为是分页查询，当然需要知道数据的总数，所以hibernate会自动的执行sql帮你查询所有的数量，但是看下图就能发现，他把我的表名的别名当做字段来**select count** 了。

1.unknown column 'xxx' in 'field list' 这明显说明有sql把你这个xxx当做字段了，而我这个是表名的别名，这样就能定位到sql错误

解决办法：写countQuery

```java
@Query(nativeQuery = true,
        countQuery ="select count(*) from ("+conferenceRoomBoQuery+") m",
        value = conferenceRoomBoQuery)

```



## jpa问题

### update delete

 需要spring事务注解 不然会报  @Modifying(clearAutomatically = true)

> javax.persistence.TransactionRequiredException: Executing an update/delete query

 返回类型是 void int 

> Modifying queries can only use void or int/Integer as return type!



jpa 分页 新建对象，一个参数会报错  RegisterTimeBo(m)，一个以上就不会 RegisterTimeBo(m,u)

> Unexpected token count     in with Pageable

```java
//不会报错
@Query("select new com.gonsin.venus.bo.meeting.RegisterTimeBo(m,u) " +
        "from RegisterTimeBean m " +
        "left join UserInfoBean u " +
        "on m.userInfoKey=u.keyOnly " +
        " where m.meetingKey=:meetingKey")
Page<RegisterTimeBo> findByMeetingKey(@Param("meetingKey") String meetingKey, Pageable pageable);

//会报错
 @Query("select new com.gonsin.venus.bo.meeting.RegisterTimeBo(m) " +
            "from RegisterTimeBean m " +
            " where m.meetingKey=:meetingKey")
    Page<RegisterTimeBo> findByMeetingKey(@Param("meetingKey") String meetingKey, Pageable pageable);
```





jpa问题：

unable to obtain isolated JDBC   sqlite 线程池最大连接设置为1（不管这个问题）

保存不到saveAndFlush 保存出问题会一直卡住才会这样

为什么保存会一直卡住  是因为id主键没有设置正确

```
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    //下面是  使用错误导致  GenericGenerator应该怎么用
//    @GeneratedValue
//    @GenericGenerator(name = "idgenerator", strategy = "identity" )
```



更新和删除再别的事务上

**[Row was updated or deleted by another transaction (or unsaved-value mapping was incorrect) ]**

主因 : 事务对程序的影响

原因一: 查询出来的对象和update的对象不是同一个  当前version版本与数据库中version版本不一致。

**解决: 用查询出的对象进行set 值, 再用同一个对象update**   version为同一个就行了

原因二: 查询出来的对象在缓存中一段时间 , 之后再进行的update

**解决: 把这个对象从缓存中剔除(如需要对象属性可先get保存到变量中) , 在update之之前再查询出来进行update**

原因三: 同一对象查询了多次 , 数据还在缓存中没有清除.

**解决: 清除缓存中的对象    @Modifying(clearAutomatically = true)**





## 复杂查询

### 条件查询



1.接口继承`JpaSpecificationExecutor<T>`

```java
@Repository
public interface UserRepository extends JpaRepository<Integer,User>, JpaSpecificationExecutor<User> {
    
}
```

2.方法调用

```java
@Service
public class UserService {
    
    @Resource
    private UserRepository userRepository;
    
    public void test(SearchParam searchParam){
                Specification<DeviceBean> spec = new Specification<DeviceBean>() {
            @Override
            public Predicate toPredicate(Root<DeviceBean> root, CriteriaQuery<?> query, CriteriaBuilder criteriaBuilder) {
                
                // root java实体类中的对象而不是 root.get是java实体类属性名而不是数据库中的字段名
                Predicate delete = criteriaBuilder.equal(root.get("delete"),false);
                if (StringUtils.isNotBlank(searchParam.getUsername())) {
                    Path<Object> custId = root.get("deviceName");
                    //当查询条件用到gt, ge, lt, le, like（分别表示>, >=, <, <=,模糊查询）时，需要表明查询对象属性的类别
                    Predicate like = criteriaBuilder.like(custId.as(String.class), "%" + searchParam.getUsername() + "%");

                    return criteriaBuilder.and(delete, like);
                }

                return delete;
            }
        };
        
       List<User> result = userRepository.findAll(spec);
    }
}
```





### 排序分

 **Sort.Direction.DESC表示降序 Sort.Direction.ASC表示升序**

```java
Sort sort = new Sort(Sort.Direction.DESC, "id"); //sort作为findAll()方法中的参数，查询得到得到的结果是经过排序的 
List<Message> result = MessageRepository.findAll(sort);
```








### 页查询

```java
//Page 类
//获取总页数
int getTotalPages();
//获取总记录数	
long getTotalElements();
//获取列表数据
List<T> getContent();

```



```java
//设置分页参数
Pageable pageable = PageRequest.of(0，5);
//分页查询
Page<Message> page = MessageRepository.findAll(pageable);

```

