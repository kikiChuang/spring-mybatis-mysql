# spring-mybatis-mysql
深入研究spring-mybatis框架对mysql数据库的使用，学习spring+springmvc+mybatis框架学习和使用。
主要讲解mybatis的知识和在mysql下的使用规范，及其其他数据库的使用如redis.
## 为什么需要mybatis对数据库进行操作
```
当我们采用面向对象语言进行编程时，我们采用面向对象分析，设计，编程。
但是到了持久层访问时，又必须重返关系型数据库的访问方式。这是糟糕的。
所以我们需要一个工具，将关系型数据库包装成面向对象的模型，这就是ORM.
采用mybatisORM框架的优势，允许直接编写SQL语句，更加灵活，便于sql语句调优。
```
## 1mysql下mybatis框架的基本sql数据操作语言（DML）语句
```
    insert语句 insert into tbl ()value()
    delete语句 DELETE FROM Person WHERE LastName = 'Wilson'
    update语句 update tbl set a = b where id =id
    select语句 SELECT * FROM Persons WHERE id=id
```
## 2mysql下的事物管理和mybatis的事物管理
```
1 问题：当外部一个事物方法调用内部另二个事物方法时，这里出现了问题，
    外部事物不会滚。内部事物出现异常的回滚，不出现异常不会滚。
  解决：问题原因没有真正的出现异常，我在末尾finally{}返回数据，被认为没有发现异常，所以没有回滚。
  默认的食物传播行为 是
2 问题：事件不回滚。
问题原因没有真正的出现异常，我在末尾finally{}返回数据，被认为没有发现异常，所以没有回滚。
--配置文件出错
@Transactiona
 属性名                                       类型                             说明
isolation     枚举org.springframework.transaction.annotation.Isolation的值   事务隔离级别
noRollbackFor     Class<? extends Throwable>[]           一组异常类，遇到时不回滚。默认为{}
noRollbackForClassName          Stirng[]               一组异常类名，遇到时不回滚，默认为{}
propagation  枚举org.springframework.transaction.annotation.Propagation的值  事务传播行为
readOnly              boolean                                                 事务读写性
rollbackFor           Class<? extends Throwable>[]                 一组异常类，遇到时回滚
rollbackForClassName    Stirng[]                                 一组异常类名，遇到时回滚
timeout               int                                            超时时间，以秒为单位
value                String                         可选的限定描述符，指定使用的事务管理器
```
## 3打印日志信息，将日志下载到某个位置等待查看
```
详情查看log4j.properties.可以控制输出级别，控制台显示，log文件输出。
```
## 4spring的异常处理对应http状态码
```
   1将各种异常用spring异常处理修饰，当发生这个异常时，返回对应的状态码，就相关原因。
   2将异常放在AOP中使用，就不用每个异常手写一次
   3可以将所有的结果都映射为异常，包括204成功无返回结果，映射为异常来返回状态码
```
## 5http接口开发
```
    httpclient发送get post put delete
    使用的jar 是httpcomponents的jar
    不推荐使用httpclient的jar
```
## 6spring的异步消息
```
1异步发送消息 ActiveMQ 消息队列的形式或主题的形式 异步接受消息 消息驱动POJO的类型 基于消息的RPC
他是定义了API不推荐使用
2异步消息机制 AMQP 跨语言 跨平台 2017426必须做完
AMQP 在client端以AMQP的形式发送，在server端以AMQP的形式接受。
github实例代码
https://github.com/chwshuang/rabbitmq/tree/master/src/main/java/com/aitongyi/rabbitmq/publish
AMQP 使用rabbitmq来实现，就像发邮件一样，但是这必须保证消息生产者有一个rabbitmq（需要下载安装的）机制，
消息接受者也有rabbitmq机制来接受。
完成了队列的消息机制。
未完成主题订阅的消息机制
```
## rabbitmq消息
```
```
## spring的consul的服务发现
```
1 服务的注册和发现
2 key vaule形式 配置文件处理
3 健康检查
4 多数据中心
```
## 7spring文件下载
```
第一基于前后端一起的
第二前后端分离，怎么办？
以springmvc restful风格的文件上传，下载
上传文件 第一种是基于流的方式上传文件 CommonsMultipartFile
上传文件 第二种是基于spring的方式 推荐使用spring的方式速度快
```
## 8spring的缓存
```
相关网址：http://www.cnblogs.com/jingmoxukong/p/5975994.html
mybatis的缓存一级缓存和二级缓存 他是对于查询结果的缓存，是执行sql结果的缓存，再次执行的时候会缓存中获得
spring-ehcache的缓存管理   可以对某个方法结果进行缓存，执行该方法时会直接返回之前的结果
配置spring-ehcache.xml
在applicationContext.xml中启动cache注解，
在方法中去使用
@Cacheable("definitionCache") --对结果进行缓存
	public List<DefinitionEntity> listFromCache() {
		return definitionDao.list(null);
}
redis作为内存数据库，作为缓存数据库
“因为redis数据库是将kv键值对存在内存中，只要主机不关机就会数据就会存在，他还会按照某些规则去自行删除”
```
## 9spring的认证
```
shiro 和 security
shiro的管理
    设置spring-shiro管理，加入web.xml中的配置，就可以进行实时检查
    pox.xml添加jar
```
# spring将传递json参数封装为对象和进行校验
```
1 使用jar
 <!--校验数据
            必须依赖正确的jar，之前就是错误的jar
        -->
        <!-- https://mvnrepository.com/artifact/javax.validation/com.springsource.javax.validation -->
        <dependency>
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
            <version>1.1.0.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.0.1.Final</version>
        </dependency>
        <!-- 校验数据jar结束-->
 2创建VO类,添加相关校验注解
 3在controller中添加@valid @RequestBody 就可以校验了在clud的post方法中我是用了这个方法
```
# spring使用拦截器进行登录验证和使用过滤器进行乱码过滤
```
1在springmvc配置文件中设置登录前验证拦截器：作用拦截没有登录的用户
2必须在web.xml中设置过滤器，根据你设置多个过滤器的顺序来设置过滤。
```
# ms-api连接数据库
```
1连接mysql    OK   使用JNDI的连接方式
2连接redis    OK   使用jedis连接方法，采用sentinel（哨兵）的方式连接可以搭建一个redis集群
数据库的长连接和短连接区别（连接池的情况下）
```
# redis的基础操作-增删改查
```
save 接口= update 接口 相同的kay进行存值时会覆盖原先的value
delete 接口 ok
select 接口 ok
问题：批量操作
```
# ms-web监控平台
```
1当压力测试时，页面需要去监控展示出多少个查询业务请求，
多少个查询数据库请求。检测软件业务的状态（多少请求多少查询），检测数据库的状态（多少数据库sql语句查询）
2当压力测试时，每次查询的耗时时间。
```
# http自动测试工具
```
测试接口的连接情况，在密集的请求下应用是否可以继续，为了分布式开发做准备
ab=apache bench测试自动化测试
测试结果 发送get方法并发1000没问题
```
# 分布式开发
```
当一个程序被大规模人使用时，就会触发分布式开发，这就考验着程序的代码质量，
主要问题：
1CPU的消耗分析
2文件IO的消耗分析   文件的上传下载模拟文件IO消耗
3网络IO的消耗分析   远程调用接口来模拟网络IO的消耗
4内存消耗分析       增加线程的使用来模拟内存消耗
5程序执行慢分析

问题：java的并发测试 就是关于多线程，高并发
```
# spring的线程池
```
1 配置线程池
 <bean id="threadPool" class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor">
        <!-- 核心线程数 -->
        <property name="corePoolSize" value="3"/>
        <!-- 最大线程数 -->
        <property name="maxPoolSize" value="10"/>
        <!-- 队列最大长度 >=mainExecutor.maxSize -->
        <property name="queueCapacity" value="25"/>
        <!-- 线程池维护线程所允许的空闲时间 -->
        <property name="keepAliveSeconds" value="300"/>
        <!-- 线程池对拒绝任务(无线程可用)的处理策略 ThreadPoolExecutor.CallerRunsPolicy策略 ,调用者的线程会执行该任务,如果执行器已关闭,则丢弃. -->
        <property name="rejectedExecutionHandler">
            <bean class="java.util.concurrent.ThreadPoolExecutor$CallerRunsPolicy"/>
        </property>
    </bean>
2使用它
    @Resource
    private ThreadPoolTaskExecutor threadPoolTaskExecutor;
     threadPoolTaskExecutor.execute(new Runnable(){
                public void run(){
                //you code
                }
            });
```
# mybatis的SQL注入工具
```
测试mybatis的sql有无注入风险。
MYSQL的慢查询   ok 在文件中设置一下
              my.ini    log_slow_queries = showlog
                        #slow_query_log = ON
                        long_query_time = 1
多个MYSQL数据库软件做读写分离
```
# JVM的性能测试及其调优
```
基础知识：
1内存分配
2GC机制
3工具检测
4调优方法
```
# 基础原理和知识
```
1 CAP定理
    一致性(Consitency)，可用性(Availability)和分区容忍性(Partition Tolerance)中三选二。
    CA无法同时选择
    一般看你的业务情况，数据不重要你可以选AP,数据重要你可以选择CP.
2 jdk8的新特性及其使用
java语言新特性
     Lambda表达式和函数式接口(Groovy、Scala类似)
     接口的修改 默认方法和静态方法
     可以使用重复注解，在同一个方法上面可以有重复注解
     扩展的注解的应用范围，注解可以使用在任何元素：局部变量，接口类型，超类，接口实现
java编译器的新特性
    参数名称
java官方库
    增加了date和time类
    增加optional,来解决空指针异常
    增加Streams （java.util.stream）将生成环境的函数式编程引入了Java库中
    提供  Nashorn JavaScript引擎
    Base64加密编码
    并行数组
    并发性
java新工具
    类依赖分析器：jdeps
3 restful和rpc
    restful是一种设计风格,提供了设计原则和约束条件，他不是架构,满足这些约束就是restful架构。
    rpc远程调用,从客户端机器通过参数传递的方式调用另一台机器的一个函数或者一个方法。并得到返回结果。
    RPC回隐藏底层的通讯细节，（不需要直接处理Socket通讯或Http通讯）
    RPC 是一个请求响应模型。客户端发起请求，服务器返回响应（类似于Http的工作方式）
    RPC 在使用形式上像调用本地函数（或方法）一样去调用远程的函数（或方法）。
4 spring使用RMI和Hessian,Burlap,HTTP invoker
    RMI发布，访问远程服务，有时不能穿过防火墙,必须在java环境中
    Hession是基于二进制有带宽优势，Burlap是XML,可读性高，非java环境
    HttP invoker
```

