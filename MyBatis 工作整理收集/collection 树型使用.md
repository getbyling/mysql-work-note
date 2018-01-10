Mybatis collection 树型使用
===========================

>Mybatis 配置内容：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cn.gaiay.business.zm.live.category.dao.LivePlateformTypeMapper">

    <!-- 自定义扩展 -->
    <!--首页直播分类获取实体-->
    <resultMap id="PlateformTypeMap" type="cn.gaiay.business.zm.live.category.model.DTO.HomePlateTypeDTO">
        <id column="code" property="code" jdbcType="VARCHAR"/>
        <result column="parent_code" property="parentCode" jdbcType="VARCHAR"/>
        <result column="name" property="name" jdbcType="VARCHAR"/>
        <result column="en_name" property="enName" jdbcType="VARCHAR"/>
        <result column="sort" property="sort" jdbcType="DOUBLE"/>
        <result column="is_top" property="isTop" jdbcType="INTEGER"/>
        <result column="top_time" jdbcType="TIMESTAMP" property="topTime"/>
        <result column="is_default" property="isDefault" jdbcType="INTEGER"/>

        <!-- 子节点 -->
        <collection property="children" column="code" select="getChildrenByParentCode"/>
    </resultMap>

    <!-- 子节点实体 -->
    <resultMap id="ChildrenPlateformTypeMap" type="cn.gaiay.business.zm.live.category.model.DTO.HomePlateTypeDTO">
        <id column="code" property="code" jdbcType="VARCHAR"/>
        <result column="parent_code" property="parentCode" jdbcType="VARCHAR"/>
        <result column="name" property="name" jdbcType="VARCHAR"/>
        <result column="en_name" property="enName" jdbcType="VARCHAR"/>
        <result column="sort" property="sort" jdbcType="DOUBLE"/>
        <result column="is_top" property="isTop" jdbcType="INTEGER"/>
        <result column="top_time" jdbcType="TIMESTAMP" property="topTime"/>
        <result column="is_default" property="isDefault" jdbcType="INTEGER"/>

        <!-- 子节点 -->
        <collection property="children" column="code" select="getChildrenByParentCode"/>
    </resultMap>

    <select id="getChildrenByParentCode" resultMap="ChildrenPlateformTypeMap">
        SELECT
            t.`code`, t.parent_code, t.`name`, t.en_name, t.sort, t.is_top, t.top_time, t.is_default
        FROM
            live_plateform_type t
        WHERE
            t.parent_code = #{code} AND t.is_delete = 0 AND t.is_enabled = 1
        ORDER BY
            t.is_top DESC, t.sort
    </select>

    <!--首页直播分类获取-->
    <select id="getPlateformTypeList" resultMap="PlateformTypeMap">
        SELECT
            t.`code`, t.parent_code, t.`name`, t.en_name, t.sort, t.is_top, t.top_time, t.is_default
        FROM
            live_plateform_type t
        WHERE
            t.parent_code = #{parentCode} AND t.is_delete = 0 AND t.is_enabled = 1
        ORDER BY
            t.is_top DESC, t.sort
    </select>
</mapper>
```

>返回实体定义：
```xml
    <!--首页直播分类获取实体-->
    <resultMap id="PlateformTypeMap" type="cn.gaiay.business.zm.live.category.model.DTO.HomePlateTypeDTO">
        <id column="code" property="code" jdbcType="VARCHAR"/>
        <result column="parent_code" property="parentCode" jdbcType="VARCHAR"/>
        <result column="name" property="name" jdbcType="VARCHAR"/>
        <result column="en_name" property="enName" jdbcType="VARCHAR"/>
        <result column="sort" property="sort" jdbcType="DOUBLE"/>
        <result column="is_top" property="isTop" jdbcType="INTEGER"/>
        <result column="top_time" jdbcType="TIMESTAMP" property="topTime"/>
        <result column="is_default" property="isDefault" jdbcType="INTEGER"/>

        <!-- 子节点 -->
        <collection property="children" column="code" select="getChildrenByParentCode"/>
    </resultMap>
```
>子节点实体定义：
```xml
    <!-- 子节点实体 -->
    <resultMap id="ChildrenPlateformTypeMap" type="cn.gaiay.business.zm.live.category.model.DTO.HomePlateTypeDTO">
        <id column="code" property="code" jdbcType="VARCHAR"/>
        <result column="parent_code" property="parentCode" jdbcType="VARCHAR"/>
        <result column="name" property="name" jdbcType="VARCHAR"/>
        <result column="en_name" property="enName" jdbcType="VARCHAR"/>
        <result column="sort" property="sort" jdbcType="DOUBLE"/>
        <result column="is_top" property="isTop" jdbcType="INTEGER"/>
        <result column="top_time" jdbcType="TIMESTAMP" property="topTime"/>
        <result column="is_default" property="isDefault" jdbcType="INTEGER"/>

        <!-- 子节点 -->
        <collection property="children" column="code" select="getChildrenByParentCode"/>
    </resultMap>
```

>根节点```sql```文 定义：
```xml
    <!--首页直播分类获取-->
    <select id="getPlateformTypeList" resultMap="PlateformTypeMap">
        SELECT
            t.`code`, t.parent_code, t.`name`, t.en_name, t.sort, t.is_top, t.top_time, t.is_default
        FROM
            live_plateform_type t
        WHERE
            t.parent_code = #{parentCode} AND t.is_delete = 0 AND t.is_enabled = 1
        ORDER BY
            t.is_top DESC, t.sort
    </select>
```
>子节点```sql```文 定义：
```xml
    <select id="getChildrenByParentCode" resultMap="ChildrenPlateformTypeMap">
        SELECT
            t.`code`, t.parent_code, t.`name`, t.en_name, t.sort, t.is_top, t.top_time, t.is_default
        FROM
            live_plateform_type t
        WHERE
            t.parent_code = #{code} AND t.is_delete = 0 AND t.is_enabled = 1
        ORDER BY
            t.is_top DESC, t.sort
    </select>
```

>实体代码定义：
```java
package cn.gaiay.business.zm.live.category.model.DTO;

import java.util.Date;
import java.util.List;

/**
 * 首页直播分类获取，返回结果
 *
 * @author lzg
 * @Date 2018年1月3日
 * @email linzhongguang@gaiay.cn
 */
public class HomePlateTypeDTO {

    String code;        //分类code
    String parentCode;  //父分类code
    String name;        //名称
    String enName;      //英文名称
    double sort;        //排序号

    int isTop;          //是否置顶 1:置顶 0:未置顶
    Date topTime;       //置顶时间
    int isDefault;      //是否为默认分类	0否1是

    List<HomePlateTypeDTO> children;    //子节点

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public String getParentCode() {
        return parentCode;
    }

    public void setParentCode(String parentCode) {
        this.parentCode = parentCode;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEnName() {
        return enName;
    }

    public void setEnName(String enName) {
        this.enName = enName;
    }

    public double getSort() {
        return sort;
    }

    public void setSort(double sort) {
        this.sort = sort;
    }

    public int getIsTop() {
        return isTop;
    }

    public void setIsTop(int isTop) {
        this.isTop = isTop;
    }

    public Date getTopTime() {
        return topTime;
    }

    public void setTopTime(Date topTime) {
        this.topTime = topTime;
    }

    public int getIsDefault() {
        return isDefault;
    }

    public void setIsDefault(int isDefault) {
        this.isDefault = isDefault;
    }

    public List<HomePlateTypeDTO> getChildren() {
        return children;
    }

    public void setChildren(List<HomePlateTypeDTO> children) {
        this.children = children;
    }
}
```

>调用代码实现
```java
package cn.gaiay.business.zm.live.category.biz.impl;

import cn.gaiay.business.zm.common.constants.ReturnCodeEnum;
import cn.gaiay.business.zm.live.category.biz.LivePlateformTypeService;
import cn.gaiay.business.zm.live.category.dao.LivePlateformTypeMapper;
import cn.gaiay.business.zm.live.category.model.DTO.*;
import cn.gaiay.business.zm.live.category.model.LivePlateformType;
import cn.gaiay.business.zm.live.common.utils.CodeEnumUtil;
import cn.gaiay.business.zm.live.common.utils.ResultModelUtil;
import cn.gaiay.framework.model.ResultModel;
import cn.gaiay.framework.utils.IdUtils;
import com.alibaba.fastjson.JSONObject;
import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import org.apache.commons.lang3.StringUtils;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.Date;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 平台分类管理操作实现
 *
 * @author lzg
 * @Date 2018年1月2日
 * @email linzhongguang@gaiay.cn
 */
@Service("livePlateformTypeService")
@Transactional
public class LivePlateformTypeServiceImpl implements LivePlateformTypeService {

    private static final Logger logger = LoggerFactory.getLogger(LivePlateformTypeServiceImpl.class);

    @Autowired
    private LivePlateformTypeMapper livePlateformTypeMapper;

    /**
     * 首页直播分类获取
     *
     * @param parentCode String	 父分类code
     * @description: 重构迭代1
     */
    public ResultModel getList(String parentCode) {
        ResultModel resultModel = new ResultModel();
        if (StringUtils.isBlank(parentCode) == true) {
            parentCode = "0";
        }

        ResultData<HomePlateTypeDTO> resultData = new ResultData();

        Page pageInfo = PageHelper.getLocalPage();
        List<HomePlateTypeDTO> pTypeList = livePlateformTypeMapper.getPlateformTypeList(parentCode);

        resultData.setTotal((int) pageInfo.getTotal());
        resultData.setData(pTypeList);
        resultModel.setResult(resultData);

        return resultModel;
    }    
}
```
