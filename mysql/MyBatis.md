# MyBatis



### 批量插入去重

**on duplicate key update**

当primary或者unique重复时，则执行update语句，如update后为无用语句，如id=id，错误不会被忽略掉。

例如，为了实现name重复的数据插入不报错，可使用一下语句：

```n1ql
INSERT INTO user (name) VALUES ('telami') ON duplicate KEY UPDATE id = id 
```

这种方法有个前提条件，就是，需要插入的约束，需要是主键或者唯一约束（在你的业务中那个要作为唯一的判断就将那个字段设置为唯一约束也就是unique key）

```sql
<insert id="batchSaveUser" parameterType="list">
    insert into user (id,username,mobile_number)
    values
    <foreach collection="list" item="item" index="index" separator=",">
        (
            #{item.id},
            #{item.username},
            #{item.mobileNumber}
        )
    </foreach>
    ON duplicate KEY UPDATE id = id
</insert>
```