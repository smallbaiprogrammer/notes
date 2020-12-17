将业务逻辑的功能封装起来，保证线程安全

一个业务存在多个数据库访问过程，这样操作起来更加安全。

```java
// 注册
// 1.查询 用户是否存在
// 2 若不存在，新增用户
// 3.若存在该用户，则提示用户
public void register(Person person){
    PersonDaoImpl personDao = new PersonDaoImpl();
    Person person1 = personDao.select(person.getName());
    if (person1 == null){
        personDao.insert(person);
        System.out.println("注册成功");
    }else {
        System.out.println("该用户已存在");
    }
}
```

 解决并发问题，采用事务操作

```java
// 采用事务来进行处理
// 自动提交改为手动提交，这就是开启事务
connection.setAutoCommit(false);
connection.commit();
connection.rollback();
// 这样不可行，因为程序执行过程中会出现很多个Conncetion 对象，你只能对一个对象的属性进行操作，所有这样直接写出来是不可以实现的。
// 解决方案一，将Connection 传进每一个函数中，这样所有函数采用的都是同一个对象，这样就可以通过设置提交来解决这个问题，但是这样的话，方法就不可以服用了，灵活性降低，其他方法不能使用了，所以不可行。所以要采用其他的方案。
// 解决方法二
// ThreadLocal 泛型 ，我们采用一个线程内的全局变量，整个事务中，我们只采用了一个线程，所以我们可以设置一个线程内的全局遍历，所以方法用到的话，都调用这一个对象，这样就可以实现共同的对象了。
    private static  ThreadLocal<Connection> threadLocal = new ThreadLocal<>();
if (connection == null){
      connection = DriverManager.getConnection(PROPERTIES.getProperty("url"), PROPERTIES.getProperty("user"), PROPERTIES.getProperty("password"));
                threadLocal.set(connection);
}
// 这样该线程内的所有方法都获得同一个Connection对象
```

关于 `threadLoacl` 的实现原理 

`threadLoacl` 通过键值对将线程和值连接在一起，也就是每一个线程都对应着一个对象，该对象保存,但是该空间覆盖范围比较大，所有尽量不要存储过多数据

事务封装，将事务的开启、提交 和 回滚，封装在工具类中，这样事务层就不会出现数据操作了



现在设计到数据库一般来说，有三层数据结构 `XxxxxView`

第一层，表示层，目的收集用户的数据和序求 展示数据

第二层 ， 业务逻辑层，`XxxxService` 

数据加工处理，调用`dao`完成业务实现，控制业务

第三层 ，数据访问层，将业务层加工后的数据同步到数据库，并且给业务层提供数据



三层架构该如何设计，第三层面向接口进行编程，将重要的方法，封装到接口中，更容易更换实现，灵活的进行项目实现

