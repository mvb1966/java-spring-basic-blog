# Spring Boot Blog Application

A simple blog application built with Spring Boot, featuring monitoring with Grafana and Prometheus.

## Features

- Blog post listing and viewing
- H2 in-memory database
- Thymeleaf templates for views
- Spring Boot Actuator for health checks and metrics
- Prometheus metrics export
- Grafana dashboard for monitoring

## Quick Start

### Running the Application

#### Option 1: Using Maven
```bash
./mvnw spring-boot:run
```

The application will be available at http://localhost:8080

#### Option 2: Using Docker Compose (Recommended for monitoring)
```bash
docker compose up -d
```

This will start:
- Blog Application (http://localhost:8080)
- Prometheus (http://localhost:9090)
- Grafana (http://localhost:3000)

## Monitoring

The application includes a complete monitoring stack with Grafana and Prometheus.

### Accessing the Monitoring Stack

1. **Grafana Dashboard**: http://localhost:3000
   - Username: `admin`
   - Password: `admin`
   - Pre-configured dashboard: "Spring Boot Blog Dashboard"

2. **Prometheus**: http://localhost:9090
   - View metrics and targets

3. **Application Metrics**: http://localhost:8080/actuator/prometheus
   - Raw Prometheus-formatted metrics

### Available Metrics

The dashboard includes panels for:
- Application uptime
- HTTP request rate and duration
- JVM memory usage
- CPU usage (application and system)
- JVM thread count
- Database connection pool metrics

For more details, see [MONITORING.md](MONITORING.md)

## Building

```bash
./mvnw clean package
```

## Testing

```bash
./mvnw test
```

## Tech Stack

- Spring Boot 2.1.9
- Spring Data JPA
- H2 Database
- Thymeleaf
- Spring Boot Actuator
- Micrometer (Prometheus)
- Maven
