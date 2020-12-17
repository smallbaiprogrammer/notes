DAO（data access object）数据访问对象

DAO 实现了业务逻辑域数据访问分离，对数据库操作和业务逻辑的技术分开，也就是DAO 只负责数据库的访问

```java
// 对同一张表的所有操作封装在某一个xxxDAO对象中
// 根据增删改查的不同功能实现具体的方法 CRUD 
// DAO 数据库和用户的中继者
public class PersonDaoImpl {
    // 新增
    public int insert(Person person){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        String sql = "insert into PERSON(name,age,borndate,email,address) values(?,?,?,?,?);";
        try{
            connection = DBUtils.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1,person.getName());
            preparedStatement.setInt(2,person.getAge());
            preparedStatement.setDate(3,null);
            preparedStatement.setString(4,person.getEmail());
            preparedStatement.setString(5, person.getAddress());
           int result =  preparedStatement.executeUpdate();
           return result;
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }finally {
            DBUtils.closeAll(connection,preparedStatement,null);
        }

        return 0;
    }
    // 修改
    public int update(Person person){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        String sql = "update person set name= ?,age = ?,borndate=?,email=?,address=? where id = ?;";
        try{
            connection = DBUtils.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setString(1,person.getName());
            preparedStatement.setInt(2,person.getAge());
            preparedStatement.setDate(3,null);
            preparedStatement.setString(4,person.getEmail());
            preparedStatement.setString(5,person.getAddress());
            preparedStatement.setInt(6,person.getId());
            int result = preparedStatement.executeUpdate();

            return result;
        } catch (SQLException throwables) {
            throwables.printStackTrace();
        }
        finally {
            DBUtils.closeAll(connection,preparedStatement,null);
        }
        return 0;}
    // 删除
    public int delete(int id){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        try{
            connection = DBUtils.getConnection();
            String sql = "delete from person where id = ?;";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setInt(1,id);
            int result = preparedStatement.executeUpdate();

            return result;
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            DBUtils.closeAll(connection,preparedStatement,null);
        }
        return 0;}
    // 查单个
    public Person select(int id){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        Person person = null;
        try{
            connection = DBUtils.getConnection();
            String sql = "select * from person where id = ?;";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.setInt(1,id);
            resultSet = preparedStatement.executeQuery();
            if (resultSet.next()){
                person = new Person();
                person.setId(resultSet.getInt("id"));
                person.setName(resultSet.getString("name"));
                person.setAge(resultSet.getInt("age"));
                person.setBornDate(resultSet.getDate("borndate"));
                person.setEmail(resultSet.getString("email"));
                person.setAddress(resultSet.getString("address"));
            }
            return person;
        }catch (Exception e){
            e.printStackTrace();
        }
        finally {
            DBUtils.closeAll(connection,preparedStatement,resultSet);
        }

        return null;
    }
    // 查所有
    public List<Person> selectAll(){
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;
        List<Person> list = new ArrayList<>();
        try{
            String sql = "select * from person;";
            connection = DBUtils.getConnection();
            preparedStatement = connection.prepareStatement(sql);
            resultSet = preparedStatement.executeQuery();
            while (resultSet.next()){
                Person person = new Person();
                person.setId(resultSet.getInt(1));
                person.setName(resultSet.getString(2));
                person.setAge(resultSet.getInt(3));
                person.setBornDate(resultSet.getDate(4));
                person.setEmail(resultSet.getString(5));
                person.setAddress(resultSet.getString(6));
                list.add(person);
            }
            return list;
        }catch (Exception e){
            e.printStackTrace();
        }
        finally {
            DBUtils.closeAll(connection,preparedStatement,resultSet);
        }
        return null;
    }
}
```

// 日期类型 

```java
Java.utils.Date
java.sql.Date  // 只能通过毫米值来创建时间对象，可直接插入到数据库
通过 SimpleDateFormat 类来进行转换
```

