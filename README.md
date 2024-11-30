# Observability
Node.js backend observability project using OpenTelemetry and Jaeger for tracing, metrics, and logs.


# Overview

This repository demonstrates the implementation of observability in a Node.js application using **OpenTelemetry** and **Jaeger**. It focuses on backend tracing, collecting telemetry data (traces, logs, and metrics), and exporting it to a tracing backend for monitoring and visualization.

## Technologies Used
- **Node.js**: Backend runtime environment.
- **Express**: API framework.
- **MongoDB**: Database for storing application data.
- **OpenTelemetry**: Observability framework.
- **Jaeger**: Tracing and monitoring backend.

## Objective
- Implement observability in a Node.js backend application.
- Configure OpenTelemetry with Jaeger for tracing.
- Analyze telemetry data to gain insights into system performance and diagnose issues.

## Prerequisites

1. **Docker**: Ensure Docker is installed and running on your system.
2. **Node.js**: Install Node.js if it's not already available on your machine.
3. **npm**: Node.js package manager for dependency installation.

## Setup Instructions

### 1. Clone the Repository
```
git clone https://github.com/OzPol/observability.git
cd <repository-folder>
```

### 2. Install Dependencies
```
npm install
```

### 3. Run MongoDB and Jaeger in Docker

#### Run MongoDB:
```
docker run -d -p 27017:27017 mongo
```

#### Run Jaeger:

##### For Bash (Linux/Mac/Windows with WSL)
```
docker run -d --name jaeger \
  -e COLLECTOR_ZIPKIN_HOST_PORT=:9411 \
  -p 5775:5775/udp \
  -p 6831:6831/udp \
  -p 6832:6832/udp \
  -p 5778:5778 \
  -p 16686:16686 \
  -p 14250:14250 \
  -p 14268:14268 \
  -p 14269:14269 \
  -p 9411:9411 \
  jaegertracing/all-in-one:1.32
```

##### For PowerShell (Windows)
```
docker run -d --name jaeger `
  -e COLLECTOR_ZIPKIN_HOST_PORT=:9411 `
  -p 5775:5775/udp `
  -p 6831:6831/udp `
  -p 6832:6832/udp `
  -p 5778:5778 `
  -p 16686:16686 `
  -p 14250:14250 `
  -p 14268:14268 `
  -p 14269:14269 `
  -p 9411:9411 `
  jaegertracing/all-in-one:1.32
```

### 4. Start the Application
Run the Node.js server:
```
node index.js
```

### 5. Verify API Endpoints
Test the API endpoints using `curl`:
- **Get all todos**:
```
curl http://localhost:3000/todo
```
- **Get a specific todo**:
```
curl http://localhost:3000/todo/1
```

### 6. View Traces in Jaeger
Open Jaeger in your browser:
```
http://localhost:16686
```

Select `todo-service` from the dropdown and analyze traces.

## Project Structure
- `index.js`: Main server file that sets up the Express app, MongoDB connection, and API endpoints.
- `tracing.js`: Configures OpenTelemetry for the project, integrating with Jaeger, MongoDB, and Express for distributed tracing.

## How It Works
1. The application is instrumented with OpenTelemetry to collect telemetry data (traces, metrics, and logs).
2. Each API request is captured as a **trace**, consisting of spans representing individual operations:
   - **HTTP requests** are traced using the `opentelemetry-instrumentation-http` module.
   - **MongoDB queries** are traced using the `opentelemetry-instrumentation-mongodb` module.