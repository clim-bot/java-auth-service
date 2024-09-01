# Java Auth Service

`java-auth-service` is a Spring Boot application that provides authentication services using JWT tokens. It supports user registration, login, and health check functionality, and stores user information in a MySQL database.

## Features

- User registration with unique usernames
- User login with JWT token generation
- JWT-based authentication for protected routes
- Health check endpoint to verify service status
- Integration with MySQL for persistent user storage

## Prerequisites

- Java 17 or later
- Maven 3.6.0 or later
- MySQL Server
- `jq` for JSON processing (optional, for command-line use)

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/java-auth-service.git
cd java-auth-service
```

### 2.Set Up MySQL Database
1. Start MySQL Server: Ensure that your MySQL server is running.
2. Create Database and User:
```
Log in to MySQL:
mysql -u root -p

Run the following SQL commands to create a new database and user:
CREATE DATABASE java_auth_db;
CREATE USER 'java_auth_user'@'localhost' IDENTIFIED BY 'yourpassword';
GRANT ALL PRIVILEGES ON java_auth_db.* TO 'java_auth_user'@'localhost';
FLUSH PRIVILEGES;
```
3. Configure Application Properties
   Edit the src/main/resources/application.properties file to set up database connection details:

```
spring.datasource.url=jdbc:mysql://localhost:3306/java_auth_db
spring.datasource.username=java_auth_user
spring.datasource.password=yourpassword
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.jpa.defer-datasource-initialization=true
jwt.secret=your_jwt_secret
```

4. Build and Run the Application
```
Use Maven to build and run the application:
mvn clean install
mvn spring-boot:run
```
The application will start on http://localhost:8080.

## Usage
### Register a New User
```
curl -X POST http://localhost:8080/auth/register \
     -H "Content-Type: application/json" \
     -d '{"username": "testuser", "password": "testpassword"}'
```

### Login a User
```
curl -X POST http://localhost:8080/auth/login \
     -H "Content-Type: application/json" \
     -d '{"username": "testuser", "password": "testpassword"}'
```

### Health Check
```
curl -X GET http://localhost:8080/auth/health
```

### Get All Users (Protected Route)
Step 1: Obtain JWT Token
```
TOKEN=$(curl -X POST http://localhost:8080/auth/login \
     -H "Content-Type: application/json" \
     -d '{"username": "testuser", "password": "testpassword"}' | jq -r '.token')
```

Step 2: Use Token to Access Protected Route
```
curl -X GET http://localhost:8080/users \
     -H "Authorization: Bearer $TOKEN"
```

## Running Tests
To run the unit and integration tests, use:
```
mvn test
```

## Troubleshooting
- Error: zsh: command not found: jq: Install jq using brew install jq on macOS, sudo apt-get install jq on Ubuntu/Debian, or other package managers for different systems.
- Database Connection Issues: Ensure MySQL server is running, and the credentials in application.properties are correct.
- JWT Secret: Make sure to set a strong secret for JWT token generation in application.properties.

## Additional Notes
- This application is set up with basic JWT authentication. For production use, consider additional security measures such as HTTPS, rate limiting, and input validation.
- Customize the database initialization strategy (spring.jpa.hibernate.ddl-auto) depending on your deployment needs (e.g., update, validate, create, none).