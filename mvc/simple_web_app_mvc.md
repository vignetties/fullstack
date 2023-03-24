
Sure, here's a simple Java web application that follows the MVC architecture:

1.  Create a new Java project in your IDE of choice, and add the following directories:

-   `src/main/java` for the Java source code
-   `src/main/webapp` for the web application resources, such as HTML, CSS, and JavaScript files

2.  Create a simple `User` model class with the following properties:
```
public class User {
    private int id;
    private String firstName;
    private String lastName;
    private String email;
    
    // Constructor, getters, and setters
}

```
3.  Create a `UserDao` interface and an implementation class that will communicate with the database. Here's an example using JDBC:
```
public interface UserDao {
    List<User> getAllUsers() throws SQLException;
    User getUserById(int id) throws SQLException;
    void addUser(User user) throws SQLException;
    void updateUser(User user) throws SQLException;
    void deleteUser(int id) throws SQLException;
}

public class UserDaoImpl implements UserDao {
    private Connection conn;
    
    public UserDaoImpl() {
        // Set up the database connection
        String url = "jdbc:mysql://localhost:3306/mydatabase";
        String user = "myusername";
        String password = "mypassword";
        conn = DriverManager.getConnection(url, user, password);
    }
    
    @Override
    public List<User> getAllUsers() throws SQLException {
        // Execute a SELECT query and return a List of User objects
    }
    
    @Override
    public User getUserById(int id) throws SQLException {
        // Execute a SELECT query with a WHERE clause and return a User object
    }
    
    @Override
    public void addUser(User user) throws SQLException {
        // Execute an INSERT query to add a new User to the database
    }
    
    @Override
    public void updateUser(User user) throws SQLException {
        // Execute an UPDATE query to update an existing User in the database
    }
    
    @Override
    public void deleteUser(int id) throws SQLException {
        // Execute a DELETE query to remove a User from the database
    }
}
```
4.  Create a simple servlet that acts as the Controller:
```
@WebServlet("/users")
public class UserController extends HttpServlet {
    private UserDao userDao;
    
    public void init() {
        userDao = new UserDaoImpl();
    }
    
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // Get all users from the database and pass them to the View for display
        List<User> userList = userDao.getAllUsers();
        request.setAttribute("users", userList);
        request.getRequestDispatcher("users.jsp").forward(request, response);
    }
    
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // Create a new User object from the form data and add it to the database
        User newUser = new User();
        newUser.setFirstName(request.getParameter("firstName"));
        newUser.setLastName(request.getParameter("lastName"));
        newUser.setEmail(request.getParameter("email"));
        userDao.addUser(newUser);
        
        // Redirect the user back to the list of users
        response.sendRedirect("users");
    }
}
```
5.  Finally, create a JSP file that acts as the View:
```
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>List of Users</title>
</head>
<body>
    <h1>List of Users</h1>
    <table>
        <tr>
            <th>ID</th>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Email</th>
        </tr>
        <c:forEach items="${users}" var="user">
            <tr>
                <td>${user.id}</td>
                <td>${user.firstName}</td>
                <td>${user.lastName}</td>
                <td>${user.email}</td>
            </tr>
        </c:forEach>
    </table>
    
    <h2>Add a New User</h2>
    <form method="post" action="${pageContext.request.contextPath}/users">
        <label for="firstName">First Name:</label>
        <input type="text" name="firstName" id="firstName" required>
        <br>
        <label for="lastName">Last Name:</label>
        <input type="text" name="lastName" id="lastName" required>
        <br>
        <label for="email">Email:</label>
        <input type="email" name="email" id="email" required>
        <br>
        <input type="submit" value="Add User">
    </form>
</body>
</html>
```
Note that this JSP file uses JSTL tags (`<c:forEach>`) to iterate over the list of users passed in from the Controller, and EL expressions (`${user.id}`) to display the properties of each user. The form at the bottom of the page submits a POST request to the Controller, which creates a new User object and adds it to the database.

## in the above example which part is belongs to model and what its doing ?

In the example I provided, the `User` class represents the Model. It defines the properties of a user (id, firstName, lastName, email) and provides getters and setters for each property.

The `UserDAO` class also belongs to the Model layer. It is responsible for accessing the database and performing CRUD operations on the `User` table. Specifically, it provides methods for creating a new user (`saveUser()`), retrieving all users (`getAllUsers()`), and retrieving a user by ID (`getUserById()`).

The Controller layer is represented by the `UserController` class, which handles incoming HTTP requests and sends data to the View for rendering. It uses the `UserDAO` class to retrieve data from the database and passes it to the View via a `ModelAndView` object.

The View layer is represented by the JSP file, which displays data to the user and provides a form for adding new users. When the form is submitted, the data is sent back to the Controller for processing.
