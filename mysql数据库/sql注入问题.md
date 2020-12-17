`SQL`注入问题

用户输入的数据中存在`SQL`关键字或语法并且参与了`SQL` 语句的编译，导致`SQL` 语句编译后的条件含义为true 一直得到挣钱去的结果，这种现象称为`SQL`注入问题

```mysql
 # 很明显 这个语句通过输入问题将后面的判断条件注释掉
 # 前面的数据一定为true
 select * from user where username = 'abx' or 1 = 1 ;#'and password = 'password'';
 
```

避免的方法，很简单

先执行`sql`语句，得到结果再与输入的数据进行比较，这样就可以避免`SQL`注入问题

```java
PreparedStatement 方法  // 重点 
    //作用
    1.预编译SQL 语句 
    2.安全 避免SQL注入问题
    3.可以动态的填充数据，执行多个同构的SQL语句
    （1） 采用占比标记方法
    // ？ 占位符 
     String sql = "select * from account where id = ? and money = ?;";
         // 预编译 
       PreparedStatement preparedStatement = connection.prepareStatement(sql);
 	preparedStatement.setString(2,password);
// 后续往里面添加变量
```

