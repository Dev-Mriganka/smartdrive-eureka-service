# SmartDrive Eureka Server

A Spring Cloud Netflix Eureka Server for service discovery and registration in the SmartDrive microservices ecosystem.

## Overview

This Eureka Server acts as a service registry that enables microservices to discover and communicate with each other without hardcoding hostnames and ports. It's a crucial component of the SmartDrive platform's microservices architecture.

## Features

- **Service Discovery**: Automatic registration and discovery of microservices
- **Health Monitoring**: Continuous health checking of registered services
- **Load Balancing**: Enables client-side load balancing through service registry
- **Fault Tolerance**: Self-preservation mode during network partitions
- **Web Dashboard**: Built-in web interface for monitoring registered services

## Technology Stack

- **Java 21**
- **Spring Boot 3.2.x**
- **Spring Cloud Netflix Eureka Server**
- **Gradle** for build management
- **Docker** for containerization

## Configuration

### Application Properties

The server is configured through `application.yml`:

```yaml
server:
  port: 8761

eureka:
  instance:
    hostname: eureka-server
  client:
    register-with-eureka: false
    fetch-registry: false
    service-url:
      defaultZone: http://eureka-server:8761/eureka/
```

### Environment Variables

- `SERVER_PORT`: Server port (default: 8761)
- `EUREKA_HOSTNAME`: Eureka instance hostname
- `EUREKA_SERVICE_URL`: Default zone URL

## Running the Application

### Local Development

```bash
# Using Gradle
./gradlew bootRun

# Using Java
./gradlew build
java -jar build/libs/eureka-server-0.0.1-SNAPSHOT.jar
```

### Docker

```bash
# Build the image
docker build -t smartdrive/eureka-server .

# Run the container
docker run -p 8761:8761 smartdrive/eureka-server
```

### Docker Compose

The Eureka Server is part of the SmartDrive Docker Compose setup:

```bash
docker-compose up eureka-server
```

## Accessing the Dashboard

Once running, access the Eureka dashboard at:
- **Local**: http://localhost:8761
- **Docker**: http://eureka-server:8761

## Registered Services

The following SmartDrive services register with this Eureka Server:

- **auth-service**: Authentication and authorization service
- **user-service**: User management service
- **api-gateway**: API Gateway and routing service
- **file-storage-service**: File upload/download service
- **file-indexing-service**: File indexing and metadata service
- **search-service**: Search functionality service
- **ai-service**: AI-powered features service

## Health Check

Health check endpoint: `GET /actuator/health`

## Development

### Building

```bash
./gradlew clean build
```

### Testing

```bash
./gradlew test
```

### Code Style

The project follows standard Java/Spring Boot conventions.

## Deployment

### Production Considerations

- Configure appropriate memory settings for JVM
- Set up proper logging configuration
- Use external configuration for environment-specific settings
- Ensure network connectivity between services and Eureka Server
- Configure security if needed (basic auth, SSL)

### Environment Variables for Production

```bash
SPRING_PROFILES_ACTIVE=prod
SERVER_PORT=8761
EUREKA_HOSTNAME=your-eureka-hostname
```

## Monitoring

- Built-in metrics via Spring Boot Actuator
- Eureka dashboard shows all registered services
- Health checks for all registered instances

## Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Auth Service  │    │   User Service  │    │  API Gateway    │
│                 │    │                 │    │                 │
└─────────┬───────┘    └─────────┬───────┘    └─────────┬───────┘
          │                      │                      │
          │                      │                      │
          └──────────────────────┼──────────────────────┘
                                 │
                    ┌─────────────┴─────────────┐
                    │    Eureka Server          │
                    │   (Service Registry)      │
                    └───────────────────────────┘
```

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is part of the SmartDrive platform.

## Support

For issues and questions, please refer to the main SmartDrive repository.
