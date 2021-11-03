# 使用useGeneratedKeys和keyProperty设置目标属性值
```java
User user = User.builder().name(name).address(address).build(); // user.id 是null
mapper.saveUser(user); // user.id 非null，为主键值
```

```java
// UserMapper.java
Integer saveUser(@Param("user") User user);
```

```xml
<insert id="saveUser" useGeneratedKeys="true" keyProperty="user.id" parameterType="com.example.sprintbootdemo.bean.User" >
    insert into user
    (<include refid="baseColumns" />)
    values
    (
    #{user.id},
    #{user.name},
    #{user.address,typeHandler=com.example.sprintbootdemo.typehandler.AddressTypeHandler}
    )
</insert>
```

# xxx