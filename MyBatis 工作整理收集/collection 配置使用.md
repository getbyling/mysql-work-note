Mybatis collection 配置使用
===========================

>Mybatis 配置内容：
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="cn.gaiay.business.groupon.dao.GrouponMemberMapper">
    <!-- 拼团状态信息实体 -->
    <resultMap id="TeamStateInfoResultMap" type="cn.gaiay.business.groupon.entity.member.TeamStateInfoVO">
        <id column="team_id" property="teamId" jdbcType="VARCHAR"/>
        <result column="product_name" property="productName" jdbcType="VARCHAR"/>
        <result column="product_type" property="productType" jdbcType="INTEGER"/>
        <result column="team_size" property="teamSize" jdbcType="INTEGER"/>
        <result column="discount" property="discount" jdbcType="DOUBLE"/>

        <result column="leader_free" property="leaderFree" jdbcType="SMALLINT"/>
        <result column="team_lead_id" property="leaderId" jdbcType="VARCHAR"/>
        <result column="create_time" property="createTime" jdbcType="TIMESTAMP"/>
        <result column="finish_time" property="successTime" jdbcType="TIMESTAMP"/>
        <result column="state" property="state" jdbcType="SMALLINT"/>

        <collection property="productPic" ofType="string" javaType="list">
            <result column="product_pic" />
        </collection>
        <!--List<MemberInfo> memberList    //成员列表：list 数据集-->
        <collection property="memberList" fetchType="eager" column="{teamId=team_id}" select="findMemberList"/>
    </resultMap>

    <!--成员列表-->
    <resultMap id="MemberResultMap" type="cn.gaiay.business.groupon.entity.MemberInfo">
        <result column="user_id" property="userId" jdbcType="VARCHAR"/>
        <result column="logo" property="userPic" jdbcType="VARCHAR"/>
    </resultMap>

    <!--拼团状态信息-->
    <select id="grouponTeamState" resultMap="TeamStateInfoResultMap" parameterType="java.util.Map">
        SELECT
          gt.id AS team_id, gt.product_name, g.product_type,  IFNULL(pp.pic_Url, gt.product_pic) product_pic,
          gt.team_size, gt.discount, gt.leader_free, gt.team_lead_id, gt.create_time, gt.finish_time, gt.state
        FROM
          groupon_team gt LEFT JOIN groupon g ON gt.groupon_id = g.id
            LEFT JOIN product_pics pp ON g.product_id = pp.product_Id
        WHERE gt.id = #{teamId}
        ORDER BY pp.sort
    </select>
  
    <!--读取拼团成员列表数据-->
    <select id="findMemberList" resultMap="MemberResultMap" parameterType="java.util.Map">
        SELECT
          gpo.user_id, ap.logo
        FROM
          groupon_order gpo LEFT JOIN account_profile ap ON gpo.user_id = ap.id
        WHERE gpo.groupon_team_id = #{teamId,jdbcType=VARCHAR}
        ORDER BY gpo.create_time
    </select>
  
</mapper>
```

>返回实体定义：
```xml
<resultMap id="TeamStateInfoResultMap" type="cn.gaiay.business.groupon.entity.member.TeamStateInfoVO">
    <id column="team_id" property="teamId" jdbcType="VARCHAR"/>
    <result column="product_name" property="productName" jdbcType="VARCHAR"/>
    <result column="product_type" property="productType" jdbcType="INTEGER"/>
    <result column="team_size" property="teamSize" jdbcType="INTEGER"/>
    <result column="discount" property="discount" jdbcType="DOUBLE"/>
    
    <result column="leader_free" property="leaderFree" jdbcType="SMALLINT"/>
    <result column="team_lead_id" property="leaderId" jdbcType="VARCHAR"/>
    <result column="create_time" property="createTime" jdbcType="TIMESTAMP"/>
    <result column="finish_time" property="successTime" jdbcType="TIMESTAMP"/>
    <result column="state" property="state" jdbcType="SMALLINT"/>
    
    <collection property="productPic" ofType="string" javaType="list">
        <result column="product_pic" />
    </collection>
    <!--List<MemberInfo> memberList    //成员列表：list 数据集-->
    <collection property="memberList" fetchType="eager" column="{teamId=team_id}" select="findMemberList"/>
</resultMap>
```

>主```sql```文 collection 定义：
```xml
<!--拼团状态信息-->
<select id="grouponTeamState" resultMap="TeamStateInfoResultMap" parameterType="java.util.Map">
    SELECT
      gt.id AS team_id, gt.product_name, g.product_type,  IFNULL(pp.pic_Url, gt.product_pic) product_pic,
      gt.team_size, gt.discount, gt.leader_free, gt.team_lead_id, gt.create_time, gt.finish_time, gt.state
    FROM
      groupon_team gt LEFT JOIN groupon g ON gt.groupon_id = g.id
        LEFT JOIN product_pics pp ON g.product_id = pp.product_Id
    WHERE gt.id = #{teamId}
    ORDER BY pp.sort
</select>
```

>主```sql```文 collection 配置使用：
```xml
<collection property="productPic" ofType="string" javaType="list">
    <result column="product_pic" />
</collection>
```

>关联```sql```文 collection 定义：
```xml  
<!--读取拼团成员列表数据-->
<select id="findMemberList" resultMap="MemberResultMap" parameterType="java.util.Map">
    SELECT
      gpo.user_id, ap.logo
    FROM
      groupon_order gpo LEFT JOIN account_profile ap ON gpo.user_id = ap.id
    WHERE gpo.groupon_team_id = #{teamId,jdbcType=VARCHAR}
    ORDER BY gpo.create_time
</select>
```

>关联```sql```文 collection 配置使用：
```xml
<!--读取拼团成员列表数据-->
<select id="findMemberList" resultMap="MemberResultMap" parameterType="java.util.Map">
    SELECT
      gpo.user_id, ap.logo
    FROM
      groupon_order gpo LEFT JOIN account_profile ap ON gpo.user_id = ap.id
    WHERE gpo.groupon_team_id = #{teamId,jdbcType=VARCHAR}
    ORDER BY gpo.create_time
</select>
```

