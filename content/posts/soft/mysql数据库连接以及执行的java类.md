应该使用json等配置文件来保存要连接的数据库信息,密码等.
先存一下,以后再说

**DB_connection.java**
```
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package com;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

/**
 *
 * @author peng
 */


public class DB_connection {
    // JDBC 驱动名及数据库 URL
    String JDBC_DRIVER = "com.mysql.jdbc.Driver";  
    String DB_URL = "jdbc:mysql://localhost:3306/javabbs?useUnicode=true&characterEncoding=UTF-8";


    // 数据库的用户名与密码，需要根据自己的设置
    String USER = "root"; 
    String PASS = "mysql";
    
    Connection conn = null;
    
//    private static DB_connection dB_connection =null;
//    public static DB_connection getInstance(){
//        if(DB_connection == null){
//           DB_connection = new DB_connection;
//        }
//        return  DB_connection;
//    }
    public Connection connect(){
        try{
            Class.forName(JDBC_DRIVER);
            
            // 打开链接
            System.out.println("连接数据库...");
            conn = DriverManager.getConnection(DB_URL,USER,PASS);
            if(!conn.isClosed())
                System.out.println("Succeeded connecting to the Database!");
        }
        catch(Exception e){
            e.printStackTrace();
        }
        return conn;
    }
        
        
    public void close(){
        try{
            // 完成后关闭
            conn.close();
        }
        catch(Exception e){
            e.printStackTrace();
        }
    }
    
}

```

**DB_executer.java**
```
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package com;

import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 *
 * @author peng
 */
public class DB_executer {
    
    public void update(String sql) throws SQLException{
        DB_connection db = new DB_connection();
        Connection conn = db.connect();
        Statement stmt = conn.createStatement();
         try{
            stmt.executeUpdate(sql);
            System.out.println("execute successful");
         }
         catch(Exception e){
             e.printStackTrace();
         }
         finally{
            stmt.close();
            db.close();
         }
    }
    
    public List<Map<String, Object>> select(String sql) throws SQLException{
        DB_connection db = new DB_connection();
        Connection conn = db.connect();
        Statement stmt = conn.createStatement();

        // 注册 JDBC 驱动
        List<Map<String, Object>> result = new ArrayList<Map<String, Object>>();
        ResultSet rs = null;
        try{
            System.out.println(sql);
            rs = stmt.executeQuery(sql);
            ResultSetMetaData rmd = rs.getMetaData();
            int count = rmd.getColumnCount();
            while(rs.next()){
                Map map = new HashMap(count);
                for(int i=1;i<=count;i++){
                    System.out.print(rs.getString(i));
                    map.put(rmd.getColumnName(i), rs.getObject(i));
                }
                result.add(map);
            }
            rs.close();
        }
        catch(Exception e){
            e.printStackTrace();
        }
        finally{
            stmt.close();
            db.close();
        }
        return result;
    }
}

```

