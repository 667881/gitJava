# JawaWeb



## MySQL

### 基本语句

#### 操作数据库

##### 1.查询数据库

```mysql
show database;
```



##### 2.创建数据库

```mysql
create database db1;	数据库不能够重复


```



##### 3.删除数据库

```mysql
drop database db1;


```



##### 4.进入数据库

```mysql
use db1;

select database();-- 查看当前使用的数据库
```



#### 操作表(DDL)

##### 1.查询表

show tables;	查询当前数据库下所有表名称

desc 表名称;	查询表结构

##### 2.创建表

create table 表名(

​			字段名1 数据类型1,

​			字段名2 数据类型2,

​			...

);

###### 数据类型

char为定长字符串，存储性能高，浪费空间

char(10)表示固定占10个字符空间

varchar为变长字符串，存储性能第，节约空间

varchar(10)表示最长为10个字符



##### 3.修改表

alter table 表名 reame to 新的表名;	修改表名

alter table 表名 add 列名 数据类型;	添加一列

alter table 表名  modify 列名 新数据类型;	修改数据类型

alter table 表名 change 列名 新列名 新数据类型;	修改列名和数据类型

alter table 表名 drop 列名;	删除列



##### 4.删除表

drop table 表名;

drop table if exists 表名;



#### 数据操作(DML)

##### 1.添加数据

instert into 表名(列名1，列名2,..) value(值1,值2,...);

insert into 表名 values(值1,值2,...);

##### 2.修改数据

update 表名 set 列名1=值1,列名2=值2...[where 条件];



##### 3.删除数据

delete from 表名 [where 条件];



#### 数据查询(DQL)

##### 1.基础查询

SELECT name,age FROM stu;	查询所有列可以用*替代，不建议使用

SELECT DISTINCT address from stu;	distinct关键字可以取出重复记录

SELECT math as 数学 FROM stu;	as关键字可以用来起别名



##### 2.条件查询

SELECT * from stu WHERE age>20;

SELECT * from stu WHERE age>20 AND age<=30;	逻辑且用and表示

SELECT * from stu WHERE age =18 OR age=20 OR age=22;	逻辑或用or表示。另一种写法为age in(18,20,22)

SELECT * from stu WHERE age BETWEEN 20 AND 30;	也可用between and表示

SELECT * from stu WHERE age =18;	注意等号写法，不等号写法为!=

SELECT * from stu WHERE english is null;	注意null的判断需要用关键字is或is not



###### 模糊查询

SELECT * FROM stu WHERE name LIKE '马%';	模糊查询使用like关键字，

通配符：_代表单个任意字符，%代表任意个数字符



日期类型

SELECT * from stu WHERE hire_date BETWEEN '1998-09-01' AND '1999-09-01';	注意需要用单引号



##### 3.排序查询

SELECT * FROM stu ORDER BY age ASC;	排序查询使用order by关键字

asc(默认)为升序排列，desc为降序排列	全称为ascend	descend



###### 多字段排序

```mysql
SELECT * FROM stu ORDER BY math DESC , english ASC;-- 按照数学成绩降序排列，如果数学成绩一样，再按照英语成绩升序排列
```



##### 4.分组查询

###### 聚合函数

(null不参与聚合函数查询)

count(列名)	统计非空数据，取值推荐用*(只要该行有数据便会统计)或主键；max(列名)；min(列名)；sum(列名)；avg(列名)

```mysql
SELECT count(id) FROM stu ;	
```



###### 分组查询

```mysql
SELECT sex,AVG(math),COUNT(*) from stu GROUP BY sex;-- 查询男同学和女同学各自数学的平均分以及对应的人数

SELECT sex,AVG(math),COUNT(*) from stu WHERE math>70 GROUP BY sex HAVING COUNT(*)>2;	-- 查询男女同学各自的平均分以及各自人数，要求：分数低于70分的不参与分组，分组之后人数大于2


```

注：分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义

where和having区别：

- 执行时机不一样：where时分组之前进行限定，不满足where条件则不参与分组，而having是分组之后对结果进行过滤

- 可判断的条件不一样：where不能对聚合函数进行判断，having可以

执行顺序：where>聚合函数>having



##### 5.分页查询

SELECT * FROM stu LIMIT 0,3;-- limit 起始索引，查询条数	limit是MySql数据库的“方言”

-- 起始索引=(当前页码-1)*每页显示的条数



### 数据库

#### 约束

约束是作用于表中列上的规则，用于限制加入表的数据，以保证其正确性，有效性以及完整性

-- 员工表

```
CREATE TABLE emp (
  id INT PRIMARY KEY auto_increment, -- 员工id，主键且自增长
  ename VARCHAR(50) NOT NULL UNIQUE, -- 员工姓名，非空并且唯一
  joindate DATE NOT NULL , -- 入职日期，非空
  salary DOUBLE(7,2) NOT NULL , -- 工资，非空
  bonus DOUBLE(7,2) DEFAULT 0 -- 奖金，如果没有奖金默认为0

);
```

主键约束：非空且唯一

非空约束：非空

唯一约束：唯一

默认约束：默认值；赋值null不属于默认约束

自动增长：当列是数字类型且唯一约束



#### 外键约束

外键约束：外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性

```mysql
CREATE TABLE dept(
	id int primary key auto_increment,
	dep_name varchar(20),
	addr varchar(20)
);
-- 员工表 
CREATE TABLE emp(
	id int primary key auto_increment,
	name varchar(20),
	age int,
	dep_id int,

-- 添加外键 dep_id,关联 dept 表的id主键
CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)

);
```



```mysql
-- 创建表时添加外键约束
CREATE TABLE 表名(
	 列名 数据类型,
	 …
	 [CONSTRAINT] [外键名称] FOREIGN KEY(外键列名) REFERENCES 主表(主表列名) 
); 

-- 添加外键 dep_id,关联 dept 表的id主键
alter table emp add CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)

-- 删除外键
alter table emp drop FOREIGN key fk_emp_dept;
```
先创建主表，再创建从表才能添加外键。先添加主表数据，再添加从表数据



#### 数据库设计

##### 表关系

###### 一对多

在多的一方建立外键，指向一方的主键



###### 多对多

建立第三张中间表，中间表至少包含两个外键，分别关联两方主键

```mysql
-- 订单表
CREATE TABLE tb_order(
	id int primary key auto_increment,
	payment double(10,2),
	payment_type TINYINT,
	status TINYINT
);

-- 商品表
CREATE TABLE tb_goods(
	id int primary key auto_increment,
	title varchar(100),
	price double(10,2)
);

-- 订单商品中间表
CREATE TABLE tb_order_goods(
	id int primary key auto_increment,
	order_id int,
	goods_id int,
	count int
);

-- 建完表后，添加外键
alter table tb_order_goods add CONSTRAINT fk_order_id FOREIGN key(order_id) REFERENCES tb_order(id);
alter table tb_order_goods add CONSTRAINT fk_goods_id FOREIGN key(goods_id) REFERENCES tb_goods(id);


```



###### 一对一

一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能

实现方式：在任意一方加入外键，关联另一方主键，兵设置外键为唯一



#### 多表查询

##### 连接查询

###### 1.内连接

隐式内连接

```mysql
select * from emp,dept WHERE emp.dep_id=dept.did;-- 相当于查询A B交集数据
```

显式内连接

```mysql
select  字段列表 FROM 表1 [INNER] JOIN表2 ON 条件;
```



###### 2.外连接

左外连接：相当于查询A表所有数据和交集部分数据（本例中A在左B在右）

```mysql
SELECT 字段列表 FROM 表1 LEFT [OUTER] JOIN 表2 ON 条件;
```

右外连接：相当于查询B表所有数据和交集部分数据

```
SELECT 字段列表 FROM 表1 RIGHT [OUTER] JOIN 表2 ON 条件;

```



##### 子查询

查询中嵌套查询，称嵌套查询为子查询

###### 单行单列

作为条件值，使用 = 等进行条件判断

###### 多行单列

作为条件值，使用in等关键字进行条件判断

###### 多行多列

作为虚拟表

```mysql
select * from (select * from emp where join_date > '2011-11-11' ) t1, dept where t1.dep_id = dept.did;


```



##### 事务

数据库的事务是一种机制，一个操作序列，包含了一组数据库操作命令。事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令要么同时成功，要么同时失败



###### 事务四大特征

原子性(Atomicity)：事务是不可分割的最小操作单位，要么同时成功，要么同时失败

一致性(Consistency)：事务完成时，必须使所有的数据都保持一致状态

隔离性(Isolation)：多个事物之间，操作的可见性

持久性(Durability)：事务一旦提交或回滚，它对数据库中的数据的改变就是永久的

```mysql
-- 开启事务
BEGIN
-- 回滚
ROLLBACK
-- 提交事务
COMMIT

```



## JDBC

JDBC 指 Java 数据库连接，是一种标准Java应用编程接口(JAVA API)

主要步骤

### DriverManager

1.注册驱动

2.获取连接

url：连接路径

```java
url="jdbc:mysql://127.0.0.1:3306/db1";
```



### Connection

1.获取执行SQL的对象

普通执行SQL对象；预编译SQL的执行SQL对象：防止SQL注入；执行存储过程的对象



2.管理事务

开启事务

```java
conn.setAutoCommit(false);
```

提交事务

```java
conn.commit();
```

回滚事务

```java
conn.rollback();
```



### Statemen

作用：执行SQL语句

executeUpdate:执行DML，DDL语句

executeQuery：执行DDL语句



### ResultSet

ResultSet：封装了DQL查询语句的结果

next()：将光标从当前位置向前移动一行并判断当前行是否为有效行

getXXX(参数)：获取数据	如：int getint(参数);

参数：int：列的编号，注意是从1开始

​			string：列的名称



### PreparedStatement

PreparedStatement作用：

1.预编译SQL并执行SQL语句

```java
//1.获取PreparedStatement对象
String sql="SELECT * FROM account WHERE name=? and password=?";  //参数值用占位符？替代。
//通过Connection对象获取，并传入sql语句
PreparedStatement pstmt = conn.prepareStatement(sql);

//2.设置参数值
pstmt.setString(1,name); //setXXX（索引，参数值），索引从1开始
//3.执行SQL
ResultSet rs = pstmt.executeQuery();  //不再传递sql

```



### druid配置详解



|                   属性                   |                             说明                             |          建议值           |
| :--------------------------------------: | :----------------------------------------------------------: | :-----------------------: |
|                   url                    |   数据库的jdbc连接地址。一般为连接oracle/mysql。示例如下：   |                           |
|                                          |    mysql : jdbc:mysql://ip:port/dbname?option1&option2&…     |                           |
|                                          |        oracle : jdbc:oracle:thin:@ip:port:oracle_sid         |                           |
|                                          |                                                              |                           |
|                 username                 |                      登录数据库的用户名                      |                           |
|                 password                 |                     登录数据库的用户密码                     |                           |
|               initialSize                |            启动程序时，在连接池中初始化多少个连接            |        10-50已足够        |
|                maxActive                 |                连接池中最多支持多少个活动会话                |                           |
|                 maxWait                  | 程序向连接池中请求连接时,超过maxWait的值后，认为本次请求失败，即连接池 |            100            |
|                                          |         没有可用连接，单位毫秒，设置-1时表示无限等待         |                           |
|        minEvictableIdleTimeMillis        | 池中某个连接的空闲时长达到 N 毫秒后, 连接池在下次检查空闲连接时，将 |        见说明部分         |
|                                          |               回收该连接,要小于防火墙超时设置                |                           |
|                                          |   net.netfilter.nf_conntrack_tcp_timeout_established的设置   |                           |
|      timeBetweenEvictionRunsMillis       |    检查空闲连接的频率，单位毫秒, 非正整数时表示不进行检查    |                           |
|                keepAlive                 | 程序没有close连接且空闲时长超过 minEvictableIdleTimeMillis,则会执 |           true            |
|                                          | 行validationQuery指定的SQL,以保证该程序连接不会池kill掉,其范围不超 |                           |
|                                          |                  过minIdle指定的连接个数。                   |                           |
|                 minIdle                  |          回收空闲连接时，将保证至少有minIdle个连接.          |     与initialSize相同     |
|             removeAbandoned              | 要求程序从池中get到连接后, N 秒后必须close,否则druid 会强制回收该 |   false,当发现程序有未    |
|                                          | 连接,不管该连接中是活动还是空闲, 以防止进程不会进行close而霸占连接。 | 正常close连接时设置为true |
|          removeAbandonedTimeout          | 设置druid 强制回收连接的时限，当程序从池中get到连接开始算起，超过此 |  应大于业务运行最长时间   |
|                                          |            值后，druid将强制回收该连接，单位秒。             |                           |
|               logAbandoned               |    当druid强制回收连接后，是否将stack trace 记录到日志中     |           true            |
|              testWhileIdle               | 当程序请求连接，池在分配连接时，是否先检查该连接是否有效。(高效) |           true            |
|             validationQuery              | 检查池中的连接是否仍可用的 SQL 语句,drui会连接到数据库执行该SQL, 如果 |                           |
|                                          |         正常返回，则表示连接可用，否则表示连接不可用         |                           |
|               testOnBorrow               |  程序 **申请** 连接时,进行连接有效性检查（低效，影响性能）   |           false           |
|               testOnReturn               |  程序 **返还** 连接时,进行连接有效性检查（低效，影响性能）   |           false           |
|          poolPreparedStatements          |                缓存通过以下两个方法发起的SQL:                |           true            |
|                                          |    public PreparedStatement prepareStatement(String sql)     |                           |
|                                          |    public PreparedStatement prepareStatement(String sql,     |                           |
|                                          |         int resultSetType, int resultSetConcurrency)         |                           |
| maxPoolPrepareStatementPerConnectionSize |                  每个连接最多缓存多少个SQL                   |            20             |
|                 filters                  |                这里配置的是插件,常用的插件有:                |      stat,wall,slf4j      |
|                                          |                    监控统计: filter:stat                     |                           |
|                                          |              日志监控: filter:log4j 或者 slf4j               |                           |
|                                          |                   防御SQL注入: filter:wall                   |                           |
|            connectProperties             |         连接属性。比如设置一些连接池统计方面的配置。         |                           |
|                                          |    druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000    |                           |
|                                          |                 比如设置一些数据库连接属性:                  |                           |
|                                          |                                                              |                           |





## Maven

Maven是专门用于管理和构建Java项目的工具

常用命令：compile clean test package install



### Maven 坐标详解

**什么是坐标？**

- Maven 中的坐标是==资源的唯一标识==
- 使用坐标来定义项目或引入项目中需要的依赖

**Maven 坐标主要组成**

- groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）
- artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
- version：定义当前项目版本号



### 依赖管理

**使用坐标引入jar包的步骤：**

- 在项目的 pom.xml 中编写 <dependencies> 标签
- 在 <dependencies> 标签中 使用 <dependency> 引入坐标
- 定义坐标的 groupId，artifactId，version



**快捷方式导入jar包的坐标：**

每次需要引入jar包，都去对应的网站进行搜索是比较麻烦的，接下来给大家介绍一种快捷引入坐标的方式

- 在 pom.xml 中右键，点击Generate，进入里面进行搜索，导入即可



#### 依赖范围

通过设置坐标的依赖范围(scope)，可以设置 对应jar包的作用范围：编译环境、测试环境、运行环境。

- compile ：作用于编译环境、测试环境、运行环境。
- test ： 作用于测试环境。典型的就是Junit坐标，以后使用Junit时，都会将scope指定为该值
- provided ：作用于编译环境、测试环境。我们后面会学习 `servlet-api` ，在使用它时，必须将 `scope` 设置为该值，不然运行时就会报错
- runtime  ： 作用于测试环境、运行环境。jdbc驱动一般将 `scope` 设置为该值，当然不设置也没有任何问题 

> 注意：
>
> - 如果引入坐标不指定 `scope` 标签时，默认就是 compile  值。以后大部分jar包都是使用默认值。





## MyBatis

MyBatis是一款优秀的持久层框架，用于简化JDBC开发

主要使用步骤

1.加载mybatis的核心配置文件

2.获取对应的sqlSession对象，来执行sql

3.执行sql

4.释放资源



### Mapper代理开发

1.定义于SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下

2.设置SQL映射文件的namespace属性为Mapper接口全限定名

3.在Mapped接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致

4.编码

​			1.通过SqlSession的getMapper方法获取Mapper接口的代理对象

​			2.调用对应方法完成sql的执行

如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用包扫描的方式简化SQL映射文件的加载

```xml
    <mappers>
        <!--加载sql映射文件-->
        <!--<mapper resource="com/itheima/mapper/UserMapper.xml"/>-->
        <package name="com.itheima.mapper"/>
    </mappers>
```



注意：数据库的字段名称和实体类中的属性名称不一样，则不能自动封装数据为Brand对象返回，解决方法第一是使用as关键字起别名。最常用的方法是使用resultMap解决

```xml
<resultMap id="brandResultMap" type="com.itheima.pojo.Brand">
    <!--id用来完成主键的映射，result用来完成一般字段的映射-->
    <result column="brand_name" property="brandName"/>  <!--column为列名，property为属性-->
    <result column="company_name" property="companyName"/>


</resultMap>
    <select id="selectAll" resultMap="brandResultMap">
        select * from tb_brand;
    </select>
```

注意：Mybatis参数占位符有两种，#{}会将其替换为?，可以防止sql注入，而使用${}*则是拼sql，存在sql注入问题

注意：如果使用<等特殊字符，需要用CDATA区进行处理，否则会被错误识别为tag符号

```xml
<![CDATA[
	<
]]>
```



##### 多参数条件查询

```java
    /*条件查询*/
    /*散装参数：如果方法中有多个参数，接口中需要使用@Pram("SQL参数占位符")*/
    List<Brand> selectByCondition(@Param("status")int status,@Param("companyName")String companyName,
                                  @Param("brandName")String brandName);

    //对象查询：传输一个对象作为参数，但需要将参数进行封装
    //map集合查询：只需保证SQL中的参数名和map集合的键的名称对应上即可设置成功

```

##### 

##### 多条件-动态条件查询

```xml
    <!--动态条件查询-->
    <select id="selectByCondition" resultMap="brandResultMap">
        select *
        from tb_brand
        <where>   /*使用提供的where标签会自动判断条件需不需要添加逻辑运算符*/
            <if test="status!=null">
                status = #{status}
            </if>

            <if test="companyName!=null and companyName!=''">
                and company_name like #{companyName}
            </if>

            <if test="brandName!=null and brandName!=''" >
                and brand_name like #{brandName};
            </if>
        </where>
    </select>
```



##### 单条件-动态条件查询

```xml
<!--    choose相当于switch，when相当于case,otherwise相当于default，-->
    <select id="selectByConditionSingle" resultMap="brandResultMap">
        select *
        from tb_brand
        <where>
            <choose>
                <when test="status !=null">
                    status = #{status}
                </when>

                <when test="brandName!=null and brandName!=''">
                    company_name like #{companyName}
                </when>

                <when test="companyName !=null">
                    brand_name like #{brandName};
                </when>
            </choose>
        </where>
    </select>
```



注：添加语句无法直接获取添加数据的主键，要想获取返回添加数据的主键，需要对xml文件进行设置

```xml
<insert id="Add" useGeneratedKeys="true" keyProperty="id">
    insert into tb_brand ( brand_name, company_name, ordered, description, status)
    values (#{brandName},#{companyName},#{ordered},#{description},#{status});
</insert>
```



注：动态修改数据可以使用<set>标签来解决逗号等问题

```xml
<!--动态修改-->
<update id="update">
    update tb_brand
    <set>
        <if test="brandName!=null and brandName!=''">
            brand_name=#{brandName},
        </if>
        <if test="companyName!=null and companyName!=''">
            company_name=#{companyName},
        </if>
        <if test="ordered!=null">
            ordered=#{ordered},
        </if>
        <if test="description!=null and description!=''">
            description=#{description},
        </if>
        <if test="status!=null">
            status=#{status}
        </if>
    </set>
    where id=#{id};
</update>
```



##### 动态修改

```
<!--动态修改-->
<update id="update">
    update tb_brand
    <set>
        <if test="brandName!=null and brandName!=''">
            brand_name=#{brandName},
        </if>
        <if test="companyName!=null and companyName!=''">
            company_name=#{companyName},
        </if>
        <if test="ordered!=null">
            ordered=#{ordered},
        </if>
        <if test="description!=null and description!=''">
            description=#{description},
        </if>
        <if test="status!=null">
            status=#{status}
        </if>
    </set>
    where id=#{id};
</update>
```



注：Mybatis不会自动提交事务，需要手动提交或设置为自动提交

```java
//2.获取对应的sqlSession对象，来执行sql。传参数true,false可以设置是否手动提交事务
SqlSession sqlSession = sqlSessionFactory.openSession();

//3.获取mapper接口的代理对象
Brandmapper mapper = sqlSession.getMapper(Brandmapper.class);
int updateNum = mapper.update(brand);

//4.手动提交事务
sqlSession.commit();
System.out.println(updateNum);
```



##### 批量删除

注：批量删除时需要不确定数量的占位符，因此可以使用<foreach>标签来进行对id数组进行遍历

```xml
    <!--Mybatis会将数组参数封装为一个Map结合
    默认：array=数组
    也可使用@Pram注解改变map集合的默认Key的名称
    -->
    <delete id="deleteByIds">
        delete from tb_brand
        where id in
            <foreach collection="ids" item="id" separator="," open="(" close=")">
                #{id}
            </foreach>
            ;
    </delete>
</mapper>
```



##### MyBatis参数传递



### 注解开发

使用注解开发会比配置文件开发更加方便，但主要是完成简单功能，配置文件适合完成复杂功能。

查询	@Select

添加	@Insert

修改	@Update

删除	@Delete







  根据《2023年全国硕士研究生招生工作管理规定》（教学〔2022〕3号）、《湖北省教育考试院关于做好2023年硕士研究生考试报名工作的通知》（鄂高考〔2022〕6号）有关要求，为做好我校2023年硕士研究生招生考试报名工作，现就有关事宜公告如下：
一、报考条件及要求
考生在报名前，请仔细核对是否符合教育部《2023年全国硕士研究生招生工作管理规定》《华中科技大学2023年硕士研究生招生简章》及各院系招生目录中对报考条件的说明。考生应认真了解并严格按照报考条件及相关政策，选择填报志愿和选择报考点。因不符合报考条件及相关政策要求，造成后续不能现场确认、考试、复试或录取的，后果由考生本人承担。
已被招生单位接收的推荐免试生，不得报名参加当年硕士研究生招生考试，否则将取消推荐免试生资格。
二、报名流程
（一）选择报考点
1．报考我校（招生单位代码10487）参加全国统一考试（含联合考试）考生（考试方式码：21），如属于以下两类情况，应选择华中科技大学报考点（代码4202）：
（1）学校在武汉市且报考华中科技大学的应届考生。
（2）武汉市报考华中科技大学的往届考生，须满足以下要求：①户籍所在地为武汉市的往届考生，须提供本人户口簿原件；或②长期在武汉市工作但户口未随迁的往届考生，须提供工作所在地公安部门办理的居住证原件及复印件，或提供本人工作所在地近3个月的社保缴费凭证，且网上确认时不应离职、社保不应停缴。
2．其他不在第1条范围内报考我校（招生单位代码10487）参加全国统一考试（含联合考试）考生（考试方式码：21），请选择户籍或工作所在地的报考点办理网上报名和网上确认（现场确认）手续。
3．所有“单独考试”考生（考试方式码：23）、“强军计划”（考试方式码：27）报考点必须选择华中科技大学报考点（代码4202）。
特别提醒：从2020年起，原华中科技大学同济医学院报考点（代码4223）与华中科技大学报考点（代码4202）合并，所有报考华中科技大学同济医学院各专业的考生均按规定选择华中科技大学报考点（代码4202）。具体考试地点（主校区、附属中学或同济校区）根据报考院系或专业由报考点统筹安排。
（二）网上报名
2022年10月5日— 25日每天9:00-22:00。网上预报名时间：2022年9月24日—27日每天9:00-22:00。预报名信息有效，无需重复网报。报名期间，考生可自行修改网报信息（考试方式、报考单位和报考点等除外）。逾期不再补报，也不得再修改报名信息。
网上报名部分重要字段说明：“招生单位”为拟报考的学校（华中科技大学10487），“报名点”为选择参加考试的地点。“姓名拼音”一栏中不要输入空格。“现工作或学习单位”如为在校应届本科生，请具体到院系。电话号码一栏中如需输入“-”，请切换输入法为中文半角输入。“报考院系”“报考专业”“考试科目”等请考生慎重选择。
（三）网上（现场）确认
请考生及时关注所选报考点和报考点所在省级教育招生考试机构发布的公告，在规定时间内按照规定方式进行现场确认，逾期不再补办。未进行确认的考生其报名无效。
注：华中科技大学报考点（代码4202）采用“网上确认”方式，时间为：2022年10月30日9:00—11月5日17:00（网上确认期间不再补报名，也不得再修改报名信息），具体要求详见报名结束后我校研究生招生信息网发布的官方公告。
三、注意事项
1．诚信报名，诚信考试。考前应仔细阅读诚信应考承诺书和《国家教育考试违规处理办法》（教育部令第33号）。考试作弊涉嫌违法，如有违犯，交由司法机关按刑法修正案（九）第284条进行处理。
2．教育部、省级教育主管部门、学校及院系将在网上报名、现场确认、初试、复试、录取检查、入学资格复核等各个阶段多次进行考生报考资格的全面审查。任何阶段，一经查实不符合报考条件或弄虚作假的考生（含推免生），即按有关规定取消其报考资格、复试资格、录取资格、入学资格或学籍，所有阶段成绩无效等手段，一切后果由考生本人承担。
3．凡报考我校“少数民族高层次骨干人才计划”的考生须在初试前将《少数民族高层次骨干人才计划报考登记表》寄送到我校研究生院招生办公室。考生报名时必须选择就业方式为“定向”。“定向就业单位所在地码”、“定向就业单位名称”等信息必须与本人提交的登记表上签章单位信息一致。


华中科技大学研究生院招生办公室
联系地址：武汉市洪山区 华中科技大学大学生活动中心
A栋303    
咨询电话：027-87541746

研究生招生信息网：http://gszs.hust.edu.cn
微信公众号：hust_gszs     