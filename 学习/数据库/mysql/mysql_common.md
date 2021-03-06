```
SELECT UNIX_TIMESTAMP(NOW()) //将当前时间转换为时间戳 2016/8/15 13:45:59--> 1471239959

```
##模糊查询
```
  SELECT 
    t.ID AS id,
    t.MSGID AS msgId,
    t.PUBNUM_NUM AS pubnum,
    t.TITLE AS title,
    t.PUSHMODE AS pushmode,
    t.PUSHTIME AS pushtime,
    t.PUSHTYPE AS pushType,
    t.`STATUS` AS STATUS,
    t.UPDATE_TIME AS updateTime,
    t.UPDATE_USER_ID AS updateUserId,
    t.CREATE_TIME AS createTime,
    t.CREATE_USER_ID AS createUserId 
  FROM
    wx_push_task t 
  WHERE 1 = 1 /*< #if pubnum?exists && pubnum!="">
    and t.PUBNUM_NUM = :pubnum < / #if>
    < #if PUSHTYPE?exists && pushType!="">
    and t.pushType = :pushType < / #if>
    < #if status?exists && status!="">
    and t.`STATUS` = :status < / #if>
    < #if pushmode?exists && pushmode!="">
    and t.PUSHMODE = :pushmode < / #if>
    < #if title?exists && title!="">*/
    AND t.TITLE LIKE CONCAT('%', 'w', '%') /*< / #if>
    < #if msgId?exists && msgId!="">
    and t.MSGID = :msgId < / #if>*/
  ORDER BY t.UPDATE_TIME DESC 
``` ##关于排序
```
1.限时促销按照活动状态【已开始，未开始，已结束】排序
活动状态分为：0已开始 1未开始 02 已结束 
已开始排在前面，未开始的排在已开始的后面。已结束按照促销时间排序显示在后面


SELECT 
  * 
FROM
  (SELECT 
    ID AS id,
    SUPP_ID AS suppId,
    NAME AS NAME,
    START_DTTM AS startDttm,
    END_DTTM AS endDttm,
    DISCOUNT_TYPE AS discountType,
    CREATE_DTTM AS createDttm,
    LAST_DTTM AS lastDttm,
    LAST_PER AS lastPer,
    URL AS url,
    IMG AS img,
    INFO AS info,
    CASE
      WHEN `STATUS` = 0 
      AND NOW() BETWEEN START_DTTM 
      AND END_DTTM 
      THEN 0 
      /*已开始*/
      WHEN `STATUS` IN (0, 1) 
      AND (
        (
          NOW() BETWEEN START_DTTM 
          AND END_DTTM
        ) 
        OR (NOW() < START_DTTM)
      ) 
      THEN 1 
      /*未开始*/
      WHEN ((`STATUS` = 2) 
        OR (NOW() > END_DTTM)) 
      THEN 2 
    END STATUS 
    /*已结束*/
  FROM
    MARKET_LIMITED 
  WHERE 1 = 1 
    
    AND SUPP_ID = '1042' 
    /*and NAME like CONCAT('%', :name, '%')*/
    
  ) ALLSULT 
WHERE 1 = 1
ORDER BY STATUS,
  endDttm DESC 




2.优惠券按照活动状态【已开始，未开始，已结束】排序
活动状态：1已开始，2未开始，3已结束
SELECT 
  * 
FROM
  (SELECT 
    ID,
    SUPPID,
    NAME,
    TYPE,
    START_TIME AS startTime,
    END_TIME AS endTime,
    FACE_VALUE AS faceValue,
    ISSUED_MODE AS issuedMode,
    ISSUED_NUM AS issuedNum,
    SURPLUS_NUM AS surplusNum,
    TAKE_MAX_NUM AS takeMaxNum,
    SATISFY_MONEY AS satisfyMoney,
    UPDATE_TIME AS updateTime,
    UPDATE_USER_ID AS updateUserId,
    CREATE_USER_ID AS createUserId,
    CREATE_TIME AS createTime,
    CASE
      WHEN `STATUS` = 1 
      AND NOW() BETWEEN START_TIME 
      AND END_TIME 
      THEN 1 
      /*已开始*/
      WHEN `STATUS` IN (1, 2) 
      AND (
        (
          NOW() BETWEEN START_TIME 
          AND END_TIME
        ) 
        OR (NOW() < START_TIME)
      ) 
      THEN 2 
      /*未开始*/
      WHEN ((`STATUS` = 3) 
        OR (NOW() > END_TIME)) 
      THEN 3 
    END `STATUS` 
    /*已结束*/
  FROM
    coupon_rule t1 
  WHERE 1 = 1 
    /*and t1.ID = :id 
    < #if name?exists && name!="">
    and t1.NAME like CONCAT('%', :name, '%') < / #if>
    < #if suppId?exists && suppId!="">*/
    AND t1.SUPPID = '1042'
    /*< #if startTime?exists && startTime!="" && endTime?exists && endTime!="">
    and (
      (
        t1.START_TIME >= :startTime 
        and t1.START_TIME <= :endTime
      ) 
      or (
        t1.END_TIME >= :startTime 
        and t1.END_TIME <= :endTime
      ) 
      or (
        t1.START_TIME <= :startTime 
        and t1.END_TIME >= :endTime
      ) 
      or (
        t1.START_TIME >= :startTime 
        and t1.END_TIME <= :endTime
      )
    ) < / #if>
    < #if type?exists && type!="">
    and t1.TYPE = :type < / #if>
    < #if issuedMode?exists && issuedMode!="">
    and t2.ISSUED_MODE = :issuedMode < / #if>*/
  ) a 
WHERE 1 = 1 /*< #if status?exists && status!="">
  and a.STATUS = :status < / #if>*/
ORDER BY a.STATUS,
  a.endTime DESC 


```