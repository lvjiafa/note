# JDBC
什么是jdbc，就是使用Java代码发送SQL语句的技术    
![](_v_images/20181206100425193_8027.png =573x)
    
**使用前提**
* 登录数据库服务器（连接数据库服务器）
    1. 数据库IP地址
    2. 端口（默认3306）
    3. 数据库用户名
    4. 密码    
*使用Driver接口connect方法连接*
```
//导Driver包，用于连接数据库
import java.sql.Driver
calss dao{
        //使用jdbc协议创建url
        //url格式：jdbc:数据库子协议://主机名:端口/连接数据库名
        private String url="jdbc:mysql://locasthost:33060/pre_study";
        //用户名和密码
        private String user="root";
        private String password="123456";
    public void test(){
        Properties p = new Proerties();
        p.setProperty("user",user);
        p.setProperty("password",password);
        //导包(mysql-connector-java-5.1.7-bin.jar)，
        //用jar包下的具体实现类Driver类给Driver接口创建对象
        Driver driver = new com.mysql.jdbc.Driver();
        //Driver对象调用connect方法连接数据库
        //用Connection接口返回连接对象
        Connection c = new driver.connect(url,p);
        System.out.println(c);
        /*
        输出连接对象
        com.mysql.jdbc.JDBC4Connection@1b8d6f7
        */
    }
}
```
*使用Driver类驱动管理器连接*
```
class dao{
        //使用jdbc协议创建url
        //url格式：jdbc:数据库子协议://主机名:端口/连接数据库名
        private String url="jdbc:mysql://locasthost:33060/pre_study";
        //用户名和密码
        private String user="root";
        private String password="123456";
    public void test2(){
        Driver d = new com.mysql.jdbc.Driver();
        Driver d2 = new com.oracle.jdbc.Driver();
        //注册驱动程序（可注册多个）
        DriverManager.registerDriver(d);
        DriverManager.registerDriver(d2);
        //连接到具体数据库
        Connection c = DriverManagr.getConnction(url,user,password);
        System.out.println(c);
        /*
        输出连接对象
        */
    }
}
```
*代码改进*
```
class dao{
        //使用jdbc协议创建url
        //url格式：jdbc:数据库子协议://主机名:端口/连接数据库名
        private String url="jdbc:mysql://localhost:33060/pre_study";
        //用户名和密码
        private String user="root";
        private String password="123456";
    public void test2(){
        //通过字节码对象的方式加载静态代码块，从而注册程序
        Class.forName("com.mysql.jdbc.Driver"); 
        //连接到具体数据库
        Connection c = DriverManagr.getConnction(url,user,password);
        System.out.println(c);
        /*
        输出连接对象
        */
    }
}
```
**[JDBC接口核心API](https://docs.oracle.com/javase/7/docs/api/)**    
java.sql包和javax.sql包
* Class Properties    
        Object setProperty(String key, String value)     
* Interface Driver：java驱动程序接口，由具体数据库厂商实现该接口    
        Connection connect(String url, Properties info)     
            url：jdbc:数据库子协议://主机名:端口/连接数据库名    
            properties需要两个值：    
            user：数据库用户名    
            password：数据库密码    
* Class DriverManager：驱动管理器类，用于管理所有的注册的驱动程序。    
        static void registerDriver(Driver driver )    
        static Connection getConnection(String url, String user, String password)     
* Interface Connection：连接对象    
        Statement createStatement()      
        PreparedStatement prepareStatement(String sql)         
        CallableStatement prepareCall(String sql)      
* Interface Statement：用于执行静态SQL语句    
        int executeUpdate(String sql)     
        ResultSet executeQuery(String sql)     
* Interface PreparedStatement：用于执行预编译SQL语句    
        int executeUpdate()     
* Interface CallableStatement：用于执行存储过程SQL语句    
        ResultSet executeQuery()
* Interface ResultSet：数据库的数据集    
        boolean next()      
        getxx()
**使用Statement执行静态SQL语句**
* DMl语句
```
public dao{
    private String url="jdbc:mysql://localhost:3306:/pre_study";
    private String user="root";
    private String password="123456";   
    public void connect(){
        Statenment s = null;
        Connection c = null;
        //1.注册驱动
        Class.forName("com.mysql.jdbc.Driver");
        //2.获取连接对象
        Connection c = DriverManager.getConnection(url,user,password);
        //3.创建Statement
        Statement s = c.createStatment();
        //4.准备SQL
        String sql="create table student(id int primary key,name varchar(20))";
        //5.发送SQL语句
        int count = s.excuteUpdate(sql);
        System.out.println(count);
        //6.关闭连接(后打开的先关闭)
        if(s!=null) c.close;
        if(c!=null) c.close;
        }
}
```
* 抽取代码写成工具类
```
public class JdbcUtil{
    private static String url="jdbc:mysql://localhoost:3306/pre_study";
    private static String user="root";
    private static String password="123456";
    /**
    *静态代码块，只加载一次
    */
    static {
    //注册驱动程序
    Class.forName("com.mysql.jdbc.Driver");
    }
    /**
    *获取连接对象
    */
    public static Connection getConnection(){
        Connection c = DriverManager.getConnection(url,user,password);
        return c;
    }
    /**
    *释放资源
    */
    public static void close(Connection c,Statement s){
        if(c!=null){
            c.close();
        }
        if(s!=null){
            s.close();
        }
    public static void close(Connection c,Statement s,ResultSet r){
        if(r!=null){
            r.close();
        }
        if(c!=null){
            c.close();
        }
        if(s!=null){
            s.close();
        }
    }
}
```
* 利用工具类
```
public class Dao{
    Connection c=null;
    Statement s=null;
    public void test(){
        //获取连接
        c = JdbcUtil.getConnection();
        //创建Statement
        s = c.creatStatement();
        //准备SQL
        String str="create table student(id int primary key,name varchar(20))";
        //执行SQL
        int count = s.excuteUpdate(sql);
        System.out.println(count);
        //关闭连接
        JdbcUtil.close(c,s);
    }
}
```
* 执行查询操作
```
public class Dao{
    public void test(){
        Connection c=null;
        Statement s=null;
        ResultSet rs=null;
         //获取连接
        c = JdbcUtil.getConnection();
        //创建Statement
        c.creatStatement();
        //准备SQL
        String str="select * from pet";
        //执行SQL
        ResultSet rs = s.executeQuery(sql);
        /*
        //移动光标
       boolean flag = rs.next();
       if(flag){
       //获取列值
           int id=rs.getInt(“id”);
           String name=rs.getSting("name");
           String owner=rs.getString("owner");
       }
       */
       //遍历结果
       while(rs.next()){
           //获取列值
           int id=rs.getInt(“id”);
           String name=rs.getSting("name");
           String owner=rs.getString("owner");
       }
       //关闭连接
        JdbcUtil.close(c,s,rs);  
    }
}
```
* PrepareStatement
```
public class Dao{    
    public void test(){
        Connection c=null;
        PrepareStatement s=null;
        //获取连接
        c = JdbcUtil.getConnection();
        //创建Statement
        c.creatStatement();
        //准备SQL
        String str="insert into pet(name,owner) values(?,?)";//?表示参数的占位符
        //执行预编译SQL
        s =  c.PrepareStatement(str);
        //设置参数值
        s.setString(1,"jacy");//第一个问号
        s.setString(2,"ketty");//第二个问号
        //发送参数，执行SQL
        int count = s.excuteUpdate();//注意这里是无参方法
        System.out.println(count);
        //关闭连接
        JdbcUtil.close();
    }
}
```
* CallableStatement    
SQL语句如下
```
delimiter $
create procedure pro_findId(in sid int)
begin
    select * from student where id=sid;
end $
```
调用如下
```
class public Dao{
    public void test(){
        Connnect conn=null;
        CallableStatement stmt=null;
        ResultSet rs=null;
        conn=JdbcUtil.getConnection();
        String sql="call pro_finaId(?)";
        stmt=conn.prepareCall(sql);
        stmt.setInt(1,4);
        rs=stmt.executeQuery();//存储过程只能用executeQuery()
        while(rs!=null){
            int id = rs.getInt("id");
            String name = rs.getString("name");
        }
        JdbcUtil.colse(conn,stmt);
    }
}
```
* 调用带输出的存储过程    
存储过程SQL语句如下
```
delimiter $
create procedrue pro_findId(in sid int,out sname varchar(20))
begin
    select name into sname from student where id=sid;
end $
```
调用如下
```
class public Dao{
    public void test(){
        Connnect conn=null;
        CallableStatement stmt=null;
        conn=JdbcUtil.getConnection();
        String sql="call pro_finaID(?,?)";
        stmt=conn.prepareCall(sql);
        //设置输入参数
        stmt.setInt(1,4);
        //注册输出参数
        /**
        *参数一：参数位置
        参数二：存储过程输出参数的jdbc类型
        */
        stmt.registOutParameter(2,java.sql.Types.VARCHAR);
        //发送参数
        stmt.executeQuery();
        //返回存储过程值
        String result=stmt.getString(2);//索引值与注册值一致
        JdbcUtil.colse(conn,stmt);
    }
}
```







