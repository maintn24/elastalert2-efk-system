# Alert Application

A comprehensive logging and alerting system built with Docker Compose that monitors web application logs and sends email alerts when specific conditions are met.

## Architecture

This application consists of the following components:

- **HTTPD Web Server**: Serves as the web application generating logs
- **Fluentd**: Log aggregation and forwarding service
- **Elasticsearch**: Log storage and search engine
- **Kibana**: Log visualization and analytics dashboard
- **ElastAlert**: Real-time alerting system for Elasticsearch

## Features

- Real-time log collection from web applications
- Centralized log storage in Elasticsearch
- Visual log analysis with Kibana
- Automated email alerts based on log patterns
- Configurable alert rules and thresholds

## Prerequisites

- Docker and Docker Compose installed
- Gmail account with App Password for email notifications

## Configuration

### Email Setup

1. Create a Gmail App Password:
   - Go to your Google Account settings
   - Enable 2-factor authentication
   - Generate an App Password for this application

2. Update the email configuration in the elastalert files:
   - Edit `elastalert/smtp_auth.yaml` with your sender Gmail credentials
   - Edit `elastalert/config.yaml` to set your sender email
   - Edit `elastalert/rules/httpd_error.yaml` to set recipient email

## Quick Start

1. Clone the repository and navigate to the project directory

2. Copy the environment file:
   ```bash
   cp .env.example .env
   ```

3. Configure your email settings (see Configuration section above)

4. Start the application:
   ```bash
   docker-compose up -d
   ```

5. Access the services:
   - Web Application: http://localhost:8080
   - Kibana Dashboard: http://localhost:5601
   - Elasticsearch: http://localhost:9200

## Alert Rules

The application includes a sample alert rule that triggers when:
- 3 or more error log entries are detected within 1 minute
- The log contains the term "error"

You can customize alert rules by editing files in the `elastalert/rules/` directory.

## File Structure

```
.
├── docker-compose.yaml          # Main Docker Compose configuration
├── elastalert/
│   ├── config.yaml            # ElastAlert main configuration
│   ├── smtp_auth.yaml         # Email authentication
│   └── rules/
│       └── httpd_error.yaml   # Alert rule for HTTP errors
├── fluentd/
│   ├── Dockerfile             # Custom Fluentd image
│   └── conf/
│       └── fluent.conf        # Fluentd configuration
└── README.md                  # This file
```

## Monitoring and Troubleshooting

- Check container logs: `docker-compose logs [service-name]`
- Verify Elasticsearch health: `curl http://localhost:9200/_cluster/health`
- Monitor alert status through Kibana dashboards

## Stopping the Application

```bash
docker-compose down
```

To remove all volumes and data:
```bash
docker-compose down -v
```

## Customization

- Add new alert rules in `elastalert/rules/`
- Modify log parsing in `fluentd/conf/fluent.conf`
- Adjust Elasticsearch settings in `docker-compose.yml`
- Configure additional log sources by updating the logging drivers