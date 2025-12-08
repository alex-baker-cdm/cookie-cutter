# Cookie Cutter OpenAPI/Swagger Documentation

This directory contains OpenAPI 3.0 specifications for the HTTP-based APIs provided by the Cookie Cutter microservices framework.

## Overview

Cookie Cutter is an opinionated framework for building event-driven and request/response based microservices. While the framework primarily focuses on message-based communication (Kafka, AMQP, MQTT, etc.), it also exposes several HTTP-based APIs for monitoring, administration, and integration purposes.

## API Documentation

### HTTP-based APIs

| API | File | Description |
|-----|------|-------------|
| Prometheus Metrics | [prometheus-metrics-api.yaml](./prometheus-metrics-api.yaml) | Exposes application metrics in Prometheus text format for monitoring and alerting |
| Kubernetes Admission Controller | [kubernetes-admission-controller-api.yaml](./kubernetes-admission-controller-api.yaml) | Webhook endpoints for Kubernetes admission control (validating and mutating webhooks) |
| gRPC Service | [grpc-service-api.yaml](./grpc-service-api.yaml) | High-performance RPC communication using Protocol Buffers over HTTP/2 |

### Non-HTTP APIs (Message-based)

The following APIs use message-based protocols and are not documented in OpenAPI format. Refer to the main documentation for configuration and usage details.

| Package | Protocol | Description |
|---------|----------|-------------|
| `@walmartlabs/cookie-cutter-kafka` | Kafka | Apache Kafka message source and sink |
| `@walmartlabs/cookie-cutter-amqp` | AMQP | RabbitMQ/AMQP message source and sink |
| `@walmartlabs/cookie-cutter-mqtt` | MQTT | MQTT message source and sink |
| `@walmartlabs/cookie-cutter-redis` | Redis | Redis client and stream source/sink |
| `@walmartlabs/cookie-cutter-azure` | Azure | Azure Queue, Blob Storage, and Cosmos DB clients |
| `@walmartlabs/cookie-cutter-gcp` | GCP | Google Pub/Sub, GCS, and BigQuery clients |
| `@walmartlabs/cookie-cutter-s3` | S3 | AWS S3 client and sink |
| `@walmartlabs/cookie-cutter-mssql` | MSSQL | Microsoft SQL Server sink |

## Using the Documentation

### Viewing with Swagger UI

You can view these OpenAPI specifications using Swagger UI:

1. **Online Swagger Editor**: Visit [editor.swagger.io](https://editor.swagger.io) and paste the YAML content
2. **Local Swagger UI**: Use Docker to run Swagger UI locally:
   ```bash
   docker run -p 8080:8080 -e SWAGGER_JSON=/docs/prometheus-metrics-api.yaml -v $(pwd):/docs swaggerapi/swagger-ui
   ```

### Generating Client Code

Use the OpenAPI Generator to create client libraries:

```bash
# Install OpenAPI Generator
npm install @openapitools/openapi-generator-cli -g

# Generate TypeScript client
openapi-generator-cli generate -i prometheus-metrics-api.yaml -g typescript-fetch -o ./generated/prometheus-client
```

## API Details

### Prometheus Metrics API

The Prometheus package (`@walmartlabs/cookie-cutter-prometheus`) exposes an HTTP endpoint for Prometheus to scrape metrics.

**Default Configuration:**
- Port: 3000
- Endpoint: `/metrics`
- Format: Prometheus text exposition format

**Metric Types:**
- Counters: Cumulative metrics (e.g., request counts)
- Gauges: Point-in-time values (e.g., active connections)
- Histograms: Distribution of values with configurable buckets

### Kubernetes Admission Controller API

The Kubernetes package (`@walmartlabs/cookie-cutter-kubernetes`) provides admission controller functionality for Kubernetes clusters.

**Default Configuration:**
- Port: 443 (HTTPS)
- Paths: Configurable via `requestPaths`
- Authentication: Mutual TLS with Kubernetes API server

**Supported Operations:**
- CREATE: Validate/mutate new resources
- UPDATE: Validate/mutate resource modifications
- DELETE: Validate resource deletions
- CONNECT: Validate connection requests

### gRPC Service API

The gRPC package (`@walmartlabs/cookie-cutter-grpc`) enables high-performance RPC communication.

**Default Configuration:**
- Host: localhost
- Port: Configurable
- Protocol: gRPC over HTTP/2
- Encoding: Protocol Buffers

**Features:**
- Unary RPC
- Server streaming
- Client streaming
- Bidirectional streaming
- Automatic retry with exponential backoff

## Related Documentation

- [Cookie Cutter Main Documentation](https://walmartlabs.github.io/cookie-cutter)
- [Prometheus Module](https://walmartlabs.github.io/cookie-cutter/docs/Module_Prometheus)
- [gRPC Module](https://walmartlabs.github.io/cookie-cutter/docs/Module_Grpc)
- [Kubernetes Integration](https://kubernetes.io/docs/reference/access-authn-authz/extensible-admission-controllers/)

## Contributing

When adding new HTTP-based APIs to Cookie Cutter, please update this documentation:

1. Create a new OpenAPI specification file in this directory
2. Follow the existing naming convention: `{api-name}-api.yaml`
3. Update this README with the new API details
4. Ensure the specification validates against OpenAPI 3.0

## License

This documentation is licensed under the Apache 2.0 License. See [LICENSE](../../LICENSE) for details.
