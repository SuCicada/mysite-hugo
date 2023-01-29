```
public class DB_connection {
    String JDBC_DRIVER;//="com.mysql.jdbc.Driver";  
    String DB_URL;// = "jdbc:mysql://localhost:3306/promotion_website?useUnicode=true&characterEncoding=UTF-8";
    String FILENAME = "/database.properties";
    

    String USER;// = "root"; 
    String PASS;// = "mysql";
    Connection conn = null;

    public DB_connection() {
        try {
            Properties prop = new Properties();
            String FILE_PATH =  this.getClass().getClassLoader().getResource(FILENAME).getPath();
            System.out.println("path  "+FILE_PATH);
            File file = new File(FILE_PATH);
            FileInputStream in = new FileInputStream(file);
            prop.load(in);
            JDBC_DRIVER = prop.getProperty("JDBC_DRIVER");
            DB_URL = prop.getProperty("DB_URL");
            USER = prop.getProperty("USER");
            PASS = prop.getProperty("PASS");
                    
        } catch (FileNotFoundException ex) {
            Logger.getLogger(DB_connection.class.getName()).log(Level.SEVERE, null, ex);
        } catch (IOException ex) {
            Logger.getLogger(DB_connection.class.getName()).log(Level.SEVERE, null, ex);
        }
    }

.................................
省略
...................................
}
```


就是使用类装载器来获取配置文件的绝对路径
输出结果是
/home/peng/文档/PROGRAM/WEBsite/promotion/build/web/WEB-INF/classes/database.properties

Properties prop = new Properties();
            **String FILE_PATH =  this.getClass().getClassLoader().getResource(FILENAME).getPath();**
            System.out.println("path  "+FILE_PATH);
            **File file = new File(FILE_PATH);
            FileInputStream in = new FileInputStream(file);
            prop.load(in);**

