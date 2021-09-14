

# 1.向数据库添加数据

```java
package com.tbbrbb.test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JDBCDemo {
    public static void main(String[] args) {
        Connection connection=null;
        Statement statement=null;

        try {
            // 1.注册驱动
            Class.forName("com.mysql.jdbc.Driver");
            //2.定义sql
            String sql ="insert into user value(null,'心小茹',123,123,null)";
            //3.获取Connection对象
            try {
             connection = DriverManager.getConnection("jdbc:mysql://82.156.183.70:3306/tbbrbb?useUnicode=true&characterEncoding=UTF-8", "tgg", "123456");
                //4.获取执行sql对象Statament
                 statement = connection.createStatement();
               //4.执行sql
                int i = statement.executeUpdate(sql);
                System.out.println(i);
                if(i>0){
                    System.out.println("添加成功");
                }else {
                    System.out.println("添加失败");
                }

            } catch (SQLException e) {
                e.printStackTrace();
            }
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }finally {
            if(statement!=null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if(connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}


```

# 2.修改数据库中数据

```java
package com.tbbrbb.test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JdbcDem02 {
    public static void main(String[] args) {
        Connection connection=null;
        Statement statement=null;

//        1.获取连接对象
        try {
            connection = DriverManager.getConnection("jdbc:mysql://82.156.183.70:3306/tbbrbb", "tgg", "123456");
//        2.定义sql
            String sql="update user set username='tgg' where ID=4";
//        3.获取执行sql对象
             statement = connection.createStatement();
//         4.执行sql
            int count = statement.executeUpdate(sql);
            System.out.println(count);
//          5.处理结果
            if(count>0){
                System.out.println("修改成功");
            }else {
                System.out.println("修改失败");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
//            6.释放资源
            if(statement!=null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if(connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }

    }
}

```

# 3.删除数据

```java
package com.tbbrbb.test;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JdbcDemo03 {
    public static void main(String[] args) throws SQLException {
        Connection connection = null;
        Statement statement = null;

//        1.获取连接对象
        connection = DriverManager.getConnection("jdbc:mysql://82.156.183.70:3306/tbbrbb", "tgg", "123456");
//        2.定义sql
        String sql = "delete from user where id=4 or id =5";
//        3.获取sql执行statament
        statement = connection.createStatement();
//         4.执行sql
        int i = statement.executeUpdate(sql);
        System.out.println(i);
        if (i > 0) {
            System.out.println("删除数据成功");
        } else {
            System.out.println("删除失败");
        }
        if (statement != null) {
            statement.close();
        }
        if (connection != null) {
            connection.close();
        }
    }

}

```

# 4.查询遍历输出数据库

``` java
package com.tbbrbb.test;

import java.sql.*;

public class JdbcDemo04 {
    public static void main(String[] args) throws SQLException {
        Connection connection=null;
        Statement statement=null;
        ResultSet rs=null;
//        1.获取连接对象
         connection = DriverManager.getConnection("jdbc:mysql://82.156.183.70:3306/tbbrbb?useUnicode=true&characterEncoding=UTF-8", "tgg", "123456");
//         2.定义sql
        String sql="select * from user ";
//          3.获取sql对象
         statement = connection.createStatement();
//      4.执行sql
        rs= statement.executeQuery(sql);
//      5.处理结果
//            5.1让游标向下移动一行
        while (rs.next()){
//            5.2获取数据
            int id = rs.getInt("id");
            String username = rs.getString("username");
            String password = rs.getString("password");
            String email = rs.getString("email");
            Timestamp time = rs.getTimestamp("time");
            System.out.println( id+","+username+","+password+","+email+","+time);
        }
    }
}

```

# 5.定义方法，查询表的数据并遍历，然后装在集合，返回

```java
package com.tbbrbb.test;

import com.tbbrbb.test.domain.Emp;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/*定义一个方法，查询emp表的数据将其封装对象，然后装在集合，返回*/
public class JdbcDemo05 {
    public static void main(String[] args) {
        List<Emp> list = new JdbcDemo05().findAll();
        System.out.println(list);
        System.out.println(list.size());
    }


    Connection connection = null;
    Statement statement = null;
    ResultSet resultSet = null;
    List<Emp> list = new ArrayList<>();
    /*查询多有emp对象*/
    public List<Emp> findAll() {
//    1.获取链接
        try {
            connection = DriverManager.getConnection("jdbc:mysql://82.156.183.70:3306/tbbrbb?useUnicode=true&characterEncoding=UTF-8", "tgg", "123456");
            //    2.定义sql
            String sql = "select * from emp";
//    3.获取执行sql的对象
            statement = connection.createStatement();
//    4.执行sql
            resultSet = statement.executeQuery(sql);
//    5.遍历结果集，封装对象，装载集合
            Emp emp = null;
            List<Emp> list = new ArrayList<>();
            while (resultSet.next()) {
//        获取数据
                int id = resultSet.getInt("id");
                String name = resultSet.getString("NAME");
                String gender = resultSet.getString("gender");
                double salary = resultSet.getDouble("salary");
                Date join_date = resultSet.getDate("join_date");
                int dept_id = resultSet.getInt("dept_id");

//        创建emp对象
                emp = new Emp();
                emp.setId(id);
                emp.setNAME(name);
                emp.setGender(gender);
                emp.setSalary(salary);
                emp.setJoindate(join_date);
                emp.setDeptid(dept_id);

                list.add(emp);
            }
          return list;

        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
            if(connection!=null){
                try {
                    connection.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(statement!=null){
                try {
                    statement.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if(resultSet!=null){
                try {
                    resultSet.close();
                } catch (SQLException e) {
                    e.printStackTrace();

                }
            }
        }
        return list;
    }
}

```

# 6. J d b c Utils工具类

```java
package com.tbbrbb.utils;

import java.io.FileReader;
import java.io.IOException;
import java.net.URL;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.Properties;

public class JdbcUtil {
    private static String url;
    private static String user;
    private static String password;
    private static String driverClassName;
//    文件的读取，只需要读取一次就可以了拿到这些值，使用静态代码块
static {
//    读取资源文件，获取值
    try {
//    1.创建properties集合类
        Properties properties = new Properties();
//        获取src路径下的文件方式 -->ClassLoader 类加载器
        ClassLoader classLoader=JdbcUtil.class.getClassLoader();
        URL resource = classLoader.getResource("jdbc.properties");
        String path = resource.getPath();
        System.out.println(path);
//    2.加载文件
        properties.load(new FileReader(path));
//      3.获取数据，赋值
        url=properties.getProperty("url");
        user=properties.getProperty("user");
        password=properties.getProperty("password");
        driverClassName=properties.getProperty("driverClassName");
//        4.注册驱动
Class.forName(driverClassName);
    } catch (IOException e) {
        e.printStackTrace();
    } catch (ClassNotFoundException e) {
        e.printStackTrace();
    }
}
//    获取链接
    public  static Connection getConnection(){

       return null;
    }
//    释放资源
    public static void close(Statement statement,Connection connection){
        if(statement!=null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(connection!=null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

    }

    public static void close(Statement statement, Connection connection, ResultSet resultSet){
        if(statement!=null){
            try {
                statement.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(connection!=null){
            try {
                connection.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
        if(resultSet!=null){
            try {
                resultSet.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

    }

}


```

# 7.使用JD B C Util工具编写

```java 
package com.tbbrbb.test;

import com.tbbrbb.test.domain.Emp;
import com.tbbrbb.utils.JdbcUtil;

import java.sql.*;
import java.util.ArrayList;
import java.util.List;

/*使用JDBCUtuil工具编写
定义一个方法，查询emp表的数据将其风中皇位对象，然后装在集合，返回*/
public class JdbcDemo06 {
    public static void main(String[] args) {
        List<Emp> list = new JdbcDemo05().findAll();
        System.out.println(list);
        System.out.println(list.size());
    }


    Connection connection = null;
    Statement statement = null;
    ResultSet resultSet = null;
    List<Emp> list = new ArrayList<>();
    /*查询多有emp对象*/
    public List<Emp> findAll() {
//    1.获取链接
        try {
            JdbcUtil.getConnection();
//            connection = DriverManager.getConnection("jdbc:mysql://82.156.183.70:3306/tbbrbb?useUnicode=true&characterEncoding=UTF-8", "tgg", "123456");
            //    2.定义sql
            String sql = "select * from emp";
//    3.获取执行sql的对象
            statement = connection.createStatement();
//    4.执行sql
            resultSet = statement.executeQuery(sql);
//    5.遍历结果集，封装对象，装载集合
            Emp emp = null;
            List<Emp> list = new ArrayList<>();
            while (resultSet.next()) {
//        获取数据
                int id = resultSet.getInt("id");
                String name = resultSet.getString("NAME");
                String gender = resultSet.getString("gender");
                double salary = resultSet.getDouble("salary");
                Date join_date = resultSet.getDate("join_date");
                int dept_id = resultSet.getInt("dept_id");

//        创建emp对象
                emp = new Emp();
                emp.setId(id);
                emp.setNAME(name);
                emp.setGender(gender);
                emp.setSalary(salary);
                emp.setJoindate(join_date);
                emp.setDeptid(dept_id);

                list.add(emp);
            }
            return list;

        } catch (SQLException e) {
            e.printStackTrace();
        }finally {
          JdbcUtil.close(statement,connection,resultSet);
        }
        return list;
    }
}

```

# 8. Spring框架 JD BC Template

``` java
package com.study.datasource.datacourse;

import com.tgg.util.JDBCUtils;
import org.springframework.jdbc.core.JdbcTemplate;

public class JdbcTemplateDemo1 {
    public static void main(String[] args) {
        /*1.导jar包*/
//    2.创建JDBCTemplate对象
        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());
//    3.调用方法
        String sql ="update user set username=? where id=?";
        int count = template.update(sql, "wmy",3);
        System.out.println(count);
    }
}

```

# 9. Template 的封装代码

``` java
package com.study.datasource.datacourse;import com.study.datasource.domain.Emp;import com.tgg.util.JDBCUtils;import org.junit.Test;import org.springframework.jdbc.core.BeanPropertyRowMapper;import org.springframework.jdbc.core.JdbcTemplate;import org.springframework.jdbc.core.RowMapper;import java.sql.Date;import java.sql.ResultSet;import java.sql.SQLException;import java.util.List;import java.util.Map;public class JdbcTemplateDemo02 {    /*修改表中数据*/    @Test    public void test() {//        1.获取JdbcTemplate对象        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//        2.定义sql        String sql = "update emp set salary=? where id=?";//        3.执行sql        int i = template.update(sql, 2, 1);        System.out.println(i);    }    /*添加一条记录*/    @Test    public void test02() {//        1.获取JDBCTemplate对象        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//        2.定义sql        String sql = "insert into emp values (?,?,?,?,?,?)";        int i = template.update(sql, null, "大妖精", "男", 2344, null, 3);        System.out.println(i);    }    /*删除一条记录*/    @Test    public void test03() {//        1.获取JDBCTemplate对象        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//        2.定义sql        String sql = "delete from emp where id=?";        int i = template.update(sql, 6);    }    //    查询id=1,封装成Map集合    @Test    public void test04() {//        1.获取JDBCTemplate对象        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//          2.定义sql        String sql = "select * from emp where id=?";        Map<String, Object> map = template.queryForMap(sql, 1);        System.out.println(map);    }    //    查询id=1和id=2,封装成list集合    /*将每条记录封装成一个个的map集合，再将map集合装载到list集合中*/    @Test    public void test05() {//        1.获取JDBCTemplate对象        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//          2.定义sql        String sql = "select * from emp where id=? or id=?";        List<Map<String, Object>> list = template.queryForList(sql, 1, 2);        System.out.println(list);    }    //    查询所有记录，将其封装成为Emp的javabean的List集合    @Test    public void test06() {//        1.获取JDBCTemplate对象        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//        2.定义sql        String sql = "select * from emp";        List<Emp> list = template.query(sql, new RowMapper<Emp>() {            @Override            public Emp mapRow(ResultSet resultSet, int i) throws SQLException {                Emp emp = new Emp();                int id = resultSet.getInt("id");                String name = resultSet.getString("NAME");                String gender = resultSet.getString("gender");                double salary = resultSet.getDouble("salary");                Date join_date = resultSet.getDate("join_date");                int dept_id = resultSet.getInt("dept_id");                emp.setId(id);                emp.setName(name);                emp.setGender(gender);                emp.setSalary(salary);                emp.setJoin_date(join_date);                emp.setDept_id(dept_id);                return emp;            }        });        for (Emp emp : list) {            System.out.println(emp);        }    }    //    查询所有记录，将其封装成为Emp的javabean的List集合    /*简化用现成的实现类    * */    @Test    public void test07() {//        1.获取JDBCTemplate对象        JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//        2.定义sql        String sql = "select * from emp";        List<Emp> list = template.query(sql, new BeanPropertyRowMapper<Emp>(Emp.class));        for (Emp emp:list){            System.out.println(emp);        }   }    //        查询总的记录数@Testpublic void test08(){//        1.获取JDBCTemplate对象    JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//           2.定义sql    String sql="select count(id) from emp ";//        3.执行sql    Long aLong = template.queryForObject(sql, Long.class);    System.out.println(aLong);}}
```



# 使用BeanUtils封装对象   [jar包](https://www.aliyundrive.com/s/airMsSDEASi)

```java
//		1.导入jar包（commons-beanutils-1.8.0.jar）https://www.aliyundrive.com/s/airMsSDEASi//        2.获取所有请求参数        Map<String, String[]> parameterMap = req.getParameterMap();//        3.创建User对象        User loginuser = new User();//        3.2使用BeanUtils封装        try {            BeanUtils.populate(loginuser,parameterMap);        } catch (IllegalAccessException e) {            e.printStackTrace();        } catch (InvocationTargetException e) {            e.printStackTrace();        }
```



