---

typora-copy-images-to: image
typora-root-url: image
---



# student

### 概念

物联网卡是三大运营商发行的一种商用级卡片，现阶段广泛用于共享单车、移动支付、智能城市、自动售卖机等领域，有独立的网元，独立的号段；而且因为是运营商开发的，物联网卡的外观和普通流量卡差不多，所使用的网络也有2G/3G/4G

SIM卡：身份识别

SimBank：是能够集中管理大量SIM卡的网关设备

eSIM卡：就是将传统SIM卡直接嵌入到设备芯片上，而不是作为独立的可移除零部件加入设备中，用户无需插入物理SIM卡。

OTA：用户可以根据自己的需要，通过OTA服务器增加或删除自己OTA卡上的业务菜单，以此实现业务的更新



IMSI国际移动用户识别码，即 sim 卡的识别码

ICCID：SIM卡卡号，相当于手机号码的身份证。

 MSISDN就是用户的手机号；

OCS，即在线计费系统（Online Charging System）能够解决用户实时信用控制、预付费使用数据业务和增值业务实时计费等问题。



终端——人与机器交互的接口，就是一种输入输出设备，相对于计算机主机而言属于外设



 **CDR)**：呼叫详细记录，它描述了呼叫接续的全过程

SFTP是一种**安全的文件传输**协议。要求客户端用户必须由服务器进行身份验证，并且数据传输必须通过安全通道（SSH）进行，即不传输明文密码或文件数据。它允许对远程文件执行各种操作。FTP是**SSH协议**（22端口）的一部分，它是一种远程登录信息。

FTP**使用TCP / IP协议**，FTP密码和数据以纯文本格式发送，大多数情况下是不加密的，安全性不高。



### MYSQL

以前复制粘贴的数据和现在自增主键冲突，先去除主键再增加主键，会让序列号从最大的开始



数据库：sql server，mysql，Oracle

数据库字段类型：3

①date：MySQL 以 'YYYY-MM-DD' 格式检索与显示date值；

②datetime:MySQL 以 'YYYY-MM-DD HH:mm:ss'格式检索与显示 DATETIME 类型

实体类写 Date 类型即可。但Mapper要的 jdbcType="TIMESTAMP"



数据库中的索引是唯一索引，插入数据库时会校验是否重复。

表中是int类型的，接收的实体类可以是boolean/String类型。

SELECT dict_value FROM sy_dict_value WHERE dict_code = 'IOT_VENDOR_APN_STATUS' AND dict_key = '0'（可以写int 0，也可以写String "0"）

select，where ，order by 字段取了别名要用别名



**三表直接jion比有子查询的快**。

不管是一对一还是一对多都可用inner join。

下面语句第二个联表要为left join，写inner join会没有的话会将第一个left join覆盖

SELECT * FROM rs_goods_iot_vendor a
 left join rs_goods_iot_tenant b
 ON a.goods_id = b.goods_id
 left join or_order_sale c
 on b.goods_id = c.goods_id

<> 与!=都是不等于的意思，但是一般都是用<>，因为!=在sql2000中是语法错误，不兼容的。

select * from user where id &lt;&gt; 217;使用Mybatis的时候，特殊字符需进行转义,如`&lt;&gt;  <>`

<=  的中间不要有空格，select   c2,c2   form    c2后不能有空格



92语法和99语法：inner join比92语法快

--自然连接
92：select * from emp e , dept d where e.deptno = d.deptno;
99：select * from emp e natural join dept d ;
--左外连接
92：select * from emp e ,dept d where e.deptno = d.deptno(+);		mysql不支持
99：select * from emp e left (outer可省) join dept d on e.deptno = d.deptno;
--全连接（99独有）
select * from emp e full join dept d on e.deptno = d.deptno;



#### sql语句

SQL语句书写顺序select、form、join on、where、group by、having、order by、limit 

**执行顺序：from、join on、where、group by、having、select、order by、limit**

group by: 多个时，要这多个都一样才会 是一组。

​		select 查询字段要与group by中的字段相同或为函数（min,max,sum,count,avg）

mysql在没有order by条件时，得到的排序顺序与 查询列/索引 有关。不一定使用主键来进行正序排序。MySQL将会以最快的形式（物理存储顺序）展示数据。切记要加上order by

having：

having 的作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数。可先用where的先用where过滤

select 类别, sum(数量) from A group by 类别 having sum(数量) > 18

```java
//基本CURD
select * from 表
INSERT INTO 表 (`k`, `l`, `v`) VALUES ('i', ...)
update 表 set 列1=值1,列2=值2... where 条件
DELETE FROM 表 WHERE ...
       
select * from student where age is not null；
select * from goods where goods_company not in('淘宝','Tmall');  //or,between 12 and 18

//往表中增改删字段
ALTER TABLE `neware_csp_busi`.`pr_staff` ADD `alias` VARCHAR(128)    #默认可null
ALTER TABLE users ALTER COLUMN realname varchar(30);   //可以修改字段为可空的
ALTER TABLE users DROP COLUMN realname
//加字段，带默认值，非空，注释
ALTER TABLE `neware_csp_busi`.`cp_tenant_pool` ADD `init_usage_bye` bigint(20) DEFAULT 0 Not NULL COMMENT '流量池充值流量，共享池默认为0'
//设置某一列可为空
ALTER TABLE `neware_csp_busi`.`rs_sim` modify COLUMN imsi varchar(20) null;

//索引，添加索引
ALTER  TABLE  `table_name`  ADD  UNIQUE index_name(`column` )   //唯一索引，不会重复
ALTER  TABLE  `table_name`  ADD  INDEX index_name (`column`) //普通索引
ALTER  TABLE  `table_name`  ADD  INDEX index_name ( column1,  column2, column3)//组合索引   
//删除索引
drop index rs_goods_product_id_tenant_id_index ON rs_goods

    
//将多行合并成一行。默认用逗号分割。group_concat(order_sale_accept_id separator ' ') 用空格分隔  
select goods_id,group_concat(order_sale_accept_id) as order_sale_accept_id from or_order_sale group by goods_id

//查数量
select (select count(0) from rs_goods) +(select count(0) from rs_goods_iot_vendor) as total  //分表的总数量
select (select count(0) from rs_goods) as t1Cnt,(select count(0) from rs_goods_iot_vendor) as t2Cnt //分表各自
select count(*) total,count(if(a.status='1',1,null)) success,count(if(a.status='2',1,null)) failure from order_busi a where 1 = 1   //同个表不同状态的数量
    
SELECT VERSION();		本项目的是5.7.24的版本
SELECT NOW();     查数据库时间
show variables like 'general%';   查看mysql的日志是否打开
show variables like 'innodb_buffer_pool%';   看mysql缓存
    
//You can't specify target table。。。不能依据某字段值做判断再来更新某字段的值，还要包一层中间表
DELETE FROM A WHERE goods_id in (select u.goods_id from (select a.goods_id FROM A a left JOIN B b ON a.goods_id=b.goods_id  where b.goods_id is null) u)
```

多字段内容并到单字段：group_concat

<img src="/image-20220726110035133.png" alt="image-20220726110035133" style="zoom: 67%;" />



#### sql函数

```java
CREATE DEFINER=`cspadmin`@`%` FUNCTION `FUN_NAME`(inTableName VARCHAR(100), inKey VARCHAR(128)) RETURNS VARCHAR(300) CHARSET utf8
BEGIN
    SET @RESULT = '';
    IF inTableName = 'pr_tenant' THEN
        SELECT NAME INTO @RESULT FROM pr_tenant WHERE tenant_id = inKey;
    ELSEIF inTableName = 'pr_org' THEN
        SELECT NAME INTO @RESULT FROM pr_org WHERE org_id = inKey;
    ELSEIF inTableName = 'pd_product' THEN
        SELECT product_name INTO @RESULT FROM pd_product WHERE product_id = inKey;
    ELSEIF inTableName = 'sy_code_province' THEN
        SELECT name_cn INTO @RESULT FROM sy_code_province WHERE prov_code = inKey;
    ELSEIF inTableName = 'sy_code_city' THEN
        SELECT name_cn INTO @RESULT FROM sy_code_city WHERE city_code = inKey;
    END IF;
    RETURN @RESULT;
END$$
DELIMITER ;

DELIMITER $$
USE `neware_csp_busi`$$
DROP FUNCTION IF EXISTS `FUN_DICT`$$
CREATE DEFINER=`cspadmin`@`%` FUNCTION `FUN_DICT`(dictCode VARCHAR(128), dictKey VARCHAR(32)) RETURNS VARCHAR(300) CHARSET utf8
BEGIN
    RETURN (SELECT dict_value FROM sy_dict_value WHERE dict_code = dictCode AND dict_key = dictKey);
END$$
DELIMITER ;

<select id="listByNethallId" resultType="site.neware..vo.NethallPlatformVo">
select nethall_id nethallId,
        ven_dor,
        FUN_DICT('SIM_VENDOR', ven_dor) vendorName,  //vo要有get/set方法才行
        prov_code provCode,
        FUN_NAME('sy_code_province', prov_code) provName,
        city_code cityCode,
        FUN_NAME('sy_code_city', city_code) cityName,
        status
        from pr_nethall
        where 1=1
             
 //获取下个序列号           
DROP FUNCTION IF EXISTS `getnextval`$$
CREATE DEFINER=`cspadmin`@`%` FUNCTION `getnextval`(seqname VARCHAR(50)) RETURNS VARCHAR(64) CHARSET utf8
    DETERMINISTIC
BEGIN
    DECLARE result   VARCHAR(64);
    UPDATE sy_seq t SET curr_value = curr_value + incre_value WHERE seq_name = seqname;
	  SET result = getcurrval(seqname);
    RETURN result;
END$$
DELIMITER ;
```

func函数没有用到redi缓存：可以用代码使用：

dictComp.getDictValue(DictConstants.PD_FEE_CLASS, dictFee.getFeeClass(), operation.getOpLocale())

#### 数据库卡死

<img src="/image-20220804174613123.png" alt="image-20220804174613123" style="zoom: 80%;" />

![image-20220804174710023](/image-20220804174710023.png)

或者用命令：

show full processlist;         

kill '4773904'     		       杀死有info 的线程



### Mybatis

javabean比数据库多字段会报错

前端传的和req接收的参数哪个多都不报错，resp传的和前端接收的参数哪个多也不报错，两者会按字段名匹配，若名一样但类型不一样会报错

```java
<include refid="And_Columns_Dynamic"/>   //条件，可以有多余的表别名，判断空的，根本没走，其实不能有
<include refid="Base_Column_VendorVO"/>  //查出项，不能有多余的表别名
```

一个表设置两个字段联合主键，这样两个都相同才会重复主键，如sy_i18n表

mybatis查询没有数据时返回的list是空集合，即size=0 并不是null，没有数据的对象时返回的是null



在mysql中[字符串](https://so.csdn.net/so/search?q=字符串&spm=1001.2101.3001.7020)日期可以直接和datetime类型之间比较，无需转换，Mysql会将字符串类型日期转换成长整型数字进行比较

#### mapper文件

userMapper，serviceMapper，goodsmapper

```java
//联表
<resultMap type="site.neware.csp.datahouse.database.syst.privilege.entity.RoleFunc" id="roleFuncInfo">
        <id column="role_id" property="roleId"/>
        <result column="func_code" property="funcCode"/>
        <result column="menu_id" property="menuId"/>
        <collection property="menuFunc" ofType="site.neware.csp.datahouse.database.syst.privilege.entity.MenuFunc">
            <id column="func_code" property="funcCode"/>
            <result column="menu_id" property="menuId"/>
            <result column="func_name" property="funcName"/>
            <association property="i18n" javaType="site.neware.csp.datahouse.database.syst.common.entity.I18n">
                <id column="key" property="key"/>
                <id column="locale" property="locale"/>
                <result column="value" property="value"/>
            </association>
        </collection>
    </resultMap>
    <select id="selectMenuFuncs" resultMap="roleFuncInfo">
    SELECT mf.`create_by`,mf.`create_time`,mf.`func_code`
    FROM `pr_role_func` rf
    left JOIN `pr_menu_func` mf
    ON rf.`func_code` = mf.`func_code`
    left JOIN `sy_i18n` i
    ON mf.`func_i18n`=i.`key`
    WHERE rf.`role_id`=#{roleId} AND rf.`menu_id`=#{menuCode} AND i.`locale`=#{locale}
    </select>


<sql id="Base_Column_VO">
        a.vendor vendor,
        FUN_DICT('SIM_VENDOR', a.vendor) vendorName,   
        a.res_type resType,
        FUN_DICT('SIM_RES_TYPE', a.res_type) resTypeName,
		a.status,
        FUN_DICT('GOODS_STATUS', a.status) statusName,
        FUN_I18n(#{locale}, FUN_DICT('GOODS_STATUS', a.status)) statusI18n,
        a.produce_max produceMax,
        (case a.produce_max when 1 then 'N' else 'Y' end) produceFlagName,
        b.batch_id batchId,
        c.file_name filename,
</sql>
<select id="list" resultType="usi.resource.entity.vo.SimVo">
        select
        <include refid="Base_Column_VO"/>
        from rs_sim a, sy_batch_job b, sy_batch_file c
        where 1=1 
        and a.op_accept = b.op_accept
        and b.job_file_id = c.file_id
        <include refid="And_Columns_Dynamic"/>
        order by a.iccid asc
</select>
//Case函数和Case搜索函数：
  //CASE WHEN sex = '1' THEN '男' WHEN sex = '2' THEN '女' ELSE '其他' END
 //select字段为数据库的字段。若表写成了别名 ，select字段写a. ,若字段写成了别名，则不能用自动生成的resultMap（是select字段和bean对应），可以用resultType，改了之后应用都要重启
数据库字段与bean名不一样时：
    用reslutMap，起别名用reslutType，驼峰命名
     
        
...where  1=1
     <if test="userName != null and userName !=''">
         and user_name like CONCAT('%',#{userName},'%')
     </if>   


<resultMap id="BaseResultMap" type="site.neware.csp.datahouse.database.syst.privilege.entity.Staff">
        <id column="staff_id" jdbcType="VARCHAR" property="staffId"/>
        <result column="tenant_id" jdbcType="VARCHAR" property="tenantId"/>
        <result column="org_id" jdbcType="VARCHAR" property="orgId"/>
</resultMap>

         
         
    <sql id="Base_Column_List">
        staff_id, tenant_id, org_id, name, password, email, mobile, fax,last_login_time
    </sql>

    <sql id="From_Table">
        from pr_staff
    </sql>

    <sql id="Where_Default">    
        where 1=1
    </sql>
// 1=1是为了避免当if条件判断都为false时，不出错，该条件始终成立。

<sql id="And_Columns_Dynamic">
        <if test="condition.tenantId != null and condition.tenantId !=''">
            and tenant_id = #{condition.tenantId}
        </if>
        <if test="condition.orgId != null and condition.orgId !=''">
            and org_id = #{condition.orgId}
        </if>
        <if test="startDate != null">
            and create_time &gt;= #{startDate}
        </if>
        <if test="endDate != null">
            and create_time &lt;= #{endDate}
        </if>
        <if test="orgIds!= null and orgIds.size()!=0">
            and org_id in
            <foreach collection="orgIds" open="(" item="oId" separator="," close=")">
                #{oId}
            </foreach>
        </if>
        <if test="condition.email != null and condition.email !=''">
            and email like concat('%', #{condition.email}, '%')
        </if>
    </sql>    
//<include refid="And_Columns_Dynamic"/>
//&gt;和&lt;	也可用于比较String/Date类型
 //Date类型不用endDate !=""

            
<sql id="And_Extend_Condition">
    <if test="condition.dictKeyIn != null and !condition.dictKeyIn.isEmpty()">
         and `dict_key` in
        <foreach collection="condition.dictKeyIn" item="item" open="(" separator=", " close=")">
           #{item,jdbcType=VARCHAR}
         </foreach>
    </if>
</sql>
      
        
<sql id="Dynamic_Table_Name">
    <choose><when test="suffix != null and suffix.length > 0">bs_user_${suffix}</when><otherwise>bs_user</otherwise></choose>
</sql>
<sql id="From_Dynamic_Table">
    from <include refid="Dynamic_Table_Name" />
 </sql>   
            
        
        
//可写一个condition类继承原来的实体类并将多的参数添加为属性,如：
private List<String> orgIds;
private Date opTimeLt;
private Date opTimeGt;
private List<String> orders;  //排序，单字段用String
//condition.setOrders(Arrays.asList(new String[]{"batch_id"}));
//<if test="cond.order!=null and 'batch_id'.compareTo(cond.order)==0">
            order by batch_id Desc
  </if>


    <select id="selectByPrimaryKeyOrEmail" resultMap="BaseResultMap">
        select
        <include refid="Base_Column_List"/>
        <include refid="From_Table"/>
        <include refid="Where_Default"/>
        and (staff_id = #{staffId,jdbcType=VARCHAR} or UPPER(email) = UPPER(#{staffId,jdbcType=VARCHAR}))
    </select>


    <select id="selectByConditionWithoutDeleted" resultMap="BaseResultMap">
        SELECT
        <include refid="Base_Column_List"/>
        <include refid="From_Table"/>
        where status &lt;&gt; #{deleteValue}
        <if test="condition.tenantId != null and condition.tenantId !=''">
            and tenant_id = #{condition.tenantId}
        </if>
        <if test="statusList!= null and statusList.size()!=0">
            and status in
            <foreach collection="statusList" open="(" item="sta" separator="," close=")">
                #{sta}
            </foreach>
        </if>
        order by status_time Desc     
    </select>
//一定要这个参数查的这里不用if而是在代码判断，否则要用if
//集合的非空判断顺序
//是数据库的字段，若是动态排序则ORDER BY ${statusTime} ，#会将传入的数据都当成一个字符串
//或者 <![CDATA[  where status <> #{deleteValue} ]]> 

    <select id="selectValidByOrgId" resultMap="BaseResultMap">
        SELECT
        <include refid="Base_Column_List"/>
        <include refid="From_Table"/>
        where status = #{validValue} and org_id = #{orgId}
    </select>


<select id="selectCountByDateRange" resultType="java.lang.Integer">
        SELECT count(*)
        <include refid="From_Table"/>
        <include refid="Where_Default"/>
        <if test="startDate != null">         
            and login_time &gt;= #{startDate}
        </if>
        <if test="endDate != null">
            and login_time &lt;= #{endDate}
        </if>
        <if test="email != null and email !=''">
            and email like concat('%', #{email}, '%')
        </if>
    </select>
//Data类型不能写!=''

//id名字不能为delete
 <delete id="deleteByMenuIds">  
        delete
        <include refid="From_Table"/>
        where role_id = #{roleId}
        and menu_id in
        <foreach collection="menuIds" open="(" item="menuId" separator="," close=")">
            #{menuId}
        </foreach>
    </delete>

//update直接跟表，不用from
<update id="update">
    update t_employee     
    set id = #{id}, name = #{name}
    where id = #{id}
</update>
<!-- 更新不为空的属性 -->
<update id="updateNotNull">
    update t_employee
    <set >
      	 <if test="userName !=null and userName!=''">
   			 user_name = #{userName},
  	 	</if>
   		<if test="userPassword !=null and userPassword !=''">
    		user_password = #{userPassword},
   		</if>
    </set>
    where id = #{id}
</update>

    
<insert id="insertHis">
    insert into pd_product_his (<include refid="Base_Column_List" />)
    values (#{productId}, #{tenantId})
</insert>

<insert id="insertSelectiveWithSuffix" parameterType=".busi.accept.entity.User">
    insert into <include refid="Dynamic_Table_Name" />
    <trim prefix="(" suffix=")" suffixOverrides=",">
      <if test="record.userId != null">
        `user_id`,
      </if>
      <if test="record.custId != null">
        `cust_id`,
      </if>
    </trim>
    <trim prefix="values (" suffix=")" suffixOverrides=",">
      <if test="record.userId != null">
        #{record.userId,jdbcType=BIGINT},
      </if>
      <if test="record.custId != null">
        #{record.custId,jdbcType=BIGINT},
      </if>
    </trim>
  </insert>
    
       
<select id="getAllOperLogTableName" resultType="java.lang.String">
        SHOW TABLES LIKE 'pr_oper_log_%';
</select>
    
    
//数据库是否有字段
  		select
        <include refid="Base_Column_VO2"/>
        from or_order_busi a, pd_product b
        where a.terminate_time is null
        and a.product_id=b.product_id
        <if test="goodsId != null and goodsId != ''">
            and a.goods_id = #{goodsId,jdbcType=VARCHAR}
        </if>
        <if test="nowDate != null">
            and a.exp_time &lt;= #{nowDate}
        </if>
        UNION ALL
        select
        <include refid="Base_Column_VO2"/>
        from or_order_busi a, pd_product b
        where a.terminate_time is not null
        and a.product_id=b.product_id
        <if test="goodsId != null and goodsId != ''">
            and a.goods_id = #{goodsId,jdbcType=VARCHAR}
        </if>
        <if test="nowDate != null">
            and terminate_time &lt;= #{nowDate}
        </if>
            
//SQL中and的优先级要高于or所以使用时要用括号将or中的条件包上
 		select
		<include refid="Where_Default" />
	    <![CDATA[ 
	    	and (reserv_time is null or reserv_time < #{reservTime,jdbcType=TIMESTAMP}) 
	    ]]>
 
//Mybatis中没有if-else的写法，取而代之的是choose-when-otherwise，当when都不符合条件，就会选择otherwise
<choose>
<when test="uname!=null and uname!=''"> and uname like concat('%',#{uname},'%') </when>
<when test="usex!=null and usex!=''">  and usex=#{usex} </when>
<otherwise> and uid > 10  </otherwise>
</choose>
 //根据条件筛选显示列         
<choose>
 <when test="cond.isManagerOrTenant != null and cond.isManagerOrTenant == true ">FUN_NAME('pr_org',d.sale_org_id) orgName,</when>
 <otherwise>FUN_NAME('pr_org',d.org_id) orgName,</otherwise>
</choose>
            
select
<include refid="Base_Column_List"/>
, substring(cycle_start_date, 1, 4) year
, substring(cycle_start_date, 1, 6) season
, substring(cycle_start_date, 1, 6) month
from rp_report_vendor_data
<if test="cond.monthList != null and cond.monthList.size()!=0">
            AND substring(cycle_start_date, 1, 6) IN
  <foreach close=")" collection="cond.monthList" index="index" item="item" open="(" separator=",">
                #{item,jdbcType=VARCHAR}
   </foreach>
</if>
```

#### 批量插入

插入库方法：

1）for循环调用Dao中的单条插入方法

2）传一个List参数，使用mybatis 的批量插入 （foreach），此方法默认语句的长度限制是 4M，此方法一次执行只获取一次session，提交一条sql语句，减少了与数据库连接时间。

```java
for (int i = 0; i < 10000; i++) {
        user = new User();				//每次都要new新对象，要不改的都是一个
        user.setId("test" + i);
        user.setName("name" + i);
        user.setDelFlag("0");
        list.add(user);
    }
userService.insertBatch(list);

<insert id="insertBatch">
    INSERT INTO t_user
            (id, name, del_flag)
    VALUES
    <foreach collection ="list" item="user" separator =",">
         (#{user.id}, #{user.name}, #{user.delFlag})
    </foreach >
</insert>
 //values中的要和前面一一对应
```

批量插入问题：

批量语句，只要有一个失败，就会全部失败。数据库会回滚全部数据。



### 慢查询优化

#### 联表查询：

解决：

分库分表

小表驱动

字段类型：使用varchar代替char

索引：**查看explain计划**

<img src="/image-20220531141236767.png" alt="image-20220531141236767" style="zoom:80%;" />

1、MySQL 5.6 引入的索引下推优化（index condition pushdown)， 可以在索引遍历过程中，对索引中包含的字段先做判断，直接过滤掉不满足条件的记录，减少回表次数

​	非主键索引的查找过程，会多一步到主键索引上搜索的操作，这个操作称为回表

​	但查询的字段在联合索引（city，name）上可以拿到city,name两个字段的信息，就不需要回表了



2、普通索引和唯一索引对比：

一是查询，InnoDB都是整页存取，唯一索引找到 id=8对应的Page 3的数据页就马上就返回了，如果是普通索引的话，会判断下一个记录是否满足id=8，直到不满足为止，如果刚好下个记录在另外的数据页，还要多一次读取数据页。

二是更新，唯一索引要将整个数据页读到内存里面去判断是否唯一，这就增加了随机IO访问，随机IO是数据库成本最高的操作之一。



3、有时候索引为什么不生效？选择索引是Mysql优化器的工作，而优化器的通常是用统计信息去判断成本。其实统计信息主要就是索引的区分度(准确来说是索引基数)，会选择区分度高的索引，可以force index(XXX)  强制使用索引。



4、关联字段建索引

```
select * from t1 join t2 on (t1.a=t2.a);
```

t1有N条记录，t2有M条记录，t2上a字段上没有索引，这个时候会采用BNJ算法：扫描表t1，读取K条记录放进join_buffer，全表扫描t2，取出每一行和join_buffer的t1数据判断出结果集，循环上述流程直到结束

如果t2的a字段上有索引，那么会采用NLJ算法，执行流程如下：扫描表t1，拿到一条记录。通过索引树定位t2的记录，拿到主键，通过主键索引树定位记录，和t1的记录作判断



5、小表是加了where条件后的数据量相对小的表。explain 计划哪个表在上面哪个就是主表

- 主表（驱动表）根据where条件和order by 的列上加上索引，聚簇索引还会回表，到主键索引树查其他的列
- 从表根据被关联的列建立索引，如果where中涉及到从表的列，可以结合关联列建立组合索引（注意最左匹配原则）。

1、mysql会自动添加一个与主键对应的唯一索引，不需要对主键再做索引

2、一个查询条件都不加，即使联表也很快，有查询条件会筛选，先on 建立临时表再where进行筛选，是业务情况可以先筛选在联表

3、当in( )的数据很大时，不走索引。in后边的集合元素数量，控制在1000个之内。

IN(子查询) 适合于外表大而内表小的情况；EXISTS适合于外表小而内表大的情况（以外层表为驱动表，先被访问，A是外表，B是内表）

```java
select colname … from A表 where a.id not in (select b.id from B表)
select colname … from A表 Left join B表 on where a.id = b.id where b.id is null  //高效的SQL语句
```

方法二：将in 的子查询 改为连接查询



4、**Mysql 使用 join 联表查询时，用主副表都有的字段去order by时：**

如果不指定字段来源的表且当查询结果中没有这个字段时，sql会报错：ambiguous，字段不明确，而当查询结果中有该字段时，则不会报错，且以查询结果中的那个字段为排序依据。

extra会有三种情况：不使用filesort（仅依赖主表索引字段排序），使用filesort（仅依赖主表的非索引字段排序），使用filesort和Using  temporary（使用临时表的字段排序，临时表是没有索引的）。当有temporary时要先考虑是不是order by 或 group by的问题

解决：**尽量用主表的字段进行排序更合适一些。inner join时mysql会以数据少的做驱动表。可改为left join或straight_join**(强制把左边的表设置为驱动表，只能替换inner jion)

```java
select a.goods_id
 FROM rs_goods_iot_vendor a straight_join rs_goods b
 ON a.goods_id = b.goods_id
 WHERE 1=1 and b.tenant_id = '1000431' order by a.goods_id asc
```

​		from  A a join B b on a.id=b.id order by a.time;    在A表建索引  time,id，time字段在前

​		默认情况下，MySQL对所有GROUP BY 进行了排序。如果想避免排序则可用 ORDER BY NULL禁止排序

5、 extra还有种 Using join buffer (Block Nested Loop) 情况，是使用了嵌套循环，一般是**关联的查询或者join字段没有用到索引**

一个表只能使用一个索引，mqsql 会选它认为最优索引执行

6、避免使用游标，因为游标的效率较差，如果游标操作的数据超过1万行，那么就应该考虑改写

7、查找语句尽量不要放在循环内



#### 深度分页查询：

select * from table where column=xxx order by xxx limit 1000000,20。

原因：

MySQL将扫描IO查询出1000020条记录。然后舍掉前面的1000000条记录，返回剩下的20条记录。

mysql有执行优化器，执行优化器会估算 cpu 代价 和 io 代价，深度分页会走全表扫描而不会走索引，当然评估绝大多数是准确的

1**、解决：延迟关联：用索引覆盖扫描**Extra（using index），然后再 INNER JOIN关联自己。**但满足下面查询方法要求id必须是主键**

SELECT  *  FROM  t  INNER JOIN (SELECT  id  FROM t WHERE x_id = 143381 LIMIT 800000,20) t1 ON t.id = t1.id

或 用子查询：	SELECT * FROM a WHERE id >=(SELECT id FROM a limit 9000000,1) ORDER BY id limit 25

数据量大时影响数据库响应时间最大因素是物理I/O操作，整条语句的执行时间比子句的执行时间短，子句执行后返回的是10000条记录，而整条语句仅返回10 条语句。

```java
 //注意：
//子查询和count的联表取决于And_Columns_VendorDynamic中的条件的表，外连接另一张表是空的也要写进，否则会不到limit 10条记录。还要看表是不是一对一的结果
//GoodsIotVendorMapper
<select id="page" resultType="site.neware.csp.database.core.goods.entity.vo.GoodsIotVo">
       select
        <include refid="Base_Column_PageVO"/>
        from
        (select a.goods_id
        from rs_goods_iot_vendor a
        straight_join rs_goods b
        on a.goods_id = b.goods_id
        where 1=1
        <include refid="And_Columns_VendorDynamic"/>
        order by a.goods_id asc
        <if test="cond.limit != null and cond.limit != ''">
            limit #{cond.limit,jdbcType=INTEGER}
        </if>
        <if test="cond.offset != null and cond.offset != ''">
            offset #{cond.offset,jdbcType=INTEGER}
        </if>
        ) u
        straight_join rs_goods_iot_vendor a
        on u.goods_id=a.goods_id
        left join rs_goods_iot_tenant d
        on a.goods_id = d.goods_id
        inner join rs_goods b
        on a.goods_id = b.goods_id
        inner join pd_product c
        on b.product_id = c.product_id
    </select>
    <select id="countByCondition" resultType="java.lang.Integer">
        SELECT count(*)
        from rs_goods_iot_vendor a,rs_goods b
        where a.goods_id = b.goods_id
        <include refid="And_Columns_VendorDynamic"/>
   </select>
 //GoodsIotVendorDao
    public PageInfo<GoodsIotVo> page(GoodsIotCond cond) {
        List<GoodsIotVo> result = getMyMapper().page(cond);
        Page<GoodsIotVo> page = new Page<>(new int[]{offset(), limit()}, true);
        page.addAll(result);
        page.setTotal(this.countByCondition(cond));
        return page.toPageInfo();
    }
//GoodsIotVendorCompImpl
public PageInfo<GoodsIotVo> page(GoodsIotCond cond) {
        PageInfo<GoodsIotVo> pageInfo = goodsIotVendorDao.page(cond);
        if(AssertUtils.isListEmpty(pageInfo.getList())) return pageInfo;
        List<GoodsIotVo> tenantGoodsList = goodsIotTenantDao.getByGoodsIds(pageInfo.getList().stream().map(GoodsIotTenant::getGoodsId).collect(Collectors.toList()));
        if (AssertUtils.isListEmpty(tenantGoodsList)) return pageInfo;
  pageInfo.getList().stream().map(this.getGoodsIotVo(tenantGoodsList)).collect(Collectors.toList());
         return pageInfo;
    }
private Function<GoodsIotVo,GoodsIotVo> getGoodsIotVo(List<GoodsIotVo> tenantGoodsList) {
        return item -> {
            for (int i = 0; i < tenantGoodsList.size(); ++i) {
                GoodsIotVo vo = tenantGoodsList.get(i);
                if (item.getGoodsId().equals(vo.getGoodsId())) {
                    item.setOrgId(vo.getOrgId());
                    tenantGoodsList.remove(i);
                    break;
                }
            }
            return item;
        };
    }
```

2、当延迟关联也不能满足查询速度的要求时，用滚动分页，即书签的方式，**要求 id 是递增的主键且一直往后查询的情况**，如下载。

滚动翻页方案比LIMIT offset,size效率高，因为此方案每次查询都是最终的结果集，而一般的分页方案使用的LIMIT 需要先查询，后截断

但这种情况下，只走主键索引，其他条件退化为通过主键索引搜索出来的结果后的过滤条件，不会命中其他索引

首先要获取复合条件的记录的最小 id，若按照Id升序排序，当查询下一页时将上一页最后那条数据的Id作为下一次的查询条件

```java
select min(id) from t where kid=2333 and type=1;
SELECT * FROM tableX WHERE id > #{lastBatchMaxId} [其他条件] ORDER BY id ASC LIMIT ${size}

		long lastBatchMaxId = 0L;
        goodsIotCond.setLimit(10000);
        for (;;) {
            List<GoodsIotExportVo> list = goodsIotVendorDao.page4Export(goodsIotCond,lastBatchMaxId, operation.getOpLocale());
            if (list.isEmpty()) {
                writer.finish();
                break;
            } else {
                lastBatchMaxId = list.stream().map( item-> Long.parseLong(item.getGoodsId())).max(Long::compareTo).orElse(Long.MAX_VALUE);
                writer.write(list, writeSheet);
            }
        }
```

查缓存反而跟慢了？

```
i18NComp.getI18nText(menu.getNameI18n(), operation.getOpLocale())
i18nComp.getI18nText(dictComp.getDictValue(DictConsts.PD_SERVICE_TYPE, serviceSimplyBo.getServiceType(), language)
```

测程序运行时间：

```
long startTime = System.currentTimeMillis();
System.out.println("程序运行时间：" + ( System.currentTimeMillis() - startTime) + "ms");
```



#### 统计同一字段不同状态的数量

`count(非空字段)<count(主键id)<count(0)≈count(1)≈count(*),count(*)`有优化，推荐count(*)

推荐count(*)

数据量大的情况下count速度会变慢。

如果 **select count(\*)** 走的是主键索引，所以 MySQL 先把整个**主键索引 **从磁盘读写到内存缓冲区，而主键索引基本等于整个表数据量，所以非常耗时

解决：

1、建二级索引。因为二级索引只包含对应的索引列及主键列，所以体积非常小，explain**第一看extra，第二看key（使用非主键索引）**

如果有二级索引，mysql优化器就会选择使用二级索引，这是在mysql5.7.18版本后有的优化，因为二级索引只存主键值，而没存真实数据，所以二级索引树比主键索引树更小，其次直接扫描二级索引树获取数据量也避免了回表操作，所以mysql优化器会选择成本更小的二级索引，不过要注意成本小，不意味着消耗时间少，这二者不是完全等同的。

2、调节mysql 缓存大小，pool-size可以缓存索引和行数据，值越大，IO读写就越少

show variables like 'innodb_buffer_pool%';     查看mysql缓存大小

![image-20220617162655278](/image-20220617162655278.png)

innodb_buffer_pool_size必须为 innodb_buffer_pool_instances 的倍数。

- innodb_buffer_pool_size<1G时，默认值为1；

- innodb_buffer_pool_size>1G时，默认值为8。

  设置缓存池大小，mysql5.7.5之后，不用重启：set global innodb_buffer_pool_size=534217728;

  set global innodb_buffer_pool_instances=4;

```java
//创建联合索引（product_id`, `tenant_id`, `org_id`），再强制使用索引，强制索引加在表后
SELECT count(1)
 FROM pd_product b straight_join rs_goods a force index(rs_goods_p_t_o)
 ON a.product_id = b.product_id
 WHERE 1=1 and a.tenant_id = '1000431' and a.org_id in ( '200006' , '1001652' );


/* 其他写法 count(if(a.`type`='1',1,NULL))。SUM(CASE WHEN a.`type`='1' THEN 1 ELSE 0 END) AS status_zero <=>
sum(if(a.`type`='1',1,0)),  用count比较好，用count的话没有显示0，用sum没有的话显示null。
如果a.`type` = 1则返回1否则返回0 */
<select id="countByStatus" resultType=",,,order.extend.OrderBusiCountDto">
        select
        count(*) totalCount,
        count(if(a.status='1',1,null)) successCount,
        count(if(a.status='2',1,null)) failureCount
        from or_order_busi a
        where 1 = 1
        <if test="orderBusiAcceptId != null and orderBusiAcceptId != ''">
            and a.order_busi_accept_id = #{orderBusiAcceptId,jdbcType=VARCHAR}
        </if>
</select>
```

#### 子查询代替关联查询

```java
//销售卡池下载
select b.iccid,(select used_usage_byte FROM rs_goods_iot_tenant_usage WHERE goods_id=b.goods_id and cycle_exp_time >= '2022-06-09 11:12:01.604' ) monthlyUsageByte
 FROM cp_tenant_pool a straight_join pd_service c
 ON a.service_id=c.service_id straight_join cp_tenant_pool_sim b
 ON a.tenant_pool_id=b.tenant_pool_id
 WHERE a.tenant_pool_id = '200133'
 LIMIT 0, 10000;
```



### 文件上传导出

#### 下载导出

list<对象>  =》response的输出流

PageExporterBuilder设置头，总行数，分页数据 =》 PageXlsxExporter 获取Builder设置数据 初始化表格，初始化方法中调用GoodsIotRest中接口的实现的方法

```java
//OrderBusiRest
@PostMapping("/download")
public void download(@RequestBody QueryOrderReq req, HttpServletResponse rsp) throws BusinessException {
        Operation operation = this.getOperation();
        InputStream is = busiOrderRestService.exportBusiOrderExcel(req, operation);
        String fileName = "BUSI_ORDER_" + DateUtils.date2Str(operation.getOpTime(),);
        super.downloadExcel(is, fileName, rsp);
}
//BaseRest
super.downloadExcel(is,..) 
outputStream = response.getOutputStream();
response.setContentType(contentType);
response.addHeader("Content-Disposition", "attachment;fileName=" + filename);
response.addHeader("Access-Control-Expose-Headers", "Content-Disposition");//前端可获取自定义响应头，多个的话，用英文逗号分割
//BatchJobRestService	,	行数据 =》 输入流
PageExporterBuilder<GoodsIotVo> builder=new PageExporterBuilder<>();//PageExporterBuilder  /ExporterBuilder设置报表工具类
。。。。
builder.setHeaders(HEADER_GOODS_IOT.stream().map(item->i18nComp.getI18nText(item, operation.getOpLocale())).collect(Collectors.toList()));  //设置列名
builder.setRowFiller((row, cells) ->{ //匿名内部类实现设置每行数据，接口中只有一个方法，可用lambda
       cells.addText(row.getTenantName()); 
       cells.addText(row.getOrgName());  //支持设置字符串/数字/百分比/时间
	   cells.addDatetime(row.getBillingExpTime(), dateTimeFormat);
});
builder.setPageGetter(new PageGetter<GoodsIotVo>(){ //匿名内部类实现获取某一页的数据和总行数
	@Override
	public int getRowCount() {return goodsIotVendorComp.countByCondition(goodsIotCond);}
	@Override
	public List<GoodsIotVo> getPage(int pageNum, int pageRows) {
                    goodsIotCond.setOffset((pageNum - 1) * pageRows);
                    goodsIotCond.setLimit(pageRows);
                    return goodsIotVendorComp.page(goodsIotCond).getList();//调用的就是查询
   }
});
return builder.build();  //成为二进制流
//PageXlsxExporter，单线程分页查询, 按查询顺序写入xlsx表格. 用SXSSFWorkbook, 内存缓存100行, 不会有内存溢出的风险
	// 初始化表格样式
    CellStyle headerStyle = ExcelStyle.getHeaderStyle(sheet.getWorkbook()); //头背景色/字体
    CellStyle textStyle = ExcelStyle.getTextStyle(sheet.getWorkbook());  //字体
    CellStyle percentStyle = ExcelStyle.getPercentStyle(sheet.getWorkbook());
。。。
    
```

##### 大数据导出

导出方法：

​	使用POI高级版功能(SXSSFWorkbook)  3.8版本以上（SXSSFWorkbook只支持.xlsx格式，不支持.xls格式 07），解压缩以及解压后存储都是在内存中完成的，内存消耗依然很大

​	使用国内开源工具包Hutools实现

​	使用EasyExcel（需要最低POI版本是3.17）

多线程查询：

​		大多数情况下并不需要查询所有的数据，而是通过分页或缓存。但是仍有这样的场景，比如需要导出一大批数据到excel中，导出数据之前，要先把数据查询出来。数据量一旦过大，单线程查询数据将会成为瓶颈，可用多线程来查询。

​		多个线程一起查，单个查询会变慢。线程数量过大时，会进行频繁得上下文切换

**思路：**

- 查询表的数据总量
- 不同的线程查询不同分段的数据
- 将各个查询数据写到表格中

报错：

CommunicationsException, druid version 1.2.8, jdbcUrl : jdbc:mysql://。。。, testWhileIdle true, idle millis 74939, minIdle 0, poolingCount 7, timeBetweenEvictionRunsMillis 60000, lastValidIdleMillis 74939

com.mysql.jdbc.exceptions.jdbc4.CommunicationsException: Communications link failure

原因：Druid连接不可用，与 MySQL 的通信链接失败。

解决：Druid连接配置了testonborrow=true，每次应用从连接池获取连接都会先判断这个连接是否有效，无效不会用这个连接。若配置了 timeBetweenEvictionRunsMillis 的，理论上应该是会定时对连接池中的连接做检查的，若实际结果没有，yml配置的druid参数失效了。必须配置：keep-alive: true

http调用异常也是一个道理， 使用了已经关闭的连接



报错：

未及时向浏览器返回数据，导致504超时。

解决：

不用同步返回，可使用异步方式（可以使用最简单的spring中的`Async`注解，加上了这个注解的方法可以立马返回响应结果，也可以交给定时任务执行，先返回个成功）。

1. 编写异步接口，该接口负责接收客户端的导出请求，然后开始执行导出（注意：这里的导出不是直接向客户端返回，而是下载到服务器本地），只要下达了导出指令，就可以马上给客户端返回一个该excel文件的唯一标志（用于以后查找该文件），接口结束。
2. 编写excel状态接口，客户端拿到excel文件的唯一标志之后，开始定时轮询调用该接口检查excel文件的导出状态
3. 编写从服务器本地返回excel文件接口，如果客户端检查到excel已经成功下载到到服务器，这时就可以请求直接下载文件了

```java
//一个sheet不压缩
Workbook excel = new SXSSFWorkbook(DFT_CACHE_LINE);  // 创建表格    
Sheet sheet = excel.createSheet(getSheetName()); // 创建Sheet
initSheet(sheet);
excel.write(out);
excel.close();

//一个excel多sheet压缩
ZipOutputStream zipOutputStream = null;
Workbook excel = null;
try {
  zipOutputStream = new ZipOutputStream(out);
  excel = new SXSSFWorkbook(DFT_CACHE_LINE);
 for (int curSheetNum = 1; curSheetNum <= sheetCnt; ++curSheetNum) {
	Sheet sheet = excel.createSheet("sheet" + curSheetNum);  // 创建Sheet
 	initMultiSheet(sheet, rowCnt, curSheetPageCnt, prePageNum);
 }
ZipEntry z = new ZipEntry("download" + ".xlsx");
zipOutputStream.putNextEntry(z);
excel.write(zipOutputStream);... }

//多个excel压缩
ZipOutputStream zipOutputStream = null;
Workbook excel = null;
try {
 zipOutputStream = new ZipOutputStream(out);
 for (int curSheetNum = 1; curSheetNum <= sheetCnt; ++curSheetNum) {
      excel = new SXSSFWorkbook(DFT_CACHE_LINE);
      Sheet sheet = excel.createSheet("sheet" + curSheetNum);
	  initMultiSheet(sheet, rowCnt, curSheetPageCnt, prePageNum);
	  ZipEntry z = new ZipEntry(curSheetNum + ".xlsx");
      zipOutputStream.putNextEntry(z);
//write()方法会自动关闭传入的参数，不能直接传zipOutputStream，要用ByteArrayOutputStream对象将流写入zip对象中
      ByteArrayOutputStream bos = new ByteArrayOutputStream();
      excel.write(bos);
      bos.writeTo(zipOutputStream);。。。}
   //zipOutputStream.closeEntry();  //会报错
```



支持下载zip文件：

 .xlsx 文件已经是压缩的 zip 文件，因此将其放在 .zip 文件中可能不会压缩太多

```java
ZipOutputStream zos = new ZipOutputStream("目标输出流");
zos.putNextEntry(new ZipEntry("AnExcelFile.xlsx"));
ByteArrayOutputStream bos = new ByteArrayOutputStream();
workbook.write(bos);
bos.writeTo(zos);
zos.closeEntry();
// Add other entries as needed
zos.close();
   

 //csv
ZipOutputStream zos = new ZipOutputStream(os);
zos.putNextEntry(new ZipEntry("download" + ".csv"));
csvFormat = CSVFormat.DEFAULT.withHeader(headers.toArray(new String[headers.size()]));
  csvPrinter = new CSVPrinter(new OutputStreamWriter(zos, "GBK"), csvFormat); 
     csvPrinter.printRecord(line);
     //写完要flush
     csvPrinter.flush();
    //关闭
     csvPrinter.close();
     zos.close();
```

单线程查询优化：

```java
esayExcel:
		String fileName = URLEncoder.encode(String.format("%s-(%s).xlsx", "订单支付数据", 	UUID.randomUUID().toString()),StandardCharsets.UTF_8.toString());
        rsp.setContentType("application/force-download");
        rsp.setHeader("Content-Disposition", "attachment;filename=" + fileName);
        ExcelWriter writer = new ExcelWriterBuilder()
                .autoCloseStream(true)
                .excelType(ExcelTypeEnum.XLSX)
                .file(rsp.getOutputStream())
                .head(GoodsIotExportVo.class)
                .build();
        // xlsx文件上上限是104W行左右,这里如果超过104W需要分Sheet
        WriteSheet writeSheet = new WriteSheet();
        writeSheet.setSheetName("target");
        long lastBatchMaxId = 0L;
        goodsIotCond.setLimit(10000);
        for (; ; ) {
            List<GoodsIotExportVo> list = goodsIotVendorDao.page4Export(goodsIotCond,lastBatchMaxId, operation.getOpLocale());
            if (list.isEmpty()) {
                writer.finish();
                break;
            } else {
                lastBatchMaxId = list.stream().map( item-> Long.parseLong(item.getGoodsId())).max(Long::compareTo).orElse(Long.MAX_VALUE);
                writer.write(list, writeSheet);
            }
        }

		select a.goods_id
        from rs_goods_iot_vendor a
        inner join rs_goods b
        on a.goods_id = b.goods_id
        where a.goods_id &gt; #{lastBatchMaxId}
        <include refid="And_Columns_VendorDynamic"/>
        <if test="cond.limit != null and cond.limit != ''">
            limit #{cond.limit,jdbcType=INTEGER}
        </if>
```

##### IO流

![在这里插入图片描述](/20190911092828933.png)

```java
节点流:
数 组: ByteArrayInputStream
文 件:   FileInputStream
字符串:   StringReader StringWriter
管 道: PipedInputStream

处理流:
缓冲流：BufferedInputStrean BufferedReader
转换流：InputStreamReader

关闭顺序：
1、一般情况下是：先打开的后关闭，后打开的先关闭。
2、如果流a依赖流b，应该先关闭流a，再关闭流b。
try {
     InputStream inputStream = new FileInputStream("D:\\test\\test.txt");
     InputStreamReader inputStreamReader = new InputStreamReader(inputStream);
     BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
     System.out.println(bufferedReader.readLine());
} catch (FileNotFoundException e) {
     e.printStackTrace();
} catch (IOException e) {
     e.printStackTrace();
}finally {
    try {
        bufferedReader.close();
        inputStreamReader.close();
        inputStream.close();      
    } catch (IOException e) {
        e.printStackTrace();
    }

3、可以只关闭处理流，不用关闭节点流。处理流关闭的时候，会调用其处理的节点流的关闭方法。如果先关闭节点流，再关闭处理流，会抛出IO异常

4、指向内存的流可以不用关闭，指向硬盘/网络等外部资源的流一定要关闭   
ByteArrayInputStream内部实现是一个byte数组。并没有占用硬盘，网络等资源。就算是不关闭，用完了垃圾回收器也会回收掉
```



#### 文件上传

**文件 =》文件=》一行行数据：**

二进制数据，由Channel 转化为输入流，BufferedReader输出流读取输入流校验，连接Sftp服务器，输入流put入channel，上传到Sftp服务器

连接Sftp服务器，开启session和channel，先用outstream接住channelMap的get的channel再返回一个文件输入流，BufferedReader输出流读取输入流，一行行读取给submit给线程run

=》AbstractJobRunner<T> （run方法，beforeHandle，handleJobItem抽象方法）

=> **CreateResFileJobRunner资**源入库处理类（execute，downloadAndRead，从Sftp服务器下载文件, 返回一个文件输入流，输出流一行一行读取submit给线程run，run中会有 handle）

=》CreateResFileJobRunner（beforeHandleJob只执行一次， handleJobItem 每次submit都会执行多次）

```java
上传：
//SpringMVC 用的是的 MultipartFile 二进制数据 来进行文件上传 首先要配置MultipartResolver:用于处理表单中的file，可在启动类中配置：
	@Bean(name = "multipartResolver")
    public MultipartResolver multipartResolver() {
        CommonsMultipartResolver resolver = new CommonsMultipartResolver();
        resolver.setDefaultEncoding("UTF-8");
        resolver.setResolveLazily(true);//resolveLazily属性启用是为了推迟文件解析，以在在UploadAction中捕获文件大小异常
        resolver.setMaxInMemorySize(409600);  //单位为字节
        resolver.setMaxUploadSize(1024 * 1024 * 1024);//上传文件大小10M 10*1024*1024
        return resolver;
    }
//controller
@PostMapping("/import/file")
    public RestResponse upload(@RequestParam("file") MultipartFile file) throws IOException {
        LineFileProfile lineFileProfile = this.checkAndUploadFile(file, importResourceFileComp, FunctionCode.RS_RES_IMPORT_UPLOAD, new String[]{"ICCID", "IMSI", "MSISDN"});
    }

//TokenBasedRest:
//checkAndUploadFile校验
将文件变为流：
ByteBuffer fileBytes = ByteBuffer.allocate((int) file.getSize());  //用于分配缓冲区数组
Channelchas.newChannel(file.getInputStream()).read(fileBytes);  //MultipartFile 读入数组
InputStream stream=new ByteArrayInputStream(fileBytes.array())； 
FilenameUtils工具类常用方法：
getExtension(String path)：获取文件的扩展名；
getName()：获取文件夹或文件的名称；
rename()： 重命名指定文件或目录
mkdir()： 创建目录
//LineFileProfile
System.getProperty("");  //获取指定键指示的系统属性，"line.separator"为获取操作系统对应的换行符
//FileUtils.getLineIterator 	//上传时判断是不是某类型文件并获得每行数据 
BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream)) //读取
WorkBook：工作簿，一个excel就是一个工作簿，一个工作簿含有多个工作表sheet
Workbook workbook = getWorkbook(inputStream, fileType);
final Sheet sheet = workbook.getSheetAt(0);
Row row = sheet.rowIterator().next();
//AbstractFileCheckComp
getValidators //根据title选校验规则
ImportResourceFileComp  //map设置好可选的校验规则
Validator校验规则：数据格式，本个输入有无重复。lambda表达式实现匿名内部类从而实现接口

//SftpFileSystemCompImpl  上传流文件到Sftp服务器
channel.put(stream, tempFileName, null == result.getSize() ? new SftpProgressMonitor() {}
 
upload方法：            
    创建SFTP连接 createSftpChannel方法：
    WEB app 需要使用JSCH来通过密码/密钥文件的方式进行SFTP/SSH访问远程LINUX机器，
     JSch jsch = new JSch();  jsch.addIdentity()  //设置私玥密码          
     Session session = jsch.getSession(username, host, port); //生成session，设置密码超时等 
    
    new UploadOperation 匿名内部类实现在本类中的接口           

    ChannelSftp方法：
    put()： 文件上传
   // get()： 文件下载
    cd()： 进入指定目录
   
//SftpProperties 属性类 和配置类：上传时路径配在application.yml，下载路径查ParamConfig
@Configuration
public class SftpConfiguration {
    @ConfigurationProperties(prefix = "serv.webmvc.sftp")
    @Bean("sftpProperties")
    public SftpProperties newSftpProperties() { return new SftpProperties();}
    @Bean("sftpFileSystemService")
    public SftpFileSystemComp newSftpFileSystemService(@Qualifier("sftpProperties") SftpProperties properties) { return new SftpFileSystemCompImpl(properties); }
}
 //配置文件时就调用了构造方法，完成赋值成为Bean           
public class SftpFileSystemCompImpl implements SftpFileSystemComp {
    private SftpProperties config;
    public SftpFileSystemCompImpl(SftpProperties properties) {
        this.config = properties;
    }
}
 
            
下载读取：
//AbstractFileJobRunner<T>
 public void onReadLine(int lineNumber, String content) {   //一行行执行
  BatchItem itemLog = initialItemLog(context, operation, content, index.get());}
// sftpcomp
 downloadAndRead： (参数里有个接口） 调用时候可以用lambda表达式，即匿名内部类来实现接口，这样可以等到用的时候才实现接口方法，一行行读取
      download：打开SftpSession，从Sftp服务器下载文件, 返回一个文件输入流
		for (int i = 0; i < sessions.size(); i++) {
             ChannelSftp channel = (ChannelSftp)sessions.get(i).openChannel("sftp");
              channel.connect();  //连接
              this.cd(channel, filePath, CSP_SFTP_PATH);  //进入目录
               channelMap.put(sessions.get(i).getHost(), channel);
         }
      读取： hannel.get(fileName, outStream);
            InputStream stream=new ByteArrayInputStream(outStream.toByteArray())
            reader = new BufferedReader(new InputStreamReader(input))
//CreateResFileJobRunner
 String[] array = itemLog.getLineContent().split("\\|");
```

Session是会话，比如打电话，从拨号到挂断这就是一个Session；Channel是通道，表示是使用联通，移动，电信

JSch是Java Secure Channel的缩写， JSch是一个SSH2的纯Java实现。它允许你连接到一个SSH服务器，并且可以使用端口转发，X11转发，文件传输等，当然你也可以集成它的功能到你自己的应用程序。

SFTP是Secure File Transfer Protocol的缩写，安全文件传送协议。SFTP 为 SSH的一部份。

SFTP 和 FTP 区别：

效率：FTP>SFTP；安全：SFTP>FTP

SFTP 和 FTP 最主要的区别就是 SFTP 有私钥，也就是在创建连接对象时，SFTP 除了用户名和密码外还需要知道私钥 privateKey 
FTP 端口号是21，22是  SFTP



上传文件太大会报：413 Request Entity Too Large 错误，

错误原因：Nginx默认的request body为1M。

解决：

找到自己主机的nginx.conf配置文件，打开
在http{}中加入 client_max_body_size 10m;
然后重启nginx



poi 的大坑，读取数据时，不会过滤空行：

原来有30条数据，直接delete删除最下面的20条，但当使用poi 解析的时候，还是读取到30行，删除的20行为空数据，因为不是右键删除的，还是空数据。

![a04c3ccd9e9e66518d65b422c756762e.png](/a04c3ccd9e9e66518d65b422c756762e.png)

解决：

一：进行上传读取Excel时，不要修改数据区外的单元格格式

二：循环判断当前行的每一个列是否为空，如果均为空，则为空行，否则为非空行





### Page分页

Page是一个ArrayListList，PageInfo是一个对象，能获取到的数据比Page多

```java
//将page内的对象转换成另一个对象
PageInfo<BatchItem> pageInfo = batchItemDao.pageByCondition(condition, offset, limit);
Page<BatchItemLogDto> page = new Page<>(new int[]{offset, limit}, true);   page.addAll(pageInfo.getList().stream().map(this.getBatchItemLogMapper(operation)).collect(Collectors.toList()));
page.setTotal(pageInfo.getTotal());
return page.toPageInfo();
private Function<BatchItem, BatchItemLogDto> getBatchItemLogMapper(Operation operation) {
   return item -> {
            BatchItemLogDto dto = new BatchItemLogDto();
            dto.setBatchItemId(item.getBatchItemid());
            dto.setStatusName(dictComp.getDictValue(item.getStatus(), opLocale()));
            dto.setOpStaff(staffComp.getStaffName(item.getOpStaff()));
            return dto;
        };
    }
//BatchItemDao，分页放在dao
PageHelper.offsetPage(offset, limit, count);//会在mybatis查询的时自动带上limit(offset,limit);count默认true；false不计算总页数
List<BatchItem> list = getMyMapper().listByCondition(partition, cond);
return new PageInfo<>(list);


//若最终page返回的对象是一样的，只在返回的对象中加属性值，可以不用new Page<>(new int[])
PageInfo<AcceptLogVo> pageInfo = acceptLogComp.pageLogAndResvByGoodsId(req.getGoodsId(),...)
for(AcceptLogVo item:pageInfo.getList()){
     item.setStaffName(staffComp.getStaffName(item.getOpStaff()));
}
return ResponseUtils.buildResponsePage(pageInfo);


//返回空列表
if (null == list || list.isEmpty()) return Collections.emptyList();


//查询多个表并分页，先查出主键，再由主键查具体的内容
<select id="listAcceptIdByCondition" resultType="java.lang.String">
       select op_accept
        from bs_accept_res
        where 1=1   <include refid="And_Columns_Dynamic"/>
        <if test="limit > 0">
            order by op_accept desc limit 0, ${offset + limit}
        </if>
    union all
        select op_accept
        from bs_accept_log   <include refid="And_Columns_Dynamic"/>
        <if test="limit > 0">
            order by op_accept desc limit 0, ${offset + limit}
        </if>
     order by op_accept desc
     <if test="limit > 0">
         limit #{offset},#{limit}
     </if>
</select>
<select id="listLogAndResvByAcceptIdList" resultType="site.neware.csp.database.entity.vo.AcceptLogVo">
        SELECT * FROM bs_accept_log
        WHERE op_accept IN
        <foreach collection="acceptIdList" item="acceptId" open="(" close=")" separator=",">
            #{acceptId,jdbcType=VARCHAR}
        </foreach>
     UNION ALL
        SELECT * FROM bs_accept_resv
        WHERE op_accept IN ...
     order by opAccept desc
</select>
<select id="countLogAndResvByCondition" resultType="Long">
 select (select count(0) from rs_goods) +(select count(0) from rs_goods_iot_vendor) as total
</select>
```

一个本来没有做分页的查询，通过输出日志看到在执行查询的时候却用了分页。导致查询到错误的数据。

在拦截器中，`PageHelper` 判断是查询语句是否需要分页，是否需要count查询，分页数据拼接查询等操作。

插件PageHelper，先获取到的Page对象，用ThreadLocal封装的变量，这个变量在当前线程内有效，内封装了分页查询要用的参数，以及查询的结果。之后会调用mapper执行查询。若没有立即调用mapper做查询，而是执行了一些其他的内容。正是这些其他的内容抛出了异常，导致后面没有调用mapper做查询，就没有在finally中清空ThreadLocal变量的值，下次使用该线程时就会有以前的变量。

**解决方法：**

- `PageHelper.startPage`后面直接跟上SQL查询语句
- 在每次请求完成后，调用pagehelper提供的清理ThreadLocal方法，或者在每次请求进来的时候，在拦截器中清理掉线程缓存数据。



### 注解

#### 概念

```json
//元注解
@Target：用来限制注解的使用范围：方法，包。。
@Retention：指定其所修饰的注解的保留策略，三种：作用于源代码（编译器可读取注解）、作用于类文件（在类文件中可读取注解）、作用于运行过程（用反射技术读取注解）
@Document：是一个标记注解，用于指示一个注解将被文档化
@Inherited：使父类的注解能被其子类继承

//redis和sft配置：
@Configuration把一个类作为一个IoC容器，它的某个方法头上如果注册了@Bean，就会作为这个Spring容器中的Bean。 
@Primary：可以放在方法或类上，多个Bean相同类型或一个接口存在多个实例bean，被注解为@Primary的Bean将作为首选者（也可在注入时用@Qualifier）
@ConfigurationProperties(prefix = "serv.webmvc.sftp") 获取配置文件中的的属性值注入类
@Value("${serv.webmvc.cors.allowedOrigins}") 获取配置文件中的的单个属性值
@Bean("sftpProperties")


//Bean的产生，mapper.xml=>mapper=>dao(@Repository)=>comp/server(@Service)
@Repository、@Service、@Controller，它们分别对应存储层Bean，业务层Bean，和展示层Bean。
@Component 通常作用于类，通过类路径扫描来自动侦测以及自动装配到Spring容器。 默认Bean名称为类名首字母小写
@Bean 注解作用于方法，标有该注解的方法中定义产生这个bean。默认Bean的名字为方法名。


//Bean注入
@Resource 默认按名称装配，当找不到才会按类型装配。若指定了name找不到则会报错。  
@Autowired 默认按类型装配，找不到报错，有多个则按名称注入，bean名称/beanId具有唯一性，若有多个报错
@Qualifier(“personDaoBean”) 可配合@Autowire按名称装配，如：一个接口存在多个实例配合使用，可以放在方法或类上。@Qualifier是根据bean id 获取指定 bean, 不受@Primary影响


@SuppressWarnings("rawtypes")   //忽略指定的警告
@Autowired
private RedisTemplate redisTemplate;

@CacheConfig(cacheNames = CacheConsts.CACHE_PRIVILEGE_ORG)都可置在类上
@Cacheable(unless="#result == null")  当返回值为null时，就不缓存

@SuppressWarnings("unchecked")
@SuppressWarnings("rawtypes")// 告诉编译器不用提示使用基本类型参数时相关的警告信息

@Scope注解 作用域 
@Lazy(true) 表示延迟初始化

    
@PostConstruct被用来修饰一个非静态的void（）方法。顺序：
Constructor(构造方法) -> @Autowired(依赖注入) -> @PostConstruct(注释的方法)->init（）方法
完成依赖注入后。一般标在某些缓存对象的初始化方法，以确保在对象使用之前可以将任务执行效果使用起来。



 //启动类
@EnableAsync: 启动类上添加@EnableAsync，我们可以在需要异步执行的方法上面添加@Async注解，即可实现异步。
@EnableAspectJAutoProxy  基于注解的方式实现AOP,两个参数作用分别为：一个是控制aop的具体实现方式,为true的话使用cglib,为false的话使用java的Proxy，默认为false。第二个参数控制代理的暴露方式,解决内部调用不能使用代理的场景，默认为false。
@EnableTransactionManagement 开启事务支持后，然后在访问数据库的Service方法上添加注解@Transactional 便可。
    

//请求：
@PostMapping("/q")《=》@RequestMapping(value = "/q", method = RequestMethod.GET)//不写method 就支持所有的请求方式
@RequestBody 加对象
@RequestParam(value = "tenant", required = false) 请求参数
@PathVariable("email") 可变的路径参数，@PostMapping(value = "/list/{tenantStatus}")
@RestController 返回json数据    @ResponseBody+controller

@Transient 表示该属性并非一个到数据库表的字段的映射，不用@Column(name = "USER_POSITION"),加了这个注解这个属性就不会序列化

@JSONField(serialize = false) 项目使用fastJson时，返回响应参数体的时候去除某个字段，例如账号查询时,是不能返回密码字段
@JsonIgnore 注解是项目中使用 Jackson的时候使用，在方法或属性上

@JsonIgnoreProperties(ignoreUnknown = true)  使json 字符串中的字段数量多于类的字段不报错

//事务：不应该运行在事务中，如果存在当前事务，当前事务被挂起
@Transactional(transactionManager = "cspTransactionManager", propagation = Propagation.NOT_SUPPORTED)
```



#### 注解使用注意事项

@EnableAsync  默认情况下，Spring将搜索相关的线程池定义（config），搜索名为“taskExecutor”的bean，通过spring给我们提供的ThreadPoolTaskExecutor就可以使用线程池。如果没有定义或无法解析，则将使用SimpleAsyncTaskExecutor来处理异步方法调用。
如下方式会使@Async失效：
一、异步方法使用static修饰
二、异步类没有使用@Component注解（或其他注解）导致spring无法扫描到异步类
三、异步方法不能与异步方法在同一个类中
四、类中需要使用@Autowired或@Resource等注解自动注入，不能自己手动new对象
五、如果使用SpringBoot框架必须在启动类中增加@EnableAsync注解
六、在Async 方法上标注@Transactional是没用的。 在Async方法调用的方法上标注@Transactional有效。
七、调用被@Async标记的方法的调用者不能和被调用的方法在同一类中不然不会起作用！！！！！！！
八、使用@Async时要求是不能有返回值的不然会报错的 因为异步要求是不关心结果的



自定义的注解，@Cacheable，@Async，@Transactional 的方法被同类中的其他方法调用不生效的原因：
没有用到代理类导致，Spring是采用动态代理(AOP)实现对bean的管理和切片，它为我们的每个class生成一个代理对象。代理对象调用时，可以触发切面逻辑，才可以切进去执行切面里编写的代码。而在同一个class中，调用的是原对象的方法，没经过拦截器。

解决方法：

①启动类或本类上添加@EnableAspectJAutoProxy(proxyTargetClass = true, exposeProxy = true)，有性能损耗

②获取本对象的代理对象，再进行调用。如：在xxxServiceImpl中，用(xxxService)(AopContext.currentProxy()).方法，来调用

③用@Autowired 注入自己 ，然后在用注入的bean调用自己的方法也可以（若使用了AOP则三级缓存value的函数式接口（实例化但未初始化对象工厂）会生成代理对象，最后容器中只有代理对象）

④将两个方法写入不同的类



@Autowired  RedisTemplate注入不来

static 



#### 自定义注解

```json
//定义注解
@Target(ElementType.METHOD) //说明了注解 所修饰的对象范围，可被用于包，方法
@Retention(RetentionPolicy.RUNTIME)//会被保留的阶段，RUNTIME：将被JVM保留,能在运行时被JVM或其他使用反射的代码所读取和使用.
public @interface TokenNeedless

@TokenNeedless //不需要登录，一般放在登录接口
@TokenNeedonly(opcode)//是否登录（有token）设置opTime，OpAccept，opcode,将operation设置到域中：getServletRequest().setAttribute()
@Authentication(code = FunctionCode.CONSOLE_LOGIN_LOGS)//校验role_menu权限有没有opcode，会记录到操作日志中
@OpCode(FunctionCode.IOT_VENDOR_DOWNLOAD) //会设置operation中的opcode
```



### maven配置

maven項目在分模块进行开发时，模块之间只能是单方向依赖/传递依赖，但不可多个模块之间互相依赖





### yml配置

##### yml配置数据源

Druid是Java语言中最好的数据库连接池

Druid经常出现连接不可用的问题：

​	validationQuery: SELECT 1

​	keepAlive的默认值是false，会被回收至只剩0个连接，要keep-alive: true

```yml
spring:
 mybatis:
 #标识数据源名称
  csp:
#   数据源基本配置
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
     #autoReconnect=true未接收数据包时保持活动状态
    url: jdbc:mysql://129.28.81.183:26033/neware_csp_busi?useUnicode=true&characterEncoding=utf8&autoReconnect=true&useSSL=false  
    type: com.alibaba.druid.pool.DruidDataSource
    initialSize: 5
    minIdle: 5
   #最大连接数量
    maxActive: 20
    # 超时等待连接时间
    maxWait: 60000
    #检查连接池中空闲连接的间隔,60秒，这个值一般小于等于maxWait
    timeBetweenEvictionRunsMillis: 60000
    # 连接在池中保持空闲而不被空闲连接回收器线程,(如果有)回收的最小时间值，单位毫秒
    minEvictableIdleTimeMillis: 300000s
    # 检测连接是否有效的sql 
    validationQuery: SELECT 1 FROM DUAL
    # 当true 时候 执行 上面的 select 1，开启保活检测，建议配置为true，不影响性能，并且保证安全性。
    testWhileIdle: true
    # 申请连接时检测,为true 的话拿到连接后会根据validationquery配置的sql检测这个连接是否可用，可用返回给应用，不可用换个连接
    testOnBorrow: false
	#每个连接最多缓存多少个SQL
    maxPoolPreparedStatementPerConnectionSize: 20
#初始化连接池时会将链接数填充到minIdle，连接池中的minIdle数量以内的连接，空闲时间超过minEvictableIdleTimeMillis，则会执行keepAlive操作，打开会一直保持minIdle的数量值
    keep-alive: true
```

##### yml配置属性

```java
   private SftpProperties config;  //构造属性类
   //构造函数注入
   public SftpFileSystemCompImpl(SftpProperties properties) { this.config = properties;}

@Configuration
public class SftpConfiguration {
    @ConfigurationProperties(prefix = "serv.webmvc.sftp")
    @Bean("sftpProperties")
    public SftpProperties newSftpProperties() {
        return new SftpProperties();
    }
    @Bean("sftpFileSystemService")
    public SftpFileSystemComp newSftpFileSystemService(@Qualifier("sftpProperties") SftpProperties properties) {
        return new SftpFileSystemCompImpl(properties);
    }
}
```



### 英文

```
Suffix 后缀
interval 间隔
cipher 密码
可见 visible
商人 merchant
Tenant 租户
recursion 递归
Thaw 解冻
tariff 资费
privillege 特权
Grant & Deny  授予与拒绝
duplicate 复制
Currency 货币
Authentication 验证
Encrypt加密
Validate 证实
suspend 暂停
recover 恢复
terminate 终止
usage 用法
provisioning 供应
debit 借方
Amount 数量
Volume 体积
comboBox 组合框
Dynamic 动态的
Vendor 供应商
recycling 回收
DOMAIN 领域
Via 通过
Cert 证书
ASYNC 异步 
regexp 正则
Earliest 最早的
optimize 优化
delegate 代表
standby 待用的 / 依靠
```



### session/token

```java
login登录请求不用token校验，返回生成自定义的session类并生成日志（token在返回的json中的key是"token",浏览器中是"Authorization"）

class SessionManager {
    //，使用jjwt，生成token，同时设置进请求头？
    JwtBuilder builder = Jwts.builder()
                .setId(session.getLoginId())
                .setSubject(session.getEmail())
                .setClaims(claims)   //声明
                .setIssuedAt(now)
                .signWith(SignatureAlgorithm.HS256, generateSecretKey());//签名算法
     builder.setExpiration(exp);//过期时间设置，查数据库配置

//session,toke放入缓存,设置有效时间
    tokenSessionCache，
     emailTokenCache  //错误次数，是否可重复登录，退出清空
}

//登出时将redis中对应的token删除，这样保证不会别人拿到token就可以随意登录，哪怕token在jwt生成的有效期内

Interceptor拦截器，@TokenNeedonly自定义注解来标识一些方法，需要校验
Operation operation = new PortalOperation(session);
//没有发送请求，没有操作，不获取session，不刷新token的ttl
```



### Interceptor拦截器

```java
得到session，封装成operation

//在请求调用（Controller方法调用之前） 1、Token失效验证 2、Token权限验证 3、更新Token失效时间
public class AuthenticationInterceptor implements HandlerInterceptor {
String token = HttpRequestUtils.getToken();
@TokenNeedonly和@Authentication两个都写只生效一个

    
//RestRequestLogInterceptor，由拦截器Interceptor插入operLog日志表
//接口上有@Authentication注解才会记录操作日志，@OpCode(code)或者@Authentication(code)或者@TokenNeedonly(code)会修改opCode，最终opCode可以写入FuncCode中,opCode就是前端操作按钮的menu_func
```



### 缓存

程序运行中，在内存保持一定时间不变的数据就是缓存。把要多次使用的东西存在一个变量/Map，里面放着一些数据，就已经是个缓存了。

#### redis

一个Redis实例有[0-15]共16个database，可以直接使用redis自带的RedisTemplate写缓存。

StringRedisTemplate继承RedisTemplate，两者的数据是不共通的。StringRedisTemplate默认采用的是String的序列化策略。RedisTemplate默认采用的是JDK的序列化策略，使用时要重新指定序列化

BatchJobComp的batchId，redisLock使用RedisClient实例

```java
@Configuration
@EnableCaching(proxyTargetClass = true)
public class RedisConfig extends CachingConfigurerSupport {
    @Bean(name = "redisTemplate")
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(factory);
        redisTemplate.setDefaultSerializer(new GenericJackson2JsonRedisSerializer());
        redisTemplate.setEnableTransactionSupport(false);
        redisTemplate.afterPropertiesSet();
        return redisTemplate;
    }
    。。。
}


@Component
public class RedisClient {   //RedisTemplate工具类，直接用
@SuppressWarnings("rawtypes")
@Autowired
private RedisTemplate redisTemplate;
 public boolean set(final String key, Object value, Long expireTime) {
        boolean result = false;
        try {
            ValueOperations<Serializable, Object> operations = redisTemplate.opsForValue();
            operations.set(key, value);
            if (expireTime > 0) {
                redisTemplate.expire(key, expireTime, TimeUnit.SECONDS);
            }
            result = true;
        } catch (Exception e) {
            logger.error("---Redis Set key:{} Object:{} ERROR: {}", key, value, e);
            return false;
        }
        return result;
    }
    
//直接用yml里配置的
@Value("${spring.redis.keyprefix}")  
protected String ACCEPT_PREFFIX_KEY;

redisLock 工具类调用 RedisClient工具类的 redisTemplate的
    conn =RedisClient.getRedisTemplate().getConnectionFactory().getConnection();
    conn.setNX(name, UUid)
    expire(name,releaseTime)
```



#### Spring Cache

方法查到的结果，和RedisTemplate用一个redis库，redisTemplate和springCache怎么指定database库

改了方法的逻辑/数据库 要刷新缓存才生效；

用了缓存的查询，改变表之后还要清除缓存。

```java
原理：Spring Cache中如果加了缓存相应的注解，就会走到这个拦截器上 ，执行拦截器继承类的execute方法。使用了AOP的Bean，会生成一个代理对象，实际调用的时候，会执行这个代理对象的一系列的Interceptor，。
Spring会自动检测我们是否引入了相应的缓存框架，如果我们引入了spring-data-redis，Spring就会自动使用RedisCacheManager，RedisCache
使用：加依赖，启动类加上@EnableCaching，缓存的方法上面添加@Cacheable注解
springcache的缓存是看方法名和返回值确定的，缓存不关心方法的执行逻辑，参数不同，缓存结果是不同的        
//原生的spring cache默认永不过期，设置要配置
@Configuration
@EnableCaching(proxyTargetClass = true)
public class RedisConfig extends CachingConfigurerSupport {
    @Value("${spring.redis.time-to-live:0}")
    private Long expireTime;
    @Autowired
    private CacheDao cacheDao;
    @Bean(name = "cacheManager")
    public CacheManager redisCacheManager(RedisConnectionFactory redisConnectionFactory) {
        Duration duration = Duration.ofMillis(expireTime);
        RedisCacheConfiguration redisCacheConfiguration = ;
        Map<String, RedisCacheConfiguration> cacheConfigurationMap = new HashMap<>();
        List<Cache> cacheList = cacheDao.selectAll();
        for (Cache cache : cacheList) {
                cacheConfigurationMap.put(cache.getCacheCode(), redisCacheConfiguration);
        }
        return RedisCacheManager.builder(redisConnectionFactory)
                .build();
}
    
@Component
@CacheConfig(cacheNames = CacheConsts.CACHE_TARIFF_CATALOG)
public class CatalogCompImpl extends AbstractCacheComp implements CatalogComp {
    @Cacheable(unless = "#result == null") 
    @Override
    public String getBrandName(String brand) {}
}

//SpringCache包含两个顶级接口，Cache（缓存）和CacheManager（缓存管理器），CacheManager去管理一堆Cache，使用redis时，实际上是操作redisTemplate
@Autowired
protected CacheManager redisCacheManager;
Cache cache = cacheManager.getCache(cacheCode);//即CacheConsts.CACHE_TARIFF_CATALOG
cache.clear();
Collection<String> cacheNames = cacheManager.getCacheNames();  //获取全部缓存名

```

springcache失效：

① @Cacheable标注的方法，如果其所在的类实现了某一个接口，那么该方法也必须出现在接口里面，否则cache无效

② 同个类的方法调用 @Cacheable标注的方法

**springCache数据库一致性解决方案**

![image-20220331202831491](/image-20220331202831491.png)



#### Redisson

Redisson为每个操作都提供了自动重试策略，当某个命令执行失败时，Redisson会自动进行重试。自动重试策略可以通过修改参数来进行优化调整。当等待时间达到指定的时间间隔以后，将自动重试下一次。全部重试失败以后将抛出错误。Redisson实例本身和Redisson框架提供的所有对象都是线程安全的。Redisson框架提供的几乎所有对象都包含了同步和异步相互匹配的方法。

Redisson的分布式的RMapCache Java对象在基于RMap的前提下实现了针对单个元素的淘汰机制。同时仍然保留了元素的插入顺序。映射缓存(MapCache)它能够保留插入元素的顺序，并且可以指明每个元素的过期时间（专业一点叫元素淘汰机制）。另外还为每个元素提供了监听器，提供了4种不同类型的监听器。有：添加、过期、删除、更新四大事件。

1.Redisson组件，缓存映射MapCache只需从缓存中查询，不需再查数据库。

2.可以解决定时任务处理不及时的问题，通过实现ApplicationRunner, Ordered两个接口，可以在应用启动和运行期间，不间断监听，并执行我们所需的业务逻辑代码。

3.解决了批次查询的数据量可能过大占用过多的缓存的问题

sequence，session（token，email），使用指定的businessRedissonClient

```java
@Configuration
public class RedisFileConfiguration { //配置redis文件引入RedissonClient和RedisTemplate
    @Autowired
    private ApplicationContext ctx;
    /**
     * Cache Redis
     */
    @Value("${spring.redis.redisson.file}")
    private String cacheConfigFile;
    /**
     * Business Reids
     */
    @Value("${spring.redis.business-redisson.file}")
    private String businessRedissonConfigFile;

    @Bean("businessRedissonClient")
    public RedissonClient businessRedissonClient() {
        Config config;
        try {
            if (businessRedissonConfigFile == null || businessRedissonConfigFile.length() == 0) {
                businessRedissonConfigFile = cacheConfigFile;
            }
            InputStream is = ctx.getResource(businessRedissonConfigFile).getInputStream();
            config = Config.fromYAML(is);
        } catch (IOException e) {
            throw new IllegalArgumentException("[business-redisson] Can't parse config", e);
        }
        return Redisson.create(config);
    }
    @Primary
    @Bean()
    public RedissonClient cacheRedissonClient() {
        Config config;
        try {
            InputStream is = ctx.getResource(cacheConfigFile).getInputStream();
            config = Config.fromYAML(is);
        } catch (IOException e) {
            throw new IllegalArgumentException(" Can't parse config", e);
        }
        return Redisson.create(config);
    }
}


@Autowired
@Qualifier("businessRedissonClient")
private RedissonClient redissonClient;

map.fastPut("key1", new SomeObject(), 3, TimeUnit.SECONDS)；//key1不存在时，ttl=3s生效
map.fastPut("key1", new SomeObject(), 0, TimeUnit.SECONDS)；//key1存在时，令ttl=0，即永久保存key1，不生效
.remove
.get
updateEntryExpiration

//监听过期就移除
this.tokenSessionCache.addListener((EntryExpiredListener<String, Session>) event -> {
            emailTokenCache.remove(session.getEmail(), token);
        });
        
RSetMultimapCache<String, String> multimap= redissonClient.getSetMultimapCache(
multimap.put("1", "a");
multimap.put("1", "b");
multimap.put("2", "e");
multimap.put("2", "f");
multimap.expireKey("2", 10, TimeUnit.MINUTES)
containsKey

//session,toke放入用map缓存，过期时间
 @Autowired
 @Qualifier("businessRedissonClient")
 private RedissonClient redissonClient;
RMapCache<String, Session> tokenSessionCache;
@PostConstruct
public void onPostConstruct() {
 this.tokenSessionCache = redissonClient.getMapCache(SessionRedisKey.TOKEN_MAP.getRedisKey());
    }
tokenSessionCache.put(token, session, 30L, TimeUnit.MILLISECONDS);

 
//sequence的名字,获取并更改,没有会创建:
long l=redissonClient.getAtomicLong(this.getKey(key)).addAndGet(increment)
boolean b=redissonClient.getAtomicLong(this.getKey(key)).compareAndSet(oldVal, newVal)
redissonClient.getMap(this.getKey(key)).addAndGet(field, increment)
redissonClient.getMapCache(" ").addAndGet

Redisson redisson= (Redisson) redissonClient;
boolean b=redissonClient.getAtomicDouble("tt").set(1.22);
```



#### Guava

缓存在很多系统和架构中都用广泛的应用,例如：

- CPU缓存
- 操作系统缓存
- 本地缓存
- 分布式缓存redis
- HTTP缓存
- 数据库缓存

Google的Guava Cache是一个全内存的本地缓存实现（不会把缓存数据放到外部文件或者其他服务器上，而是存放到了应用内存中），它提供了线程安全的实现机制。

**Guava Cache 与 ConcurrentMap**二者很相似， ConcurrentMap 会一直保存所有添加的元素,直到显式地移除。相对地,Guava Cache 为了限制内存占用,通常都设定为 自动回收元素(基于容量回收,定时回收)。

​	Redis 缓存是深拷贝：从 Redis 中获取缓存时，系统中的数据对象是 Redis 缓存的副本，对该对象的任何操作都不会影响 Redis 中的缓存，后续再次获取还是修改之前的数据。
除非执行Redis的更新操作

​	Guava直接获取则是浅拷贝：

**Redis的特点**：缓存在内存，支持多种数据结构，可持久化：AOF / RDB

guava：是单个应用运行时的本地缓存，数据没有持久化存放到文件或外部服务器

Guava Cache有两种创建方式：CacheLoader、Callable

```java
Cache<String, Sequence> sequence =CacheBuilder.newBuilder()
                .refreshAfterWrite(3600l, TimeUnit.SECONDS) //自动定时刷新
                .build(new CacheLoader<String, Sequence>() {
                    @Override
                    public Sequence load(String s) throws Exception {
                        return getMySqlSequenceDefineWithSync(s);
                    }
                });

Callable：可在get的时候指定,如果有缓存则返回;否则运算、缓存、然后返回
 sequence.get(code.getValue(), () -> this.getMySqlSequenceDefineWithSync(code.getValue()))   
```



### Sequence 

```java
//SeqCompImpl，由redis产生增加，要原子引用类
//Callable：可在get的时候指定,如果有缓存则返回;否则运算、缓存、然后返回
@Component
public class SequenceCompImpl implements SequenceComp {
@PostConstruct
Cache<String, Sequence> sequence =CacheBuilder.newBuilder()
                .refreshAfterWrite(3600l, TimeUnit.SECONDS) //自动定时刷新
                .build(new CacheLoader<String, Sequence>() {
                    @Override
                    public Sequence load(String s) throws Exception {
                        return getMySqlSequenceDefineWithSync(s);
                    }
                });

	//此方法返回两个结果可供lambda表达式用
this.getNextIndex(code, 1, (sequence, index) -> {
            sequenceNo.set(this.formatTemplate(sequence, index));
        });  //sequence是数据库的，用guava本地缓存，index使用redis缓存

设置本地缓存定时查库，定时查库的sequence时，比较库的index和reids的index，哪个小就更新哪个
主动查也会更新


//SeqCompImpl，由数据库直接生成增加，要事务
@Component 
public class SeqCompImpl implements SeqComp {
    @Override
    @Transactional(transactionManager = "cspTransactionManager", propagation = Propagation.NOT_SUPPORTED)
    public String getNextSequence(SequenceEnums seqCode) {
        return sequenceDao.selectNextval(seqCode.getValue());
    } 
}  
//seqMapper,flushCache为true，调用致本地缓存和二级缓存被清空。useCache只有select用，默认为true，结果进行二级缓存
<select id="selectNextVal" parameterType="java.lang.String" resultType="java.lang.String" useCache="false" flushCache="true">
		select getnextval(#{seqName})
</select>
    

```



### jdk1.8特性

#### lambda表达式

##### Function

```java
Function<T, R>：输入T类型的参数，运行相关逻辑后，返回R类型的结果。
Function<String, Integer> functionA = x -> {
         Objects.requireNonNull(x, "参数不能为空");
         x = x.replace(" ", "");
         return x.length();
     };
```

##### BICONSUMER＜T,U＞

Java 8 BiConsumer，BiFunction和BiPredicate 函数接口。

BiConsumer不返回任何值，而是执行定义的操作。

BiFunction返回一个值。

BiPredicate执行定义的操作并返回布尔值。

```java
 public String getNextSequence(SequenceEnums code) {
this.getNextIndex(code, 1, (sequence, index) -> { sequenceNo.set(this.formatTemplate(sequence, index));});
 }

//作用，相当于得到返回两个返回值，两个结果可以作为调用此方法的lambda参数，
private void getNextIndex(SequenceEnums code, int size, BiConsumer<Sequence, Long> resultFunction) {
    resultFunction.accept(sequence, result);
}
```

#### stream

集合是空的不会进入stream流中操作

```java
//map 实体类转换为Vo ，给返回的Vo添加其他的属性值
page.addAll(curInfo.getList().stream().map(this.getCurGoodsIotOrderVo(operation)).collect(Collectors.toList()));
private Function<OrderBusiVo, GoodsIotOrderVo> getCurGoodsIotOrderVo(Operation op {
        return item -> {
            GoodsIotOrderVo orderVo = new GoodsIotOrderVo();
			orderVo.set(......);
            return orderVo;
        };
}
//map  实体类转换为String
ApiList.stream().map(AuthApi::getApiCode).collect(Collectors.toList())

//两集合过滤交集，过滤一个元素，只有一个元素findAny比findFirst效率要高
Iterator<String> it = menuCodes.iterator();
 while (it.hasNext()) {
            String menuCode = it.next();
            Optional<Menu> menu = menus.stream().filter(m -> menuCode.equals(m.getMenuId())).findFirst();
            if (!menu.isPresent()) continue;
            MenuDto menuDto = BeanUtils.copyProperties(menu.get(), MenuDto.class);    
//两集合取交集
List<String> finalTableNames =tableNames.stream().filter(item -> allOperLogTableName.contains(prefix+item)).collect(Collectors.toList());
//根据传入filter条件过滤tree
public TreeNode<T> filterHidden() { return this.filter(node -> !node.isHidden()); }
public TreeNode<T> filter(Predicate<TreeNode<T>> filter) {
 if (!AssertUtils.isListEmpty(this.getChildren())) {
  this.setChildren(this.getChildren().stream().filter(child -> null != child.filter(filter)).collect(Collectors.toList()));
        }
  return this;
}          
         
      
addTwo("Node", ".js", (x, y) -> System.out.println(x + y)); // Node.js
static <T> void addTwo(T a1, T a2, BiConsumer<T, T> c) {
c.accept(a1, a2);
}

IntStream，LongStream和DoubleStream。使用新接口可以减轻不必要的自动装箱，从而提效率
LongStream.iterate((index - ((long) sequence.getStepSize() * (size - 1))), i -> i + sequence.getStepSize()).limit(size).forEach(i -> {data.add(this.formatTemplate(sequence, i)); });
 
 Stream.of(Arrays.asList(1, 2, 3, 4, 5, 6), Arrays.asList(8, 9, 10, 11, 12)).flatMap(test -> test.stream()).collect(Collectors.toList())
       
    
//reduce:
一： // 第一个参数是上次函数执行的返回值（也称为中间结果），第二个参数是stream中的元素
int ans = users.stream()
    			.mapToInt(Person::getSalary)//返回数值流，减少拆箱封箱操作，避免占用内存
                 .reduce((x, y) -> x += y);// int
Integer sum = s.reduce((a, b) -> a + b).get();
Integer max = s.reduce((a, b) -> a >= b ? a : b).get();
二: //与方式一相比设置了累加器的初始值10000
     .reduce(10000, (x, y) -> x += y);
三：//如果Stream是非并行时，第三个参数实际上是不生效的。
    //当Stream是并行时，第三个参数就有意义了，它会将不同线程计算的结果调用combiner做汇总后返回，要考虑会不会出现重复数据，传入给Reduce等方法的集合类，需要是线程安全的。
Stream.of("aa", "ab", "c").reduce(new ArrayList<String>(), 
                                  (r, t) -> {r.add(t); return r; },
                                  (r1, r2) -> r1));  //[aa, ab, c]
Stream.of(1, 2).parallel().reduce(4, (s1, s2) -> s1 + s2, (s1, s2) -> s1 + s2));//并行（异步）：4+1=5，4+2=6，5+6=11。若同步：4+1=5，5+2=7
```

#### 日期时间API

不用Date：包含日期+时间

① 打印出可读性差

② 很多方法已经弃用（构造）

③ 线程安全问题：Date 时间类是可变类，在多线程环境下对共享 Date变量进行操作时要 注意线程安全问题

```java
//获取
Date date = new Date();
SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
String time = sdf.parse(result.getTime())
```

用LocalDateTime：

- LocalDateTime：日期+时间
- ZonedDateTime：日期+时间+时区

```java
//获取
LocalDateTime rightNow = LocalDateTime.now();
System.out.println( "当前月份：" + rightNow.getMonth() );
//格式化
String result3 = rightNow.format( DateTimeFormatter.ofPattern("yyyy/MM/dd") );
//解析
LocalDateTime time = LocalDateTime.parse("2002-01-02 11:21",DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm"));
```



### 雪花算法

Twitter的SnowFlake就是分布式id生成算法。

![img](/13382703-b64e38457ddd13e2.jpg)

1. 所有生成的id按时间趋势递增
2. 整个分布式系统内不会产生重复id（因为有datacenterId和workerId来做区分）



### 遍历

for，增强for，迭代器，stream流
Iterator<String> it = list.iterator();while(it.hasNext()){sout(it.next());}
list.forEach(str-> System.out.println(str));

若增强for，或者stream遍历为空的集合，不会进入遍历



### 方法

```java
Integer.parseInt(value);可能会有NumberFormatExcetpion异常，要加入try/catch语句
new String(int)；转为string

                
BeanUtils.copyProperties(target, source);加try/catch
set.addAll(list);
Collections.sort(funcList, Comparator.comparing(MenuFunc::getSort))
PrivilegeStatusEnum.ACTIVE.getValue()


List<String> list=Collections.singletonList("111")
返回的是不可变的集合，但是这个长度的集合只有1，可以减少内存空间。但是返回的值依然是Collections的内部实现类，同样没有add的方法，调用add，set方法会报错
 

SqlSessionFactory().openSession().getConnection().createStatement();
statement.executeQuery(sql)

     
null != condition

MonthTableDao<ENTITY, CONDITION extends DynamicTableName> 


//启动类，继承。。就可以使用外部tomcat，自己可以设置端口号，项目名。不需要用外部tomcat的话继承不继承都可以
public class BootPortalApp extends SpringBootServletInitializer {
    public static void main(String[] args) { SpringApplication.run(BootPortalApp.class, args); }
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {  return builder.sources(BootPortalApp.class);
    }
}
  
  
TimeUnit.MINUTES.toMillis(10);分转毫秒
volatile String combinedTime  登录时缓存时间，不会一直访问数据库
StringUtils.leftPad("abc", 10, "*");   //返回结果：*******abc
   
boolean res = obj instanceof Class；一个对象是否为一个类/接口的实例
    
//和string.intern()方法类似
map.computeIfAbsent("AA", k -> new ArrayList<>());  
```



### json

```java
JobSelfData<T> extends BatchJob{
	private T data;     //T中有List<User> 
}
//转换思路,不考虑泛型，转换后拿到里面的 batchJob.getJobPara()，JsonUtils工具要写map
Map<String, Class> classMap = new HashMap<>();
classMap.put("costList", GoodsCost.class);
 CreateResJobData newJobData= (CreateResJobData) JsonUtils.json2Object(selfJobData.getJobPara(),CreateResJobData.class, classMap);
selfJobData.setData(newJobData);  //重新设置回去
//或者直接用alibaba.fastjson，，，JSON.toJSONString(batchJob)
CreateResJobData newJobData = JSON.parseObject(selfJobData.getJobPara(), CreateResJobData.class);


JobItemData<T> {
  private JobSelfData<T> jobSelfData;
}
JobItemData<CreateResJobData> j=JSON.parseObject(json, new TypeReference<JobItemData<CreateResJobData>>(){});
```





Base64是编码,不是压缩和加密,编码后只会增加字节数;字节数会成为原字节数的4/3；



线程池/redis配置文件 --》RedisConfiguration中有bean或者@Value("${serve.global.sequence.refreshAfterWrite:3600}") --》RedissonClient



### tk.mybatis

双击mybatis-generator插件下的mybatis-generator:generate指令，开始自动构建。构建完成后，项目中就多了entity、dao、mapper



### copyProperties属性

1. springframework的BeanUtils通过**反射来读取和写入**，省去了其他繁杂的步骤,性能自然不差
2. cglib的BeanCopier，基于字节码的方式，其实就是通过字节码方式转换成性能最好的get、set方式
3. Apache BeanUtils.copyProperties(target, source)者**提供类型转换功能**，集中了日志、转换、解析等，导致性能变差。

```java
.copyProperties(Object source, Class<T> target)  //看清楚哪个是source
.copyProperties(Object source, T target)
```



注意点：

如果存在属性完全相同的内部类，但是不是同一个内部类，即分别属于各自的内部类，则spring会认为属性不同，不会copy；可以将原内部类直接set入目标类中
泛型只在编译期起作用，不能依靠泛型来做运行期的限制；

spring 的 BeanUtils.copyProperties(Object source, T target)，会将属性值为null的也拷贝覆盖target的属性值

Spring的BeanUtils的CopyProperties方法需要对应的属性有getter和setter方法；（Boolean类型的 isXXX()是不支持复制的，getXXX()是支持的，只支持boolean类型的isXXX()复制，但视情况而定，最好写成getXXX() ）

spring和apache的copy属性的方法源和目的参数的位置正好相反，所以导包和调用的时候都要注意一下





### 枚举

1、枚举类的对象默认都是public static final

2、枚举类的构造器都是private,所以无法在外部创建其实例，这也决定了枚举类实例的个数的确定性（写了几个就是几个）。

3、enum类不可被继承。默认继承Enum类

```
enum SessionRedisKey implements 接口；
SessionRedisKey.EMAIL_LOCK_MAP.getRedisKey()  可以调用接口中的default方法
```



### 网络

#### 转发和重定向

| **区别**               | **转发forward()**                                            | **重定向sendRedirect()**             |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------ |
| **根目录**             | 包含项目访问地址                                             | 没有项目访问地址                     |
| **地址栏**             | 不会发生变化                                                 | 会发生变化                           |
| **哪里跳转**           | 服务器端进行的跳转                                           | 浏览器端进行的跳转                   |
| **请求域中数据**       | 不会丢失（一次请求）                                         | 会丢失（两次请求）                   |
| **使用**               | 查询数据库                                                   | 增删改数据库                         |
| **HttpServletRequest** | request.getRequestDispatcher("/地址").forward(request, response); | response.sendRedirect("two");        |
| **不用@ResponseBody**  | return "index";（不同controller，return "forward:/user/index";） | return "redirect:http://user/index"; |

在同一controller中则不用使用"/"从根目录开始,而如果是在不同的controller则一定要从根目录开始



不同的请求体，后端远程调用 url+请求体 ：

```java
public static String doHttpPost(String url, String param, String authorization) throws Exception {
        BufferedReader in = null;
        HttpURLConnection conn = null;
        try {
            conn = (HttpURLConnection) new URL(url).openConnection();
            conn.setRequestProperty("Content-Type", "application/json; charset=utf-8");
conn.setRequestMethod("POST");。。。
// Write request content to HTTP server by UTF-8 charset
            if (null != param && !"".equals(param.trim())) {
                OutputStream outputStream = conn.getOutputStream();
                outputStream.write(param.getBytes("UTF-8"));
                outputStream.flush();
                outputStream.close();
            }
            // Read Http Response
            in = new BufferedReader(new InputStreamReader(conn.getInputStream(), "utf-8"));
```

#### 远程调用

**实现远程调用的方式**

Http接口（web接口、RestTemplate+Okhttp）、Feign、RPC调用（Dubbo、Socket编程）、Webservice。

 

**什么是Feign？**

Feign是Spring Cloud提供的一个声明式的伪Http客户端，它使得调用远程服务就像调用本地服务一样简单，只需要创建一个接口并添加一个注解即可。

Nacos注册中心很好的兼容了Feign，Feign默认集成了Ribbon，所以在Nacos下使用Fegin默认就实现了负载均衡的效果。

主启动类上：@EnableFeignClients

```java
@FeignClient("gulimall-coupon")     //服务名，从注册中心找
public interface CouponFeignService {
    @RequestMapping("coupon/coupon/member/list")    //controller方法地址
    public R membercoupons(@PathVariable("memberId") long memberId);
}
//gulimall-coupon：被调用者
@RestController
@RequestMapping("coupon/coupon")
public class CouponController {
    @RequestMapping("/member/list")
    public R membercoupons(@PathVariable("memberId") long memberId) {}
```

**什么是Dubbo？**

Dubbo是阿里巴巴开源的基于Java的高性能RPC分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，以及SOA服务治理方案。

Spring-cloud-alibaba-dubbo是基于SpringCloudAlibaba技术栈对dubbo技术的一种封装，目的在于实现基于RPC的服务调用。

如果项目对性能要求不是很严格，可以选择使用Feign，它使用起来更方便。

如果需要提高性能，避开基于Http方式的性能瓶颈，可以使用Dubbo。



**RPC**

自定义数据格式，基于原生TCP通信，速度快，效率高。早期的webservice，现在热门的dubbo.

RPC的框架：webservie(cxf)、dubbo
RMI的框架：hessian



RPC和http的区别
 RPC要求服务提供方和服务调用方都需要使用相同的技术，要么都hessian(只支持POST请求和Java，还需要服务器提供完整的接口代码(包名.类名.方法名必须完全一致)，要么都dubbo。而http无需关注语言的实现，只需要遵循rest规范



#### Http

http其实是一种网络传输协议，基于TCP，规定了数据传输的格式。现在客户端浏览器与服务端通信基本都是采用Http协议。也可以用来进行远程服务调用。缺点是消息封装臃肿。

现在热门的Rest风格，就可以通过http协议来实现。

http的实现技术：HttpClient

长短连接：

长连接多用于每个用户操作频繁，点对点的通讯，而且连接数不能太多情况。短连接频繁的socket 创建对资源的浪费

WEB网站的http服务一般都用短链接，并发量大，但每个用户无需频繁操作情况。长连接对于服务端来说会耗费一定的资源。



#### WebSocket

可以把 WebSocket 看成是 HTTP 协议为了支持长连接所打的一个大补丁，以前 HTTP 协议中所谓的 keep-alive connection 是指在一次 TCP 连接中完成多个 HTTP 请求，但是对每个请求仍然要单独发 header

WebSocket 通过第一个 HTTP request 建立了 TCP 连接之后，之后的交换数据都不需要再发 HTTP request了，使得这个长连接变成了一个真.长连接



### 日志

Spring Boot 能够使用Logback, Log4J2 , java util logging 作为日志记录工具。Spring Boot 默认使用Logback作为日志记录工具。

Sping Boot 默认输出ERROR , WARN , INFO 级别的日志

日志优先级别标准顺序为：

ALL < TRACE < DEBUG < INFO < WARN < ERROR < FATAL < OFF



### postman

![image-20220216171619241](/image-20220216171619241.png)

list类型：

{
    "logIds": ["4444"]，"name": "" , "offset": 0,  "limit": 10}

}

要添加请求头，还不能把@TokenNeedonly删除

![image-20220216171555005](/image-20220216171555005.png)







### git

即时更新。有冲突=》最好先备份自己的=》全部接受其他人的



### 下拉框

```java
  //动态选一个另一个出来相关联的下拉框
    @TokenNeedonly
    @PostMapping("/queryNethallName")
    public RestResponse queryNethallName(@RequestBody ComboReq req) {
        List<Nethall> list = nethallDao.selectByVendor(req.getSearchKey());
        List<ComboBox> comboBoxList = new ArrayList<>();
        for (Nethall nethall : list) {
            ComboBox cb = new ComboBox();
            cb.setId(nethall.getNethallId());
            cb.setText(nethall.getName());
            comboBoxList.add(cb);
        }
        return RestResponse.success(comboBoxList);
    }

//动态下拉框，选一个另一个或两个也可确定
 @TokenNeedless
 @PostMapping(value = "queryDict")
    public RestResponse queryDict(@RequestBody ComboReq req) {
     list = dictComboService.getComboList(req.getCode(), super.getLocale());
    }
//DictComboCompImpl
   @Override
    public List<ComboBox> getComboList(String tenantId, String dictCode, String locale) {
        if (DictTypeEnum.isDynamic(config.getDictType())) {  
            DictDynamicLoadHandler handler = context.getBean(config.getDictBean(), DictDynamicLoadHandler.class);  //动态下拉框,找到接口的实现类（实现类名，接口），实现类名配在数据库
            return handler.getComboBoxList(tenantId, dictCode, locale);
        } else {    //非动态下拉框
         return this.getComboBoxListByDict(tenantId, dictCode, locale);//直接去查dic_value 
        }
    }
//LocationDictDynamicLocalHander，选一个另一个也可确定
@Component
public class LocationDictDynamicLocalHander implements DictDynamicLoadHandler {
    @Override
    public List<ComboBox> getComboBoxList(String tenantId, String dictCode, String locale, Object... params) {
        List<ComboBox> list = new ArrayList<>();
        List<Area> areas = areaDao.selectAll();
        for (Area area : areas) {
            ComboBox comboBox = new ComboBox();
            comboBox.setId(area.getArea());
            comboBox.setText(i18NComp.getI18nText(area.getName(), locale));
            comboBox.setTips(area.getTimeZone());  //放入关联的一个属性
            list.add(comboBox);
        }
        return list;
    }
}
//DictDynamicLoadConfig，选一个另两个也可确定，匿名内部类实现
@Component
public class DictDynamicLoadConfig {
    @Bean("areaDictDynamicLocalHandler")
    public DictDynamicLoadHandler areaDictDynamicLocalHandler() {
        int[] sort = {0};
        return (tenantId, dictCode, locale, params) -> areaComp.getAll().stream().map(area -> {
            ComboBox comboBox = new ComboBox() {
                private String timezone = area.getTimeZone();
                private String currency = area.getCurrency();
                public String getTimezone() { return timezone;}
                public String getCurrency() {   return currency;}
            };
            comboBox.setId(area.getArea());
            comboBox.setText(i18NComp.getI18nText(area.getName(), locale));
            comboBox.setSort(sort[0]);
            sort[0] = sort[0] + 1;
            return comboBox;
        }).collect(Collectors.toList());
    }
```





### 架构

#### 项目依赖

framework  ->  mq/redis /data-cdr/data-busi	->  lib-iot/business	->	apps/app-iot-scanner	->	app-portal / app-apigw

lib-iot(data-busi/redis)

business(mq/redis/data-busi)

apps(business)

app-iot-scanne(lib-iot)

#### 数据库

资源 （从vendor拿设备卡，流量卡，共享卡）=》商品《=  产品（区分商品（商品，套餐，服务））

=》订单（生产订单（商品），业务订单（下套餐，变更），销售套餐（卖商品给tenant））



#### builder

被多个类用到

set内容多可写一个 BatchJobBuilder.buildBatchJob（）类





#### refreshCache写成抽象方法

```java
AbstractCacheComp   //刷新缓存的工具类

@Component
@CacheConfig(cacheNames = CacheConsts.CACHE_TARIFF_PRODUCT)
public class ProductCompImpl extends AbstractCacheComp implements ProductComp {
    @Override
    public void refreshCache() {
        super.refreshCache(CacheConsts.CACHE_TARIFF_PRODUCT);
    }
}
public class ProductRestService extends AbstractTariffServcie {
productComp.refreshCache();
}
```

<img src="/image-20220226105052146.png" alt="image-20220226105052146" style="zoom:80%;" />



#### xxmapper写成泛型：

原来的mapper代码语句：

```java
Mysql中timestamp的格式为"YYYY-MM-DD:HH-MM-SS"
SELECT NOW() FROM table_name;	查看数据库的时间是多少
select sysdate()；电脑的系统时间
<select id="selectNow" resultType="java.lang.Long" useCache="false" flushCache="true">
        select REPLACE(unix_timestamp(current_timestamp(3)),'.','')
</select>

staffRoleMapper.select(obj)，//obj有属性
Mapper.selectByPrimaryKey(key/obj)，对主键加上注解 @Id才会生效。无法识别 int 类型，要包装类型
updateByPrimaryKeySelective   会对字段进行判断再更新(如果为Null就忽略更新)
.selectAll()
menuMapper.selectByExample(example); //builder.build())

WeekendSqls<Staff> whereSql = WeekendSqls.custom();
whereSql.andEqualTo(Staff::getOrgId, condition.getOrgId())
whereSql.andIn(Staff::getOrgId, orgIds)
whereSql.andLike(Tenant::getName, "%" + tenantName.trim() + "%");
List<Role> rootRoles = roleDao.selectByExample(Example.builder(Role.class).where(whereSql).build());

WeekendSqls<CdrSftpConfig> whereSql = WeekendSqls.custom();
PageInfo<CdrSftpConfig> pageInfo = cdrSftpConfigDao.pageByExample(Example.builder(CdrSftpConfig.class).where(whereSql).orderByDesc("createTime").build(), offset, limit);

PageInfo<Tenant> pageInfo = tenantDao.pageByCondition(whereSql, new String[]{"tenantId"}, offset, limit);
 PageHelper.offsetPage(offset, limit, count);
 return new PageInfo<>(this.selectByCondition(sqlsCriteria, orderBy));
 Example.Builder builder = Example.builder(Tenant.class);
 if (null != sqlsCriteria) {
    builder.where(sqlsCriteria);
 }
 if (!AssertUtils.isArrayEmpty(orderBy)) {
      builder.orderBy(orderBy);
  }
 return tenantMapper.selectByExample(builder.build());

```

改成继承，相当与工具类

```java
public class MapperDao<M extends Mapper<E>, E> implements Mapper<E> {
    @Autowired
    private M myMapper;
    public M getMyMapper() {
        return this.myMapper;
    }。。。
}
public interface CacheMapper extends Mapper<Cache> {}
public class CacheDao extends MapperDao<CacheMapper, Cache> {
  public int updateAll(Cache update) {
        return getMyMapper().updateAllSelective(update);
 }
public class CacheMgmtComp extends AbstractCacheComp{} //在service或comp层加缓存

    
//插入一个list时
public class InsertListMapperDao<M extends Mapper<E>, E> extends MapperDao<M, E> implements InsertListMapper<E> {
    public InsertListMapper<E> getInsertListMapper() {
        return (InsertListMapper<E>) getMyMapper();
    }
    @Override
    public int insertList(List<? extends E> list) {
        if (null != list && list.size() > 0) {
            return getInsertListMapper().insertList(list);
        }
        return 0;
    }
}
 public class BatchItemDao extends InsertListMapperDao<BatchItemMapper, BatchItem> {}
     

也可直接使用mapper中的方法： CacheDao.insert();     
    
insert：  插入n条记录，返回影响行数n。（n>=1，n为0时实际为插入失败）
update：更新n条记录，返回影响行数n。（n>=0）
delete： 删除n条记录，返回影响行数n。（n>=0）
```

updateByPrimaryKeySelective 和 updateByPrimaryKey 区别：

前者只是更新新的 model 中不为空的字段。(注意是新的Model）

后者则会将为空的字段在数据库中置为NULL。（在新的mode中没有set的，都会置为null）

一般用updateByPrimaryKeySelective(E e)



#### 工具类

```java
Controller类 继承 BaseRest {} 
RestResponse.success()
RestResponse.success(pageList);
RestResponse.file(lineFileProfile)
RestResponse.failure(e.getRetCode(), e.getRetMesg())
    
继承TokenBasedRest/HttpRequestUtils获取token，operation：
String locale = HttpRequestUtils.getLocale();
getServletRequest().getHeader("Authorization");  //得到请求头数据（token，语言locale）
HttpRequestUtils.setOperation(operation); //HttpRequestUtils.getOperation();
this.checkAndUploadFile() //文件上传


//HttpUtils：
tring response = HttpUtils.doHttpPost(url, json, token);

//DateUtils日期工具类：
DateUtils.date2Str(new Date(), DateUtils.PATTERN_YM)
String dateTimeFormat = DateUtils.getDateTimeFormat(operation.getOpLocale());       params.put("billingExpTime",DateUtils.date2Str(tenantPoolVo.getBillingExpTime(),dateTimeFormat));
    
//BeanUtils属性拷贝工具：
BeanUtils.copyProperties(jobSelfData, newJobSelfData);
JsonResult taskRsp = (JsonResult) JsonUtils.json2Object(taskResultStr, JsonResult.class);

//JsonUtils 工具类
String businessDtoJson = JsonUtils.object2Json(businessDto);
PhaseBillingDto Dto = (PhaseBillingDto)JsonUtils.json2Object(s, PhaseBillingDto.class);
List<Ocs> list = (List<Ocs>)JsonUtils.json2List(quotaListString, Ocs.class);



//RedisClient封装了RedisTemplate的操作
Integer count = (Integer) redisClient.get(CacheConsts.TIMER_JOB_CNT_KEY);
redisKeyUtils.defaultGenerate(KEY_PREFIX, key); 
redisLock 工具类使用RedisClient封装的RedisTemplate

<p>Dear Customer,</p><p>Pool Product Will Expire : </p<p>poolName : {poolName}</p><P>productName : {productName}<p/><p>billingExpTime : {billingExpTime}</p><P>SimTotal：{SimTotal}</p><p>Regards,</p><p>CSP.</p>

FileUtils.getLineIterator //上传时判断是不是某类型文件并获得每行数据
    
StringUtils.join(arr,"|")   //将数组或集合以某拼接符拼接到一起形成新的字符串

    
//ComboBo下拉框工具类
return RestResponse.success("productCombo", ComboBox.copyToComboBoxWithTips(products, "productId", "productName", "productDesc"));

//校验iccid
ValidationResult validationResult = null;
for(String item:arr){
    validationResult=ICCID.verify(item);
}
if(null != validationResult && !validationResult.isSuccess()){
throw new BusinessException(BusinessResult.ERR_PARAMETER_FORMATS,validationResult.getErrorMessage());}
//分割String iccids
String[] arr = StringUtils.mixedValueToArray(req.getIccids());


//ExporterBuilder导出文件工具类
builder.setHeaders
//Cells 设置excel文件内容工具类
    

//MQUtils
MQUtils.getMessagePostProcessor()

//BusinessException ，异常工具类
//设置获取报错异常消息类，BusinessResult
throw new BusinessException(BusinessResult.ERR_GOODS_NOT_EXIST,e);
throw new BusinessException(BusinessResult.FAILURE, "没有可操作资源");

//转化流量单位
IotUtils.parseKB2B();
UsageUtils.byte2Mb();

//生成uuid,
//xxxxxxxx-xxxx- xxxx-xxxxxxxxxxxxxxxx(8-4-4-16)，其中每个 x 是 0-9 或 a-f 范围内的一个十六进制的数字,共32个数字
RandomUtils.getUUID32();
```



#### 校验判断

```java
//可能为空的写右边
if(Sim.SIM_RES_TYPE_IOT_NB.equals(busiType)){}
if(null!=busiType){}


1、自定义校验：
ValidateMyString，ValidateMyNumber,ValidateMyDatetime
@MyValidator
public class CreateNethallReq implements RestRequest {
    @MyString
private String name;	}

2、hibernate.validator校验:
@RestController
@Validated
public class TenantRest extends TokenBasedRest {
 @SuperTenantOnly
    @PostMapping("create")
    public RestResponse createTenant(@RequestBody @Valid TenantCreateReq req) {}
}
 public class TenantCreateReq implements RestRequest {
    @NotNull
    @Valid
    private Tenant tenant;
   public static class Tenant {
        @NotNull
        @Size(min = 1,max = 50)
        private String name;
        @Size(min = 1,max = 50)
        private String alias;
        @Size(min = 1,max = 60)
        private String www;
        @Pattern(regexp = "^[A-Z]{2,3}$")
        private String areaCode;
        @Pattern(regexp = "^[A-Z]{3,3}$")
        private String currency;
        @Pattern(regexp = "^.{0,256}$")
        private String remark = "";		}
    
 }

//ParamCheckUtils校验：
  ReqRegex，TenantReqRegex类
ParamCheckUtils.assertStringNotEmpty(editCronReq.getTaskCode(), "taskcode");
ParamCheckUtils.pattern(TenantReqRegex.TENANT_NAME, req.getName(), "name");
//AssertUtils校验：
AssertUtils.assertStringNotEmpty(httpReq.getTenantId(), "Invalid param : tenantId."); 
AssertUtils.isListEmpty(list)
//有无操作权限
 dataPermissionsCheckService.assertDataPermissions(operation, staffDto.getTenantId(), staffDto.getOrgId());

//iccid校验
ValidationResult validationResult = null;
for (String item : arr) {
    validationResult = ICCID.verify(item);
}
if (null != validationResult && !validationResult.isSuccess()) {
    throw new BusinessException(BusinessResult.ERR_PARAMETER_FORMATS, validationResult.getErrorMessage());
}
```



#### 异常

```java
//BusinessException ，异常工具类
    public BusinessException(String retCode) {
        super(BusinessResult.getResultMessage(retCode));  
        this.retCode = retCode;
    }
	public BusinessException(String retCode, Throwable th) {
        super(BusinessResult.getResultMessage(retCode), th);   //(报错消息，造成原因)
        this.retCode = retCode;
    }
//设置获取报错异常消息类，BusinessResult
throw new BusinessException(BusinessResult.ERR_GOODS_NOT_EXIST);
throw new BusinessException(BusinessResult.ERR_SQL_EXCEPTION, e);
throw new BusinessException(BusinessResult.FAILURE, e.getMessage(), e); //e有可能是BusinessException，已经自定义了异常消息，这样不会覆盖掉


//有异常之后的操作，可以消化掉，也可以继续抛出
try {
    String goodsId = this.getGoods(getIccid(itemLog)).getGoodsId();
    CreateOrderDto createOrderDto = initCreateOrderDto(jobContextData, operation, goodsId);
    createOrderAccept.businessAccept(createOrderDto);
    costDeductions(jobContextData, goodsId);
} catch (Exception e) {
    processFailureItem(itemLog, e);
    throw new BusinessException(e);
}
//获取到自己想要的异常
 if (e instanceof BusinessException) {
     BusinessException ex = (BusinessException) e;
     update.setStatusRemark(getErrorMessage(ex.getRetMesg()));
}
 public String getErrorMessage(String message) {
        if (AssertUtils.isStringEmpty(message)) {
            return SchedConsts.OPERATION_FAIL;
        } else if (message.length() >= 128) {
            message = message.substring(0, 128);
        }
        return message;
    }

//可以理解就是sout，不过它可以由日志级别控制打不打印写入日志，下面这句并不是出错的时候才会打印
logger.error("jobItem error  : {} - {}", itemLog.getBatchId(), e);
日志级别由高到底是：fatal,error,warn,info,debug,低级别的会输出高级别的信息，高级别的不会输出低级别的信息，如等级设为Error的话，warn,info,debug的信息不会输出
```

try catch和throw区别：

try catch将出错的地方在catch中处理，只有try中错误之后的代码不执行，try外的代码执行

throw会将错误抛出，之后的代码都不执行

代码可能会出错又不想影响后面的程序，try-catch中用throw，throw中的内容会被catch中的覆盖

catch 中如果没有再 throw 抛出异常, 那么catch之后的代码是可以继续执行的，但是try中 , 报错的那一行代码之后 一直到 try 结束为止的这一段代码 , 是不会再执行的

##### 调用接口方法超时

// Future是一个接口，该接口用来返回异步的结果



### 定时任务框架Quartz

Quartz是一个完全由Java编写的开源任务调度的框架，通过触发器设置作业定时运行规则，控制作业的运行时间。其中quartz集群通过故障切换和负载平衡的功能，能给调度器带来高可用性和伸缩性

**Quartz 三要素：**

- Scheduler：任务调度器，所有的任务都是从这里开始。
- Trigger：触发器，定义任务执行的方式、间隔。
- JobDetail & Job ： 定义任务具体执行的逻辑。（有个map）

yml：

```
基础配置
调度器线程池配置
数据库配置
```



配置文件一般为quartz.properties文件，但是如果使用yml文件格式的配置，则quartz.properties里面的配置会失效；

将schedule相关信息保存在RDB中，有两种实现：JobStoreTX和JobStoreCMT

前者为application自己管理事务；
后者为application server管理事务，即全局JTA；

#### 定时Job

抽象方法必须重写。

抽象类中可写方法体为空的普通方法，子类重写了就会调用子类的方法，没有重写也不报错。

```java
JobSelfData<CreateOrderJobData> jobSelfData= (JobSelfData<CreateOrderJobData>)jobContextData.getSelfJobData(); //batchJob+data,T data是batchJob中的param，即req
CreateOrderJobData createOrderJobData = jobSelfData.getData(); //batchJob中的param，即req
String targetData = (String) jobContextData.getTargetData();//一条条要用到的数据
```

##### 商品管理

portal/GoodsRestService(build BatchJob，属性JobPara是req)  -》 business/JobInterComp（doHttpPost）

-》timer/JobRest  -》**JobMgmtService**（submit，入库，加任务到容器，三要素）

-》SchedJobFactory（任务容器找到batchJob，由FuncCode找库中配好的Bean名字，由BatchBean名字在spring容器处理类，**batchJob,转jobSelfData（T data）**，execute）

-》**AbstractJobRunner<T>-**（initialJobStatus，缓存中initialJobCount，将req拿出成<T> data并**jobSelfData转JobContextData（map）**，建线程池，run中handle，插入BatchItem批量日志，移除缓存中的）

-》AbstractArrayJobRunner<T>(execute，生成BatchItem；req/data中的array[i]作为TargetData和jobSelfData合为map传入线程池run，遍历执行每个详单，更新缓存中) / AbstractSearchJobRunner<E, T>（jobSelfData和根据req条件再查一次的类合为map，一页页操作）/ AbstractFileJobRunner

-》ManageGoodsArrayJobRunner（明确data类型（即req也是JobPara），由action和id改库）/ ManageGoodsSearchJobRunner (由action和重查的类改库) / CreateResFileJobRunner

-》GoodsServiceImpl

```java
JobDetail jobDetail = JobBuilder.newJob(SchedJobFactory.class).requestRecovery(true).withIdentity(jobCode, JOB_GROUP).build();

@PersistJobDataAfterExecution  //成功执行execute方法后（没有发生任何异常），更新JobDetail中JobDataMap的数据，使JobDetail实例在下一次执行的时候，JobDataMap中是更新后的数据
@DisallowConcurrentExecution //不要并发地执行同一个JobDetail实例
@Component
public class SchedJobFactory implements Job {
@Override
public void execute(JobExecutionContext context) throws JobExecutionException {
 BatchJob batchJob = (BatchJob) context.getMergedJobDataMap().get(SchedConsts.BATCH_JOB_DTO_NAME);
        try {
            if (null != batchJob) {
                // 获取当前Job的处理类
                BatchBean batchBean = batchJobService.getJobBean(batchJob.getFuncCode());
                IJobRunner job = (IJobRunner) ctx.getBean(batchBean.getFuncBean());
                JobSelfData jobSelfData = new JobSelfData();
                BeanUtils.copyProperties(batchJob, jobSelfData);
                job.execute(jobSelfData);
                
public abstract class AbstractJobRunner<T> extends AbstractComp implements IJobRunner<T> {
     @Override
    public void execute(JobSelfData<T> jobSelfData) {
       

ExecutorService executor = Executors.newFixedThreadPool(poolSize, new ThreadFactory() { ////创建一个定长线程池，可控制线程最大并发数，超出的线程会在队列中等待
            private AtomicInteger suffix = new AtomicInteger(0);
            @Override
            public Thread newThread(Runnable r) {  //线程名
                return new Thread(r, prefix + StringUtils.leftPad(suffix.incrementAndGet() + "", 2, "0"));
            }
        });
executorService.submit(new JobItemRunner(itemJobContextData, itemLog, operation));
protected class JobItemRunner implements Runnable {
     @Override
     public void run() {}
}
```



##### 订单创建

。。。。

=》CreateResFileJobRunner（beforeHandleJob只执行一次， handleJobItem 每次submit都会执行多次）

=> AbstractAccept

=》

```java
//OrderBusiRestService
if (method.equals(BusinessConsts.OPER_METHOD_RANGE)) {
 batchJob.setJobType(BatchJob.JOB_TYPE_RANGE);
 batchJob.setFuncCode(FunctionCode.OR_ORDER_CREATE_RANGE);
}
CreateOrderBatchJobData createOrderBatchJobData = buildJobData(req, operation, orderBusiAccept.getOrderBusiAcceptId());
batchJob.setJobPara(JsonUtils.object2Json(createOrderBatchJobData));
jobInterService.sumbitJob(batchJob);
return RestResponse.success();
private CreateOrderBatchJobData buildJobData(CreateOrderBusiReq req, Operation operation, String orderAcceptId) {
        CreateOrderBatchJobData jobData = BeanUtils.copyProperties(req, CreateOrderBatchJobData.class);
        jobData.setOrderType(Order.ORDER_TYPE_BUSI);
        jobData.setOrderAcceptId(orderAcceptId);
        jobData.setOperation(operation);
        return jobData;
}


//CreateResFileJobRunner，定时任务
CreateOrderDto createOrderDto = new CreateOrderDto();
 createOrderAccept.businessAccept(createOrderDto);
//AbstractAccept
            accept.initialAccept(businessDto);// 参数初始化 2. 参数合法性校验
            accept.trylockAccept(businessDto);  // 分布式锁
            accept.checkupAccept(businessDto); //数据权限校验 2. 业务规则校验 3. 其他校验
            accept.processAccept(businessDto);  // 业务模型处理 ,业务指令处理 ,业务日志处理
            accept.successAccept(businessDto); //成功后的特殊处理
        } catch (Exception e) {
            accept.failureAccept(businessDto, e); //打印错误日志
        } finally {
            accept.finallyAccept(businessDto);  //分布式解锁
        }
@Transactional(transactionManager = "cspTransactionManager")
public void processAccept(BusinessDto businessDto) throws BusinessException, {
        this.processBusiMold(businessDto);
        this.processInstruct(businessDto);
        this.processBusiLogs(businessDto);
    }
	//上锁
		String identifi = redisLock.tryLock(ACCEPT_PREFFIX_KEY + lockKey);
        businessDto.getIdentifiMap().put(lockKey, identifi);
	//解锁
	if (!AssertUtils.isMapEmpty(businessDto.getIdentifiMap())) {
            Iterator<Map.Entry<String, String>> it = businessDto.getIdentifiMap().entrySet().iterator();
            while (it.hasNext()) {
                Map.Entry<String, String> entry = it.next();
                redisLock.releaseLock(ACCEPT_PREFFIX_KEY + entry.getKey(), entry.getValue());
            }
        }



//CreateOrderAccept ，受理类， 重写
this.processBusiMold(businessDto);  =>  orderServiceImpl
this.processInstruct(businessDto); //没有userId设置为0；=》  InstructGenerateService
this.processBusiLogs(businessDto);//bs_accept_log_resv表

//生成指令
//InstructGenerateService  
public Map<String, String> initialEventValues(){ //是map放userId，是user放user属性
instructTranslateService.translateBusiOrder(InstructGenerateDto.PREFIX_USER, userMap)
}
 public void generateInsts(InstructGenerateDto generateDto){//生成业务指令栈
     //商品产品订购指令
     //业务产品（套餐&服务）订购指令。。。
 }
//InstructTranslateService 指令生成服务
public Map<String, String> translateBusiOrder(String prefix, Object obj){ //反射获取user属性
if (obj instanceof Map) {} //是个Map
Method[] methods = obj.getClass().getMethods();
Object value = method.invoke(obj);//静态方法可省略对象，直接用null替代,非静态方法需要提供底层的类对象
 if (value instanceof Date) {}  //是Date类型
}
//InstructNewPriorityOrderGenerator/InstructAddBusiOrdersGenerator。。。
//业务订购指令主副产品设置优先级，设置InstructDispatchDto（AcceptInst，List<InstLog>）
//InstLog插入bs_inst_resv表


//事务问题
@Configuration("cspMybatisConfiguration")
@EnableTransactionManagement
@EnableAutoConfiguration(exclude = {DataSourceAutoConfiguration.class, XADataSourceAutoConfiguration.class, MybatisAutoConfiguration.class})
public class CSPMybatisConfiguration implements EnvironmentAware {
    private Environment environment;
    @Bean("cspDataSource")
    @ConfigurationProperties(prefix = "spring.mybatis.csp")
    public DataSource datasource() {  return new DruidDataSource(); }
    @Bean(name = "cspTransactionManager")
    public PlatformTransactionManager annotationDrivenTransactionManager(@Qualifier("cspDataSource") DataSource datasource) {
        return new DataSourceTransactionManager(datasource);
    }
```



#### 定时Task

改了数据库的配置还要重启定时任务应用，否则容器的中还是原来的配置

**JobMgmtService**（加任务到容器，三要素）-》SchedJobFactory（由bean名字在任务容器找到Task处理类）

=》SchedJobFactory（execute接口ITaskRunner的方法）

=》SimpleTaskRunner（实现ITaskRunner的execute方法，执行processSimpleTask抽象方法）

=》TenantPoolProductWillExpireAlarmTaskRunner（实现processSimpleTask方法）

=》AbstratTaskRunner（生成batchJob交由AbstractJobRunner<T>处理，更改TimerTaskLog的status）

```java
//TaskMgmtService
     //应用启动时, 加载库中配置的有效定时任务到容器中.
    public void initTask() throws Exception {
        List<TimerTask> timerTaskList = timerTaskComp.listAvaiableTasks();
        for (TimerTask timerTask : timerTaskList) {
            this.addTask(timerTask);
        }
    }
  public void addTask(TimerTask task) throws SchedException {
            // requestRecovery:故障恢复机制:false
 JobDetail jobDetail = JobBuilder.newJob(SchedJobFactory.class).build();
 jobDetail.getJobDataMap().put(SchedConsts.BATCH_TASK_DTO_NAME, task);
  CronScheduleBuilder cronBuilder = CronScheduleBuilder.cronSchedule(task.getTaskCron()); //获取corn表达式
   CronTrigger trigger =
newTrigger().withSchedule(cronBuilder.withMisfireHandlingInstructionDoNothing()).build();
scheduler.scheduleJob(jobDetail, trigger);
    }   



//定时task设置operation的方法
buildOperation
    
    
    
@Component("tenantPoolUsageWillExceedAlarmTaskRunner")
public class TenantPoolUsageWillExceedAlarmTaskRunner extends SimpleTaskRunner {
```

##### 告警发邮件

```java
//task任务写入emailSend表，邮件可以设置发送时间，一般不设置，由邮件定时任务扫到就发邮件
private void createEmail(TenantPoolRuleAlarm alarm, TenantPoolVo vo, Operation op) {
        Map<String, String> params = new HashMap<>();
        params.put("PoolName", vo.getPoolName());
      
        EmailSend emailSend = new EmailSend();
        emailSend.setSendTo(alarm.getAlarmWayValue()); //地址
        emailComp.createEmail(EmailTpl.POOL_SIM_SUSPEND_NUM, emailSend, params, op);
    }
```

##### 游标Cursor

通常对一张表中大量数据处理时由于数据量太大都要使用分页分批查询处理，否则数据量太大会导致OOM

Cursor查询适用于这种场景下可以替代分页查询

```java
//使用@Transactional注解来维持数据库连接
@Transactional
public void doRecord() throws Exception {
    Cursor<Foo> cursor = recordMapper.getAllRecord()；
    cursor.forEach(record-> {
         System.out.println(record);
    });


//IotLifeCycleTaskRunner
private void processT2SLifeCycleTenant(ItemStreamReader<GoodsIotTenant> goodsIotTenants, Operation operation) throws Exception {
        try {
            goodsIotTenants.open(new ExecutionContext());
            GoodsIotTenant goodsIotTenant;
            while ((goodsIotTenant = goodsIotTenants.read()) != null) {
                this.submitSilentTenant(goodsIotTenant, operation);;
            }
        } finally {
            goodsIotTenants.close();
        }
    }
//GoodsIotTenantDao
public ItemStreamReader<GoodsIotTenant> createTenantT2SReader() {  //返回输出流
 MyBatisCursorItemReader<GoodsIotTenant> reader = new MyBatisCursorItemReader<>();
 reader.setSqlSessionFactory(cspSqlSessionFactory);
 reader.setQueryId("site.neware.csp.database.core.GoodsIotTenantMapper.cursorTenantT2S");
 return reader;
}
//GoodsIotTenantMapper
Cursor<GoodsIotTenant> cursorTenantT2S();
//GoodsIotTenantMapper
<select fetchSize="-2147483648" id="cursorTenantT2S" resultMap="BaseResultMap">
		select
		<include refid="Base_Column_List" />
		<include refid="From_Table" />
		<include refid="Where_Default" />
		<![CDATA[and trial_exp_time > now()]]>
		and phase_status = '20' 
</select>
```

##### 定时发指令

```java
//SendInstructTaskRunner
@Component("sendInstructTaskRunner")
public class SendInstructTaskRunner extends SimpleTaskRunner {
@Override
protected void processSimpleTask(TimerTask timedTask, Operation operation) {
List<AcceptLog> acceptLogList = acceptLogDao.selectResvTodo(operation.getOpTime(), DEFAULT_SCAN_SIZE);
        // AcceptLog 按uid取模拆分到对应的list中.
        Map<String, List<AcceptLog>> instModUidMap = buildAcceptMap(acceptLogList);
        // 批量提交
        List<Future<Boolean>> futureList = dispatchFuture(instModUidMap);
        // 判断所有的callable均已执行完成.
        if (!AssertUtils.isListEmpty(futureList)) {
            while (true) {
                boolean isAllDone = true;
                for (Future<Boolean> future : futureList) {
                    isAllDone = isAllDone & future.isDone();
               	if (isAllDone) {
                    break;
                }
            }
        }
    }
    
private List<Future<Boolean>> dispatchFuture(Map<String,List<AcceptLog>> instModUidMap) {
        ExecutorService executor = Executors.newFixedThreadPool(this.getInstPoolSize());
        List<InstSendCallable> callableList = new ArrayList<>();
        Iterator<String> it = instModUidMap.keySet().iterator();
        while (it.hasNext()) {
            String key = it.next();
            InstSendCallable callable = new InstSendCallable(instModUidMap.get(key));
            callableList.add(callable);
        }
        List<Future<Boolean>> futureList = null;
        futureList = executor.invokeAll(callableList);//执行继承了Callable接口的类的集合
        executor.shutdown();
        return futureList;
    }
   
  class InstSendCallable implements Callable<Boolean> {
        private List<AcceptLog> acceptList;
        InstSendCallable(List<AcceptLog> acceptList) {this.acceptList = acceptList;}
        @Override
        public Boolean call() throws Exception {
          for (AcceptLog acceptLog : acceptList) { //派发状态为Initial的指令
        String dispatchResult = instructDispatchService.dispatch4Initial(acceptLog);}

}
```

### 调度管理

job:portal/ScheConsoleRest => ScheConsoleMgmtService(走内网发http请求) => timer/JobRest
task:                                                      															  =>TaskMgmtService

```java
//ScheConsoleMgmtService
 public List<BatchJobDto> selectAllJob(Operation operation) {
     String queryJobUrl = getHttpUrl(JOB_QUERY_SUFFIX);
     String jobResultStr = HttpUtils.doHttpGet(queryJobUrl);
     Map<String, Object> map = JSON.parseObject(jobResultStr, Map.class);
      batchJobDtoJsons = (List<JSONObject>) map.get("jobList");
     batchJobDto = (BatchJobDto) JSONObject.toJavaObject((JSONObject) batchJobDtoJsons.get(i), BatchJobDto.class);
 }
     //修改定时任务Task的Cron表达式
  public void updateTaskCron(String taskCode, String taskGroup, String taskCron) {   
     String queryTaskUrl =getHttpUrl(TASK_UPDATE_SUFFIX) + "?taskCode=" + taskCode + "&taskGroup=" + taskGroup + "&cronExp=" + URLEncoder.encode(taskCron, "utf-8");
            taskResultStr = HttpUtils.doHttpGet(queryTaskUrl);
  }
     // * 获取所有定时任务Task
    public List<TimerTask> selectAllTask(Operation operation) {
        Map<String, Class> classMap = new HashMap<String, Class>();
        Map<String, Object> map = null;
String queryTaskUrl = getHttpUrl(TASK_QUERY_SUFFIX) + "?taskCode=&taskGroup=";
            String taskResultStr = HttpUtils.doHttpGet(queryTaskUrl);
   classMap.put("taskList", TimerTask.class);
   map = (Map<String, Object>) JsonUtils.json2Object(taskResultStr, Map.class, classMap);
        List<TimerTask> timerTasks = (List<TimerTask>) map.get("taskList");
        //Task排序
        if (!AssertUtils.isListEmpty(timerTasks)) {
            Collections.sort(timerTasks, new Comparator<TimerTask>() {
                @Override
                public int compare(TimerTask task1, TimerTask task2) {
                    return task1.getTaskName().compareTo(task2.getTaskName());
                }
            });
            Collections.sort(timerTasks, new Comparator<TimerTask>() {
                @Override
                public int compare(TimerTask task1, TimerTask task2) {
                    return task1.getTaskGroup().compareTo(task2.getTaskGroup());
                }
            });
        }
        return timerTasks;
    }
//JobMgmtService
 *//查询容器中所有的定时任务.
    public List<BatchJob> listAllJobs() throws SchedException {
            List<BatchJob> joblist = new ArrayList<BatchJob>();
Set<JobKey> jobKeys = scheduler.getJobKeys(GroupMatcher.anyJobGroup());//获得所有运行中的Job
            for (JobKey jobKey : jobKeys) {
                JobDetail jobDetail = scheduler.getJobDetail(jobKey);
 BatchJob job = (BatchJob) jobDetail.getJobDataMap().get(SchedConsts.BATCH_JOB_DTO_NAME);
                if (!AssertUtils.isNull(job)) {
                    joblist.add(batchJobComp.getJob(job.getBatchId()));
                }
            }
            return joblist;
    }
//修改定时任务
public void updateJob(String batchId, String reserveTime) throws SchedException {
            String jobCode = JOB_PREFIX + batchId;
Timestamp startTime = DateUtils.str2Timestamp(reserveTime, DateUtils.PATTERN_YMDHMS);
            TriggerKey triggerKey = TriggerKey.triggerKey(jobCode, JOB_GROUP);
            // 按新的trigger重新设置job执行
    SimpleTrigger simpleTrigger = (SimpleTrigger) scheduler.getTrigger(triggerKey);
            simpleTrigger = simpleTrigger.getTriggerBuilder().withIdentity(triggerKey).startAt(startTime).build();
            scheduler.rescheduleJob(triggerKey, simpleTrigger);
            modifyJobDetails(jobCode, BatchJob.STATUS_INITIALIZE, startTime);
            batchJobComp.modifyJobStatus(batchId, BatchJob.STATUS_INITIALIZE);
            batchJobComp.modifyJobReservTime(batchId, startTime);
    }
 //TaskMgmtService
public void updateTask(String taskCode, String taskGroup, String state, String cronExp) {
        this.validCronExpression(cronExp);
            // 1.获取旧TimedTask,再设置新TimedTask放入容器
    TimerTask taskDto = modifyDtoStateAndCronExp(taskCode, taskGroup, state, cronExp);
            // 2.删除Task
            delete(taskCode, taskGroup);
            // 3.新增Task
            addTask(taskDto);
            timerTaskComp.modifyTaskCronExpress(taskCode, taskGroup, cronExp);//改数据库
    }
      public boolean validCronExpression(String cron) throws SchedException {
            CronTriggerImpl trigger = new CronTriggerImpl();
            trigger.setCronExpression(cron);
            Date date = trigger.computeFirstFireTime(null);
            return date != null && date.after(new Date());
    }
```



### Accept

jobRunner=>hadler=>accept=>

控制台已经打印了语句，但数据库没变化，可能是指令没发成功，事务回滚了



### 权限管理

```java
//有无操作权限
 dataPermissionsCheckService.assertDataPermissions(operation, staffDto.getTenantId(), staffDto.getOrgId());

 //只允许查看当前登录商户及以下组织工号，root可看全部（仅非管理员且没传orgIds时要权限）
//前端不会传orgIds
List<String> resOrgIds =  orgComp.getChildrenIdList(operation.getOrgId());  //包含自身
if (GlobalConsts.SUPER_TENANT_ID.equals(operation.getTenantId())) resOrgIds =null;
page = staffDao.pageByCondition(condition, resOrgIds, statusList, offset, limit);
//前端可能传orgIds，相信前端传的orgIds，不相信的话就要过滤一下
List<String> resOrgIds = orgComp.getPrivilegeOrgIds(operation.getTenantId(), operation.getOrgId(), req.getOrgIds());


//只能商户权限
Integer orgLevel = orgComp.get(operation.getOrgId()).getLevel();
if (orgLevel == Org.ROOT_ORG_LEVEL) {   }
//商户或管理员有权限
boolean flag = orgComp.isManagerOrTenant(operation);

//处理前端传的tenantId和orgId
OrgQueryParamDto orgQueryParam = orgComp.getOrgQueryParam(usageReq.getTenantId(), usageReq.getOrgIds(), operation.getTenantId(), operation.getOrgId());
usageReq.setTenantId(orgQueryParam.getTenantId());
usageReq.setOrgIds(orgQueryParam.getOrgIds());
```



### 水平分表操作

每个表的结构都一样； 垂直分表是每个表结构不一样

#### 日志系统

##### 操作日志

将涉及的表全量数据查出再合并，再根据查询条件查

```java
//根据时间分表，YM后缀
//动态表名获取：
tk.mybatis的分表功能，创建DynamicTableName抽象类，实现IDynamicTableName接口，之后所有与数据库表对应的实体类都要继承该抽象类
public abstract class DynamicTableName implements IDynamicTableName {
    @Transient
    @JSONField(serialize = false)
    private String tableSuffix;
    public String getTableSuffix() { return tableSuffix;}
    public void setTableSuffix(String tableSuffix) {  this.tableSuffix = tableSuffix;}
}
@Table(name = "pr_oper_log")
public class OperLog extends DynamicTableName {
     @Override //由分表依据设置。类实例.getDynamicTableName(),insert/update/delete可用
    public String getDynamicTableName() { 
        String suffix = this.getTableSuffix();
        if (AssertUtils.isStringEmpty(suffix)) {
            if (null != this.getCreateTime()) {
                suffix = DateUtils.date2Str(this.getCreateTime(), DateUtils.PATTERN_YM);
            } else if (!AssertUtils.isStringEmpty(this.getLogId())) {
                suffix = this.getLogId().substring(0, 6);
        }
        String prefix = OperLog.class.getAnnotation(Table.class).name();
        return prefix + "_" + suffix;
    }
}
//根据时间插入，
OperLog record=new OperLog();
record.setTableSuffix(DateUtils.date2Str(new Date(), DateUtils.PATTERN_YM));
.insert(record)   //表后缀相当于类的属性了，直接调用插入方法即可
//根据时间查询，先找表后缀，
 public static List<String> getYearMonthBetween(Date start, Date end) {//31号不足的月份变为28号，28号不能变为31号
        if (end.getTime() < start.getTime()) {
            return new ArrayList<>();
        }
        Calendar startCal = Calendar.getInstance();
        startCal.setTime(start);
        startCal.set(startCal.get(Calendar.YEAR), startCal.get(Calendar.MONTH), 1);
        Calendar endCal = Calendar.getInstance();
        endCal.setTime(end);
        endCal.set(endCal.get(Calendar.YEAR), endCal.get(Calendar.MONTH), 2);
        List<String> result = new ArrayList<>();
        while (startCal.before(endCal)) {
            result.add(DateUtils.date2Str(startCal.getTime(), DateUtils.PATTERN_YM));
            startCal.add(Calendar.MONTH, 1);
        }
        return result;
    }
//且表后缀在真实表名范围内取交集
public List<String> getFinalTableNames(List<String> tableNames) {
        List<String> allOperLogTableName = operLogDao.getAllOperLogTableName();
        String prefix = OperLog.class.getAnnotation(Table.class).name() + "_";
        return tableNames.stream().filter(item -> allOperLogTableName.contains(prefix + item)).collect(Collectors.toList());}
<select id="getAllOperLogTableName" resultType="java.lang.String">
        SHOW TABLES LIKE 'pr_oper_log_%';
</select>
//表结果结合，子查询出来的的结果，每个派生表都要有自己的别名，查count(1)也要别名，相当于：
//select a.* from ((select .. from . where ..) UNION (select ... from ... where..))  as a
//mybatis中传入表名，写个foreach就行
<sql id="From_Table_Dynamic">
   <foreach collection="tableNames" item="tn" separator="union all" open="(" close=")">
            SELECT
            <include refid="Base_Column_List"/>
            FROM pr_oper_log_${tn}
    </foreach>
</sql>
<select id="selectByCondition" resultMap="BaseResultMap">
        SELECT OPER_UNION.*
        FROM  <include refid="From_Table_Dynamic"/> as OPER_UNION
        <include refid="Where_Default"/>
        <include refid="And_Columns_Dynamic"/>
        order by request_time desc
</select>
        
//由拦截器Interceptor插入operLog表
@Component
public class RestRequestLogInterceptor implements HandlerInterceptor {
    @Override //将req/resp等设置进ThreadLocal的值RestRequestLogDto的属性中
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        RestRequestLogHolder.get().setUrl(request.getRequestURI());
        RestRequestLogHolder.get().setAccessip(HttpRequestUtils.getRequestIp(request));
        HashMap<String, String> parameterMap = new HashMap<>();
        Enumeration<String> enumeration = request.getParameterNames();
        while (enumeration.hasMoreElements()) {
            String name = enumeration.nextElement();
            parameterMap.put(name, request.getParameter(name));
        }
        RestRequestLogHolder.get().setParameters(parameterMap);
        return true;
    }
     @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) {
        this.saveRequestLog(request, handler);
    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        // 发生异常时，将不会执行 postHandle，在此处进行日志补偿记录
        RestResponse restResponse = RestRequestLogHolder.get().getResponse();
        if (restResponse != null && !ResultCode.SUCCESS.equals(restResponse.getCode())) {
            this.saveRequestLog(request, handler);
        }
        RestRequestLogHolder.remove();
    }
}
//RestRequestLogHolder
 private static ThreadLocal<RestRequestLogDto> record = new ThreadLocal<>();
  //接口上有@Authentication注解才会记录操作日志，@OpCode(code)或者@Authentication(code)或者@TokenNeedonly(code)会修改opCode，最终opCode可以写入FuncCode中，opCode就是前端操作按钮的menu_func


```

##### 批量日志

```java
//根据BATCH_ID最后一位分表，生成批量任务即生成BatchJob，插入BatchJob表
//查询，改，删时将前端传的参数id===》partition表后缀传入
record.setBatchId(batchId);
record.getDynamicTableName();
    public String getDynamicTableName() {  //分表依据，根据类中属性分表名
    }
<sql id="From_Table">
  	from sy_batch_item_${partition}
</sql>    
//插入多条数据时分表：    
 public class BatchItemDao extends InsertListMapperDao<BatchItemMapper, BatchItem> {
@Transactional(rollbackFor = Exception.class)
    public int insert(List<BatchItem> records) {
        int count = 0;
        Map<String, List<BatchItem>> map = records.stream().reduce(new HashMap<>(), (all, record) -> {
            String tableName = record.getDynamicTableName();
            List<BatchItem> list = all.computeIfAbsent(tableName, k -> new ArrayList<>());
            list.add(record);
            return all;
        }, (map1, map2) -> {
            for (Map.Entry<String, List<BatchItem>> entry2 : map2.entrySet()) {
                if (map1.containsKey(entry2.getKey())) {
                    map1.get(entry2.getKey()).addAll(entry2.getValue());
                } else {
                    map1.put(entry2.getKey(), entry2.getValue());
                }
            }
            return map1;
        });

        for (Map.Entry<String, List<BatchItem>> entry : map.entrySet()) {
            count += getMyMapper().insertList(entry.getValue());
        }
        return count;
    }
//插入一个时分表，//实体类继承了DynamicTableName，实体类中重写了分表方法，若插入的实例有对应的分表属性可自动分表
     public int createJobItem(BatchItem batchItem) {  
        return batchItemDao.insert(batchItem);
    }
// 
@Override
protected void beforeHandleJob(JobContextData jobContextData, Operation operation){
    BatchJob update = new BatchJob();
   update.setFuncCode(funcCode);    //不可以用operation.opcode;和前端传的不是一个Operation
   batchJobComp.modifyByJobId(jobSelfData.getBatchId(), update);
}
```

##### 综合业务日志

```java
//根据业务类型分表 ，bs_acct,bs_user
//AcceptLogDao根据不同的表查询并且分页
<select id="listAcceptIdByCondition" resultType="java.lang.String">
  		select * from (select op_accept
        from bs_accept_resv
        where 1=1
        <include refid="And_Columns_ListByCondition"/>
        <if test="limit > 0">
            order by op_accept desc limit 0, ${offset + limit}
        </if>) t1
    union all
        select * from (select op_accept
        from bs_accept_log
        where 1=1。。。。) t2
    order by op_accept desc
    <if test="limit > 0">
        limit #{offset,jdbcType=INTEGER}, #{limit,jdbcType=INTEGER}
	</if>
</select>
            
<select id="listLogAndResvByAcceptIdList" resultType="site.entity.vo.AcceptLogVo">
        SELECT * FROM bs_accept_log
        WHERE op_accept IN
        <foreach collection="acceptIdList" item="acceptId" open="(" close=")" separator=",">
            #{acceptId,jdbcType=VARCHAR}
        </foreach>
    UNION ALL
        SELECT * FROM bs_accept_resv
        WHERE op_accept IN ...
     order by opAccept desc
</select>
<select id="countLogAndResvByCondition" resultType="Long">
 select (select count(0) from rs_goods) +(select count(0) from rs_goods_iot_vendor) as total
</select>


//InstLogDao
@Repository
public class InstLogDao extends MapperDao<InstLogMapper, InstLog> {
     public int insert(InstLog instLog) {
        switch (instLog.getStatus()) {
            case InstLog.INST_UNDO:
            case InstLog.INST_DOIN:
            case InstLog.INST_FAIL:
            case InstLog.INST_CNCL:
            case InstLog.INST_RESV:
                return this.insertResv(instLog);
            case InstLog.INST_SUCC:
                return this.insertSucc(instLog);
            case InstLog.INST_ERRR:
                return this.insertFail(instLog);
        }
        return 0;
    }
    public int insertSucc(InstLog instLog) {
        String suffix = "log_" + this.getSuffix(instLog.getUserId() + ""); //由userId分表
        return getMyMapper().insertWithSuffix(suffix, instLog);
    }
    public int insertFail(InstLog instLog) {
        String suffix = "fail";
        return getMyMapper().insertWithSuffix(suffix, instLog);
    }
    public int insertResv(InstLog instLog) {
        String suffix = "resv";
        return getMyMapper().insertWithSuffix(suffix, instLog);
    }
}


//user类没有getDynamicTableName()方法，表名传入suffix
//插入，查询不入历史表。改，删入历史表
   public static final String SUFFIX_HISTORY = "his";
   public int insert(User record) {
        return getMyMapper().insertWithSuffix(null, record);
    }
    public int insertHis(User record) {
        return getMyMapper().insertWithSuffix(SUFFIX_HISTORY, record);
    }
<sql id="Dynamic_Table_Name">
    <choose><when test="suffix != null and suffix.length > 0">bs_user_${suffix}</when><otherwise>bs_user</otherwise></choose>
  </sql>
  <sql id="From_Dynamic_Table">
    from <include refid="Dynamic_Table_Name" />
 </sql> 
```



#### cdr用量查询

比操作日志优化：将涉及的表根据查询条件查数据查出再合并

既根据useid分表，又根据月份分表

```java
//CdrImsiSumDao 
```



#### 指令

```
log_  根据userId后一位分表的
```



### MQ

交换机：

Direct :直接交换器，工作方式类似于单播，Exchange会将消息发送完全匹配ROUTING_KEY的Queue

topic:主题交换器，工作方式类似于组播，Exchange会将消息转发和ROUTING_KEY匹配模式相同的所有队列

```java
//MQConfig 配置
@Configuration
public class MQConfig {
   //资源入库
    @Bean
    public Exchange resourceImportExchange() {
        return ExchangeBuilder.directExchange(MQConsts.CSP_X_RESOURCE_IMPORT).durable(true).build();}//持久化，durable 的唯一含义就是队列和交换机会在重启之后重新建立,它不表示消息一定被消费
    @Bean
    public Queue resourceImportQueue() {
        return new Queue(MQConsts.CSP_Q_RESOURCE_IMPORT);}
    @Bean
    public Binding resourceImportBinding() {
        return BindingBuilder.bind(resourceImportQueue()).to(resourceImportExchange()).with(MQConsts.CSP_K_RESOURCE_IMPORT).noargs();}
    @Bean
    public SimpleRabbitListenerContainerFactory simpleRabbitListenerContainerFactory(SimpleRabbitListenerContainerFactoryConfigurer configurer, ConnectionFactory connectionFactory) {
        SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
        factory.setPrefetchCount(100); //每次从broker取的待消费的消息数，若限制先进先出消费顺序就要为1，即不能设置并发消费
        factory.setConcurrentConsumers(10);  //设置并发消费，即线程数
        factory.setMaxConcurrentConsumers(20);  // 最大并发
        configurer.configure(factory, connectionFactory);
        return factory;
    }
    @Bean
    public RabbitListenerContainerFactory<?> rabbitListenerContainerFactory(ConnectionFactory connectionFactory) {
        SimpleRabbitListenerContainerFactory factory = new SimpleRabbitListenerContainerFactory();
        factory.setConnectionFactory(connectionFactory);
        factory.setMessageConverter(new Jackson2JsonMessageConverter());  //消息转换方式
        return factory;
    }
//MQSender
@Service
@Primary
public class MQSender {
    private static final Logger logger = LoggerFactory.getLogger(MQSender.class);
    @Autowired
    protected RabbitTemplate rabbitTemplate;
    public <T> void send(String exchange, String routingKey, T message) {
        rabbitTemplate.convertAndSend(exchange, routingKey, message, MQUtils.getMessagePostProcessor(), MQUtils.getCorrelationData());//交换机，路由键，消息，消息后置处理,消息id
    }
}
//CreateResFileJobRunner，发送者
 @Override
    protected void handleJobItem(JobContextData jobContextData, BatchItem itemLog, Operation operation) throws SchedException, BusinessException {
        String jobItemDataJson = JSON.toJSONString(this.initialJobItemData((JobSelfData<CreateResJobData>) jobContextData.getSelfJobData(), itemLog, operation));
        mqSender.send(MQConsts.CSP_X_RESOURCE_IMPORT, MQConsts.CSP_K_RESOURCE_IMPORT, jobItemDataJson);
    } 
//消费者/监听者，队列里存的是json数据   
@Component
@RabbitListener(queues = MQConsts.CSP_Q_RESOURCE_IMPORT, containerFactory = "simpleRabbitListenerContainerFactory")
public class GoodsStockInMQReceiver extends MQReceiver<String> {
    @Override
    protected void handleMessage(String message) {
            JobItemData<CreateResJobData> jobItemData = JSON.parseObject(message, new TypeReference<JobItemData<CreateResJobData>>() {});
            JobContextData jobContextData = new JobContextData();
            jobContextData.putSelfJobData(jobItemData.getJobSelfData());
            createResFileJobItemHandler.handle(jobContextData, jobItemData.getItemLog(), jobItemData.getOperation());
    }
}
```

有队列没有监听者不会报错，有监听者没队列会报错



默认情况下，rabbitmq消费者为单线程串行消费。设置并发消费两个关键属性concurrentConsumers：设置的是对每个listener在初始化的时候设置的并发消费者的个数；prefetchCount：每次从broker里面取的待消费的消息的个数。




### json

区别于序列化，序列化是将对象状态转换为可保持或可传输的格式的过程。

JSON，JavaScript Object Notation，一种更轻、更友好的用于接口(AJAX、REST等)数据交换的格式。 

googlede Gson。当我们利用net.sf.json.JSONObject解析中的toBean方法时，如果它的属性里面包含复杂对象，那么在我们调用这个复杂对象时就会出现这个错误：java.lang.ClassCastException:net.sf.ezmorph.bean.MorphDynaBean cannot be cast to XXX



有内部类的用 fastJson 兼容性好一点

```java
public class A implements Serializable{ 
     private List<b> b; //get、set方法 省略 
}
public class B {
    private List<C> c; //get、set方法 省略 
}
public class C{}

//Gson
Map<String,Class<?>> classMap = new HashMap<String,Class<?>>(); 
classMap.put("b", B.class); 
classMap.put("c", C.class);
//字符串转换成json对象
JSONObject jsonObject = JSONObject.fromObject(String，jsonString);
//转换成实体类对象
Entity entity = (Entity) JSONObject.toBean(jsonObject, A.class, classMap); 

//fastjson
A<B<C>> res= JSON.parseObject(json, new TypeReference<A<B<C>>>(){});
```



子类对象可以被视为是其父类的一个对象，但父类对象不能被当作是某一个子类的对象。

形式参数定义的是父类对象，可使用子类对象作为实际参数。

```java
B extends A
c extends A
convert(b);   convert(c);
public String convert(A a){
   return JsonUtils.object2Json(a);
}
```



### 延时任务

延时任务：

- 生成订单30分钟未支付，则自动取消
- 生成订单60秒后,给用户发短信

和定时任务的区别：

定时任务有明确的触发时间，延时任务没有

定时任务有执行周期，而延时任务在某事件触发后一段时间内执行，没有执行周期

定时任务一般执行的是批处理操作是多个任务，而延时任务一般是单个任务

方案分析:

##### (1)数据库轮询

通常是在小型项目中使用，即通过一个线程定时的去扫描数据库，通过订单时间来判断是否有超时的订单，然后进行update或delete等操作，可用 quartz 实现

优点:简单易行，支持集群操作

缺点:(1)对服务器内存消耗大

(2)存在延迟，比如你每隔3分钟扫描一次，那最坏的延迟时间就是3分钟

(3)假设你的订单有几千万条，每隔几分钟这样扫描一次，数据库损耗极大

##### (2)JDK的延迟队列

利用JDK自带的DelayQueue来实现，这是一个无界阻塞队列，该队列只有在延迟期满时才能从中获取元素，放入DelayQueue中的对象，是必须实现Delayed接口的。消费者用Poll()/take() 方法获取并移除队列的超时元素

优点:效率高,任务触发时间延迟低。

缺点:(1)服务器重启后，数据全部消失，怕宕机 (2)集群扩展相当麻烦 (3)因为内存条件限制的原因，比如下单未付款的订单数太多，那么很容易就出现OOM异常

##### (3)时间轮算法

用Netty的HashedWheelTimer来实现

##### (4)redis缓存

利用redis的zset,zset是一个有序集合，每一个元素(member)都关联了一个score,通过score排序来取集合中的值。订单超时时间戳与订单号分别设置为score和member,系统扫描第一个元素判断是否超时

##### (5)使用消息队列

我们可以采用rabbitMQ的延时队列。RabbitMQ具有以下两个特性，可以实现延迟队列。RabbitMQ可以针对Queue和Message设置 x-message-tt，来控制消息的生存时间，如果超时，则消息变为dead letter

优点: 高效,可以利用rabbitmq的分布式特性轻易的进行横向扩展,消息支持持久化增加了可靠性。

缺点：本身的易用度要依赖于rabbitMq的运维.因为要引用rabbitMq,所以复杂度和成本变高。



### 编译脚本

.bat文件和相对应的.sh文件，一个是为了在window系统上执行的文件，另一个是linux下的批处理文件

compile-serv-win.bat

```shell
echo  starting mvn clean...
call  mvn   clean               #call命令用来调用另一个批处理脚本或者函数或标签

echo  starting mvn package...
call  mvn   package

del   deploy_serv\* /f /q      #/f 强制删除只读文件。/q 安静模式。删除全局通配符时，不要求确认。
mkdir deploy_serv\public
mkdir deploy_serv\neware

copy  framework\build\serv\*.jar    deploy_serv\neware\basic /y     #有原文件覆盖原文件yes

del   deploy_serv\public\*-1.0-SNAPSHOT.jar

call  mvn   clean
::pause
```

compile-iot-mac.sh

```shell
#!/bin/bash
echo  "Deploying IOT..."       #输出
#echo  "starting mvn clean..."
#mvn clean

#echo  "starting mvn package..."
#mvn package

rm -rf deploy_iot
mkdir deploy_iot
mkdir deploy_iot/public
mkdir deploy_iot/neware

cp  framework/build/serv/*.jar    deploy_iot/neware/basic
cp  mq/build/serv/*.jar 	        deploy_iot/neware/basic 

rm -rf   deploy_iot/public/site.neware.csp.*.jar
rm -rf   deploy_iot/public/lib-iot*.jar

echo  "Completed."
```

### corn表达式

0 0/3 * * * ?    离当前最近的00秒开始，每三分钟一次（ 09:48:00 	09:51:00	 09:54:00）

0/10 * * * * ?  离当前最近的00秒开始，每10秒一次（09:49:00	 09:49:10）

0 0 * * * ?    最近的整点开始，每小时一次（10:00:00	11:00:00）

0 0 0 * * ?    每天 00:00:00 执行

### Debug

先启动后打断点也可以成功



### 单元测试

junit单元测试，不用启动Boot

```java
@RunWith(SpringRunner.class)
@SpringBootTest(classes = BootPortalApp.class)
public class PoolTest {
    @Autowired
    private TenantPoolComp tenantPoolComp;
    @Autowired
    private DateTimeComp dateTimeComp;
    @Test
    public void test1() {
        TenantPoolCondition cond = new TenantPoolCondition();
        Date nowDate = dateTimeComp.getNowDate();
        cond.setBillingExpTime(DateUtils.addDay(nowDate, 3));
        cond.setTenantPoolId("200094");
        cond.setNowDate(nowDate);
        List<TenantPoolVo> tenantPools = tenantPoolComp.listByCondition(cond);
        System.out.println(tenantPools);
    }
}
```



http测试

有鉴权，要启动Boot

```java

```



### 算账/用量

计算机是二进制的。浮点数没有办法是用二进制进行精确表示。

float只能用来进行科学计算或工程计算，在大多数的商业计算中，一般采用java.math.BigDecimal类来进行精确计算。

 BigDecimal b1 = new BigDecimal(Double.toString(2.0));	//String.valueOf(b2)

double b=b1.add/subtract/multiply/div(b2).doubleValue();



```java
public static double byte2Mb(long usageByte) {
    return Double.parseDouble(FORMAT.format((float) usageByte / 1024 / 1024));
}
```





### 报表

```
AbstractVendorReportRunner
根据 ReportVendorConfig 扫描 到数据，表中原来有记录就更新，没有就插入

```

## 其他软件

### V2rayN

百度搜：免费v2ray 节点 =》

v2ray 节点每日更新 =》ss 复制粘贴  / SSR 软件选服务器shadowsocks ，手填ssR

测试真延迟

### github

**连不上github：**

方法一：

修改hosts文件CDN

可以在站长工具 - DNS查询：http://tool.chinaz.com/dns?type=1&host=github.com&ip=，选择TTL最小的那个IP地址

用swithHost，没有权限就修改host文件的权限，在最后添加如：13.250.177.223 github.com

文件路径为：C:\Windows\System32\drivers\etc\hosts，右击host文件：

<img src="/image-20220602110353269.png" alt="image-20220602110353269" style="zoom:67%;" />

<img src="/image-20220602110425291.png" alt="image-20220602110425291" style="zoom:67%;" />

<img src="/image-20220602110455112.png" alt="image-20220602110455112" style="zoom:67%;" />



方法二：添加谷歌插件，镜像地址访问：Replace Google CDN，没啥用

方案三：下载代理工具：https://github.com/docmirror/dev-sidecar

**使用github：**

**t 键 可以文件目录展开搜索。**

 L 键跳转行，可如下复制连接：

<img src="/image-20220602113329521.png" alt="image-20220602113329521" style="zoom:67%;" />

B 键可以快速查看该文件的改动记录

。键代码可以直接在一个 网页版 VS Code 编辑器中打开，只能打开前端的代码

**github后 加入 1s**：如：https://github1s.com/lefex/FE。可以查看后端代码

加上 gitpod.io/#/ 前缀可在线运行项目，并且自动安装了依赖包



### typora

复制图片到另一个文件，但图片还有可能引用原来的文件夹，可以新建images，设置图片存入/引入，再设置：

![image-20220218174059913](/image-20220218174059913.png)



在Markdown中生成可以跳转到正文的目录的方法：

```
<!-- GFM-TOC -->
* [](#)
* [](#)
* [](#)
<!-- GFM-TOC -->
```

[]是前台显示，()中是实际链接。



### excel

隔行插入：

新增一列数字，在数字下复制一份，点击自定义排序即可

当前行到最底下行：

ctrl+shift+ 向下箭头

### jvisualVm

在发生内存溢出时（如果发生gc了 那么将得不到溢出时的日志 ），点击堆 dump，会生成.hprof文件，查看.hprof文件就可以分析出内存溢出情况。（在dump时 应用会暂停）

<img src="/image-20220614151641675.png" alt="image-20220614151641675" style="zoom: 80%;" />

导入.hprof文件:

<img src="/image-20220614152835308.png" alt="image-20220614152835308" style="zoom:80%;" />

点击错误线程:

<img src="/image-20220614152931954.png" alt="image-20220614152931954" style="zoom:80%;" />

找到我们自己创建的类：

### idea

反撤销 ctrl+shift+Z

Ctrl + Shift + F 也是和 IDEA 的全局搜索快捷键有冲突的，建议关闭搜狗拼音的简繁切换快捷键。

### JDK

新建 java\jdk\ 文件夹

双击  jdk-8u181-windows-x64.exe 运行，更改安装目录

Windows*系统中配置环境变量：

​		高级系统设置

![image-20220829221014917](/image-20220829221014917.png)

​    	 Win7/8 系统 ：   ;%JAVA_HOME%\bin;JAVA_HOME%\jre\bin;

![image-20220829221313705](/image-20220829221313705.png)

​			win10：   %JAVA_HOME%\bin         %JAVA_HOME%\jre\bin

![image-20220829221303026](/image-20220829221303026.png)

CLASSPATH 

变量值：.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar

![image-20220829221536169](/image-20220829221536169.png)

测试：cmd，javac，java -version