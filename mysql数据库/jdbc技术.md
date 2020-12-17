jdbc技术  java database connectivity

jdbc 是 Java连接数据库的操作

Java中定义了访问数据库的接口

```java
// 注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        // 获得连接
        String url  = "jdbc:mysql://localhost:3306/companydb";
        String user = "root";
        String password= "666666";
        // 连接数据库
        Connection connection = DriverManager.getConnection(url,user,password);
        if (connection != null){
            System.out.println("成功连接到数据库");
        }else {
            System.out.println("连接失败");
        }
        // 通过connection 对象获得 statement 对象 用于数据库的访问
        Statement statement = connection.createStatement();
        // 编写SQL 语句就可以执行
        String sql = "insert into account(money) values(10000);";
        // DML 语句执行对象
        int result = statement.executeUpdate(sql);
        // 处理结果
        if (result == 1){
            System.out.println("执行成功");
        }else {
            System.out.println("执行失败");
        }
        // 释放资源 先开后关
        statement.close();
        connection.close();
```

ResultSet 结果集

```java
// 查询返回的结果集
Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/companydb";
        String name = "root";
        String password = "666666";
        Connection connection = DriverManager.getConnection(url,name,password);
        Statement statement = connection.createStatement();
        String sql = "select * from account;";
        // result 同样也是表结构获取
        ResultSet resultSet = statement.executeQuery(sql);
        // 要以 特点的方法进行遍历

        // 判断下一行是否为空
        while (resultSet.next()){
            // 根据列名来获取
            System.out.println(resultSet.getString("id") +" "+
                    resultSet.getString("money"));
            // 根据列数获得数据
            System.out.println(resultSet.getInt(1)+ " " + resultSet.getInt(2));
        }
        resultSet.close();
        statement.close();
        connection.close();
```

 综合案例 

登录界面