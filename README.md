# java-todos
JDBC + SQLite

> JDBC - Java Database Connectivity

### Output
> Let's create a Todo's Application using Java and SQLite.


### Part 1 (Back End Dev't Perspective)
This part will cover the Todo's App Project setup which includes:

* Create new Java Application using Netbeans IDE
* Setup SQLite Database Driver using Netbeans Services
* Write a SQLite Class that handles the Todo's App API
* Test and Verify the Todo's App API

#### Step 1 : Create new Java Application using Netbeans IDE
![alt text][01]
[01]: Midterm/Activity2/01.jpg "Logo Title Text 2"

#### Step 2 : Setup SQLite Database Driver using Netbeans Services
![alt text][02]
[02]: Midterm/Activity2/02.jpg "Logo Title Text 2"

#### Step 3 : Write a SQLite Class that handles the Todo's App API
```java
    static java.sql.Connection conn  = null;
    static java.sql.Statement stmt = null;
    static String url = "jdbc:sqlite:<FILE>";
    static String error = "";
```

```java
    public static boolean openDB(){
        boolean result = false;
        try{
            Class.forName("org.sqlite.JDBC");
            conn = java.sql.DriverManager.getConnection(url);

            System.out.println("OK! SQLite DB session connected.");
            result = true;
        }
        catch(Exception e){
            error = e.getMessage();
            System.out.println("Open DB Error:" + e.getMessage());
        } 
        return result;
    }
```

```java
    public static boolean closeDB(){
        boolean result = false;
        try{
            conn.close();
            
            System.out.println("OK! SQLite DB session closed.");
            result = true;
        }
        catch(Exception e){
            error = e.getMessage();
            System.out.println("Close DB Error: " + e.getMessage());
        }
        return result;
    } 
```

```java
    public static boolean create(String table, String values){
        boolean result = false;
        try{
            SQLite.stmt = conn.createStatement();
            String query = "INSERT INTO "+ table +" VALUES(" + values + ")";
            stmt.executeUpdate(query);
            //You can include exception handling here. (e.g. Duplicate Data, etc.)
            result = true;
        }
        catch(Exception e){
            System.out.println("Create Error: " + e.getMessage());
        }
        return result;
    }
```

```java
    if(SQLite.openDB()){
        int id = 4; //If you reveive an error, probably sqlite detect duplicate ID value.
        String task = "Task 4";
        String isdone = "NO";
        if(create("task","'"+id+"'"+","+"'"+task+"'"+","+"'"+isdone+"'")){
            System.out.println("Inserted successfully!");
        }
        SQLite.closeDB();
    }   
```

```java
    public static boolean update(String table, String set, int id){
        boolean result = false;
        try{
            SQLite.stmt = conn.createStatement();
            String query = "UPDATE "+ table +" SET " + set + " WHERE id=" + id;
            stmt.executeUpdate(query);
            //You can include exception handling here. (e.g. Duplicate Data, etc.)
            result = true;
        }
        catch(Exception e){
            System.out.println("Create Error: " + e.getMessage());
        }
        return result;
    }
```

```java
    //Test update method
    if(SQLite.openDB()){
        int id = 4;
        String isdone = "YES";
        if(update("task", "ISDONE='"+isdone+"'", id)){
            System.out.println("Updated successfully!");
        }
        SQLite.closeDB();
    } 
```

```java
    public static boolean delete(String table, int id){
        boolean result = false;
        try{
            SQLite.stmt = conn.createStatement();
            String query = "DELETE FROM "+ table + " WHERE id=" + id;
            stmt.executeUpdate(query);
            result = true;
        }
        catch(Exception e){
            System.out.println("Create Error: " + e.getMessage());
        }
        return result;
    } 
```

```java
        Test delete method
        if(SQLite.openDB()){
            int id = 4;
            if(delete("task", id)){
                System.out.println("Deleted successfully!");
            }
            SQLite.closeDB();
        } 
```

```java
    public static String[][] read(String table){
        String[][] records = null;
        try{
            SQLite.stmt = conn.createStatement();
            
            //Count total rows
            ResultSet rs = stmt.executeQuery("SELECT count(*) FROM " + table);
            int totalRows = rs.getInt(1);
            
            //Count total columns
            rs = stmt.executeQuery("SELECT * FROM " + table);
            ResultSetMetaData rsmd = rs.getMetaData();
            int totalColumns = rsmd.getColumnCount();
            
            //Initialize 2D Array "records" with totalRows by totalColumns
            records = new String[totalRows][totalColumns];
            
            //Retrieve the record and store it to 2D Array "records"
            int row=0;
            while(rs.next()){                
                for(int col=0,index=1;col&lt;totalColumns;col++,index++){
                    records[row][col] = rs.getObject(index).toString();
                }
                row++;
            }            
        }
        catch(Exception e){
            System.out.println("Read Error: " + e.getMessage());
        }
        return records;
    }
```

```java
        //Test read method
        if(SQLite.openDB()){
            String[][] r = read("task");
            System.out.println("Task ID:" + r[0][0]);
            System.out.println("Task NAME:" + r[0][1]);
            System.out.println("Task ISDONE?:" + r[0][2]);
            SQLite.closeDB();
        }
```

#### Step 4 : Test and Verify the Todo's App API
