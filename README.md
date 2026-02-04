# ITP Identity Service

A comprehensive OAuth2 Authorization Server built with Spring Boot, providing secure authentication and authorization services with OpenID Connect (OIDC) support.

## Overview

ITP Identity Service (IAM Server) is a robust identity and access management solution that implements the OAuth2 authorization framework and OpenID Connect protocol. It provides centralized authentication and authorization services for microservices architecture, enabling secure user management, role-based access control (RBAC), and OAuth2 client management.

## Features

### Core Features
- **OAuth2 Authorization Server**: Full implementation of OAuth2 2.1 specification
- **OpenID Connect Support**: OIDC provider with standardized authentication
- **User Management**: Complete user lifecycle management with profile support
- **Role-Based Access Control (RBAC)**: Fine-grained permission system with roles and permissions
- **Client Management**: OAuth2 client registration and management
- **Custom Login Page**: Thymeleaf-based custom login interface
- **Token Customization**: Customizable JWT tokens with additional claims
- **Service Discovery**: Eureka client integration for microservices architecture
- **Centralized Configuration**: Spring Cloud Config client support

### Security Features
- Password encryption using industry-standard algorithms
- Account status management (expiration, locking, credentials expiration)
- User profile management with personal information
- Secure session management
- OAuth2 authorization consent management

## Technology Stack

### Backend
- **Java 21**: Latest LTS version of Java
- **Spring Boot 4.0.1**: Core framework
- **Spring Security**: Authentication and authorization
- **Spring Security OAuth2 Authorization Server**: OAuth2/OIDC implementation
- **Spring Data JPA**: Database access and ORM
- **Spring Cloud 2025.1.0**: Microservices infrastructure
- **Spring Cloud Netflix Eureka**: Service discovery
- **Spring Cloud Config**: Centralized configuration

### Database
- **PostgreSQL**: Primary relational database
- **Hibernate**: ORM with JPA

### Frontend
- **Thymeleaf**: Server-side template engine for login pages

### Build Tool
- **Gradle**: Build automation and dependency management

### Additional Technologies
- **Lombok**: Reducing boilerplate code
- **JUnit**: Testing framework

## Prerequisites

Before running this application, ensure you have:

- Java 21 or higher installed
- PostgreSQL database server running
- Gradle 8.x or higher (or use the included Gradle wrapper)
- (Optional) Spring Cloud Config Server running on port 8888
- (Optional) Eureka Server for service discovery

## Database Setup

1. Create a PostgreSQL database:
```sql
CREATE DATABASE db_iam;
CREATE USER itpusr WITH PASSWORD 'itp@168';
GRANT ALL PRIVILEGES ON DATABASE db_iam TO itpusr;
```

2. The application uses PostgreSQL on port `16850` by default. Update the connection details in `application.yml` if needed.

## Configuration

The main configuration is located in `src/main/resources/application.yml`:

```yaml
spring:
  application:
    name: iamserver
  datasource:
    url: jdbc:postgresql://localhost:16850/db_iam
    username: itpusr
    password: itp@168
  security:
    oauth2:
      authorizationserver:
        issuer: http://localhost:9090
server:
  port: 9090
```

### Key Configuration Points

- **Server Port**: 9090 (default)
- **Database**: PostgreSQL on port 16850
- **Issuer URI**: http://localhost:9090
- **DDL Auto**: create-drop (automatically recreates schema on startup)
- **Config Server** (optional): http://localhost:8888

## Installation & Running

### Using Gradle Wrapper (Recommended)

1. Clone the repository:
```bash
git clone <repository-url>
cd itp-identity-service
```

2. Build the application:
```bash
./gradlew build
```

3. Run the application:
```bash
./gradlew bootRun
```

### Using JAR file

```bash
./gradlew bootJar
java -jar build/libs/iam-service-0.0.1-SNAPSHOT.jar
```

The application will start on `http://localhost:9090`

## API Endpoints

### OAuth2 Authorization Server Endpoints

- **Authorization Endpoint**: `http://localhost:9090/oauth2/authorize`
- **Token Endpoint**: `http://localhost:9090/oauth2/token`
- **Token Introspection**: `http://localhost:9090/oauth2/introspect`
- **Token Revocation**: `http://localhost:9090/oauth2/revoke`
- **JWK Set**: `http://localhost:9090/oauth2/jwks`
- **OpenID Configuration**: `http://localhost:9090/.well-known/openid-configuration`
- **User Info**: `http://localhost:9090/userinfo`

### Custom Endpoints

- **Login Page**: `http://localhost:9090/login`

## Project Structure

```
src/main/java/istad/makara/identity/
├── config/                    # Configuration classes
│   └── jpa/                   # JPA auditing configuration
├── controller/                # REST and web controllers
├── domain/                    # Entity classes
│   ├── User.java
│   ├── Role.java
│   ├── Permission.java
│   ├── Client.java
│   ├── Authorization.java
│   └── AuthorizationConsent.java
├── features/                  # Feature modules
│   ├── oauth2/               # OAuth2 specific features
│   ├── role/                 # Role management
│   └── user/                 # User management
├── security/                  # Security configuration
│   ├── SecurityConfig.java
│   ├── SecurityBean.java
│   ├── SecurityInit.java
│   ├── UserDetailsServiceImpl.java
│   ├── CustomUserDetails.java
│   └── CustomTokenCustomizer.java
└── ItpIdentityServiceApplication.java
```

## Data Model

### User Entity
- UUID-based identification
- Username, email, and password
- Personal information (names, phone, gender, date of birth)
- Profile and cover images
- Account status flags (enabled, locked, expired)
- Many-to-many relationships with Roles and Permissions

### Role & Permission
- Hierarchical role-based access control
- Users can have multiple roles
- Users can have direct permissions
- Roles contain collections of permissions

### OAuth2 Client
- Client registration information
- Authentication methods
- Authorization grant types
- Redirect URIs and scopes
- Token settings

## Development

### Database Schema Management

The application uses Hibernate with `ddl-auto: create-drop`, which means:
- Database schema is created on application startup
- Schema is dropped on application shutdown
- **Warning**: This is suitable for development only. Change to `validate` or `update` for production.

### Logging

Debug logging is enabled for:
- Spring Security: DEBUG level
- OAuth2 Authorization Server: TRACE level
- OIDC Authentication: TRACE level

Configure logging in `application.yml` as needed.

## Integration with Microservices

This identity service is designed to work within a microservices ecosystem:

1. **Eureka Service Discovery**: Automatically registers with Eureka server
2. **Spring Cloud Config**: Retrieves configuration from config server
3. **OAuth2 Resource Servers**: Other services can validate tokens issued by this server

## Security Considerations

- Always use HTTPS in production environments
- Change default database credentials
- Use strong password encoding (configured in SecurityBean)
- Implement proper token expiration policies
- Enable CSRF protection for production
- Configure CORS policies appropriately
- Store sensitive configuration in environment variables or secret management systems

## Testing

Run tests using:
```bash
./gradlew test
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Create a Pull Request

## License

[Add your license information here]

## Contact & Support

For questions or support, please contact the development team.

## Acknowledgments

Built with Spring Boot and Spring Security OAuth2 Authorization Server.
