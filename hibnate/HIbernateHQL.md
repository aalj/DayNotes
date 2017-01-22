### HQL 查询语法

> 已有持久化对象Customer 和 Order 

1. 查询所有记录(需要使用Query接口)

   `from customer`

2. 查询使用别名

   `from Customer as c`或者`from Customer c` 别名关键字as可以省略

3. 可以使用别名带参数

   `from Customer as c where c.name=?`(需要验证不使用as是否有效)

   > 注: 在HQL中不支持`select * from Customer` 的写法 
   >
   > 但是:可以写成 select 别名 `from Customer as 别名`
   >
   > 例如: `select c from Customer c`

4. 排序查询

   `from Customer c order by c.id desc `

5. 分页查询

   `from Customer`  然后是用`query.setFirstResult()`(设置查询开始的内容)`query.setMaxResults()`(设置每页查询的多少)

6. 通过使用`? `号方式绑定参数 然后查询单个对象

   `from Customer where name=?`然后通过query设置参数`query.setString("","")` 

7. 使用名称绑定

   `from Customer where name=:name and id=:id`然后通过`query.setString("name","value");`进行参数设置

8. 投影查询

   `select c.name from Customer c ` 根据`selete`  后面的查询数量判断返回的是` Object` 还是` Object[ ]`

9. 模糊查询

   `form Customer where name like = ?  `调用`query.setParameter(0,"小%") ` 进行查询参数设置