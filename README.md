# Login & Registration System

A modern, full-stack web application built with Java Servlets and JSP that provides user authentication functionality with secure password management.

## 📋 Project Overview

This is an advanced Java Mini Project demonstrating a complete user authentication system. It implements a login/registration system with database integration, password hashing, and a user-friendly interface. The application is built as a Maven-based WAR (Web Application Archive) project and can be deployed on any Java Application Server (Tomcat, JBoss, etc.).

## 🎯 Features

- **User Registration**: New users can create an account with a username and password
- **User Login**: Existing users can securely log in to the system
- **Session Management**: Tracks authenticated users using HTTP sessions
- **Password Security**: Uses BCrypt for secure password hashing and verification
- **Database Persistence**: Stores user credentials in a MySQL database
- **Responsive Design**: Modern UI built with Tailwind CSS
- **Dark Mode Support**: Toggle between light and dark themes with localStorage persistence
- **Error Handling**: User-friendly error messages for failed operations
- **Clean Logout**: Session invalidation and secure logout functionality

## 🏗️ Project Structure

```
Advanced_Java_Mini_Project_2/
├── pom.xml                          # Maven configuration file
├── README.md                        # Project documentation
└── src/
    └── main/
        ├── java/com/example/
        │   ├── DatabaseUtil.java    # Database connection utility
        │   ├── LoginServlet.java    # Login request handler
        │   ├── LogoutServlet.java   # Logout request handler
        │   └── RegisterServlet.java # Registration request handler
        └── webapp/
            ├── login.jsp            # Login page
            ├── register.jsp         # Registration page
            ├── welcome.jsp          # Welcome/dashboard page
            ├── css/
            │   └── styles.css       # Custom CSS styles
            └── WEB-INF/
                ├── web.xml          # Deployment descriptor
                └── lib/             # External JAR dependencies
```

## 🔧 Technical Stack

### Backend
- **Language**: Java 11
- **Web Framework**: Java Servlets & JSP (JavaServer Pages)
- **Build Tool**: Maven 3.x
- **Database**: MySQL 8.0
- **Database Driver**: MySQL Connector/J 8.0.28
- **Password Hashing**: jBCrypt 0.4

### Frontend
- **Markup**: HTML5
- **Styling**: Tailwind CSS
- **Scripting**: JavaScript (Vanilla)
- **Features**: Dark mode toggle, responsive layout

### Server Requirements
- Java Application Server (Tomcat 9.0+, JBoss, GlassFish, etc.)
- MySQL Database Server 8.0+

## 📦 Dependencies

### Maven Dependencies
```xml
<!-- Servlet API 4.0 -->
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
</dependency>

<!-- JSP API 2.3 -->
<dependency>
    <groupId>javax.servlet.jsp</groupId>
    <artifactId>javax.servlet.jsp-api</artifactId>
    <version>2.3.3</version>
</dependency>

<!-- MySQL Connector -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.28</version>
</dependency>

<!-- jBCrypt for password hashing -->
<dependency>
    <groupId>org.mindrot</groupId>
    <artifactId>jbcrypt</artifactId>
    <version>0.4</version>
</dependency>
```

## 🚀 Getting Started

### Prerequisites
- Java Development Kit (JDK) 11 or higher
- Maven 3.6 or higher
- MySQL Server 8.0 or higher
- Apache Tomcat 9.0+ (or another Java Application Server)

### Database Setup

1. **Create the database**:
   ```sql
   CREATE DATABASE registration_form;
   ```

2. **Create the users table**:
   ```sql
   USE registration_form;
   
   CREATE TABLE users (
       id INT AUTO_INCREMENT PRIMARY KEY,
       username VARCHAR(50) NOT NULL UNIQUE,
       password VARCHAR(255) NOT NULL,
       created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );
   ```

### Application Configuration

1. **Update database credentials** in [DatabaseUtil.java](src/main/java/com/example/DatabaseUtil.java):
   ```java
   private static final String URL = "jdbc:mysql://localhost:3306/registration_form?useSSL=false";
   private static final String USER = "your_mysql_username";
   private static final String PASSWORD = "your_mysql_password";
   ```

2. **Build the project**:
   ```bash
   mvn clean package
   ```

3. **Deploy the WAR file** to your application server (e.g., Tomcat):
   - Copy `target/login-registration-system-1.0-SNAPSHOT.war` to `$CATALINA_HOME/webapps/`
   - Rename to `login-registration-system.war` (optional)
   - Restart the application server

4. **Access the application**:
   - Navigate to `http://localhost:8080/login-registration-system`

## 🔐 Core Components

### 1. DatabaseUtil.java
**Purpose**: Database connection management utility

**Key Methods**:
- `getConnection()`: Establishes a connection to MySQL database using JDBC

**Features**:
- Loads MySQL JDBC driver dynamically
- Implements connection pooling via DriverManager
- Handles ClassNotFoundException and SQLException

### 2. RegisterServlet.java
**Purpose**: Handles user registration requests

**Functionality**:
- Accepts POST requests with `username` and `password` parameters
- Validates input parameters
- Hashes password using BCrypt (`BCrypt.hashpw()`)
- Inserts new user record into the database
- Redirects to login page on success or shows error on failure

**Security Features**:
- Uses prepared statements to prevent SQL injection
- Implements bcrypt hashing for secure password storage
- Generates random salt for each password hash

### 3. LoginServlet.java
**Purpose**: Handles user authentication

**Functionality**:
- Accepts POST requests with `username` and `password`
- Queries database for user credentials
- Verifies password using BCrypt (`BCrypt.checkpw()`)
- Creates HTTP session if authentication succeeds
- Provides specific error messages for invalid credentials

**Security Features**:
- Uses prepared statements for SQL injection prevention
- BCrypt verification prevents brute-force attacks
- Session-based authentication
- Redirects to welcome page on successful login

### 4. LogoutServlet.java
**Purpose**: Handles user logout

**Functionality**:
- Supports both GET and POST requests
- Invalidates user session
- Redirects to login page
- Clears all session attributes

### JSP Pages

#### login.jsp
- User login form with username and password fields
- Dark mode toggle button
- Error message display
- Link to registration page
- Responsive design with Tailwind CSS

#### register.jsp
- User registration form
- Username and password input fields
- Dark mode toggle functionality
- Error handling and display
- Link back to login page

#### welcome.jsp
- Displays logged-in username
- Shows welcome message
- Logout button
- Session-based authentication check
- Dark mode support

## 🔄 Application Flow

```
User Access
    ↓
[login.jsp] ← ← ← ← ← ← ← ← ← ← [register.jsp]
    ↓                              ↓
LoginServlet (POST)          RegisterServlet (POST)
    ↓                              ↓
Database Query               Database Insert
(Verify credentials)         (Store hashed password)
    ↓                              ↓
   ✓ Valid?                    ✓ Success?
    ↓                              ↓
Create Session               Redirect to Login
    ↓
[welcome.jsp]
    ↓
LogoutServlet (POST)
    ↓
Invalidate Session
    ↓
[login.jsp]
```

## 🔒 Security Features

1. **Password Hashing**: Uses industry-standard BCrypt algorithm
   - Salted hashing prevents rainbow table attacks
   - Adaptive algorithm slows down brute-force attempts
   - Default cost factor of 10+ iterations

2. **SQL Injection Prevention**: Prepared statements used throughout
   - Parameter binding prevents malicious SQL injection
   - Database input sanitization

3. **Session Management**: HTTP session-based authentication
   - Session invalidation on logout
   - Session attributes store user identity

4. **Error Handling**: Generic error messages prevent information disclosure
   - Doesn't reveal if username exists
   - Doesn't expose database errors to users

## 📝 Build & Deployment

### Build the Project
```bash
mvn clean package
```

This generates `login-registration-system-1.0-SNAPSHOT.war` in the `target/` directory.

### Deployment Options

**Option 1: Apache Tomcat**
```bash
cp target/login-registration-system-1.0-SNAPSHOT.war $CATALINA_HOME/webapps/
$CATALINA_HOME/bin/startup.sh
```

**Option 2: Docker Deployment** (Create Dockerfile)
```dockerfile
FROM tomcat:9.0-alpine
COPY target/login-registration-system-1.0-SNAPSHOT.war /usr/local/tomcat/webapps/
EXPOSE 8080
CMD ["catalina.sh", "run"]
```

## 🧪 Testing

### Manual Testing

1. **Register a new user**:
   - Go to registration page
   - Enter username and password
   - Click "Sign Up"
   - Should redirect to login page

2. **Login with valid credentials**:
   - Enter registered username and password
   - Click "Sign In"
   - Should redirect to welcome page showing username

3. **Login with invalid credentials**:
   - Try non-existent username or wrong password
   - Should show appropriate error message

4. **Logout**:
   - Click logout button on welcome page
   - Should redirect to login page
   - Session should be invalidated

## 🌙 Dark Mode Feature

- Toggle button available on all pages (top-right corner)
- Theme preference stored in browser's localStorage
- Persists across sessions
- Uses Tailwind CSS dark mode classes

## 📱 Responsive Design

- Built with Tailwind CSS framework
- Mobile-first approach
- Works seamlessly on:
  - Desktop browsers
  - Tablets
  - Mobile devices

## 🐛 Troubleshooting

### Database Connection Failed
- Verify MySQL server is running
- Check credentials in DatabaseUtil.java
- Ensure database and table exist
- Verify MySQL Connector JAR is in classpath

### Application Server Issues
- Ensure Java version 11+ is installed
- Check application server is running
- Verify WAR file is properly deployed
- Check application server logs for errors

### Password Hashing Issues
- Ensure jBCrypt library is included in deployment
- Verify BCrypt cost factor (default 10+)
- Check password length limits in database schema

## 📄 License

This project is provided as-is for educational and learning purposes.

## 👨‍💻 Development

### Compilation
```bash
mvn compile
```

### Running Tests (if applicable)
```bash
mvn test
```

### Clean Build
```bash
mvn clean
```

## 🎓 Learning Outcomes

This project demonstrates:
- Servlet-based request handling
- JSP template development
- JDBC database connectivity
- Password security best practices
- HTTP session management
- MVC architecture concepts
- Maven project management
- Front-end styling with Tailwind CSS
- JavaScript DOM manipulation

## 📞 Support

For issues or questions:
1. Check application server logs
2. Verify database configuration
3. Ensure all dependencies are properly installed
4. Review error messages displayed on the application

---

**Last Updated**: January 29, 2026
**Java Version**: 11+
**Maven Version**: 3.6+
