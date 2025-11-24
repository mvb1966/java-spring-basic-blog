# Monitoring with Grafana and Prometheus

This project includes a complete monitoring setup using Grafana and Prometheus for the Spring Boot blog application.

## Overview

The monitoring stack consists of:
- **Spring Boot Actuator**: Exposes application metrics endpoints
- **Micrometer**: Collects and exports metrics in Prometheus format
- **Prometheus**: Scrapes and stores metrics from the application
- **Grafana**: Visualizes metrics with pre-configured dashboards

## Quick Start

### Prerequisites
- Docker and Docker Compose installed
- Ports 3000 (Grafana), 8080 (Blog App), and 9090 (Prometheus) available

### Starting the Monitoring Stack

1. Build and start all services:
   ```bash
   docker-compose up -d
   ```

2. Wait for all services to start (approximately 30-60 seconds)

3. Access the services:
   - **Blog Application**: http://localhost:8080
   - **Grafana Dashboard**: http://localhost:3000
   - **Prometheus**: http://localhost:9090
   - **Actuator Metrics**: http://localhost:8080/actuator/prometheus

### Grafana Access

- **URL**: http://localhost:3000
- **Username**: `admin`
- **Password**: `admin`

The dashboard "Spring Boot Blog Dashboard" will be automatically provisioned and available.

## Dashboard Panels

The pre-configured dashboard includes:

1. **Application Uptime**: Shows how long the application has been running
2. **HTTP Request Rate**: Real-time request rate per endpoint
3. **HTTP Request Duration**: Average response time per endpoint
4. **JVM Memory Usage**: Memory consumption by different JVM areas
5. **CPU Usage**: Application and system CPU usage
6. **JVM Threads**: Number of live and daemon threads

## Metrics Endpoints

The application exposes the following actuator endpoints:

- `/actuator/health` - Application health status
- `/actuator/info` - Application information
- `/actuator/prometheus` - Prometheus-formatted metrics
- `/actuator/metrics` - Available metrics list

## Customization

### Adding Custom Metrics

You can add custom metrics in your Spring Boot application using Micrometer:

```java
@Component
public class CustomMetrics {
    private final Counter customCounter;
    
    public CustomMetrics(MeterRegistry registry) {
        this.customCounter = Counter.builder("custom_metric")
            .description("My custom metric")
            .register(registry);
    }
    
    public void incrementCounter() {
        customCounter.increment();
    }
}
```

### Modifying Dashboards

1. Access Grafana at http://localhost:3000
2. Navigate to the dashboard
3. Click the gear icon (⚙️) to edit
4. Add or modify panels as needed
5. Save the dashboard
6. Export the JSON and save it to `monitoring/grafana/dashboards/`

### Prometheus Configuration

Edit `monitoring/prometheus.yml` to:
- Change scrape intervals
- Add new scrape targets
- Configure alerting rules

## Stopping the Stack

To stop all services:
```bash
docker-compose down
```

To stop and remove all data volumes:
```bash
docker-compose down -v
```

## Troubleshooting

### Services Not Starting

Check the logs for individual services:
```bash
docker-compose logs blog-app
docker-compose logs prometheus
docker-compose logs grafana
```

### No Data in Grafana

1. Verify the application is running: http://localhost:8080
2. Check Prometheus targets: http://localhost:9090/targets
3. Ensure the blog-app target shows as "UP"
4. Check actuator endpoint: http://localhost:8080/actuator/prometheus

### Dashboard Not Loading

1. Check that dashboard file exists in `monitoring/grafana/dashboards/`
2. Verify provisioning configuration in `monitoring/grafana/provisioning/`
3. Restart Grafana: `docker-compose restart grafana`

## Production Considerations

For production deployments, consider:

1. **Security**: Change default Grafana credentials
2. **Persistence**: Use named volumes or external storage
3. **High Availability**: Run multiple Prometheus/Grafana instances
4. **Alerting**: Configure Prometheus alerting rules
5. **Retention**: Adjust Prometheus data retention policies
6. **Access Control**: Implement proper authentication and authorization
