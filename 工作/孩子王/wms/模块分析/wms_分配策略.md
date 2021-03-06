[TOC]
##常量(WmsConstants)
```
/wms-common/src/main/java/com/haiziwang/kwms/common/domain/constant/WmsConstants.java

  /**
     * mst_wh_sku 表的数据落地时带上的默认分配策略名称
     */
     public   static   final  String DEFAULT_ALLOCATION_STRATEGY =  "标准分配策略" ;
```
##bean(MstAllocationStrategy)
```
/wms-interface-common/src/main/java/com/haiziwang/kwms/common/domain/bean/MstAllocationStrategy.java

package  com.haiziwang.kwms.common.domain.bean;
import  java.io.Serializable;
import  java.util.Date;
public   class  MstAllocationStrategy  implements  Serializable {
     /**
     * 内部主键
     */
     private  Long sysNo;
     /**
     * 定位策略编码
     */
     private  String allocationStrategyNo;
     /**
     * 描述
     */
     private  String allocationStrategyDesc;
     /**
     * 仓库内部主键
     */
     private  String whSysNo;
     /**
     * 仓库编码
     */
     private  String whNo;
     /**
     * 仓库名称
     */
     private  String whName;
     /**
     * 货主内部主键
     */
     private  String companySysNo;
     /**
     * 货主编码
     */
     private  String companyCode;
     /**
     * 货主名称
     */
     private  String companyName;
     /**
     * 备注
     */
     private  String comments;
     /**
     * 是否有效：1是 0否
     */
     private  Integer yn;
     /**
     * 时间戳
     */
     private  Date ts;
     /**
     * 创建时间
     */
     private  Date createTime;
     private  Date updateTime;
     /**
     * 创建人
     */
     private  String createPin;
     private  String updatePin;
     private  Integer start;
     private  Integer rows;
     private   static   final   long  serialVersionUID = 1L;
     public  Long getSysNo() {
         return  sysNo;
    }
     public   void  setSysNo(Long sysNo) {
         this .sysNo = sysNo;
    }
     public  String getAllocationStrategyNo() {
         return  allocationStrategyNo;
    }
     public   void  setAllocationStrategyNo(String allocationStrategyNo) {
         this .allocationStrategyNo = allocationStrategyNo;
    }
     public  String getAllocationStrategyDesc() {
         return  allocationStrategyDesc;
    }
     public   void  setAllocationStrategyDesc(String allocationStrategyDesc) {
         this .allocationStrategyDesc = allocationStrategyDesc;
    }
     public  Integer getYn() {
         return  yn;
    }
     public   void  setYn(Integer yn) {
         this .yn = yn;
    }
     public  String getWhNo() {
         return  whNo;
    }
     public   void  setWhNo(String whNo) {
         this .whNo = whNo;
    }
     public  String getWhName() {
         return  whName;
    }
     public   void  setWhName(String whName) {
         this .whName = whName;
    }
     public  String getCompanyCode() {
         return  companyCode;
    }
     public   void  setCompanyCode(String companyCode) {
         this .companyCode = companyCode;
    }
     public  String getCompanyName() {
         return  companyName;
    }
     public   void  setCompanyName(String companyName) {
         this .companyName = companyName;
    }
     public  String getComments() {
         return  comments;
    }
     public   void  setComments(String comments) {
         this .comments = comments;
    }
     public  String getWhSysNo() {
         return  whSysNo;
    }
     public   void  setWhSysNo(String whSysNo) {
         this .whSysNo = whSysNo;
    }
     public  String getCompanySysNo() {
         return  companySysNo;
    }
     public   void  setCompanySysNo(String companySysNo) {
         this .companySysNo = companySysNo;
    }
     public  Date getUpdateTime() {
         return  updateTime;
    }
     public   void  setUpdateTime(Date updateTime) {
         this .updateTime = updateTime;
    }
     public  String getCreatePin() {
         return  createPin;
    }
     public   void  setCreatePin(String createPin) {
         this .createPin = createPin;
    }
     public  String getUpdatePin() {
         return  updatePin;
    }
     public   void  setUpdatePin(String updatePin) {
         this .updatePin = updatePin;
    }
     public  Date getTs() {
         return  ts;
    }
     public   void  setTs(Date ts) {
         this .ts = ts;
    }
     public  Date getCreateTime() {
         return  createTime;
    }
     public   void  setCreateTime(Date createTime) {
         this .createTime = createTime;
    }
     public  Integer getStart() {
         return  start;
    }
     public   void  setStart(Integer start) {
         this .start = start;
    }
     public  Integer getRows() {
         return  rows;
    }
     public   void  setRows(Integer rows) {
         this .rows = rows;
    }
}


```
##接口(MstAllocationStrategyDao)
```
/wms-interface-dao/src/main/java/com/haiziwang/kwms/downlink/dao/MstAllocationStrategyDao.java

package  com.haiziwang.kwms.downlink.dao;
import  com.haiziwang.kwms.common.dao.base.BaseDao;
import  com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy;
/**
 * Copyright: 2016 Haiziwang
 * *
 * Author:  Daniel Kong
 * Date:    2016 - 10 - 21
 * Desc:    分配策略 基础数据的 增删改查 服务
 */
public   interface  MstAllocationStrategyDao  extends  BaseDao<MstAllocationStrategy, Long> {
     /**
     * 根据策略编号或策略描述查询单条策略
     *
     *  @param  mstAllocationStrategy 查询条件
     *  @return  符合条件的分配策略
     */
    MstAllocationStrategy selectStrategy(MstAllocationStrategy mstAllocationStrategy);
}
```

##实现类(MstAllocationStrategyDaoImpl)
```
/wms-interface-dao/src/main/java/com/haiziwang/kwms/downlink/dao/impl/MstAllocationStrategyDaoImpl.java

package  com.haiziwang.kwms.downlink.dao.impl;
import  com.haiziwang.kwms.common.dao.base.BaseDaoSupport;
import  com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy;
import  com.haiziwang.kwms.downlink.dao.MstAllocationStrategyDao;
import  org.springframework.stereotype.Repository;
/**
 * Copyright: 2016 Haiziwang
 * *
 * Author:  Daniel Kong
 * Date:    2016 - 10 - 21
 * Desc:    分配策略 基础数据的 增删改查 服务
 */
@ Repository( "mstAllocationStrategyDao" )
public   class  MstAllocationStrategyDaoImpl  extends  BaseDaoSupport<MstAllocationStrategy, Long>  implements  MstAllocationStrategyDao {
     private   static   final  String NAMESPACE =  "mybatis.mapper.MstAllocationStrategyMapper" ;
     protected  MstAllocationStrategyDaoImpl() {
         super (NAMESPACE);
    }
     /**
     * 根据策略编号或策略描述查询单条策略
     *
     *  @param  mstAllocationStrategy 查询条件
     *  @return  符合条件的分配策略
     */
     @ Override
     public  MstAllocationStrategy selectStrategy(MstAllocationStrategy mstAllocationStrategy) {
         return   this .getMySqlSessionTemplate().selectOne(NAMESPACE +  ".selectStrategy" , mstAllocationStrategy);
    }
}
```

##mapper文件(MstAllocationStrategyMapper)
```
/wms-interface-dao/src/main/resources/mybatis.mapper/MstAllocationStrategyMapper.xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="mybatis.mapper.MstAllocationStrategyMapper">
    <resultMap id="BaseResultMap" type="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy">
        <id column="sys_no" property="sysNo" jdbcType="BIGINT"/>
        <result column="allocation_strategy_no" property="allocationStrategyNo" jdbcType="VARCHAR"/>
        <result column="allocation_strategy_desc" property="allocationStrategyDesc" jdbcType="VARCHAR"/>
        <result column="wh_sys_no" property="whSysNo" jdbcType="VARCHAR"/>
        <result column="wh_no" property="whNo" jdbcType="VARCHAR"/>
        <result column="wh_name" property="whName" jdbcType="VARCHAR"/>
        <result column="company_sys_no" property="companySysNo" jdbcType="VARCHAR"/>
        <result column="company_code" property="companyCode" jdbcType="VARCHAR"/>
        <result column="company_name" property="companyName" jdbcType="VARCHAR"/>
        <result column="comments" property="comments" jdbcType="VARCHAR"/>
        <result column="yn" property="yn" jdbcType="INTEGER"/>
        <result column="ts" property="ts" jdbcType="TIMESTAMP"/>
        <result column="create_time" property="createTime" jdbcType="TIMESTAMP"/>
        <result column="create_pin" property="createPin" jdbcType="VARCHAR"/>
        <result column="update_pin" property="updatePin" jdbcType="VARCHAR"/>
        <result column="update_time" property="updateTime" jdbcType="TIMESTAMP"/>
    </resultMap>
    <sql id="Base_Column_List">
    sys_no, allocation_strategy_no, allocation_strategy_desc, wh_sys_no, wh_no, wh_name, company_sys_no,
    company_code, company_name, comments, yn, ts, create_time, create_pin, update_pin,update_time
    </sql>
    <!--根据策略编号或策略描述查询单条策略-->
    <select id="selectStrategy" resultMap="BaseResultMap"
            parameterType="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy">
        SELECT
        <include refid="Base_Column_List"/>
        FROM mst_allocation_strategy
        <where>
            <if test="allocationStrategyNo != null and allocationStrategyNo != ''">
                AND allocation_strategy_no=#{allocationStrategyNo}
            </if>
            <if test="allocationStrategyDesc != null and allocationStrategyDesc != ''">
                AND allocation_strategy_desc=#{allocationStrategyDesc}
            </if>
            AND yn = 1
        </where>
    </select> </mapper>   

```

##测试用例(TestMstAllocationStrategyDao)
```
/wms-interface-dao/src/test/java/com/haiziwang/kwms/downlink/dao/TestMstAllocationStrategyDao.java

package  com.haiziwang.kwms.downlink.dao;
import  com.alibaba.fastjson.JSON;
import  com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy;
import  com.haiziwang.kwms.common.domain.constant.WmsConstants;
import  org.junit.Test;
import  org.junit.runner.RunWith;
import  org.slf4j.Logger;
import  org.slf4j.LoggerFactory;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.test.context.ContextConfiguration;
import  org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
/**
 * Copyright: 2016 Haiziwang
 * *
 * Author:  Daniel Kong
 * Date:    2016 - 10 - 21
 * Desc:    MstAllocationStrategyDao 的测试用例
 */
@ ContextConfiguration(locations = { "classpath:/config/test-spring-outbound-config.xml" })
@ RunWith(SpringJUnit4ClassRunner. class )
public   class  TestMstAllocationStrategyDao {
     private   static   final  Logger logger = LoggerFactory.getLogger(TestMstAllocationStrategyDao. class );
     @ Autowired
     private  MstAllocationStrategyDao mstAllocationStrategyDao;
     @ Test
     public   void  testSelectStrategy()  throws  Exception {
        MstAllocationStrategy queryStrategy =  new  MstAllocationStrategy();
        queryStrategy.setAllocationStrategyDesc(WmsConstants.DEFAULT_ALLOCATION_STRATEGY);
        MstAllocationStrategy strategy = mstAllocationStrategyDao.selectStrategy(queryStrategy);
        logger.info(JSON.toJSONString(strategy));
    }
}
```

##接口(MasterDataService)
```
/wms-interface-service/src/main/java/com/haiziwang/kwms/downlink/service/MasterDataService.java

package  com.haiziwang.kwms.downlink.service;
import  com.haiziwang.kwms.common.domain.bean.*;
import  com.haiziwang.kwms.common.domain.enums.SequenceEnum;
/**
 * Copyright: 2016 Haiziwang
 * *
 * Author:  Daniel Kong
 * Date:    2016 - 05 - 24
 * Desc:    为订单下发提供基础数据查询支持 (缓存优先)
 */
public   interface  MasterDataService {
     /**
     * 根据仓库编码从缓存或者数据库中获取仓库信息
     *
     *  @param  whNo 仓库编码
     *  @return  仓库信息
     */
    MstWh getMstWhFromNo(String whNo);
    
     /**
     * 获取 "标准分配策略"
     */
    MstAllocationStrategy selectStandardAllocStrategy();
    
}
```

##实现类(MasterDataServiceImpl)
```
/wms-interface-service/src/main/java/com/haiziwang/kwms/downlink/service/impl/MasterDataServiceImpl.java

package  com.haiziwang.kwms.downlink.service.impl;
import  com.haiziwang.kwms.common.domain.bean.*;
import  com.haiziwang.kwms.common.domain.constant.StringConstants;
import  com.haiziwang.kwms.common.domain.constant.WmsConstants;
import  com.haiziwang.kwms.common.domain.enums.SequenceEnum;
import  com.haiziwang.kwms.common.domain.enums.WMS3ExceptionCode;
import  com.haiziwang.kwms.common.domain.util.StringUtils;
import  com.haiziwang.kwms.downlink.dao.*;
import  com.haiziwang.kwms.downlink.service.MasterDataService;
import  com.haiziwang.kwms.downlink.service.MstSequenceService;
import  com.haiziwang.kwms.platform.service.KMemService;
import  org.slf4j.Logger;
import  org.slf4j.LoggerFactory;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.stereotype.Service;
/**
 * Copyright: 2016 Haiziwang
 * *
 * Author:  Daniel Kong
 * Date:    2016 - 05 - 24
 */
@ Service( "masterDataService" )
public   class  MasterDataServiceImpl  implements  MasterDataService {
     private   static   final  Logger logger = LoggerFactory.getLogger(MasterDataServiceImpl. class );
     private   static   final  String REDIS_KEY_WH_MAP =  "MasterData_Wh_" ;
     private   static   final  String REDIS_KEY_VENDOR_MAP =  "MasterData_Vendor_" ;
     private   static   final  String REDIS_KEY_CARRIER_MAP =  "MasterData_Carrier_" ;
     private   static   final  String REDIS_KEY_SHOP_MAP =  "MasterData_Shop_" ;
     private   static   final  String REDIS_KEY_SKU_MAP =  "MasterData_Sku_" ;
     private   static   final  String REDIS_KEY_ADDRESS_MAP =  "MasterData_Address_" ;
     private   static   final  String REDIS_KEY_ALLOCATION_STRATEGY =  "MasterData_Standard_AllocStrategy" ;
     private   static   final  String REDIS_KEY_PUTAWAY_STRATEGY =  "MasterData_Standard_PutawayStrategy" ;
     @ Autowired
     private  MstWhDao mstWhDao;
     @ Autowired
     private  MstVendorDao mstVendorDao;
     @ Autowired
     private  MstCarrierDao mstCarrierDao;
     @ Autowired
     private  MstSkuDao mstSkuDao;
     @ Autowired
     private  MstAddressDao mstAddressDao;
     @ Autowired
     private  MstShopDao mstShopDao;
     @ Autowired
     private  MstAllocationStrategyDao mstAllocationStrategyDao;
     @ Autowired
     private  MstPutawayStrategyDao mstPutawayStrategyDao;
     @ Autowired
     private  MstSequenceService mstSequenceService;
     @ Autowired
     private  KMemService kMemService;
     /**
     * 根据仓库编码从缓存或者数据库中获取仓库信息
     *
     *  @param  whNo 仓库编码
     *  @return  仓库信息
     */
     @ Override
     public  MstWh getMstWhFromNo(String whNo) {
        MstWh mstWh =  null ;
         boolean  isInCache =  true ;
        mstWh = kMemService.readObject(REDIS_KEY_WH_MAP + whNo, MstWh. class );
         if  (mstWh ==  null ) {
             //如果缓存没有该仓库信息，从数据库读取
            isInCache =  false ;
            mstWh = mstWhDao.selectByWhNo(whNo);
        }
         if  (mstWh ==  null  || mstWh.getSysNo() ==  null ) {
            logger.error(WMS3ExceptionCode.ORDER_DOWN_WH_NOT_EXIST.getDesout());
             throw   new  RuntimeException(WMS3ExceptionCode.getExceptionMsg(WMS3ExceptionCode.ORDER_DOWN_WH_NOT_EXIST));
        }
         if  (!isInCache)
            kMemService.writeObject(REDIS_KEY_WH_MAP + whNo, mstWh);
         return  mstWh;
    }
   
     /**
     * 获取 "标准分配策略"
     */
     @ Override
     public  MstAllocationStrategy selectStandardAllocStrategy() {
        MstAllocationStrategy retVal =  null ;
         boolean  isInCache =  true ;
        retVal = kMemService.readObject(REDIS_KEY_ALLOCATION_STRATEGY, MstAllocationStrategy. class );
         if  (retVal ==  null  || retVal.getSysNo() ==  null ) {
             //如果缓存没有该信息，从数据库读取
            isInCache =  false ;
            MstAllocationStrategy queryStrategy =  new  MstAllocationStrategy();
            queryStrategy.setAllocationStrategyDesc(WmsConstants.DEFAULT_ALLOCATION_STRATEGY);
            retVal = mstAllocationStrategyDao.selectStrategy(queryStrategy);
        }
         if  (!isInCache && retVal !=  null )
            kMemService.writeObject(REDIS_KEY_ALLOCATION_STRATEGY, retVal, 600);  // 十分钟过期
         return  retVal;
    }
}
```

##测试用例(TestMasterDataService)
```
/wms-interface-service/src/test/java/com/haiziwang/kwms/downlink/service/TestMasterDataService.java

package  com.haiziwang.kwms.downlink.service;
import  com.alibaba.fastjson.JSON;
import  com.haiziwang.kwms.common.domain.bean.*;
import  org.junit.Test;
import  org.junit.runner.RunWith;
import  org.slf4j.Logger;
import  org.slf4j.LoggerFactory;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.test.context.ContextConfiguration;
import  org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
/**
 * Copyright: 2016 Haiziwang
 * *
 * Author:  Daniel Kong
 * Date:    2016 - 06 - 27
 * Desc:    MasterDataService 的单元测试
 */
@ ContextConfiguration(locations = { "classpath:/config/test-spring-outbound-config.xml" })
@ RunWith(SpringJUnit4ClassRunner. class )
public   class  TestMasterDataService {
     private   static   final  Logger logger = LoggerFactory.getLogger(TestMasterDataService. class );
     @ Autowired
     private  MasterDataService masterDataService;
     @ Test
     public   void  testGetMstWhFromNo() {
        MstWh mstWh = masterDataService.getMstWhFromNo( "9002" );
        logger.info(JSON.toJSONString(mstWh));
    }
    
     @ Test
     public   void  testSelectStandardAllocStrategy()  throws  Exception {
        MstAllocationStrategy strategy = masterDataService.selectStandardAllocStrategy();
        logger.info(JSON.toJSONString(strategy));
    }
}
```

##log4j.properties
```
/wms-interface-web/src/main/resources/log4j.properties

更新 log配置，最多放 10 个备份  
########################
# Rolling File         #
########################
log4j.appender.ROLLING_FILE= org.apache.log4j.RollingFileAppender
log4j.appender.ROLLING_FILE.File= ${log.location}/${log.name}
log4j.appender.ROLLING_FILE.Append= true
log4j.appender.ROLLING_FILE.MaxFileSize= 50MB
log4j.appender.ROLLING_FILE.MaxBackupIndex= 10
log4j.appender.ROLLING_FILE.layout= org.apache.log4j.PatternLayout
log4j.appender.ROLLING_FILE.layout.ConversionPattern= [KWMS   %d{yyy-MM-dd   HH:mm:ss,SSS}](%p)   -   %c   -   (%F:%L)   %m   %n  

```

##mapper文件(MstAllocationStrategyMapper)
```

/wms-rpc-outbound-dao/src/main/resources/mybatis.mapper/MstAllocationStrategyMapper.xml

<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="mybatis.mapper.MstAllocationStrategyMapper" >
  <resultMap id="BaseResultMap" type="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy" >
    <id column="sys_no" property="sysNo" jdbcType="BIGINT" />
    <result column="allocation_strategy_no" property="allocationStrategyNo" jdbcType="VARCHAR" />
    <result column="allocation_strategy_desc" property="allocationStrategyDesc" jdbcType="VARCHAR" />
    <result column="wh_sys_no" property="whSysNo" jdbcType="VARCHAR" />
    <result column="wh_no" property="whNo" jdbcType="VARCHAR" />
    <result column="wh_name" property="whName" jdbcType="VARCHAR" />
    <result column="company_sys_no" property="companySysNo" jdbcType="VARCHAR" />
    <result column="company_code" property="companyCode" jdbcType="VARCHAR" />
    <result column="company_name" property="companyName" jdbcType="VARCHAR" />
    <result column="comments" property="comments" jdbcType="VARCHAR" />
    <result column="yn" property="yn" jdbcType="INTEGER" />
    <result column="ts" property="ts" jdbcType="TIMESTAMP" />
    <result column="create_time" property="createTime"  jdbcType="TIMESTAMP" />
    <result column="create_pin" property="createPin" jdbcType="VARCHAR" />
    <result column="update_pin" property="updatePin" jdbcType="VARCHAR" />
    <result column="update_time" property="updateTime"  jdbcType="TIMESTAMP" />
  </resultMap>
  
  <sql id="Base_Column_List" >
    sys_no, allocation_strategy_no, allocation_strategy_desc, wh_sys_no, wh_no, wh_name, 
    company_sys_no, company_code, company_name, comments, 
    yn, ts, create_time, create_pin, update_pin,update_time
  </sql>
    <sql id="query_where">
        <if test="sysNo != null and sysNo != ''" >
            and sys_no = #{sysNo}
        </if>
        <if test="allocationStrategyNo != null and allocationStrategyNo != ''" >
            and allocation_strategy_no=#{allocationStrategyNo}
        </if>
        <if test="allocationStrategyDesc != null and allocationStrategyDesc != ''" >
            and allocation_strategy_desc like concat('%', #{allocationStrategyDesc}, '%')
        </if>
        <if test="whSysNo != null and whSysNo !=''" >
            and wh_sys_no=#{whSysNo}
        </if>
        <if test="companySysNo != null and companySysNo !=''" >
            and company_sys_no=#{companySysNo}
        </if>
        <if test="whNo != null and whNo !=''" >
            and wh_no=#{whNo}
        </if>
        <if test="companyCode != null and companyCode != ''" >
            and company_code=#{companyCode}
        </if>
        <if test="yn != null and yn != ''" >
            and yn=#{yn}
        </if>
    </sql>
  
  <select id="selectMstAllocationStrategyByList" resultMap="BaseResultMap" parameterType="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy" >
    select 
    <include refid="Base_Column_List" />
    from mst_allocation_strategy
      where 1=1
      <include refid="query_where" />
    order by sys_no desc limit #{start}, #{rows}
  </select>
  
  <select id="selectMstAllocationStrategyByCount" resultType="java.lang.Integer" parameterType="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy" >
    select 
        count(1)
    from mst_allocation_strategy
      where 1=1
      <include refid="query_where" />
  </select>  
  
  <select id="selectByPrimaryKey" resultMap="BaseResultMap" parameterType="java.lang.Long" >
    select 
    <include refid="Base_Column_List" />
    from mst_allocation_strategy
    where sys_no = #{pk,jdbcType=BIGINT}
  </select>
  
  <delete id="deleteByPrimaryKey" parameterType="java.lang.Long" >
    delete from mst_allocation_strategy
    where sys_no = #{pk,jdbcType=BIGINT}
  </delete>
  
  <insert id="insert" parameterType="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy" >
      INSERT INTO mst_allocation_strategy
        (
        allocation_strategy_no, allocation_strategy_desc, 
         wh_sys_no, wh_no, wh_name, company_sys_no, company_code, company_name, comments, 
          yn, ts, create_time, 
          create_pin, update_pin,update_time
        )
        SELECT #{allocationStrategyNo}, #{allocationStrategyDesc}, 
        t1.wh_sys_no,t1.wh_no,t1.wh_name,t2.company_sys_no,t2.company_code,
        t2.company_name,#{comments},#{yn},NOW(),NOW(),#{createPin},#{updatePin},NOW() 
        FROM (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE  wh _no = #{ whNo }) t1,
        (SELECT sys_no AS company_sys_no,company_code,company_name FROM mst_company WHERE sys_no = #{companySysNo}) t2
        <selectKey keyProperty="sysNo" order="AFTER" resultType="Long">
            select last_insert_id() as sys_no
        </selectKey>
  </insert>
  <insert id="insertSelective" useGeneratedKeys="true" keyProperty="sysNo" parameterType="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy" >
    insert into mst_allocation_strategy
    <trim prefix="(" suffix=")" suffixOverrides="," >
      <if test="sysNo != null" >
        sys_no,
      </if>
      <if test="allocationStrategyNo != null" >
        allocation_strategy_no,
      </if>
      <if test="allocationStrategyDesc != null" >
        allocation_strategy_desc,
      </if>
      <if test="whNo != null" >
        wh_no,
      </if>
      <if test="whName != null" >
        wh_name,
      </if>
      <if test="companyCode != null" >
        company_code,
      </if>
      <if test="companyName != null" >
        company_name,
      </if>
      <if test="comments != null" >
        comments,
      </if>
      <if test="yn != null" >
        yn,
      </if>
      <if test="ts != null" >
        ts,
      </if>
        create_time,
      <if test="createPin != null" >
        create_pin,
      </if>
      <if test="updatePin != null" >
        update_pin,
      </if>
    </trim>
    <trim prefix="values (" suffix=")" suffixOverrides="," >
      <if test="sysNo != null" >
        #{sysNo,jdbcType=BIGINT},
      </if>
      <if test="allocationStrategyNo != null" >
        #{allocationStrategyNo,jdbcType=VARCHAR},
      </if>
      <if test="allocationStrategyDesc != null" >
        #{allocationStrategyDesc,jdbcType=VARCHAR},
      </if>
      <if test="whNo != null" >
        #{whNo,jdbcType=VARCHAR},
      </if>
      <if test="whName != null" >
        #{whName,jdbcType=VARCHAR},
      </if>
      <if test="companyCode != null" >
        #{companyCode,jdbcType=VARCHAR},
      </if>
      <if test="companyName != null" >
        #{companyName,jdbcType=VARCHAR},
      </if>      
      <if test="comments != null" >
        #{comments,jdbcType=VARCHAR},
      </if>
      <if test="yn != null" >
        #{yn,jdbcType=INTEGER},
      </if>
      <if test="ts != null" >
        #{ts,jdbcType=TIMESTAMP},
      </if>
        now(),
      <if test="createPin != null" >
        #{createPin,jdbcType=VARCHAR},
      </if>
      <if test="updatePin != null" >
        #{updatePin,jdbcType=VARCHAR},
      </if>
    </trim>
  </insert>
  
  <update id="updateByPrimaryKeySelective" parameterType="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy" >
    update mst_allocation_strategy
    <set >
      <if test="allocationStrategyNo != null" >
        allocation_strategy_no = #{allocationStrategyNo,jdbcType=VARCHAR},
      </if>
      <if test="allocationStrategyDesc != null" >
        allocation_strategy_desc = #{allocationStrategyDesc,jdbcType=VARCHAR},
      </if>
      <if test="whSysNo != null  and whSysNo != ''"  >
          wh_sys_no = #{whSysNo},
          wh_no = (select t1.wh_no from (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE sys_no = #{whSysNo}) t1),
        wh_name = (select t1.wh_name from (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE sys_no = #{whSysNo}) t1),
      </if>
      <if test="whNo != null and whNo != ''" >
        wh_sys_no = (select t1.wh_sys_no from (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE wh_no = #{whNo}) t1),
        wh_no = (select t1.wh_no from (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE wh_no = #{whNo}) t1),
        wh_name = (select t1.wh_name from (SELECT sys_no AS wh_sys_no,wh_no,wh_name FROM mst_wh WHERE wh_no = #{whNo}) t1),
      </if>
      <if test="companySysNo != null" >
          company_sys_no = #{companySysNo,jdbcType=VARCHAR},
          company_code = (select t1.company_code from (SELECT sys_no AS company_sys_no,company_code,company_name FROM mst_company WHERE sys_no = #{companySysNo}) t1),
        company_name = (select t1.company_name from (SELECT sys_no AS company_sys_no,company_code,company_name FROM mst_company WHERE sys_no = #{companySysNo}) t1),
      </if>
      <if test="comments != null" >
        comments = #{comments,jdbcType=VARCHAR},
      </if>
      <if test="yn != null" >
        yn = #{yn,jdbcType=INTEGER},
      </if>
      <if test="ts != null" >
        ts = #{ts,jdbcType=TIMESTAMP},
      </if>
      <if test="createPin != null" >
        create_pin = #{createPin,jdbcType=VARCHAR},
      </if>
      <if test="updatePin != null" >
        update_pin = #{updatePin,jdbcType=VARCHAR},
      </if>
      update_time = now()
    </set>
    where sys_no = #{sysNo,jdbcType=BIGINT}
  </update>
  
  <update id="updateByPrimaryKey" parameterType="com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy" >
    update mst_allocation_strategy
    set allocation_strategy_no = #{allocationStrategyNo,jdbcType=VARCHAR},
      allocation_strategy_desc = #{allocationStrategyDesc,jdbcType=VARCHAR},
      wh_no = #{whNo,jdbcType=VARCHAR},
      wh_name = #{whName,jdbcType=VARCHAR},
      company_code=#{companyCode,jdbcType=VARCHAR},
      company_name=#{companyName,jdbcType=VARCHAR},
      comments = #{comments,jdbcType=VARCHAR},
      yn = #{yn,jdbcType=INTEGER},
      ts = #{ts,jdbcType=TIMESTAMP},
      create_time = #{createTime,jdbcType=TIMESTAMP},
      create_pin = #{createPin,jdbcType=VARCHAR},
      update_pin = #{updatePin,jdbcType=VARCHAR},
      update_time = #{updateTime,jdbcType=TIMESTAMP}
    where sys_no = #{sysNo,jdbcType=BIGINT}
  </update> </mapper>   

```

##rule(sys-operate-log-rule)
```
/wms-rpc-outbound-dao/src/main/resources/rules/sys-operate-log-rule.xml

<rules>
    <rule>
        <namespace>mybatis.mapper.SysOperateLogMapper</namespace>
        <shards>partition2</shards>
    </rule> </rules>   

```
##接口（AllocationStrategyBusinessService）
```
/wms-rpc-outbound-service/src/main/java/com/haiziwang/kwms/outbound/business/AllocationStrategyBusinessService.java

package com.haiziwang.kwms.outbound.business;

import com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyListResDto;
import com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyReqDto;
import com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyResDto;
import org.springframework.transaction.annotation.Transactional;

import java.lang.reflect.InvocationTargetException;
import java.util.List;

public interface AllocationStrategyBusinessService
{

    /**
     * @param reqDto 请求参数对象
     * @return
     * @throws NoSuchMethodException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @desc 根据查询条件查询列表
     */
    @Transactional
    public MstAllocationStrategyListResDto selectMstAllocationStrategyByList(MstAllocationStrategyReqDto reqDto)
            throws IllegalAccessException, InvocationTargetException, NoSuchMethodException;



    /**
     * @param reqDto
     * @throws NoSuchMethodException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @desc 新增新纪录
     */
    @Transactional
    public void addMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
            throws IllegalAccessException, InvocationTargetException, NoSuchMethodException;

    /**
     * @param reqDto
     * @return
     * @throws NoSuchMethodException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @desc 新增新纪录
     */
    @Transactional
    public void delMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
            throws IllegalAccessException, InvocationTargetException, NoSuchMethodException;

    /**
     * @param reqDto
     * @return
     * @throws NoSuchMethodException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @desc 查询纪录
     */
    @Transactional
    public MstAllocationStrategyResDto selectMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
            throws IllegalAccessException, InvocationTargetException, NoSuchMethodException;

    /**
     * @param reqDto
     * @return
     * @throws NoSuchMethodException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @desc 更新纪录
     */
    @Transactional
    public void updateMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
            throws IllegalAccessException, InvocationTargetException, NoSuchMethodException;

    /**
     * @param reqDto
     * @return
     * @throws NoSuchMethodException
     * @throws InvocationTargetException
     * @throws IllegalAccessException
     * @desc 批量更新纪录
     */
    @Transactional
    public void updateMstAllocationStrategy(List<MstAllocationStrategyReqDto> reqDto)
            throws IllegalAccessException, InvocationTargetException, NoSuchMethodException;

    @Transactional
    public Integer selectMstAllocationStrategyByCount(MstAllocationStrategyReqDto reqDto);

}

```

##实现类（AllocationStrategyBusinessServiceImpl）
```
/wms-rpc-outbound-service/src/main/java/com/haiziwang/kwms/outbound/business/impl/AllocationStrategyBusinessServiceImpl.java

package  com.haiziwang.kwms.outbound.business.impl;
import  java.lang.reflect.InvocationTargetException;
import  java.util.Date;
import  java.util.List;
import  com.haiziwang.kwms.common.domain.util.StringUtils;
import  org.apache.commons.beanutils.PropertyUtils;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.stereotype.Service;
import  org.springframework.transaction.annotation.Transactional;
import  com.haiziwang.kwms.common.domain.bean.MstAllocationStrategy;
import  com.haiziwang.kwms.common.domain.bean.MstAllocationStrategyDetail;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyListResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyResDto;
import  com.haiziwang.kwms.common.domain.enums.OrderBusinessTypeEnum;
import  com.haiziwang.kwms.common.domain.enums.ResultStateEnum;
import  com.haiziwang.kwms.common.domain.enums.RuleSpaceEnum;
import  com.haiziwang.kwms.common.domain.enums.RuleTimeEnum;
import  com.haiziwang.kwms.common.domain.exception.WMS3UnCheckedException;
import  com.haiziwang.kwms.outbound.business.AllocationStrategyBusinessService;
import  com.haiziwang.kwms.outbound.dao.MstAllocationStrategyDao;
import  com.haiziwang.kwms.outbound.dao.MstAllocationStrategyDetailDao;
@ Service( "allocationStrategyBusinessService" )
public   class  AllocationStrategyBusinessServiceImpl  implements  AllocationStrategyBusinessService
{
     @ Autowired
     private  MstAllocationStrategyDao mstAllocationStrategyDao;
     @ Autowired
     private  MstAllocationStrategyDetailDao mstAllocationStrategyDetailDao;
     @ Override
     public  MstAllocationStrategyListResDto selectMstAllocationStrategyByList(MstAllocationStrategyReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
        MstAllocationStrategyListResDto resDto =  new  MstAllocationStrategyListResDto();
        MstAllocationStrategy mstAllocationStrategy =  new  MstAllocationStrategy();
        PropertyUtils.copyProperties(mstAllocationStrategy, reqDto);
         int  cont = mstAllocationStrategyDao.selectMstAllocationStrategyDaoByCont(mstAllocationStrategy); //根据条件查询总记录数
        List<MstAllocationStrategy> list = mstAllocationStrategyDao.selectMstAllocationStrategyByList(mstAllocationStrategy);
        resDto.setResultCode(String.valueOf(ResultStateEnum.SUCCESS.getCode()));
        resDto.setResultMsg(ResultStateEnum.SUCCESS.getDesc());
        resDto.setList(list);
        resDto.setTotalCont(cont);
         return  resDto;
    }
     @ Override
     public  Integer selectMstAllocationStrategyByCount(MstAllocationStrategyReqDto reqDto)
    {
        MstAllocationStrategy mstAllocationStrategy =  new  MstAllocationStrategy();
        mstAllocationStrategy.setAllocationStrategyNo(reqDto.getAllocationStrategyNo());
         return  mstAllocationStrategyDao.selectMstAllocationStrategyDaoByCont(mstAllocationStrategy);
    }
     @ Override
     @ Transactional
     public   void  addMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException, WMS3UnCheckedException
    {
        MstAllocationStrategy mstAllocationStrategy =  new  MstAllocationStrategy();
        mstAllocationStrategy.setAllocationStrategyNo(reqDto.getAllocationStrategyNo());
         int  n = mstAllocationStrategyDao.selectMstAllocationStrategyDaoByCont(mstAllocationStrategy);
         if  (n > 0)
        {
             throw   new  WMS3UnCheckedException(ResultStateEnum.FAIL.getCode().toString(),  "重复的编码" );
        }
        PropertyUtils.copyProperties(mstAllocationStrategy, reqDto);
         //        mstAllocationStrategy.setAllocationStrategyNo(mstSequenceService.getSequence(SequenceEnum.ALLOCATION));
        mstAllocationStrategyDao.addMstAllocationStrategy(mstAllocationStrategy);
        List<MstAllocationStrategyDetail> list = reqDto.getList();
        Date d =  new  Date();
         if  ( null  != list && !list.isEmpty())
        {
             for  (MstAllocationStrategyDetail detail : list)
            {
                detail.setAllocationStrategySysNo(mstAllocationStrategy.getSysNo());
//                detail.setIsRep(0);
                detail.setCreateTime(d);
                detail.setCreatePin(reqDto.getCreatePin());
                detail.setUpdatePin(reqDto.getCreatePin());
                detail.setUpdateTime(d);
                mstAllocationStrategyDetailDao.addMstAllocationStrategyDetail(detail);
            }
        }
        
    }
     @ Override
     @ Transactional
     public   void  delMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
        MstAllocationStrategy mstAllocationStrategy =  new  MstAllocationStrategy();
        PropertyUtils.copyProperties(mstAllocationStrategy, reqDto);
        mstAllocationStrategyDao.deleteMstAllocationStrategyBySynNo(mstAllocationStrategy);
        MstAllocationStrategyDetail detail =  new  MstAllocationStrategyDetail();
        detail.setAllocationStrategySysNo(mstAllocationStrategy.getSysNo());
        mstAllocationStrategyDetailDao.delMstAllocationStrategyDetailByMoveSynNo(detail);
    }
     @ Override
     public  MstAllocationStrategyResDto selectMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
        MstAllocationStrategyResDto resDto =  new  MstAllocationStrategyResDto();
        MstAllocationStrategy mstAllocationStrategy =  new  MstAllocationStrategy();
        PropertyUtils.copyProperties(mstAllocationStrategy, reqDto);
        mstAllocationStrategy = mstAllocationStrategyDao.queryOneBySysNo(mstAllocationStrategy);
        PropertyUtils.copyProperties(resDto, mstAllocationStrategy);
        MstAllocationStrategyDetail detail =  new  MstAllocationStrategyDetail();
        detail.setAllocationStrategySysNo(mstAllocationStrategy.getSysNo());
        List<MstAllocationStrategyDetail> list = mstAllocationStrategyDetailDao.selectMstAllocationStrategyDetailByList(detail);
         if  ( null  != list && !list.isEmpty())
        {
             for  (MstAllocationStrategyDetail detailInfo : list)
            {
                detailInfo.setOrderTypeName(OrderBusinessTypeEnum.getByCode(detailInfo.getOrderType()).getDesc());
                 if  ( null  != detailInfo.getAllocRuleTime() && ! "" .equals(detailInfo.getAllocRuleTime()))
                {
                    detailInfo.setAllocRuleTimeName(RuleTimeEnum.getByCode(detailInfo.getAllocRuleTime()).getDesc());
                }
                 if  ( null  != detailInfo.getAllocRuleSpace() && ! "" .equals(detailInfo.getAllocRuleSpace()))
                {
                    detailInfo.setAllocRuleSpaceName(RuleSpaceEnum.getByCode(detailInfo.getAllocRuleSpace()).getDesc());
                }
            }
        }
        resDto.setList(list);
         return  resDto;
    }
     /**
     *  @param  reqDto
     *  @return
     *  @throws  NoSuchMethodException
     *  @throws  InvocationTargetException
     *  @throws  IllegalAccessException
     *  @desc  编辑，删除明细，重新插入
     */
     @ Override
     @ Transactional
     public   void  updateMstAllocationStrategy(MstAllocationStrategyReqDto reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
        MstAllocationStrategy mstAllocationStrategy =  new  MstAllocationStrategy();
        PropertyUtils.copyProperties(mstAllocationStrategy, reqDto);
        mstAllocationStrategyDao.updateMstAllocationStrategyBySynNo(mstAllocationStrategy);
        MstAllocationStrategyDetail detail =  new  MstAllocationStrategyDetail();
        detail.setAllocationStrategySysNo(mstAllocationStrategy.getSysNo());
        mstAllocationStrategyDetailDao.delMstAllocationStrategyDetailByMoveSynNo(detail);
        List<MstAllocationStrategyDetail> list = reqDto.getList();
        Date date =  new  Date();
         for  (MstAllocationStrategyDetail d : list)
        {
            d.setAllocationStrategySysNo(mstAllocationStrategy.getSysNo());
            d.setCreateTime(date);
            d.setCreatePin(reqDto.getCreatePin());
            d.setUpdatePin(reqDto.getCreatePin());
            d.setUpdateTime(date);
            mstAllocationStrategyDetailDao.addMstAllocationStrategyDetail(d);
        }
    }
     @ Override
     @ Transactional
     public   void  updateMstAllocationStrategy(List<MstAllocationStrategyReqDto> reqDto)
             throws  IllegalAccessException, InvocationTargetException, NoSuchMethodException
    {
         for  (MstAllocationStrategyReqDto d : reqDto)
        {
            MstAllocationStrategy mstAllocationStrategy =  new  MstAllocationStrategy();
            PropertyUtils.copyProperties(mstAllocationStrategy, d);
            mstAllocationStrategyDao.updateMstAllocationStrategyBySynNo(mstAllocationStrategy);
        }
    }
}
```

##Controller(MstAllocationStrategyController)
```
/wms-rpc-outbound-web/src/main/java/com/haiziwang/kwms/outbound/controller/MstAllocationStrategyController.java

package  com.haiziwang.kwms.outbound.controller;
import  java.util.ArrayList;
import  java.util.List;
import  org.slf4j.Logger;
import  org.slf4j.LoggerFactory;
import  org.springframework.beans.factory.annotation.Autowired;
import  org.springframework.web.bind.annotation.RequestBody;
import  org.springframework.web.bind.annotation.RequestMapping;
import  org.springframework.web.bind.annotation.RequestMethod;
import  org.springframework.web.bind.annotation.RequestParam;
import  org.springframework.web.bind.annotation.RestController;
import  com.alibaba.fastjson.JSON;
import  com.haiziwang.kwms.common.domain.bean.EnumBean;
import  com.haiziwang.kwms.common.domain.bean.SysDataFilter;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyListResDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyReqDto;
import  com.haiziwang.kwms.common.domain.dto.mst.MstAllocationStrategyResDto;
import  com.haiziwang.kwms.common.domain.enums.RuleSpaceEnum;
import  com.haiziwang.kwms.common.domain.enums.RuleTimeEnum;
import  com.haiziwang.kwms.common.domain.util.PageRespJson;
import  com.haiziwang.kwms.common.domain.util.RespJson;
import  com.haiziwang.kwms.common.domain.util.StringUtils;
import  com.haiziwang.kwms.outbound.service.ISysDataFilterService;
import  com.haiziwang.kwms.outbound.service.MstAllocationStrategyService;
/**
 * Created by Administrator on 2016/2/26.
 */
@ RestController
public   class  MstAllocationStrategyController {
     private   static   final  Logger logger = LoggerFactory.getLogger(MstAllocationStrategyController. class );
     @ Autowired
     private  MstAllocationStrategyService mstAllocationStrategyService;
    
     @ Autowired
     private  ISysDataFilterService sysDataFilterService;
     /**
     * 根据查询条件获取 分配策略 列表
     *
     *  @param  draw               请求ID （Datatable 后端分页功能需要该参数）
     *  @param  start               待查询的开始索引，该参数用于SQL查询语句 limit a, b 的 a 部分
     *  @param  rows                返回记录条数，该参数用于SQL查询语句 limit a, b 的 b 部分
     *  @param  allocationStrategyDesc 描述
     *  @return  符合查询条件的 分配策略 列表
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/list" , method = RequestMethod.GET)
     public  PageRespJson getAllocationStrategyList( @ RequestParam( "draw" )  int  draw,
                                                @ RequestParam( "start" )  int  start,
                                                @ RequestParam( "rows" )  int  rows,
                                                @ RequestParam(value =  "allocationStrategyDesc" , required =  false ) String allocationStrategyDesc,
                                                @ RequestParam(value =  "whNo" , required =  false ) String whNo,
                                                @ RequestParam(value =  "companySysNo" , required =  false ) String companySysNo) {
        logger.info( "查询分配策略列表" );
         /**
         * 根据url参数构建 分配策略 请求bean
         */
        MstAllocationStrategyReqDto reqDto =  new  MstAllocationStrategyReqDto(draw, start, rows);
        reqDto.setAllocationStrategyDesc(allocationStrategyDesc);
        reqDto.setWhNo(whNo);
        reqDto.setCompanySysNo(companySysNo);
        reqDto.setStart(start);
        reqDto.setRows(rows);
        MstAllocationStrategyListResDto mstAllocationStrategyListResDto =  null ;
         try  {
            mstAllocationStrategyListResDto = mstAllocationStrategyService.selectMstAllocationStrategyByList(reqDto);
        }  catch  (Exception e) {
             return  PageRespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()), draw);
        }
         if  (mstAllocationStrategyListResDto ==  null )
            mstAllocationStrategyListResDto =  new  MstAllocationStrategyListResDto();
         return  PageRespJson.buildSuccessResponse(mstAllocationStrategyListResDto.getList(), draw, mstAllocationStrategyListResDto.getTotalCont());
    }
     @ RequestMapping(value =  "/mst-allocation-strategy/alllist" , method = RequestMethod.GET)
     public  PageRespJson getAllocationStrategyAllList( @ RequestParam(value =  "whSysNo" , required =  false ) String whSysNo,
     @ RequestParam(value =  "whNo" , required =  false ) String whNo) {
        logger.info( "查询所有分配策略列表" );
         /**
         * 根据url参数构建 分配策略 请求bean
         */
        MstAllocationStrategyReqDto reqDto =  new  MstAllocationStrategyReqDto(1, 0, 10000);
        reqDto.setStart(0);
        reqDto.setRows(10000);
        reqDto.setWhSysNo(whSysNo);
        reqDto.setWhNo(whNo);
        MstAllocationStrategyListResDto mstAllocationStrategyListResDto =  null ;
         try  {
            mstAllocationStrategyListResDto = mstAllocationStrategyService.selectMstAllocationStrategyByList(reqDto);
        }  catch  (Exception e) {
             return  PageRespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()), 1);
        }
         if  (mstAllocationStrategyListResDto ==  null )
            mstAllocationStrategyListResDto =  new  MstAllocationStrategyListResDto();
         return  PageRespJson.buildSuccessResponse(mstAllocationStrategyListResDto.getList(), 1, mstAllocationStrategyListResDto.getTotalCont());
    }
     /**
     * 创建新的 分配策略
     *
     *  @param  reqDto 待创建的分配策略
     *  @return  创建成功/失败的结果
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/create" , method = RequestMethod.POST, headers = {
             "Accept=application/json; charset=UTF-8" ,  "Content-Type=application/json" })
     public  RespJson createAllocationStrategy( @ RequestBody MstAllocationStrategyReqDto reqDto) {
        logger.info( "创建新的分配策略："  + JSON.toJSONString(reqDto));
         try  {
            mstAllocationStrategyService.addMstAllocationStrategy(reqDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
         return  RespJson.buildSuccessResponse();
    }
    
     /**
     * 修改分配策略
     *
     *  @param  reqDto 待修改的分配策略
     *  @return  修改成功/失败的结果
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/update" , method = RequestMethod.POST, headers = {
             "Accept=application/json; charset=UTF-8" ,  "Content-Type=application/json" })
     public  RespJson updateAllocationStrategy( @ RequestBody MstAllocationStrategyReqDto reqDto) {
        logger.info( "修改分配策略："  + JSON.toJSONString(reqDto));
         try  {
            mstAllocationStrategyService.updateMstAllocationStrategy(reqDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
         return  RespJson.buildSuccessResponse();
    }
     /**
     * 查询单条 分配策略
     *
     *  @param  sysNo 分配策略内部主键
     *  @return  查询到的分配策略信息
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/info" , method = RequestMethod.GET)
     public  RespJson getAllocationStrategyDetailList( @ RequestParam( "id" )  long  sysNo) {
        logger.info( "查询单条 分配策略：sysNo = "  + sysNo);
        MstAllocationStrategyReqDto reqDto =  new  MstAllocationStrategyReqDto();
        reqDto.setSysNo(sysNo);
         try  {
            MstAllocationStrategyResDto resDto = mstAllocationStrategyService.selectMstAllocationStrategy(reqDto);
             return  RespJson.buildSuccessResponse(resDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
    }
     /**
     * 删除单条 分配策略
     *
     *  @param  sysNo 分配策略内部主键
     *  @return
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/delete" , method = RequestMethod.GET)
     public  RespJson delAllocationStrategyList( @ RequestParam( "id" )  long  sysNo) {
        logger.info( "删除单条 分配策略：sysNo = "  + sysNo);
        MstAllocationStrategyReqDto reqDto =  new  MstAllocationStrategyReqDto();
        reqDto.setSysNo(sysNo);
         try  {
            MstAllocationStrategyResDto resDto = mstAllocationStrategyService.delMstAllocationStrategy(reqDto);
             return  RespJson.buildSuccessResponse(resDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
    }
    
    
     /**
     * 获取时间定位规则列表 (Add by liuxy)
     *
     *  @return  符合条件的信息集
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/ruleTimeList" , method = RequestMethod.GET)
     public  RespJson selectRuleTimeList() {
        List<EnumBean> enumValueList =  new  ArrayList<EnumBean>();
         for  (RuleTimeEnum ruleTimeEnum : RuleTimeEnum.values()) {
            EnumBean enumBean =  new  EnumBean();
            enumBean.setKey(String.valueOf(ruleTimeEnum.getCode()));
            enumBean.setValue(ruleTimeEnum.getDesc());
            enumValueList.add(enumBean);
        }
         return  RespJson.buildSuccessResponse(enumValueList);
    }
    
    
     /**
     * 获取空间定位规则列表 (Add by liuxy)
     *
     *  @return  符合条件的信息集
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/ruleSpaceList" , method = RequestMethod.GET)
     public  RespJson selectRuleSpaceList() {
        List<EnumBean> enumValueList =  new  ArrayList<EnumBean>();
         for  (RuleSpaceEnum ruleSpaceEnum : RuleSpaceEnum.values()) {
            EnumBean enumBean =  new  EnumBean();
            enumBean.setKey(String.valueOf(ruleSpaceEnum.getCode()));
            enumBean.setValue(ruleSpaceEnum.getDesc());
            enumValueList.add(enumBean);
        }
         return  RespJson.buildSuccessResponse(enumValueList);
    }
    
     /**
     * 根据sysNo查找对应的数据筛选器信息
     *
     *  @param  sysNo 数据筛选器编码
     *  @return  查询到的数据筛选器信息
     */
     @ RequestMapping(value =  "/mst-allocation-strategy/getDataFilterInfo" , method = RequestMethod.GET)
     public  RespJson getDataFilterInfo( @ RequestParam( "sysNo" ) Long sysNo) {
        logger.info( "查询单条 数据筛选器信息：sysNo = "  + sysNo);
        SysDataFilter reqDto =  new  SysDataFilter();
        reqDto.setSysNo(sysNo);
         try  
        {
            SysDataFilter resDto = sysDataFilterService.selectSysDataFilerInfo(reqDto);
             return  RespJson.buildSuccessResponse(resDto);
        }  catch  (Exception e) {
             return  RespJson.buildFailureResponse(StringUtils.getValidString(e.getMessage()));
        }
    }
    
}
```

##allocation-strategy-add-controller.js
```
/wms-web/src/main/webapp/js/controllers/strategy/allocation-strategy-add-controller.js

wmsApp.controller( 'allocateStrategyAddController' , [ '$scope' ,  '$rootScope' , '$modal' , '$compile' ,  '$modalInstance' ,  'authService' ,  'masterDataService' , 'allocationStrategyService' ,  'DTInstances' ,  'DTOptionsBuilder' ,  'DTColumnDefBuilder' ,
     function  ($scope, $rootScope,$modal, $compile, $modalInstance, authService, masterDataService, allocationStrategyService, DTInstances, DTOptionsBuilder, DTColumnDefBuilder) {
        $scope.criteria = {
            toWhSysNo:  '' ,
            companySysNo:  ''
        };
        $scope.isDisabled =  true ;
        
        $scope.selectIdSet = {};
        $scope.allIds = [];
        $scope.allCheckItem =  false ;
         /**
          *   选中某一个
          */
        $scope.checkItemfrom =  function  (sysNo) {
             if ($scope.selectIdSet[sysNo] == -1){
                 delete   $scope.selectIdSet[sysNo];
            } else {
                $scope.selectIdSet[sysNo] = -1;
            }
        };
        
         /**
          *   提交到后台的bean对象
          */
        $scope.allocStrategyBean = {
            allocationStrategyDesc :  '' ,
            whNo:  '' ,
            companySysNo:  '' ,
            list:[]
        };
         // 设置默认仓库
        $scope.whNo = authService.getUser().whNo;
         /**
          *   获取仓库列表
          */
        $scope.toWhOptions = [];
         if  ($scope.toWhOptions.length === 0) {
            masterDataService.getWhList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.toWhOptions = reply.data;
                }
            });
        }
        $scope.changeToWh =  function  (toWh) {
            $scope.criteria.toWhSysNo = !(toWh.index) ?  ''  : $scope.toWhOptions[toWh.index].sysNo;
        };
         /**
          *   获取货主列表
          */
        $scope.companyOptions = [];
         if  ($scope.companyOptions.length === 0) {
            masterDataService.getCompanyList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.companyOptions = reply.data;
                }
            });
        }
        $scope.changeCompany =  function  (company) {
            $scope.criteria.companySysNo = !(company.index) ?  ''  : $scope.companyOptions[company.index].sysNo;
        };
        
//        /**
//         * 出库类型
//         1 销售出库
//         2 内配出库
//         3 退厂出库
//         4 备件出库
//         5 取件出库
//         6 报废出库
//         */
//        var getorderType = function (data) {
//            if(data == "1"){
//                return "线上销售"
//            }else if(data == "2"){
//                return "门店配送"
//            }else if(data == "3"){
//                return "调拨出库"
//            }else if(data == "4"){
//                return "退厂出库"
//            }else{
//                return ""
//            }
//        };
//
//
//        /**
//         * 时间定位规则：1先进先出、2后进先出、3先到期先出
//         * @param data
//         * @returns {string}
//         */
//        var getallocRuleTime = function (data) {
//            if(data == "1"){
//                return "先进先出"
//            }else if(data == "2"){
//                return "后进先出"
//            }else if(data == "3"){
//                return "先到期先出"
//            }else{
//                return ""
//            }
//        };
//
//
//        /**
//         * 空间定位规则：1清空储位、2最少拣货储位
//         * @param data
//         */
//        var allocRuleSpace = function(data){
//            if(data == "1"){
//                return "清空储位"
//            }else if(data == "2"){
//                return "最少拣货储位"
//            }else{
//                return ""
//            }
//        }
        
         /**
          *   定义表格的所有列
          */
        $scope.dtColumnDefs = [DTColumnDefBuilder.newColumnDef(0).notSortable(),
            DTColumnDefBuilder.newColumnDef(1).notSortable(),
            DTColumnDefBuilder.newColumnDef(2).notSortable(),
            DTColumnDefBuilder.newColumnDef(3).notSortable(),
            DTColumnDefBuilder.newColumnDef(4).notSortable(),
            DTColumnDefBuilder.newColumnDef(5).notSortable(),
            DTColumnDefBuilder.newColumnDef(6).notSortable(),
            DTColumnDefBuilder.newColumnDef(7).notSortable(),
            DTColumnDefBuilder.newColumnDef(8).notSortable(),
            DTColumnDefBuilder.newColumnDef(9).notSortable(),
            DTColumnDefBuilder.newColumnDef(10).notSortable(),
            DTColumnDefBuilder.newColumnDef(11).notSortable()];
        
         /**
          *   DataTables   的前端分页设置
          */
        $scope.dtOptions = DTOptionsBuilder.newOptions().withBootstrap()
            .withOption( 'order' , [])
            .withOption( 'bFilter' ,  false )
            .withOption( 'bLengthChange' ,  false )
            .withOption( 'displayLength' , 5)
            .withOption( 'autoWidth' ,  false )
            .withOption( 'scrollX' ,  '100%' );
         /**
          *   保存
          */
        $scope.startAllocate =  function  () {
             if  ($scope.whNo ==  ""  || $scope.whNo ==  undefined ) {
                alert( "请选择仓库" )
                 return   false ;
            }
             if  ($scope.criteria.companySysNo ==  "" ) {
                alert( "请选择货主" )
                 return   false ;
            }
            
             if  ($scope.allocationStrategyDesc ==  ""  || $scope.allocationStrategyDesc ==  undefined ){
                alert( "请输入描述" )
                 return   false ;
            }
            
            $scope.allocStrategyBean.allocationStrategyDesc = $scope.allocationStrategyDesc;
            $scope.allocStrategyBean.whNo = $scope.whNo;
            $scope.allocStrategyBean.companySysNo = $scope.criteria.companySysNo;
            
            $rootScope.loadingPromise = startAllocate($scope.allocStrategyBean).success( function  (res) {
                 if (res.resultCode ==  "0" ){
                    alert(res.resultMsg);
                } else {
                    alert( "新增成功!" );
                    $modalInstance.close(1);
                }
            });
            
        };
         var  startAllocate =  function (allocStrategyBean){
             return  allocationStrategyService.startAllocate(allocStrategyBean)
        };
        
         /**
          *   新增分配策略明细
          */
        $scope.addAllocateStrategyDetail =  function (){
             var  modalInstance = $modal.open({
                templateUrl:  'views/strategy/allocation-strategy-detail-add.html' ,
                controller:  'allocateStrategyDetailAddController' ,
                backdrop:  'static'
            });
             return  modalInstance.result.then( function  (reply) {
                  reply.allocationDetailNo = $scope.allocStrategyBean.list.length + 1;
                  $scope.allIds.push(reply.allocationDetailNo);
                  $scope.allocStrategyBean.list.push(reply);
            });
        }
        
         /**
          *   修改分配策略明细
          */
        $scope.editAllocateStrategyDetail =  function (){
             var  allocateidArr = [];
             for  ( var  index  in  $scope.allIds)
            {
                 if ($scope.selectIdSet[$scope.allIds[index]] == -1) {
                    allocateidArr.push($scope.allocStrategyBean.list[parseInt($scope.allIds[index]) -1].allocationDetailNo);
                }
            }
             if (allocateidArr.length != 1){
                alert( "请选择一条分配策略明细" )
            } else {
                allocationStrategyService.setOrderType($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].orderType);
                allocationStrategyService.setAllocRuleTime($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleTime);
                allocationStrategyService.setAllocRuleSpace($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleSpace);
                allocationStrategyService.setPickLocDataFilterSysNo($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterSysNo);
                allocationStrategyService.setRepLocDataFilterSysNo($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterSysNo);
                allocationStrategyService.setIsRep($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].isRep);
                
                 var  modalInstance = $modal.open({
                    templateUrl:  'views/strategy/allocation-strategy-detail-edit.html' ,
                    controller:  'allocateStrategyDetailEditController' ,
                    backdrop:  'static'
                });
                 return  modalInstance.result.then( function  (reply) {
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].orderType = reply.orderType;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].orderTypeName = reply.orderTypeName;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleTime = reply.allocRuleTime;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleTimeName = reply.allocRuleTimeName;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleSpace = reply.allocRuleSpace;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleSpaceName = reply.allocRuleSpaceName;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterSysNo = reply.pickLocDataFilterSysNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterNo = reply.pickLocDataFilterNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterDesc = reply.pickLocDataFilterDesc;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterSysNo = reply.repLocDataFilterSysNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterNo = reply.repLocDataFilterNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterDesc = reply.repLocDataFilterDesc;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].isRep = reply.isRep;
                    
                     // 清空勾选项
                    $scope.checkItemfrom(allocateidArr[0]);
                    $scope.dtInstance.rerender();
                });
            }
        }
         /**
          *   对话框关闭响应
          */
        $scope.close =  function  () {
            $modalInstance.close( 'refresh' );
            $modalInstance.dismiss( 'close' );
        };
        
         /**
          *   获得   dtInstance
          */
        DTInstances.getLast().then( function  (dtInstance) {
            $scope.dtInstance = dtInstance;
        });     }]);   

```
##allocation-strategy-controller.js
```
/wms-web/src/main/webapp/js/controllers/strategy/allocation-strategy-controller.js

wmsApp.controller( 'allocationStrategyController' , [ '$scope' ,  '$rootScope' ,  '$modal' ,  '$compile' ,  'authService' ,  'masterDataService' ,  'allocationStrategyService' ,  'DTInstances' ,  'DTOptionsBuilder' ,  'DTColumnDefBuilder' ,
     function  ($scope, $rootScope, $modal, $compile, authService, masterDataService, allocationStrategyService, DTInstances, DTOptionsBuilder, DTColumnDefBuilder) {
        $scope.criteria = {
            toWhSysNo:  '' ,
            companySysNo:  ''
        };
        $scope.records = {};
        $scope.selectIdSet = {};
        $scope.allIds = [];
        $scope.allCheckItem =  false ;
        $scope.isDisabled =  true ;
        
         var  bInit =  true ;
         /**
          *   选中某一个
          */
        $scope.checkItem =  function  (sysNo) {
             if  ($scope[ "checkItem"  + sysNo])
                $scope.selectIdSet[sysNo] = -1;
             else
                 delete   $scope.selectIdSet[sysNo];
        };
         /**
          *   全部选中   或者   不选中
          */
        $scope.checkAll =  function  () {
             for  ( var  index  in  $scope.allIds)
                $scope[ "checkItem"  + $scope.allIds[index]] = $scope.allCheckItem;
             if  ($scope.allCheckItem) {
                 for  ( var  index  in  $scope.allIds)
                    $scope.selectIdSet[$scope.allIds[index]] = -1;
            }  else
                $scope.selectIdSet = {};
        };
         // 设置默认仓库
        $scope.whNo = authService.getUser().whNo;
        
         /**
          *   获取仓库列表
          */
        $scope.toWhOptions = [];
         if  ($scope.toWhOptions.length === 0) {
            masterDataService.getWhList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    reply.data.unshift({whNo: '' , whName: '--请选择--' });
                    $scope.toWhOptions = reply.data;
                }
            });
        }
         // $scope.changeToWh = function (toWh) {
         //     $scope.criteria.toWhSysNo = !(toWh.index) ? '' : $scope.toWhOptions[toWh.index].sysNo;
         // };
         /**
          *   获取货主列表
          */
        $scope.companyOptions = [];
         if  ($scope.companyOptions.length === 0) {
            masterDataService.getCompanyList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.companyOptions = reply.data;
                }
            });
        }
        $scope.changeCompany =  function  (company) {
            $scope.criteria.companySysNo = !(company.index) ?  ''  : $scope.companyOptions[company.index].sysNo;
        };
        
        
         /**
          *   构建checkbox列
          */
         function  actionCheckBox(data) {
             return   ' <input ng-model="checkItem'  + data.sysNo +
                 '" type="checkbox" name="checkItem" ng-change="checkItem(\''  + data.sysNo +
                 '\')"/>' ;
        }
         /**
          *   构建自定义列的生成
          */
         var  createOrderNoHtml =  function  (data) {
             return   '<a href="" ng-click="viewItem('  + data.sysNo +  ')">'  + data.allocationStrategyNo +  '</a>' ;
        };
        
         /**
          *   定义表格的所有列，将列与服务端返回的字段进行绑定
          */
        $scope.dtColumnDefs = [
            DTColumnDefBuilder.newColumnDef(0).withOption( 'data' ,  null ).renderWith(actionCheckBox).notSortable().withOption( 'width' ,  '5%' ),
            DTColumnDefBuilder.newColumnDef(1).withOption( 'data' ,  null ).renderWith(createOrderNoHtml).notSortable(),
            DTColumnDefBuilder.newColumnDef(2).withOption( 'data' ,  'allocationStrategyDesc' ).notSortable(),
            DTColumnDefBuilder.newColumnDef(3).withOption( 'data' ,  'whNo' ).notSortable(),
            DTColumnDefBuilder.newColumnDef(4).withOption( 'data' ,  'whName' ).notSortable(),
            DTColumnDefBuilder.newColumnDef(5).withOption( 'data' ,  'companyCode' ).notSortable(),
            DTColumnDefBuilder.newColumnDef(6).withOption( 'data' ,  'companyName' ).notSortable()
        ];
         /**
          *   创建单行
          */
         var  createdRow =  function  (row, data) {
            $scope.records[data[ 'sysNo' ]] = data;
            $compile(angular.element(row).contents())($scope);
        };
         /**
          *   点击超链接查看详情
          */
        $scope.viewItem =  function  (itemId) {
            allocationStrategyService.setAllocationStrategySysNo(itemId);
             var  modalInstance = $modal.open({
                templateUrl:  'views/strategy/allocation-strategy-modal.html' ,
                controller:  'allocationStrategyModalController' ,
                backdrop:  'static'
            });
             return  modalInstance.result.then( function  (reply) {
                 //alert(JSON.stringify(reply));
                $scope.dtInstance.rerender();
            });
        }
         /**
          *   根据搜索条件进行查询
          */
        $scope.search =  function  (criteria) {
            $scope.dtInstance.rerender();
        };
        
         /**
          *   新增分配策略
          */
        $scope.addAllocateStrategy =  function (){
             var  modalInstance = $modal.open({
                templateUrl:  'views/strategy/allocation-strategy-add.html' ,
                controller:  'allocateStrategyAddController' ,
                backdrop:  'static'
            });
             return  modalInstance.result.then( function  (reply) {
                $scope.dtInstance.rerender();
            });
        }
        
         /**
          *   修改分配策略
          */
        $scope.editAllocateStrategy =  function (){
             var  allocateidArr = [];
             for  ( var  index  in  $scope.allIds)
            {
                 if ($scope.selectIdSet[$scope.allIds[index]] == -1) {
                    allocateidArr.push($scope.records[$scope.allIds[index]].sysNo);
                }
            }
             if (allocateidArr.length != 1){
                alert( "请选择一条分配策略" )
            } else {
                allocationStrategyService.setSysNo(allocateidArr[0]);
                 var  modalInstance = $modal.open({
                    templateUrl:  'views/strategy/allocation-strategy-edit.html' ,
                    controller:  'allocateStrategyEditController' ,
                    backdrop:  'static'
                });
                 return  modalInstance.result.then( function  (reply) {
                    $scope.dtInstance.rerender();
                });
            }
        }
         /**
          *   DataTables   的后端分页设置
          */
        $scope.dtOptions = DTOptionsBuilder.newOptions().withBootstrap()
            .withOption( 'ajax' ,  function  (data, callback) {
                 if  (bInit) {
                    bInit =  false ;
                    $scope.records = {};
                    $scope.total = 0;
                     return  callback({data: [], recordsTotal: 0, recordsFiltered: 0});
                }
                 else
                {
                    initTable(data, callback);
                }
            }).withOption( 'bFilter' ,  false )
            .withOption( 'bLengthChange' ,  false )
            .withDataProp( 'data' )
            .withOption( 'displayLength' , 10)
            .withOption( 'serverSide' ,  true )
            .withPaginationType( 'full_numbers' )
            .withOption( 'createdRow' , createdRow)
            .withOption( 'order' , [])
            .withOption( 'autoWidth' ,  false )
            .withOption( 'scrollX' ,  '100%' );
         /**
          *   从服务端获取符合查询条件的列表
          */
         var  getAllocationStrategyList =  function  (draw, start, length) {
             return  allocationStrategyService.getAllocationStrategyList({
                draw: draw,
                start: start,
                rows: length,
                allocationStrategyDesc:$scope.allocationStrategyDesc,
                whNo: $scope.whNo,
                companySysNo:$scope.criteria.companySysNo
            });
        };
        
        
         /**
          *   初始化数据表
          */
         function  initTable(data, callback){
            $rootScope.loadingPromise = getAllocationStrategyList(data.draw, data.start,
                data.length).success( function  (res) {
                     for  ( var  index  in  $scope.allIds)
                         delete   $scope[ "checkItem"  + $scope.allIds[index]];
                    $scope.selectIdSet = {};
                    $scope.allIds = [];
                    $scope.allCheckItem =  false ;
                    $scope.records = {};
                    $scope.total = res.recordsTotal;
                     for  ( var  index  in  res.data)
                        $scope.allIds.push(res.data[index].sysNo);
                    callback(res);
                });
        }
         /**
          *   获得   dtInstance
          */
        DTInstances.getLast().then( function  (dtInstance) {
            $scope.dtInstance = dtInstance;
        });
    }]);   

```
##allocation-strategy-detail-add-controller.js
```
/wms-web/src/main/webapp/js/controllers/strategy/allocation-strategy-detail-add-controller.js

wmsApp.controller( 'allocateStrategyDetailAddController' , [ '$scope' ,  '$rootScope' , '$modal' , '$compile' ,  '$modalInstance' ,  'masterDataService' , 'allocationStrategyService' ,  'DTOptionsBuilder' ,  'DTColumnDefBuilder' ,
     function  ($scope, $rootScope,$modal, $compile, $modalInstance, masterDataService, allocationStrategyService, DTOptionsBuilder, DTColumnDefBuilder) {
        $scope.criteria = {
            orderTypeValue:  '' ,
            orderTypeValueName:  '' ,
            ruleTimeValue:  '' ,
            ruleTimeValueName:  '' ,
            ruleSpaceValue:  '' ,
            ruleSpaceValueName:  '' ,
            pickLocDataFilterSysNo: '' ,
            repLocDataFilterSysNo:  ''
            
        };
         // 是否补货， 默认0
        $scope.isRep =  '0' ;
         /**
          *   获取出库类型列表
          */
        $scope.orderTypeOptions = [];
         if  ($scope.orderTypeOptions.length === 0) {
            masterDataService.getOrderTypeList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.orderTypeOptions = reply.data;
                }
            });
        }
        $scope.changeOrderType =  function  (orderType) {
            $scope.criteria.orderTypeValue = !(orderType.index) ?  ''  : $scope.orderTypeOptions[orderType.index].key;
            $scope.criteria.orderTypeValueName = !(orderType.index) ?  ''  : $scope.orderTypeOptions[orderType.index].value;
        };
         // 默认按时间规则定位
        $scope.rule = 0;
        $scope.isRuleTimeShow =  true ;
        $scope.isRuleSpaceShow =  false ;
         /**
          *   定位规则
          */
        $scope.ruleOptions = [];
         if  ($scope.ruleOptions.length === 0) {
            $scope.ruleOptions = [
                {key:0,value: '按时间定位规则' },
                {key:1,value: '按空间定位规则' }];
        }
        $scope.changeRule=  function  () {
             if  ($scope.rule == 0)
            {
                $scope.isRuleTimeShow =  true ;
                $scope.isRuleSpaceShow =  false ;
                $scope.criteria.ruleSpaceValue =  '' ;
                $scope.criteria.ruleSpaceValueName =  '' ;
            }
             else
            {
                $scope.isRuleTimeShow =  false ;
                $scope.isRuleSpaceShow =  true ;
                $scope.criteria.ruleTimeValue =  '' ;
                $scope.criteria.ruleTimeValueName =  '' ;
            }
        };
         /**
          *   获取时间定位规则列表
          */
        $scope.ruleTimeOptions = [];
         if  ($scope.ruleTimeOptions.length === 0) {
            masterDataService.getRuleTimeList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.ruleTimeOptions = reply.data;
                }
            });
        }
        $scope.changeRuleTime =  function  (ruleTime) {
            $scope.criteria.ruleTimeValue = !(ruleTime.index) ?  ''  : $scope.ruleTimeOptions[ruleTime.index].key;
            $scope.criteria.ruleTimeValueName = !(ruleTime.index) ?  ''  : $scope.ruleTimeOptions[ruleTime.index].value;
        };
         /**
          *   获取空间定位规则列表
          */
        $scope.ruleSpaceOptions = [];
         if  ($scope.ruleSpaceOptions.length === 0) {
            masterDataService.getRuleSpaceList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.ruleSpaceOptions = reply.data;
                }
            });
        }
        $scope.changeRuleSpace =  function  (ruleSpace) {
            $scope.criteria.ruleSpaceValue = !(ruleSpace.index) ?  ''  : $scope.ruleSpaceOptions[ruleSpace.index].key;
            $scope.criteria.ruleSpaceValueName = !(ruleSpace.index) ?  ''  : $scope.ruleSpaceOptions[ruleSpace.index].value;
        };
        
         /**
          *   获取拣货储位过滤规则筛选器列表
          */
        $scope.pickLocDataFilterOptions = [];
         if  ($scope.pickLocDataFilterOptions.length === 0) {
            masterDataService.getSysDataFilterList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.pickLocDataFilterOptions = reply.data;
                }
            });
        }
        $scope.changePickLocDataFilter =  function  (pickLocDataFilter) {
            $scope.criteria.pickLocDataFilterSysNo = !(pickLocDataFilter.index) ?  ''  : $scope.pickLocDataFilterOptions[pickLocDataFilter.index].sysNo;
            $scope.criteria.pickLocDataFilterNo = !(pickLocDataFilter.index) ?  ''  : $scope.pickLocDataFilterOptions[pickLocDataFilter.index].dataFilterNo;
            $scope.criteria.pickLocDataFilterDesc = !(pickLocDataFilter.index) ?  ''  : $scope.pickLocDataFilterOptions[pickLocDataFilter.index].dataFilterDesc;
        };
        
         /**
          *   获取补货储位过滤规则筛选器列表
          */
        $scope.repLocDataFilterOptions = [];
         if  ($scope.repLocDataFilterOptions.length === 0) {
            masterDataService.getSysDataFilterList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.repLocDataFilterOptions = reply.data;
                }
            });
        }
        $scope.changeRepLocDataFilter =  function  (repLocDataFilter) {
            $scope.criteria.repLocDataFilterSysNo = !(repLocDataFilter.index) ?  ''  : $scope.repLocDataFilterOptions[repLocDataFilter.index].sysNo;
            $scope.criteria.repLocDataFilterNo = !(repLocDataFilter.index) ?  ''  : $scope.repLocDataFilterOptions[repLocDataFilter.index].dataFilterNo;
            $scope.criteria.repLocDataFilterDesc = !(repLocDataFilter.index) ?  ''  : $scope.repLocDataFilterOptions[repLocDataFilter.index].dataFilterDesc;
        };
       
         /**
          *   保存
          */
        $scope.saveAllocateDetail =  function  () {
             if  ($scope.criteria.orderTypeValue ==  "" ) {
                alert( "请选择出库类型" )
                 return   false ;
            }
             if  ($scope. rule == 0 && $scope.criteria.ruleTimeValue ==  ""  )
            {
                alert( "请选择时间定位规则" )
                 return   false ;
            }
             if  ($scope. rule == 1 && $scope.criteria.ruleSpaceValue ==  ""  )
            {
                alert( "请选择空间定位规则" )
                 return   false ;
            }
             if  ($scope.criteria.pickLocDataFilterSysNo ==  "" )
            {
                alert( "请选择拣货储位过滤规则" )
                 return   false ;
            }
            
             if  ($scope.criteria.repLocDataFilterSysNo ==  "" )
            {
                alert( "请选择补货储位过滤规则" )
                 return   false ;
            }
            $modalInstance.close({
                    orderType:$scope.criteria.orderTypeValue, 
                    orderTypeName:$scope.criteria.orderTypeValueName, 
                    allocRuleTime: $scope.criteria.ruleTimeValue, 
                    allocRuleTimeName: $scope.criteria.ruleTimeValueName,
                    allocRuleSpace: $scope.criteria.ruleSpaceValue,
                    allocRuleSpaceName: $scope.criteria.ruleSpaceValueName,
                    pickLocDataFilterSysNo: $scope.criteria.pickLocDataFilterSysNo,
                    pickLocDataFilterNo: $scope.criteria.pickLocDataFilterNo,
                    pickLocDataFilterDesc: $scope.criteria.pickLocDataFilterDesc,
                    repLocDataFilterSysNo: $scope.criteria.repLocDataFilterSysNo,
                    repLocDataFilterNo: $scope.criteria.repLocDataFilterNo,
                    repLocDataFilterDesc: $scope.criteria.repLocDataFilterDesc,
                    isRep: $scope.isRep
                    
//                    pickLocDataFilterSysNo: $scope.ecInvInfo.sysNo,
//                    pickLocDataFilterNo: $scope.ecInvInfo.dataFilterNo,
//                    pickLocDataFilterDesc: $scope.ecInvInfo.dataFilterDesc,
//                    repLocDataFilterSysNo: $scope.ecInvRepInfo.sysNo,
//                    repLocDataFilterNo: $scope.ecInvRepInfo.dataFilterNo,
//                    repLocDataFilterDesc: $scope.ecInvRepInfo.dataFilterDesc
                });
        };
        
        
         /**
          *   对话框关闭响应
          */
        $scope.close =  function  () {
//            $modalInstance.close('refresh');
            $modalInstance.dismiss( 'close' );
        };     }]);   

```
##allocation-strategy-detail-edit-controller.js
```
/wms-web/src/main/webapp/js/controllers/strategy/allocation-strategy-detail-edit-controller.js

wmsApp.controller( 'allocateStrategyDetailEditController' , [ '$scope' ,  '$rootScope' , '$modal' , '$compile' ,  '$modalInstance' ,  'masterDataService' , 'allocationStrategyService' ,  'DTOptionsBuilder' ,  'DTColumnDefBuilder' ,
     function  ($scope, $rootScope,$modal, $compile, $modalInstance, masterDataService, allocationStrategyService, DTOptionsBuilder, DTColumnDefBuilder) {
        $scope.criteria = {
            orderTypeValue:  '' ,
            orderTypeValueName:  '' ,
            ruleTimeValue:  '' ,
            ruleTimeValueName:  '' ,
            ruleSpaceValue:  '' ,
            ruleSpaceValueName:  '' ,
            pickLocDataFilterSysNoValue :  '' ,
            repLocDataFilterSysNoValue:  ''
        };
         /**
          *   获取出库类型列表
          */
        $scope.orderTypeOptions = [];
         if  ($scope.orderTypeOptions.length === 0) {
            masterDataService.getOrderTypeList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.orderTypeOptions = reply.data;
                }
            });
        }
        $scope.changeOrderType =  function  (orderType) {
            $scope.criteria.orderTypeValue = $scope.orderType;
            $scope.criteria.orderTypeValueName = !$( "#orderType option:selected" ).val() ?  ''  : $( "#orderType option:selected" ).text();
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.pickLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.pickLocInfo = resp.data;
                }
            });
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.repLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.repLocInfo = resp.data;
                }
            });
        };
         /**
          *   定位规则
          */
        $scope.ruleOptions = [];
         if  ($scope.ruleOptions.length === 0) {
            $scope.ruleOptions = [
                {key:0,value: '按时间定位规则' },
                {key:1,value: '按空间定位规则' }];
        }
        $scope.changeRule=  function  () {
             if  ($scope.rule == 0)
            {
                $scope.isRuleTimeShow =  true ;
                $scope.isRuleSpaceShow =  false ;
                $scope.allocRuleSpace =  '' ;
                $scope.criteria.ruleSpaceValue =  '' ;
                $scope.criteria.ruleSpaceValueName =  '' ;
            }
             else
            {
                $scope.isRuleTimeShow =  false ;
                $scope.isRuleSpaceShow =  true ;
                $scope.allocRuleTime =  '' ;
                $scope.criteria.ruleTimeValue =  '' ;
                $scope.criteria.ruleTimeValueName =  '' ;
            }
        };
        
         /**
          *   获取时间定位规则列表
          */
        $scope.ruleTimeOptions = [];
         if  ($scope.ruleTimeOptions.length === 0) {
            masterDataService.getRuleTimeList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.ruleTimeOptions = reply.data;
                }
            });
        }
        $scope.changeRuleTime =  function  (ruleTime) {
            $scope.criteria.ruleSpaceValue =  '' ;
            $scope.criteria.ruleSpaceValueName =  '' ;
             if  ( null  == $scope.allocRuleTime)
            {
                $scope.criteria.ruleTimeValue =  '' ;
            }
             else
            {
                $scope.criteria.ruleTimeValue = $scope.allocRuleTime;
            }
            $scope.criteria.ruleTimeValueName = !$( "#ruleTime option:selected" ).val() ?  ''  : $( "#ruleTime option:selected" ).text();
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.pickLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.pickLocInfo = resp.data;
                }
            });
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.repLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.repLocInfo = resp.data;
                }
            });
        };
         /**
          *   获取空间定位规则列表
          */
        $scope.ruleSpaceOptions = [];
         if  ($scope.ruleSpaceOptions.length === 0) {
            masterDataService.getRuleSpaceList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.ruleSpaceOptions = reply.data;
                }
            });
        }
        $scope.changeRuleSpace =  function  (ruleSpace) {
            $scope.criteria.ruleTimeValue =  '' ;
            $scope.criteria.ruleTimeValueName =  '' ;
             if  ( null  == $scope.allocRuleSpace)
            {
                $scope.criteria.ruleSpaceValue =  '' ;
            }
             else
            {
                $scope.criteria.ruleSpaceValue = $scope.allocRuleSpace;
            }
            $scope.criteria.ruleSpaceValueName = !$( "#ruleSpace option:selected" ).val() ?  ''  : $( "#ruleSpace option:selected" ).text();
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.pickLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.pickLocInfo = resp.data;
                }
            });
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.repLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.repLocInfo = resp.data;
                }
            });
        };
        
         /**
          *   获取拣货储位过滤规则筛选器列表
          */
        $scope.pickLocDataFilterOptions = [];
         if  ($scope.pickLocDataFilterOptions.length === 0) {
            masterDataService.getSysDataFilterList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.pickLocDataFilterOptions = reply.data;
                }
            });
        }
        $scope.changePickLocDataFilter =  function  (pickLocDataFilter) {
            $scope.criteria.pickLocDataFilterSysNoValue = $scope.pickLocDataFilterSysNo;
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.pickLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.pickLocInfo = resp.data;
                }
            });
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.repLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.repLocInfo = resp.data;
                }
            });
            
        };
        
         /**
          *   获取补货储位过滤规则筛选器列表
          */
        $scope.repLocDataFilterOptions = [];
         if  ($scope.repLocDataFilterOptions.length === 0) {
            masterDataService.getSysDataFilterList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.repLocDataFilterOptions = reply.data;
                }
            });
        }
        $scope.changeRepLocDataFilter =  function  (repLocDataFilter) {
            $scope.criteria.repLocDataFilterSysNoValue = $scope.repLocDataFilterSysNo;
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.repLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.repLocInfo = resp.data;
                }
            });
            
            allocationStrategyService.getSysDataFilterInfo({
                sysNo: $scope.criteria.pickLocDataFilterSysNoValue
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.pickLocInfo = resp.data;
                }
            });
        };
        
        
        
        $scope.orderType = allocationStrategyService.getOrderType().toString();
         if  (allocationStrategyService.getAllocRuleTime() !=  null  && allocationStrategyService.getAllocRuleTime() !=  "" )
        {
            $scope.allocRuleTime = allocationStrategyService.getAllocRuleTime().toString();
        }
         if  (allocationStrategyService.getAllocRuleSpace() !=  null  && allocationStrategyService.getAllocRuleSpace() !=  "" )
        {
            $scope.allocRuleSpace = allocationStrategyService.getAllocRuleSpace().toString();
        }
        $scope.pickLocDataFilterSysNo = allocationStrategyService.getPickLocDataFilterSysNo();
        $scope.repLocDataFilterSysNo = allocationStrategyService.getRepLocDataFilterSysNo();
        
        $scope.criteria.orderTypeValue = allocationStrategyService.getOrderType().toString();
         if  (allocationStrategyService.getAllocRuleTime() !=  null  && allocationStrategyService.getAllocRuleTime() !=  "" )
        {
            $scope.criteria.ruleTimeValue = allocationStrategyService.getAllocRuleTime().toString();
            $scope.rule = 0;
            $scope.isRuleTimeShow =  true ;
            $scope.isRuleSpaceShow =  false ;
        }
         if  (allocationStrategyService.getAllocRuleSpace() !=  null  && allocationStrategyService.getAllocRuleSpace() !=  "" )
        {
            $scope.criteria.ruleSpaceValue = allocationStrategyService.getAllocRuleSpace().toString();
            $scope.rule = 1;
            $scope.isRuleTimeShow =  false ;
            $scope.isRuleSpaceShow =  true ;
        }
        $scope.criteria.pickLocDataFilterSysNoValue = allocationStrategyService.getPickLocDataFilterSysNo();
        $scope.criteria.repLocDataFilterSysNoValue = allocationStrategyService.getRepLocDataFilterSysNo();
        $scope.isRep = allocationStrategyService.getIsRep();
        allocationStrategyService.getSysDataFilterInfo({
            sysNo: $scope.criteria.pickLocDataFilterSysNoValue
        }).success( function  (resp) {
             if  (resp && (resp.resultCode ===  '1' )) {
                $scope.pickLocInfo = resp.data;
            }
        });
        
        allocationStrategyService.getSysDataFilterInfo({
            sysNo: $scope.criteria.repLocDataFilterSysNoValue
        }).success( function  (resp) {
             if  (resp && (resp.resultCode ===  '1' )) {
                $scope.repLocInfo = resp.data;
            }
        });
        
         /**
          *   保存
          */
        $scope.saveAllocateDetail =  function  () {
             if  ($scope.criteria.orderTypeValue ==  ""  || $scope.criteria.orderTypeValue ==  null ) {
                alert( "请选择出库类型" )
                 return   false ;
            }
             if  ($scope. rule == 0 && $scope.criteria.ruleTimeValue ==  ""  )
            {
                alert( "请选择时间定位规则" )
                 return   false ;
            }
             if  ($scope. rule == 1 && $scope.criteria.ruleSpaceValue ==  ""  )
            {
                alert( "请选择空间定位规则" )
                 return   false ;
            }
            
             if  ($scope.criteria.pickLocDataFilterSysNoValue ==  ""  || $scope.criteria.pickLocDataFilterSysNoValue ==  null ){
                alert( "请选择拣货储位过滤规则" )
                 return   false ;
            }
            
             if  ($scope.criteria.repLocDataFilterSysNoValue ==  ""  || $scope.criteria.repLocDataFilterSysNoValue ==  null ){
                alert( "请选择补货储位过滤规则" )
                 return   false ;
            }
            
             if  ($scope.criteria.orderTypeValueName ==  "" )
            {
                $scope.criteria.orderTypeValueName = $( "#orderType option:selected" ).text();
            }
             if  ($scope.criteria.ruleTimeValueName ==  ""  && $scope.criteria.ruleTimeValue !=  "" )
            {
                $scope.criteria.ruleTimeValueName = $( "#ruleTime option:selected" ).text();
            }
             if  ($scope.criteria.ruleSpaceValueName ==  ""  && $scope.criteria.ruleSpaceValue != "" )
            {
                $scope.criteria.ruleSpaceValueName = $( "#ruleSpace option:selected" ).text();
            }
            $modalInstance.close({
                    orderType:$scope.criteria.orderTypeValue, 
                    orderTypeName:$scope.criteria.orderTypeValueName, 
                    allocRuleTime: $scope.criteria.ruleTimeValue, 
                    allocRuleTimeName: $scope.criteria.ruleTimeValueName,
                    allocRuleSpace: $scope.criteria.ruleSpaceValue,
                    allocRuleSpaceName: $scope.criteria.ruleSpaceValueName,
                    pickLocDataFilterSysNo: $scope.criteria.pickLocDataFilterSysNoValue,
                    pickLocDataFilterNo: $scope.pickLocInfo.dataFilterNo,
                    pickLocDataFilterDesc: $scope.pickLocInfo.dataFilterDesc,
                    repLocDataFilterSysNo: $scope.criteria.repLocDataFilterSysNoValue,
                    repLocDataFilterNo: $scope.repLocInfo.dataFilterNo,
                    repLocDataFilterDesc: $scope.repLocInfo.dataFilterDesc,
                    isRep: $scope.isRep
                    
                });
        };
         /**
          *   对话框关闭响应
          */
        $scope.close =  function  () {
            $modalInstance.dismiss( 'close' );
        };     }]);   

```
##allocation-strategy-edit-controller.js
```
/wms-web/src/main/webapp/js/controllers/strategy/allocation-strategy-edit-controller.js

wmsApp.controller( 'allocateStrategyEditController' , [ '$scope' ,  '$rootScope' , '$modal' , '$compile' ,  '$modalInstance' ,  'masterDataService' , 'allocationStrategyService' ,  'DTInstances' ,  'DTOptionsBuilder' ,  'DTColumnDefBuilder' ,
     function  ($scope, $rootScope,$modal, $compile, $modalInstance, masterDataService, allocationStrategyService, DTInstances, DTOptionsBuilder, DTColumnDefBuilder) {
        $scope.criteria = {
            toWhSysNo:  '' ,
            companySysNo:  ''
        };
        $scope.isDisabled =  true ;
        $scope.selectIdSet = {};
        $scope.allIds = [];
        $scope.allCheckItem =  false ;
         /**
          *   选中某一个
          */
        $scope.checkItemfrom =  function  (sysNo) {
             if ($scope.selectIdSet[sysNo] == -1){
                 delete   $scope.selectIdSet[sysNo];
            } else {
                $scope.selectIdSet[sysNo] = -1;
            }
        };
        
         /**
          *   提交到后台的bean对象
          */
        $scope.allocStrategyBean = {
            sysNo:  '' ,
            allocationStrategyDesc :  '' ,
            whSysNo:  '' ,
            companySysNo:  '' ,
            list:[]
        };
         /**
          *   获取仓库列表
          */
        $scope.toWhOptions = [];
         if  ($scope.toWhOptions.length === 0) {
            masterDataService.getWhList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.toWhOptions = reply.data;
                }
            });
        }
        $scope.changeToWh =  function  (toWh) {
            $scope.criteria.toWhSysNo = !$( "#toWh option:selected" ).val() ?  ''  : $( "#toWh option:selected" ).val();
        };
         /**
          *   获取货主列表
          */
        $scope.companyOptions = [];
         if  ($scope.companyOptions.length === 0) {
            masterDataService.getCompanyList().success( function  (reply) {
                 if  (reply && reply.resultCode ===  "1"  && reply.data) {
                    $scope.companyOptions = reply.data;
                }
            });
        }
        
        $scope.changeCompany =  function  (company) {
            $scope.criteria.companySysNo = !$( "#company option:selected" ).val() ?  ''  : $( "#company option:selected" ).val();
        };
        
         /**
          *   定义表格的所有列
          */
        $scope.dtColumnDefs = [DTColumnDefBuilder.newColumnDef(0).notSortable(),
            DTColumnDefBuilder.newColumnDef(1).notSortable(),
            DTColumnDefBuilder.newColumnDef(2).notSortable(),
            DTColumnDefBuilder.newColumnDef(3).notSortable(),
            DTColumnDefBuilder.newColumnDef(4).notSortable(),
            DTColumnDefBuilder.newColumnDef(5).notSortable(),
            DTColumnDefBuilder.newColumnDef(6).notSortable(),
            DTColumnDefBuilder.newColumnDef(7).notSortable(),
            DTColumnDefBuilder.newColumnDef(8).notSortable(),
            DTColumnDefBuilder.newColumnDef(9).notSortable(),
            DTColumnDefBuilder.newColumnDef(10).notSortable(),
            DTColumnDefBuilder.newColumnDef(11).notSortable()];
        
         /**
          *   DataTables   的前端分页设置
          */
        $scope.dtOptions = DTOptionsBuilder.newOptions().withBootstrap()
            .withOption( 'order' , [])
            .withOption( 'bFilter' ,  false )
            .withOption( 'bLengthChange' ,  false )
            .withOption( 'displayLength' , 5)
            .withOption( 'autoWidth' ,  false )
            .withOption( 'scrollX' ,  '100%' );
         /**
          *   根据sysNo查找出对应的分配策略信息
          */
        $scope.searchAllocationStrategy =  function (){
            $rootScope.loadingPromise = allocationStrategyService.getAllocationStrategyInfo({
                id: allocationStrategyService.getSysNo()
            }).success( function  (resp) {
                 if  (resp && (resp.resultCode ===  '1' )) {
                    $scope.allocationStrategyObj = resp.data;
                    $scope.whNo = $scope.allocationStrategyObj.whNo;
                    $scope.allocationStrategyDesc = $scope.allocationStrategyObj.allocationStrategyDesc;
                    $scope.companySysNo = parseInt(resp.data.companySysNo);
                     // 存放至提交到后台的bean对象中
                    $scope.allocStrategyBean.list = $scope.allocationStrategyObj.list;
                     // 将行号放置allIds中，供修改时使用
                     for ( var  i=0;i<$scope.allocationStrategyObj.list.length;i++)
                    {
                        $scope.allIds.push($scope.allocationStrategyObj.list[i].allocationDetailNo);
                    }
                }  else
                    alert(resp.resultMsg ? resp.resultMsg :  '未能成功获取分配策略信息' );
            },  function  (err) {
                alert( '获取分配策略信息失败' );
            });
        }
        
        $scope.searchAllocationStrategy();
         /**
          *   保存
          */
        $scope.updateAllocate =  function  () {
             if  ($scope.whNo ==  "" ) {
                alert( "请选择仓库" )
                 return   false ;
            }
             if  ($scope.companySysNo ==  "" ) {
                alert( "请选择货主" )
                 return   false ;
            }
             if  ($scope.allocationStrategyDesc ==  "" ){
                alert( "请输入描述" )
                 return   false ;
            }
            
            $scope.allocStrategyBean.sysNo = allocationStrategyService.getSysNo();
            $scope.allocStrategyBean.allocationStrategyDesc = $scope.allocationStrategyDesc;
            $scope.allocStrategyBean.whNo = $scope.whNo;
            $scope.allocStrategyBean.companySysNo = $scope.companySysNo
            $rootScope.loadingPromise = updateAllocate($scope.allocStrategyBean).success( function  (res) {
                 if (res.resultCode ==  "0" ){
                    alert(res.resultMsg);
                } else {
                    alert( "修改成功!" );
                    $modalInstance.close(1);
                }
            });
            
        };
         var  updateAllocate =  function (allocStrategyBean){
             return  allocationStrategyService.updateAllocate(allocStrategyBean)
        };
        
         /**
          *   新增分配策略明细
          */
        $scope.addAllocateStrategyDetail =  function (){
             var  modalInstance = $modal.open({
                templateUrl:  'views/strategy/allocation-strategy-detail-add.html' ,
                controller:  'allocateStrategyDetailAddController' ,
                backdrop:  'static'
            });
             return  modalInstance.result.then( function  (reply) {
                  reply.allocationDetailNo = $scope.allocStrategyBean.list.length + 1;
                  $scope.allIds.push(reply.allocationDetailNo);
                  $scope.allocStrategyBean.list.push(reply);
            });
        }
        
         /**
          *   修改分配策略明细
          */
        $scope.editAllocateStrategyDetail =  function (){
             var  allocateidArr = [];
             for  ( var  index  in  $scope.allIds)
            {
                 if ($scope.selectIdSet[$scope.allIds[index]] == -1) {
                    allocateidArr.push($scope.allocStrategyBean.list[parseInt($scope.allIds[index]) -1].allocationDetailNo);
                }
            }
             if (allocateidArr.length != 1){
                alert( "请选择一条分配策略明细" )
            } else {
                allocationStrategyService.setOrderType($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].orderType);
                allocationStrategyService.setAllocRuleTime($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleTime);
                allocationStrategyService.setAllocRuleSpace($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleSpace);
                allocationStrategyService.setPickLocDataFilterSysNo($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterSysNo);
                allocationStrategyService.setRepLocDataFilterSysNo($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterSysNo);
                allocationStrategyService.setIsRep($scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].isRep);
                 var  modalInstance = $modal.open({
                    templateUrl:  'views/strategy/allocation-strategy-detail-edit.html' ,
                    controller:  'allocateStrategyDetailEditController' ,
                    backdrop:  'static'
                });
                 return  modalInstance.result.then( function  (reply) {
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].orderType = reply.orderType;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].orderTypeName = reply.orderTypeName;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleTime = reply.allocRuleTime;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleTimeName = reply.allocRuleTimeName;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleSpace = reply.allocRuleSpace;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].allocRuleSpaceName = reply.allocRuleSpaceName;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterSysNo = reply.pickLocDataFilterSysNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterNo = reply.pickLocDataFilterNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].pickLocDataFilterDesc = reply.pickLocDataFilterDesc;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterSysNo = reply.repLocDataFilterSysNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterNo = reply.repLocDataFilterNo;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].repLocDataFilterDesc = reply.repLocDataFilterDesc;
                    $scope.allocStrategyBean.list[parseInt(allocateidArr[0]) -1].isRep = reply.isRep;
                    
                     // 清空勾选项
                    $scope.checkItemfrom(allocateidArr[0]);
                    $scope.dtInstance.rerender();
                });
            }
        }
         /**
          *   对话框关闭响应
          */
        $scope.close =  function  () {
            $modalInstance.close( 'refresh' );
            $modalInstance.dismiss( 'close' );
        };
        
         /**
          *   获得   dtInstance
          */
        DTInstances.getLast().then( function  (dtInstance) {
            $scope.dtInstance = dtInstance;
        });     }]);   

```

##allocation-strategy-service.js
```
/wms-web/src/main/webapp/js/services/strategy/allocation-strategy-service.js

/**
  *   Desc:      分配策略服务
  */
wmsApp.factory( 'allocationStrategyService' , [ 'restProxyService' ,  'WMS_API_PROVIDER' ,  'WMS_API_END_POINT' ,
     function  (restProxyService, WMS_API_PROVIDER, WMS_API_END_POINT) {
         /**
          *   共享变量
          */
         var  allocationStrategySysNo;
        
         /**
          *   共享变量
          */
         var  sysNo;
        
         /**
          *   出库类型
          */
         var  orderType;
        
         /**
          *   时间定位规则信息
          */
         var  allocRuleTime;
        
         /**
          *   空间定位规则
          */
         var  allocRuleSpace;
        
         /**
          *   拣货储位过滤规则
          */
         var  pickLocDataFilterSysNo;
        
         /**
          *   补货储位过滤规则
          */
         var  repLocDataFilterSysNo;
         /**
          *   是否补货
          */
         var  isRep;
        
         /**
          *   函数列表
          */
         return  {
            getAllocationStrategyList:  function  (params) {
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-allocation-strategy/list' , params);
            },
            getAllocationStrategyInfo:  function  (params) {
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-allocation-strategy/info' , params);
            },
            startAllocate:  function  (params) {
                 return  restProxyService.sendHttpPost(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-allocation-strategy/create' , params);
            },
            updateAllocate:  function  (params) {
                 return  restProxyService.sendHttpPost(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-allocation-strategy/update' , params);
            },
            getSysDataFilterInfo:  function  (params) {
                 return  restProxyService.sendHttpGet(WMS_API_PROVIDER, WMS_API_END_POINT +  '/mst-allocation-strategy/getDataFilterInfo' , params);
            },
             /**
              *   Getter   &   Setter   函数
              */
            getAllocationStrategySysNo:  function  () {
                 return  allocationStrategySysNo;
            },
            setAllocationStrategySysNo:  function  (aSysNo) {
                allocationStrategySysNo = aSysNo;
            },
            
             /**
              *   Getter   &   Setter   函数
              */
            getSysNo:  function  () {
                 return  sysNo;
            },
            setSysNo:  function  (aSysNo) {
                sysNo = aSysNo;
            },
             /**
              *   Getter   &   Setter   函数
              */
            getOrderType:  function  () {
                 return  orderType;
            },
            setOrderType:  function  (aOrderType) {
                orderType = aOrderType;
            },
             /**
              *   Getter   &   Setter   函数
              */
            getAllocRuleTime:  function  () {
                 return  allocRuleTime;
            },
            setAllocRuleTime:  function  (aAllocRuleTime) {
                allocRuleTime = aAllocRuleTime;
            },
             /**
              *   Getter   &   Setter   函数
              */
            getAllocRuleSpace:  function  () {
                 return  allocRuleSpace;
            },
            setAllocRuleSpace:  function  (aAllocRuleSpace) {
                allocRuleSpace = aAllocRuleSpace;
            },
            
             /**
              *   Getter   &   Setter   函数
              */
            getPickLocDataFilterSysNo:  function  () {
                 return  pickLocDataFilterSysNo;
            },
            setPickLocDataFilterSysNo:  function  (aPickLocDataFilterSysNo) {
                pickLocDataFilterSysNo = aPickLocDataFilterSysNo;
            },
            
             /**
              *   Getter   &   Setter   函数
              */
            getRepLocDataFilterSysNo:  function  () {
                 return  repLocDataFilterSysNo;
            },
            setRepLocDataFilterSysNo:  function  (aRepLocDataFilterSysNo) {
                repLocDataFilterSysNo = aRepLocDataFilterSysNo;
            },
             /**
              *   Getter   &   Setter   函数
              */
            getIsRep:  function  () {
                 return  isRep;
            },
            setIsRep:  function  (aIsrep) {
                isRep = aIsrep;
            }
        }
    } ]);   

```
##allocation-strategy.html
```
/wms-web/src/main/webapp/views/strategy/allocation-strategy.html

<div id="content-header">
    <h1>分配策略查询</h1>
</div>
<form class="form-horizontal form-bordered" role="form">
    <div class="form-body">
        <!--<div class="form-inline col-md-12">-->
        <div class="row">
            <div class="col-md-4">
                <div class="form-group">
                    <label class="control-label col-md-4">描述：</label>
                    <div class="col-md-8">
                        <input type="text" class="form-control" maxlength="32" autocomplete="off" ng-model="allocationStrategyDesc">
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="form-group">
                    <label class="control-label col-md-4">仓库名称：</label>
                    <div class="col-md-8">
                        < select class="form-control" id="whSysNo" name="toWhSysNo" ng-model="whNo"   ng - options ="s. whNo  as s.whName for s in toWhOptions" ng- disabled =" isDisabled ">
                        </select>
                    </div>
                </div>
            </div>
            <div class="col-md-4">
                <div class="form-group">
                    <label class="control-label col-md-4">货主名称：</label>
                    <div class="col-md-8">
                        <select class="form-control" id="company" data-ng-model="company.index"
                               ng-change="changeCompany(company)">
                               <option value="">--请选择--</option>
                               <option ng-repeat="company in companyOptions" ng-value="$index">{{company.companyName}}</option>
                        </select>
                    </div>
                </div>
            </div>
        </div>
        <!--</div>-->
        <div class="form-inline col-md-12">
            <div class="control-group pull-right">
                <input type="button" value="查询" class="btn btn-success" ng-click="search(criteria)">
                <input type="button" value="新增" class="btn btn-primary" ng-click="addAllocateStrategy()">
                <input type="button" value="编辑" class="btn btn-warning" id="editBtn" ng-click="editAllocateStrategy()">
            </div>
        </div>
    </div>
    <div class="control-group col-md-12 ">
        <table datatable="ng" dt-options="dtOptions" dt-instance="dtInstance" dt-columns="dtColumns"
               dt-column-defs="dtColumnDefs" class="table table-striped table-bordered table-hover">
            <thead>
            <tr>
                <th> <input type="checkbox" id="checkItemAll" ng-model="allCheckItem" ng-change="checkAll()"/></th>
                <th>定位策略编码</th>
                <th>描述</th>
                <th>仓库编码</th>
                <th>仓库名称</th>
                <th>货主编号</th>
                <th>货主名称</th>
            </tr>
            </thead>
            <tbody>
            <tr ng-repeat="item in items">
            </tbody>
        </table>
    </div>
    <table class="pull-right">
        <thead>
        <tr>
            <th>
                <div><span>总计{{total ? total : 0}}条</span></div>
            </th>
        </tr>
        </thead>
    </table> </form>   

```

##allocation-strategy-add.html
```
/wms-web/src/main/webapp/views/strategy/allocation-strategy-add.html

<div class="">
    <div class="modal-content">
        <div class="modal-header">
            <button type="button" class="close" ng-click="close()">×</button>
            <h4 class="modal-title">新增分配策略</h4>
        </div>
        <div class="modal-body">
            <!-- Row 1 -->
            <div class="form-inline col-md-12">
                <div class="control-group col-md-4">
                     <div class="bolder ">仓库：</div>
                     < select class="form-control" id="whSysNo" name="toWhSysNo" ng-model="whNo"   ng - options ="s. whNo  as s.whName for s in toWhOptions" ng- disabled =" isDisabled ">
                    </select>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">货主：</div>
                    <select class="form-control" id="company" data-ng-model="company.index"
                           ng-change="changeCompany(company)">
                       <option value="">--请选择--</option>
                       <option ng-repeat="company in companyOptions" ng-value="$index">{{company.companyName}}</option>
                   </select>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">描述：</div>
                    <input type="text" class="form-control" maxlength="32" autocomplete="off" ng-model="allocationStrategyDesc">
                </div>
            </div>
            
            <div class="clearfix"></div>
            <!-- Row 2 -->
            <div class="form-inline col-md-12 pull-right">
                <div class="control-group pull-right">
                    <input type="button" value="保存" class="btn btn-success" ng-click="startAllocate()">
                </div>
            </div>
            <div class="clearfix"></div>
            <!-- Row 3 -->
            <div class="form-inline col-md-12 pull-right">
                <div class="control-group pull-right">
                    <input type="button" value="新增" class="btn btn-primary" ng-click="addAllocateStrategyDetail()">
                    <input type="button" value="编辑" class="btn btn-warning" id="editBtn" ng-click="editAllocateStrategyDetail()">
                </div>
            </div>
            <div class="form-inline col-md-12">
                <table datatable="ng" dt-options="dtOptions"
                       dt-columns="dtColumnDefs" class="table table-bordered table-hover">
                    <thead>
                    <tr>
                        <th width="5%"></th>
                        <th>行号</th>
                        <th>出库类型</th>
                        <th>时间分配规则</th>
                        <th>空间分配规则</th>
                        <th>分配规则优先级</th>
                        <th>拣货储位过滤规则内部主键</th>
                        <th>拣货储位过滤规则编码</th>
                        <th>拣货储位过滤规则描述</th>
                        <th>补货储位过滤规则内部主键</th>
                        <th>补货储位过滤规则编码</th>
                        <th>补货储位过滤规则描述</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr ng-repeat="allocationStrategyDetail in allocStrategyBean.list">
                        <td><input ng-model="checkItem" type="checkbox" name="checkItem" ng-change="checkItemfrom({{ allocationStrategyDetail.allocationDetailNo }})"/></td>
                        <td>{{ allocationStrategyDetail.allocationDetailNo }}</td>
                        <td>{{ allocationStrategyDetail.orderTypeName }}</td>
                        <td>{{ allocationStrategyDetail.allocRuleTimeName }}</td>
                        <td>{{ allocationStrategyDetail.allocRuleSpaceName }}</td>
                        <td>{{ allocationStrategyDetail.allocRuleSeq }}</td>
                        <td>{{ allocationStrategyDetail.pickLocDataFilterSysNo }}</td>
                        <td>{{ allocationStrategyDetail.pickLocDataFilterNo }}</td>
                        <td>{{ allocationStrategyDetail.pickLocDataFilterDesc }}</td>
                        <td>{{ allocationStrategyDetail.repLocDataFilterSysNo }}</td>
                        <td>{{ allocationStrategyDetail.repLocDataFilterNo }}</td>
                        <td>{{ allocationStrategyDetail.repLocDataFilterDesc }}</td>
                    </tr>
                        
                    </tbody>
                </table>
            </div>
            
        </div>
    </div> </div>   

```

##allocation-strategy-detail-add.html
```
/wms-web/src/main/webapp/views/strategy/allocation-strategy-detail-add.html

<div class="">
    <div class="modal-content">
        <div class="modal-header">
            <button type="button" class="close" ng-click="close()">×</button>
            <h4 class="modal-title">新增分配策略明细</h4>
        </div>
        <div class="modal-body" style="height: 200px;">
            <!-- Row 1 -->
            <div class="form-inline col-md-12">
                <div class="control-group col-md-4">
                     <div class="bolder ">出库类型：</div>
                     <select class="form-control" id="orderType" data-ng-model="orderType.index"
                                        ng-change="changeOrderType(orderType)">
                         <option value="">--请选择--</option>
                         <option ng-repeat="orderType in orderTypeOptions" ng-value="$index">{{orderType.value}}</option>
                     </select>
                   </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">定位规则：</div>
                    <select class="form-control" id="rule" ng-model="rule" ng-options="item.key as item.value for item in ruleOptions"
                            ng-change="changeRule()">
                    </select>
                </div>
                <div class="control-group col-md-4"  ng-show="isRuleTimeShow">
                     <div class="bolder ">时间定位规则：</div>
                     <select class="form-control" id="ruleTime" data-ng-model="ruleTime.index"
                            ng-change="changeRuleTime(ruleTime)">
                        <option value="">--请选择--</option>
                        <option ng-repeat="ruleTime in ruleTimeOptions" ng-value="$index">{{ruleTime.value}}</option>
                    </select>
                </div>
                <div class="control-group col-md-4"   ng-show="isRuleSpaceShow" >
                    <div class="bolder ">空间定位规则：</div>
                    <select class="form-control" id="ruleSpace" data-ng-model="ruleSpace.index"
                            ng-change="changeRuleSpace(ruleSpace)">
                        <option value="">--请选择--</option>
                        <option ng-repeat="ruleSpace in ruleSpaceOptions" ng-value="$index">{{ruleSpace.value}}</option>
                    </select>
                </div>
                
                <div class="control-group col-md-4">
                     <div class="bolder ">拣货储位过滤规则：</div>
                     <select class="form-control" id="pickLocDataFilter" data-ng-model="pickLocDataFilter.index"
                                ng-change="changePickLocDataFilter(pickLocDataFilter)">
                            <option value="">--请选择--</option>
                            <option ng-repeat="pickLocDataFilter in pickLocDataFilterOptions" ng-value="$index">{{pickLocDataFilter.dataFilterDesc}}</option>
                     </select>
                </div>
                
                <div class="control-group col-md-4">
                     <div class="bolder ">补货储位过滤规则：</div>
                     <select class="form-control" id="repLocDataFilter" data-ng-model="repLocDataFilter.index"
                                ng-change="changeRepLocDataFilter(repLocDataFilter)">
                            <option value="">--请选择--</option>
                            <option ng-repeat="repLocDataFilter in repLocDataFilterOptions" ng-value="$index">{{repLocDataFilter.dataFilterDesc}}</option>
                     </select>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">是否补货：</div>
                    <div class="form-group">
                        <div class="radio">
                            <label>
                                <input type="radio" name="isRep" value="1" ng-model="isRep">
                                是
                            </label>
                        </div>
                        <div class="radio">
                            <label>
                                <input type="radio" name="isRep" value="0" ng-model="isRep">
                                否
                            </label>
                        </div>
                    </div>
                </div>
            </div>
            <div class="clearfix"></div>
            <!-- Row 2 -->
            <div class="form-inline col-md-12 pull-right">
                <div class="control-group pull-right">
                    <input type="button" value="保存" class="btn btn-success" ng-click="saveAllocateDetail()">
                </div>
            </div>
            
            
        </div>
    </div> </div>   

```

##allocation-strategy-detail-edit.html
```
/wms-web/src/main/webapp/views/strategy/allocation-strategy-detail-edit.html

<div class="">
    <div class="modal-content">
        <div class="modal-header">
            <button type="button" class="close" ng-click="close()">×</button>
            <h4 class="modal-title">编辑分配策略明细</h4>
        </div>
        <div class="modal-body" style="height: 200px;">
            <!-- Row 1 -->
            <div class="form-inline col-md-12">
                <div class="control-group col-md-4">
                     <div class="bolder ">出库类型：</div>
                     <select class="form-control" id="orderType" ng-model="orderType" ng-options="item.key as item.value for item in orderTypeOptions"
                                        ng-change="changeOrderType(orderType)">
                         <option value="">--请选择--</option>
                     </select>
                   </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">定位规则：</div>
                    <select class="form-control" id="rule" ng-model="rule" ng-options="item.key as item.value for item in ruleOptions"
                            ng-change="changeRule()">
                    </select>
                </div>
                <div class="control-group col-md-4" ng-show="isRuleTimeShow">
                     <div class="bolder ">时间定位规则：</div>
                     <select class="form-control" id="ruleTime" ng-model="allocRuleTime" ng-options="item.key as item.value for item in ruleTimeOptions"
                            ng-change="changeRuleTime(ruleTime)">
                        <option value="">--请选择--</option>
                    </select>
                </div>
                <div class="control-group col-md-4"  ng-show="isRuleSpaceShow" >
                    <div class="bolder ">空间定位规则：</div>
                    <select class="form-control" id="ruleSpace" ng-model="allocRuleSpace" ng-options="item.key as item.value for item in ruleSpaceOptions"
                            ng-change="changeRuleSpace(ruleSpace)">
                        <option value="">--请选择--</option>
                    </select>
                </div>
                <div class="control-group col-md-4">
                     <div class="bolder ">拣货储位过滤规则：</div>
                     <select class="form-control" id="pickLocDataFilter" ng-model="pickLocDataFilterSysNo" ng-options="item.sysNo as item.dataFilterDesc for item in pickLocDataFilterOptions"
                     ng-change="changePickLocDataFilter(pickLocDataFilter)">
                            <option value="">--请选择--</option>
                     </select>
                </div>
                <div class="control-group col-md-4">
                     <div class="bolder ">补货储位过滤规则：</div>
                     <select class="form-control" id="repLocDataFilter" ng-model="repLocDataFilterSysNo" ng-options="item.sysNo as item.dataFilterDesc for item in repLocDataFilterOptions"
                     ng-change="changeRepLocDataFilter(repLocDataFilter)">
                            <option value="">--请选择--</option>
                     </select>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">是否补货：</div>
                    <div class="form-group">
                        <div class="radio">
                            <label>
                                <input type="radio" name="isRep" value="1" ng-model="isRep">
                                是
                            </label>
                        </div>
                        <div class="radio">
                            <label>
                                <input type="radio" name="isRep" value="0" ng-model="isRep">
                                否
                            </label>
                        </div>
                    </div>
                </div>
            </div>
            <div class="clearfix"></div>
            <!-- Row 2 -->
            <div class="form-inline col-md-12 pull-right">
                <div class="control-group pull-right">
                    <input type="button" value="保存" class="btn btn-success" ng-click="saveAllocateDetail()">
                </div>
            </div>
            
            
        </div>
    </div>
</div>
```

##allocation-strategy-edit.html
```
/wms-web/src/main/webapp/views/strategy/allocation-strategy-edit.html

<div class="">
    <div class="modal-content">
        <div class="modal-header">
            <button type="button" class="close" ng-click="close()">×</button>
            <h4 class="modal-title">修改分配策略</h4>
        </div>
        <div class="modal-body">
            <!-- Row 1 -->
            <div class="form-inline col-md-12">
                <div class="control-group col-md-4">
                     <div class="bolder ">仓库：</div>
                     < select class="form-control" id="whSysNo" name="toWhSysNo" ng-model="whNo"   ng - options ="s. whNo  as s.whName for s in toWhOptions" ng- disabled =" isDisabled ">
                    </select>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">货主：</div>
                    <select class="form-control" id="company" ng-model="companySysNo" ng-options="item.sysNo as item.companyName for item in companyOptions">
                    </select>
                </div>
                <div class="control-group col-md-4">
                    <div class="bolder ">描述：</div>
                    <input type="text" class="form-control" maxlength="32" autocomplete="off" ng-model="allocationStrategyDesc">
                </div>
            </div>
            
            <div class="clearfix"></div>
            <!-- Row 2 -->
            <div class="form-inline col-md-12 pull-right">
                <div class="control-group pull-right">
                    <input type="button" value="保存" class="btn btn-success" ng-click="updateAllocate()">
                </div>
            </div>
            <div class="clearfix"></div>
            <!-- Row 3 -->
            <div class="form-inline col-md-12 pull-right">
                <div class="control-group pull-right">
                    <input type="button" value="新增" class="btn btn-primary" ng-click="addAllocateStrategyDetail()">
                    <input type="button" value="编辑" class="btn btn-warning" id="editBtn" ng-click="editAllocateStrategyDetail()">
                </div>
            </div>
            <div class="form-inline col-md-12">
                <table datatable="ng" dt-options="dtOptions"
                       dt-columns="dtColumnDefs" class="table table-bordered table-hover">
                    <thead>
                    <tr>
                        <th width="5%"></th>
                        <th>行号</th>
                        <th>出库类型</th>
                        <th>时间分配规则</th>
                        <th>空间分配规则</th>
                        <th>分配规则优先级</th>
                        <th>拣货储位过滤规则内部主键</th>
                        <th>拣货储位过滤规则编码</th>
                        <th>拣货储位过滤规则描述</th>
                        <th>补货储位过滤规则内部主键</th>
                        <th>补货储位过滤规则编码</th>
                        <th>补货储位过滤规则描述</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr ng-repeat="allocationStrategyDetail in allocationStrategyObj.list">
                        <td><input ng-model="checkItem" type="checkbox" name="checkItem" ng-change="checkItemfrom({{ allocationStrategyDetail.allocationDetailNo }})"/></td>
                        <td>{{ allocationStrategyDetail.allocationDetailNo }}</td>
                        <td>{{ allocationStrategyDetail.orderTypeName }}</td>
                        <td>{{ allocationStrategyDetail.allocRuleTimeName }}</td>
                        <td>{{ allocationStrategyDetail.allocRuleSpaceName }}</td>
                        <td>{{ allocationStrategyDetail.allocRuleSeq }}</td>
                        <td>{{ allocationStrategyDetail.pickLocDataFilterSysNo }}</td>
                        <td>{{ allocationStrategyDetail.pickLocDataFilterNo }}</td>
                        <td>{{ allocationStrategyDetail.pickLocDataFilterDesc }}</td>
                        <td>{{ allocationStrategyDetail.repLocDataFilterSysNo }}</td>
                        <td>{{ allocationStrategyDetail.repLocDataFilterNo }}</td>
                        <td>{{ allocationStrategyDetail.repLocDataFilterDesc }}</td>
                    </tr>
                        
                    </tbody>
                </table>
            </div>
            
        </div>
    </div>
</div>
```



