# JDBC

> 简介：java 语言操作关系型数据库的一套 api （sun 公司）


同一套 java 代码，操作不同的关系型数据库

![](image/image_GtgISsjn-E.png#alt=)

步骤：

- 
注册驱动 

   - 

[MySQL :: MySQL Community Downloads](https://dev.mysql.com/downloads/)
- 
获取 连接

- 
定义 sql 语句

- 
获取 执行  sql 对象

- 
执行 sql

- 
处理返回结果 

- 
释放资源 


```java
import java.sql.*;

public class Jdbc_demo {

    public static void main(String[] args) throws ClassNotFoundException, SQLException {
//        注册驱动 （驱动注册可以不写）
        Class.forName("com.mysql.cj.jdbc.Driver");

//        获取连接
//String url="jdbc:mysql:///svue"; //如果是本机地址并且端口是默认的可以简写
        String url="jdbc:mysql://localhost:3306/svue";
        String username="root";
        String password="725482520";
       Connection conn= DriverManager.getConnection(url,username,password);

//        定义 sql
        String sql="update  book set  name=\"沈扬\"  where  id=1;";

//        获取执行 sql 的对象 Statement
        Statement statement=conn.createStatement();

//        执行 sql
      int s=   statement.executeUpdate(sql);

//        处理结果
        System.out.println(s);
//   释放资源
        statement.close();
        conn.close();

    }
}
```

java: static

静态资源是随着类的加载而执行，而且只执行一次

- DriverManager

作用： 

- 注册驱动

driver的源码：

```java
   static {
        try {
            java.sql.DriverManager.registerDriver(new Driver());
        } catch (SQLException E) {
            throw new RuntimeException("Can't register driver!");
        }
    }
```

- 获取数据库连接

参数：

- 
url : 语法： jdbc:mysql://ip地址:端口号/数据库名称?参数键值对1&参数键值对2

- 
user : 用户名

- 
password ：密码

- 
Connection

作用：

   - 
获取执行 SQL 的对象

   - 
管理事务


     

- 
获取执行 SQL 对象

   - 
普通执行 SQL 对象

      - Statement createStatement()
   - 
预编译 SQL 的执行 SQL 对象 ： 防止 SQL 注入

      - PreparedStatement prepareStatement(sql)
   - 
执行存储过程的对象

      - CallableStatement prepareCall(sql)

```java
import java.sql.*;
import java.util.Iterator;
import java.util.Objects;
import java.util.Set;
public class Jdbc_demo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
//        注册驱动
//        Class.forName("com.mysql.cj.jdbc.Driver");

//        获取连接
        String url="jdbc:mysql:///svue"; //如果是本机地址并且端口是默认的可以简写
        String username="root";
        String password="725482520";

       Connection conn= DriverManager.getConnection(url,username,password);
        Statement statement=null;

try{
    //       开启事务
    conn.setAutoCommit(false);

    //        定义 sql
    String sql="select  * from  book;";

//        获取执行 sql 的对象 Statement
     statement=conn.createStatement();
//        执行 sql
    ResultSet s=   statement.executeQuery(sql);
//        处理结果
    System.out.println(s);
//    提交事务
    conn.commit();
    System.out.println("事务成功提交了");
}catch (Exception e){
    e.printStackTrace();
//    回滚事务
    conn.rollback();
    System.out.println("回滚事务了");
}
//   释放资源
        statement.close();
        conn.close();

    }
    
}
```

- 
Statement 

   - 执行 SQL 语句

```java
            ResultSet s=   statement.executeQuery(sql);
//            statement.executeQuery()//查询 返回值 ResultSet
//           statement.executeLargeUpdate()//更新 返回值  int 受影响的行数
//==========================示例
import java.sql.*;
import java.util.Iterator;
import java.util.Objects;
import java.util.Set;

public class Jdbc_demo1 {

    public static void main(String[] args) throws ClassNotFoundException, SQLException {
//        注册驱动
//        Class.forName("com.mysql.cj.jdbc.Driver");

//        获取连接
        String url="jdbc:mysql:///svue"; //如果是本机地址并且端口是默认的可以简写
        String username="root";
        String password="725482520";
        Connection conn= DriverManager.getConnection(url,username,password);

        String sql="select * from book;";

        Statement statement=conn.createStatement();

        ResultSet resultSet = statement.executeQuery(sql);

        while (resultSet.next()){//光标向下移动一行，并且判断 当前行是否有效
//       int i=     resultSet.getInt(1);
//      String s=      resultSet.getString(2);
//      String s1=      resultSet.getString(3);
                   int i=     resultSet.getInt("id");
      String s=      resultSet.getString("name");
      String s1=      resultSet.getString("author");
//      它的 get 方法有重载，它们的参数对应数据库表的列名或列号 从 1 开始 就可以拿到值了
            System.out.println(i+"    "+s+"    "+s1);
        }

//        释放资源
        resultSet.close();
        statement.close();
        conn.close();

    }

}
```

- 
PreparedStatement 

   - 
预防SQL注入问题
```java
package jdbc;
import pojo.Book;
import java.sql.*;
import java.util.ArrayList;
public class Jdbc_demo3 {
    private static ArrayList<Book> arrayList=new ArrayList<>();
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
//        注册驱动
//        Class.forName("com.mysql.cj.jdbc.Driver");
//        获取连接
        String url="jdbc:mysql:///svue"; //如果是本机地址并且端口是默认的可以简写
        String username="root";
        String password="725482520";
        Connection conn= DriverManager.getConnection(url,username,password);
//        String sql="select * from book;";
        Statement statement=conn.createStatement();
        String name="zhangsan";
        String ps="123456";
//       SQL 语句中的参数值， 使用 ？ 占位符替代
        String sql="select * from user where username= ? and password=?";
        PreparedStatement preparedStatement = conn.prepareStatement(sql);
//        设置参数 参数的位置编号   参数的值
        preparedStatement.setString(1,name);
        preparedStatement.setString(2,ps);
//        执行 不需要传递 SQL 语句
        ResultSet resultSet = preparedStatement.executeQuery();
//        判断逻辑c
//        。。。。。
//        释放资源
        resultSet.close();
        statement.close();
        conn.close();
    }
}
```



使用 useServerPrepStmts=true 来开启预编译

String url="jdbc:mysql:///svue?useServerPrepStmts=true"; //如果是本机地址并且端口是默认的可以简写

加在参数后面。

在获取 PreparedStatement 对象的时候，就会把 sql 语句发送给 mysql 服务器检查。

- 数据库连接池

> 负责分配，管理数据库连接。 它允许程序使用一个现有的数据库连接，而不是再重新建立一个


好处：资源复用，提升系统响应速度，避免数据库连接遗漏

标准接口：DataSource

常见的有： DBCP,C3P0,Druid

- Driud连接池是阿里巴巴开源的数据库连接池项目

[https://druid.apache.org/](https://druid.apache.org/)

中文文档：

[http://www.apache-druid.cn/GettingStarted/chapter-1.html](http://www.apache-druid.cn/GettingStarted/chapter-1.html)

使用步骤：

- 
导入 jar包 druid

   - 

[Central Repository: com/alibaba/druid (maven.org)](https://repo1.maven.org/maven2/com/alibaba/druid/)
- 
定义配置文件

   - druid.properties
```
#驱动加载
#mysql8.0的驱动名称
driverClassName=com.mysql.cj.jdbc.Driver
url=jdbc:mysql://localhost:3306/svue?serverTimezone=GMT%2B8&useUnicode=true&characterEncoding=utf8&autoReconnect=true&useSSL=false
#连接数据库的用户名
username=root
#连接数据库的密码
password=725482520
#初始化时池中建立的物理连接个数
initialSize=10
#最大的可活跃的连接池数量
maxActive=20
#获取连接时最大等待时间，单位毫秒，超过连接就会失效
maxWait=3000
```

- 
加载配置文件

- 
获取数据库连接池对象

- 
获取连接


```java
import com.alibaba.druid.pool.DruidDataSourceFactory;
import javax.sql.DataSource;
import java.io.FileInputStream;
import java.sql.Connection;
import java.util.Properties;
public class druid_demo {
    public static void main(String[] args) throws Exception {
//        加载配置文件
        Properties properties=new Properties();
        properties.load(new FileInputStream("D:\\work\\java项目work\\Archive\\jdbcdemo\\src\\druid.properties"));
//        获取连接池对象
   DataSource dataSource= DruidDataSourceFactory.createDataSource(properties);
//        获取数据库连接 Connection
        Connection connection=dataSource.getConnection();
        System.out.println(connection);
//        System.out.println(System.getProperty("user.dir"));
    }
}
```

- 小案例

sql 脚本：

```sql

use svue;

create table tb_brand
(
    id int primary key  auto_increment,
    brand_name varchar(20),
    company_name varchar(20),
#     排序字段
    ordered int,
#     描述信息
    description varchar(100),
#      0 禁用  1  启用
    status int
);

insert into tb_brand (brand_name, company_name, ordered, description, status) 
values ('三只松鼠','三只松鼠股份有限公司',5,'好吃不上火',0),
       ('华为','华为技术有限公司',100,'华为致力于把数字带入每个人‘每个家庭’每个组织，构建万物互联的智能安乐',1),
       ('小米','小米科技有限公司',50,'are you ok',1);

select *
from tb_brand;
```

实体类

```java
package jdbc;
// 品牌  对应数据库中的表
public class druid_demo2 {
// alt + 鼠标左键 可以整列编辑
//    在实体类基本类型建议使用对应的包装类型
//    主键
private   Integer    id                          ;
//    品牌名称
    private   String  brand_name                 ;
//    企业名称
    private   String   company_name             ;
//    排序字段
    private  Integer   ordered                          ;
    //     描述信息
    private String   description                     ;
//    0 禁用  1  启用
    private  Integer   status                           ;
    
}
```
