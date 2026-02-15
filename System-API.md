# System API

The System API provides endpoints for health monitoring, configuration, and system information.

## Base URL

System endpoints are available at various paths under `/api`

---

## Health Monitoring

### GET /api/health

Get the overall health status of the Source Saver application.

**Response:**

```json
{
  "status": "healthy",
  "timestamp": "2024-01-15T16:30:00Z",
  "uptime": 3600,
  "version": "1.0.0",
  "environment": "production"
}
```

**Status Codes:**

- `200` - System is healthy
- `503` - System is unhealthy

---

### GET /api/ready

Get the readiness status of the application (useful for load balancers and orchestrators).

**Response:**

```json
{
  "status": "ready",
  "timestamp": "2024-01-15T16:30:00Z",
  "checks": {
    "obsConnected": true,
    "sourceManagerInitialized": true,
    "databaseConnected": true
  }
}
```

**Status Codes:**

- `200` - Application is ready to serve requests
- `503` - Application is not ready (still initializing or has critical issues)

---

## Configuration

### GET /api/config

Get the current application configuration and feature flags.

**Response:**

```json
{
  "serverUrl": "http://localhost:3000",
  "obsConnected": true,
  "features": {
    "memoryOptimization": true,
    "sourcePreloading": true,
    "maxConcurrentSources": 10,
    "performanceMonitoring": true,
    "webSocketEnabled": true
  },
  "limits": {
    "maxSourceWidth": 7680,
    "maxSourceHeight": 4320,
    "maxRefreshRate": 60,
    "maxSourcesPerScene": 20
  },
  "environment": {
    "nodeEnv": "production",
    "denoVersion": "1.40.0",
    "platform": "win32"
  }
}
```

**Status Codes:**

- `200` - Success
- `503` - OBS Manager not initialized

---

## Performance Metrics

### GET /api/performance/metrics

Get comprehensive performance metrics for the entire application.

**Query Parameters:**

- `hours` (optional): Number of hours of history to retrieve (default: 1, max: 24)
- `detailed` (optional): Include detailed breakdown (`true`/`false`, default: `false`)

**Response:**

```json
{
  "timestamp": "2024-01-15T16:30:00Z",
  "system": {
    "cpu": {
      "usage": 15.2,
      "cores": 8,
      "model": "Intel Core i7-9700K"
    },
    "memory": {
      "usage": 512.5,
      "total": 16384.0,
      "percentage": 3.1
    },
    "uptime": 3600
  },
  "application": {
    "activeConnections": 5,
    "totalRequests": 1250,
    "averageResponseTime": 45.2,
    "errorRate": 0.02
  },
  "database": {
    "connections": 3,
    "totalConnections": 15,
    "currentConnections": 3,
    "queryTime": 12.5
  },
  "webSocket": {
    "connections": 2,
    "totalConnections": 8,
    "currentConnections": 2,
    "messagesPerSecond": 5.2
  },
  "sources": {
    "total": 12,
    "active": 4,
    "averageCpuUsage": 8.3,
    "averageMemoryUsage": 125.6,
    "averageRenderTime": 16.7
  },
  "obs": {
    "connected": true,
    "fps": 60.0,
    "renderTime": 8.3,
    "skippedFrames": 0,
    "totalFrames": 216000
  }
}
```

**Status Codes:**

- `200` - Success

---

### GET /api/performance/history

Get historical performance data for trend analysis.

**Query Parameters:**

- `hours` (optional): Number of hours of history (default: 24, max: 168)
- `interval` (optional): Data point interval in minutes (default: 5, options: 1, 5, 15, 30, 60)

**Response:**

```json
{
  "timeRange": {
    "start": "2024-01-14T16:30:00Z",
    "end": "2024-01-15T16:30:00Z",
    "interval": 5,
    "dataPoints": 288
  },
  "metrics": [
    {
      "timestamp": "2024-01-15T16:30:00Z",
      "cpu": 15.2,
      "memory": 512.5,
      "activeSources": 4,
      "responseTime": 45.2
    },
    {
      "timestamp": "2024-01-15T16:25:00Z",
      "cpu": 14.8,
      "memory": 508.1,
      "activeSources": 4,
      "responseTime": 43.7
    }
  ],
  "summary": {
    "averageCpu": 15.0,
    "averageMemory": 510.0,
    "averageResponseTime": 44.5,
    "peakCpu": 18.5,
    "peakMemory": 520.2
  }
}
```

**Status Codes:**

- `200` - Success

---

## System Information

### GET /api/system/info

Get detailed system information and runtime details.

**Response:**

```json
{
  "application": {
    "name": "Source Saver",
    "version": "1.0.0",
    "startTime": "2024-01-15T15:30:00Z",
    "uptime": 3600,
    "environment": "production"
  },
  "runtime": {
    "deno": {
      "version": "1.40.0",
      "v8Version": "12.1.285.6",
      "typescriptVersion": "5.3.3"
    },
    "platform": {
      "os": "windows",
      "arch": "x64",
      "version": "10.0.22631"
    }
  },
  "features": {
    "webSocketSupport": true,
    "compressionEnabled": true,
    "rateLimitingEnabled": true,
    "errorTrackingEnabled": true,
    "performanceMonitoringEnabled": true
  },
  "configuration": {
    "port": 3000,
    "maxConcurrentSources": 10,
    "performanceMonitoringInterval": 30000,
    "metricsRetentionDays": 7
  }
}
```

**Status Codes:**

- `200` - Success

---

### GET /api/system/logs

Get recent application logs (requires appropriate permissions).

**Query Parameters:**

- `level` (optional): Log level filter (`error`, `warn`, `info`, `debug`)
- `limit` (optional): Number of log entries to return (default: 100, max: 1000)
- `since` (optional): ISO timestamp to get logs since that time

**Response:**

```json
{
  "logs": [
    {
      "timestamp": "2024-01-15T16:30:00Z",
      "level": "info",
      "message": "Source activated successfully",
      "context": {
        "sourceId": "source-123-abc",
        "sourceName": "StreamLabs Donations"
      }
    },
    {
      "timestamp": "2024-01-15T16:29:45Z",
      "level": "warn",
      "message": "High CPU usage detected",
      "context": {
        "cpuUsage": 85.2,
        "threshold": 80.0
      }
    }
  ],
  "total": 2,
  "hasMore": false
}
```

**Status Codes:**

- `200` - Success
- `403` - Insufficient permissions (in production)

---

## Maintenance Operations

### POST /api/system/gc

Trigger garbage collection (development/testing only).

**Response:**

```json
{
  "success": true,
  "message": "Garbage collection triggered",
  "memoryBefore": 512.5,
  "memoryAfter": 487.2,
  "freedMemory": 25.3,
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**Status Codes:**

- `200` - Garbage collection completed
- `403` - Not available in production
- `503` - Operation failed

---

### POST /api/system/cache/clear

Clear application caches.

**Request Body (Optional):**

```json
{
  "cacheTypes": ["source-content", "performance-metrics", "obs-scenes"]
}
```

**Response:**

```json
{
  "success": true,
  "message": "Caches cleared successfully",
  "clearedCaches": [
    "source-content",
    "performance-metrics",
    "obs-scenes"
  ],
  "freedMemory": 15.7,
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**Status Codes:**

- `200` - Caches cleared successfully
- `400` - Invalid cache type specified

---

## Error Responses

### Service Unavailable

```json
{
  "error": "Service temporarily unavailable",
  "retryAfter": 30
}
```

### System Overloaded

```json
{
  "error": "System is currently overloaded. Please try again later.",
  "currentLoad": 95.2,
  "threshold": 90.0
}
```

### Maintenance Mode

```json
{
  "error": "System is in maintenance mode",
  "estimatedDuration": 300,
  "message": "Scheduled maintenance in progress"
}
```

---

## WebSocket Events

System-level events are broadcast to all connected WebSocket clients:

### Performance Alert

```json
{
  "event": "system:performanceAlert",
  "data": {
    "type": "high_cpu_usage",
    "value": 85.2,
    "threshold": 80.0,
    "severity": "warning",
    "timestamp": "2024-01-15T16:30:00Z"
  }
}
```

### System Status Change

```json
{
  "event": "system:statusChange",
  "data": {
    "previousStatus": "healthy",
    "currentStatus": "degraded",
    "reason": "High memory usage",
    "timestamp": "2024-01-15T16:30:00Z"
  }
}
```

### Maintenance Mode

```json
{
  "event": "system:maintenanceMode",
  "data": {
    "enabled": true,
    "estimatedDuration": 300,
    "message": "Scheduled maintenance starting",
    "timestamp": "2024-01-15T16:30:00Z"
  }
}
```

---

## Monitoring Best Practices

### Health Checks

- Use `/api/health` for basic health monitoring
- Use `/api/ready` for readiness probes in container orchestrators
- Monitor both endpoints with different intervals (health: 30s, ready: 10s)

### Performance Monitoring

- Regularly check `/api/performance/metrics` for system health
- Set up alerts for high CPU/memory usage
- Monitor response times and error rates

### Log Management

- Use appropriate log levels in production
- Implement log rotation and retention policies
- Monitor error logs for application issues

---

**Back to**: [API Documentation Home](Home)
