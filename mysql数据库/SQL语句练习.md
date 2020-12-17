``` mysql
# 修改数据库中的内容 
# 627 变更性别
update salary 
set sex = case sex
when 'f' then 'm'
else 'f'
end;
# 查找数据库中倒数第二大的数据
# 176 第二高的薪水
# 第一种思路 倒序然后采用限制
SELECT 
	(SELECT DISTINCT SALARY 
    	FROM EMPLOYEE
    	ORDER BY SALARY DESC LIMIT 1 OFFSET 1) AS SECONDHITHESTSALARY;
 # 第二种思路  
 IF NULL
 SELECT IFNULL(
 (SELECT DISTINCT SALARY FROM EMPLOYEE
  ORDER BY SALARY DESC 
  LIMIT 1 OFFSET 1),NULL) AS SecondHighestSalary;
  # 数据练习操作
  SELECT EMAIL FROM (SELECT Email ,count(Email) as num from Person  group by Email) AS TEMP WHERE NUM > 1;
  # 工资超过经理的员工
  SELECT a.Name as 'Employee' from Employee as a,Employee as b where a.ManagerId = b.id and a.salary > b.salary;
```

登录操作

```Java
public static void main(String[] args){
   try{
        Class.forName("com.mysql.jdbc.Driver");
        String url = "jdbc:mysql://localhost:3306/companydb";
        String use = "root";
        String password = "666666";
        Connection connection = DriverManager.getConnection(url, use, password);
       Statement statement = connection.createStatement();
       demo1 dm = new demo1();
       dm.createTable(statement);
       System.out.println("请注册");
       System.out.println("请输入账户名，密码以及手机号");
       Scanner scanner = new Scanner(System.in);
       String username = scanner.nextLine();
       String passworduser = scanner.nextLine();
       String telephone = scanner.nextLine();
       dm.insert(statement,username,passworduser,telephone);
       System.out.println("注册成功");

       System.out.println("请登录");
       System.out.println("输入账户名和密码");
       Scanner scanner1 = new Scanner(System.in);
       String getname = scanner1.nextLine();
       String getpassword = scanner1.nextLine();
       String searchpassword = dm.search(statement,getname);
       if (getpassword.equals(searchpassword)){
           System.out.println("登录成功");
       }else {
           System.out.println("登录失败");
       }

       statement.close();
       connection.close();
    }catch (ClassNotFoundException | SQLException e){
       System.out.println("数据库连接失败");
       e.printStackTrace();
   }
}
// 创建数据表
public void createTable(Statement statement){
    try{
        String sql = "create table `user`(" +
                " id INT PRIMARY KEY AUTO_INCREMENT," +
                "USERNAME VARCHAR(20) UNIQUE NOT NULL," +
                "PASSWORD VARCHAR(20) NOT NULL ," +
                "PHONE VARCHAR(11))CHARSET=UTF8;";
        int reslut = statement.executeUpdate(sql);

    }catch (Exception e){
        System.out.println("数据表创建失败");
        e.printStackTrace();
    }
}
// 表中插入数据

public void insert(Statement statement,String username,String password,String telephone){
    try{
        String sql = "INSERT INTO USER(USERNAME,PASSWORD,PHONE)"+" " +
                "VALUES('" +username+"','"+password+"','"+telephone+
                "');";
       int result =  statement.executeUpdate(sql);
       System.out.println("成功插入数据");
    }catch (Exception e){
        System.out.println("插入数据失败");
        e.printStackTrace();
    }
}

// 查询数据

public String search(Statement statement,String name){
    try {
        String sql = "select PASSWORD from user where USERNAME = '"+name+"';";
        ResultSet resultSet = statement.executeQuery(sql);
        String s = "";
        while (resultSet.next()){
            s = resultSet.getString(1);
        }
        return s;
    }catch (Exception e){
        System.out.println("查询数据失败");
        e.printStackTrace();
        return null;
    }
}
```