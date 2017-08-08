# java-todos
JDBC + SQLite

> [JDBC - Java Database Connectivity](http://docs.oracle.com/javase/tutorial/jdbc/basics/index.html)

> [SQLite - Embedded SQL database engine](https://www.sqlite.org/about.html)

### Output
> Let's create a Todo's Application using Java and SQLite.


### Part 1 (Back End Dev't Perspective)
This part will cover the Todo's App Project setup which includes:

* Create new Java Application using Netbeans IDE
* Setup SQLite Database Driver using Netbeans Services
* Write a SQLite Class that handles the Todo's App API
* Test and Verify the Todo's App API

#### Step 1 : Create new Java Application using Netbeans IDE
![step 1](https://github.com/clydeatuic/java-todos/blob/master/01.JPG)


#### Step 2 : Setup SQLite Database Driver using Netbeans Services
![step 2](https://github.com/clydeatuic/java-todos/blob/master/02.JPG)

#### Step 3 : Write a SQLite Class that handles the Todo's App API
```java
//Static Variables
static java.sql.Connection conn  = null;
static java.sql.Statement stmt = null;
static String url = "jdbc:sqlite:<FILE>";
static String error = "";
```

```java
//Open DB Session Method
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
//Close DB Session Method
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
//Create Record Method
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
//Update Record Method
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
//Delete Record Method
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
//Read Record Method
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
            for(int col=0,index=1;col<totalColumns;col++,index++){
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

#### Step 4 : Test and Verify the Todo's App API

```java
//Main method (Optional! This is intended for testing...)
public static void main(String [] args){
	//Test code snippets here...
}
```

```java
//Test code snippet for openDB and closeDB methods
if(SQLite.openDB()){
    int id = 4; //If you receive an error, probably sqlite detect duplicate ID value.
    String task = "Task 4";
    String isdone = "NO";
    if(create("task","'"+id+"'"+","+"'"+task+"'"+","+"'"+isdone+"'")){
        System.out.println("Inserted successfully!");
    }
    SQLite.closeDB();
}   
```

```java
//Test code snippet for update method
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
//Test code snippet for delete method
if(SQLite.openDB()){
    int id = 4;
    if(delete("task", id)){
        System.out.println("Deleted successfully!");
    }
    SQLite.closeDB();
} 
```

```java
//Test code snippet for read method
if(SQLite.openDB()){
    String[][] r = read("task");
    System.out.println("Task ID:" + r[0][0]);
    System.out.println("Task NAME:" + r[0][1]);
    System.out.println("Task ISDONE?:" + r[0][2]);
    SQLite.closeDB();
}
```
#### Done for Part 1

> Feel free to create an issue and if you find it interesting drop a star. Thanks! :octocat:

### Part 2 (Front End Dev't Perspective)

This part will cover the Todo's App Project User Interface which includes:

* Create JFrame that uses components (JTextField, JButton, JLabel, JTable etc.)
* Create a CRUD User Interfaces:
- [ ] UI for CREATE Task
- [ ] UI for RETRIEVE/READ Task
- [ ] UI for UPDATE Task
- [ ] UI for DELETE Task
* Create a Lastname1_Lastname2_TodosAPP.jar file

Note: By Pair (Front-End and Back-End)
