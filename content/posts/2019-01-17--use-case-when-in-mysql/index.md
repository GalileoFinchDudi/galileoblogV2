---
title: Mysql中使用case函数修改不同表的表数据
author: Galileo Finch
category: "MySql Sql"
cover: caspar-camille-rubin-224229-unsplash.jpg
---

该问题来自永哥遇到的一个sql问题，需求是`更新的时候  有一个type 字段  我需要在type为0时更新货主表的名字  在type为1时更新司机表的名字  我在xml里面直接写case when报错 有什么别的办法么`。

分析一下需求：要根据type值的不同，可以做两个sql

```sql
update A set A.time = now() where id  in (select id from user where type = 'A')
update B set B.time = now() where id  in (select id from user where type = 'B')
```

理论上也能做，然后尝试了一下能否利用case函数

```xml
<update id="updateUserShipperMemberNameAndPhone" parameterType="java.util.List">
    <foreach collection="list" item="item" index="index"  open="" close="" separator=";">
        UPDATE dz_user AS du,dz_user_shipper AS dus,dz_user_driver AS dud
        <set>
            <if test="item.name != null and item.name != ''">
                CASE WHEN du.type = 0 THEN dus.contacts_name WHEN du.type = 1 THEN dud.username END = #{item.name},
            </if>
            <if test="item.phone != null and item.phone != ''">
                du.phone = #{item.phone},
            </if>
            du.update_time = NOW(),CASE WHEN du.type = 0 THEN dus.update_time WHEN du.type = 1 THEN dud.update_time END = now()
        </set>
        WHERE du.id = CASE WHEN du.type = 0 THEN dus.user_id WHEN du.type = 1 THEN dud.user_id END AND du.id = #{item.memberUserId}
    </foreach>
</update>
```

这个sql会报一个错误:

>You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'CASE WHEN...

简单看了一下[stackoverflow](https://stackoverflow.com/questions/15766102/i-want-to-use-case-statement-to-update-some-records-in-sql-server-2005)，发现别人写的then后的参数都是一个固定的数值，上面的写法想要返回的其实是一个字段名。修改了一下原有写法，如下：

```xml
<update id="updateUserShipperMemberNameAndPhone" parameterType="java.util.List">
    <foreach collection="list" item="item" index="index"  open="" close="" separator=";">
        UPDATE dz_user AS du,dz_user_shipper AS dus,dz_user_driver AS dud
        <set>
            <if test="item.name != null and item.name != ''">
                dus.contacts_name = CASE
                    WHEN du.type = 0 THEN #{item.name}
                    ELSE dus.contacts_name
                    END ,
                dud.username = CASE
                    WHEN du.type = 1 THEN  #{item.name}
                    ELSE dud.username
                    END ,
            </if>
            <if test="item.phone != null and item.phone != ''">
                du.phone = #{item.phone},
            </if>
            du.update_time = NOW(),
            dus.update_time = CASE
                WHEN du.type = 0 THEN now()
                ELSE dus.update_time
                END ,
            dud.update_time = CASE
                WHEN du.type = 1 THEN  now()
                ELSE dud.update_time
                END ,
        </set>
        WHERE
        du.id = CASE
            WHEN du.type = 0 THEN dus.user_id
            WHEN du.type = 1 THEN dud.user_id
            ELSE dus.contacts_name
            END
        AND du.id = #{item.memberUserId}
    </foreach>
</update>
```

我将两个字段都进行了update，但是如果非目标type，则update成原来的值。成功更新数据。
