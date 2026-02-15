# WebSocket API

Source Saver provides real-time communication through WebSocket connections for live updates and bidirectional
communication.

## Connection

### WebSocket URL

```
ws://localhost:3000
```

### Connection Example (JavaScript)

```javascript
const socket = new WebSocket('ws://localhost:3000');

socket.onopen = function (event) {
  console.log('Connected to Source Saver WebSocket');
};

socket.onmessage = function (event) {
  const message = JSON.parse(event.data);
  console.log('Received:', message);
};

socket.onclose = function (event) {
  console.log('WebSocket connection closed');
};

socket.onerror = function (error) {
  console.error('WebSocket error:', error);
};
```

---

## Message Format

All WebSocket messages follow a consistent JSON format:

```json
{
  "event": "event_name",
  "data": {
    // Event-specific data
  },
  "timestamp": "2024-01-15T16:30:00Z",
  "id": "msg-123-abc"
}
```

**Fields:**

- `event` - The event type identifier
- `data` - Event-specific payload
- `timestamp` - ISO 8601 timestamp when the event occurred
- `id` - Unique message identifier

---

## Client-to-Server Events

### Subscribe to Events

Subscribe to specific event types to receive targeted updates.

**Send:**

```json
{
  "action": "subscribe",
  "events": ["sources:*", "obs:sceneChanged", "performance:update"]
}
```

**Response:**

```json
{
  "event": "subscription:confirmed",
  "data": {
    "subscribedEvents": ["sources:*", "obs:sceneChanged", "performance:update"],
    "subscriptionId": "sub-456-def"
  }
}
```

### Unsubscribe from Events

Unsubscribe from specific event types.

**Send:**

```json
{
  "action": "unsubscribe",
  "events": ["performance:update"]
}
```

**Response:**

```json
{
  "event": "subscription:updated",
  "data": {
    "subscribedEvents": ["sources:*", "obs:sceneChanged"],
    "unsubscribedEvents": ["performance:update"]
  }
}
```

### Ping/Pong

Keep the connection alive and measure latency.

**Send:**

```json
{
  "action": "ping",
  "timestamp": "2024-01-15T16:30:00Z"
}
```

**Response:**

```json
{
  "event": "pong",
  "data": {
    "clientTimestamp": "2024-01-15T16:30:00Z",
    "serverTimestamp": "2024-01-15T16:30:00.123Z"
  }
}
```

---

## Server-to-Client Events

### Source Events

#### Source Created

```json
{
  "event": "sources:create",
  "data": {
    "source": {
      "id": "source-789-ghi",
      "name": "New Stream Alert",
      "url": "https://streamlabs.com/widgets/alertbox",
      "width": 800,
      "height": 600,
      "isActive": false
    }
  },
  "timestamp": "2024-01-15T16:30:00Z"
}
```

#### Source Updated

```json
{
  "event": "sources:update",
  "data": {
    "source": {
      "id": "source-123-abc",
      "name": "Updated Stream Alert",
      "url": "https://streamlabs.com/widgets/alertbox",
      "width": 1000,
      "height": 700,
      "isActive": true
    },
    "changes": ["name", "width", "height", "isActive"]
  },
  "timestamp": "2024-01-15T16:31:00Z"
}
```

#### Source Activated

```json
{
  "event": "sources:activate",
  "data": {
    "sourceId": "source-123-abc",
    "sourceName": "Stream Alert",
    "isActive": true,
    "activatedAt": "2024-01-15T16:32:00Z"
  },
  "timestamp": "2024-01-15T16:32:00Z"
}
```

#### Source Deactivated

```json
{
  "event": "sources:deactivate",
  "data": {
    "sourceId": "source-123-abc",
    "sourceName": "Stream Alert",
    "isActive": false,
    "deactivatedAt": "2024-01-15T16:33:00Z"
  },
  "timestamp": "2024-01-15T16:33:00Z"
}
```

#### Source Deleted

```json
{
  "event": "sources:delete",
  "data": {
    "sourceId": "source-123-abc",
    "sourceName": "Stream Alert"
  },
  "timestamp": "2024-01-15T16:34:00Z"
}
```

### OBS Events

#### Scene Changed

```json
{
  "event": "obs:sceneChanged",
  "data": {
    "previousScene": "Main Scene",
    "currentScene": "BRB Scene",
    "sceneIndex": 1
  },
  "timestamp": "2024-01-15T16:35:00Z"
}
```

#### Connection Status Changed

```json
{
  "event": "obs:connectionChanged",
  "data": {
    "connected": true,
    "connectionId": "obs-conn-789",
    "obsVersion": "30.0.0",
    "websocketVersion": "5.0.0"
  },
  "timestamp": "2024-01-15T16:36:00Z"
}
```

#### OBS Stats Updated

```json
{
  "event": "obs:statsUpdate",
  "data": {
    "stats": {
      "cpuUsage": 15.2,
      "memoryUsage": 512.5,
      "fps": 60.0,
      "renderTime": 8.3,
      "outputSkippedFrames": 0,
      "outputTotalFrames": 3600
    }
  },
  "timestamp": "2024-01-15T16:37:00Z"
}
```

### Scene Events

#### Source Added to Scene

```json
{
  "event": "scenes:sourceAdded",
  "data": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "Stream Alert",
    "config": {
      "x": 100,
      "y": 50,
      "width": 800,
      "height": 600,
      "visible": true
    }
  },
  "timestamp": "2024-01-15T16:38:00Z"
}
```

#### Source Removed from Scene

```json
{
  "event": "scenes:sourceRemoved",
  "data": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "Stream Alert"
  },
  "timestamp": "2024-01-15T16:39:00Z"
}
```

#### Source Configuration Updated in Scene

```json
{
  "event": "scenes:sourceConfigUpdated",
  "data": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "Stream Alert",
    "config": {
      "x": 200,
      "y": 100,
      "visible": false
    },
    "changes": ["x", "y", "visible"]
  },
  "timestamp": "2024-01-15T16:40:00Z"
}
```

### Performance Events

#### Performance Update

```json
{
  "event": "performance:update",
  "data": {
    "system": {
      "cpu": 15.2,
      "memory": 512.5,
      "uptime": 3600
    },
    "application": {
      "activeConnections": 5,
      "averageResponseTime": 45.2
    },
    "sources": {
      "total": 12,
      "active": 4,
      "averageCpuUsage": 8.3
    }
  },
  "timestamp": "2024-01-15T16:41:00Z"
}
```

#### Performance Alert

```json
{
  "event": "performance:alert",
  "data": {
    "type": "high_cpu_usage",
    "value": 85.2,
    "threshold": 80.0,
    "severity": "warning",
    "source": "system",
    "message": "System CPU usage is above threshold"
  },
  "timestamp": "2024-01-15T16:42:00Z"
}
```

### System Events

#### System Status Change

```json
{
  "event": "system:statusChange",
  "data": {
    "previousStatus": "healthy",
    "currentStatus": "degraded",
    "reason": "High memory usage",
    "checks": {
      "obsConnected": true,
      "sourceManagerInitialized": true,
      "memoryUsage": "high"
    }
  },
  "timestamp": "2024-01-15T16:43:00Z"
}
```

#### Error Occurred

```json
{
  "event": "system:error",
  "data": {
    "error": "Failed to activate source",
    "sourceId": "source-123-abc",
    "errorCode": "ACTIVATION_FAILED",
    "details": {
      "reason": "OBS connection lost",
      "retryable": true
    }
  },
  "timestamp": "2024-01-15T16:44:00Z"
}
```

---

## Event Patterns

### Wildcard Subscriptions

You can subscribe to event patterns using wildcards:

- `sources:*` - All source events
- `obs:*` - All OBS events
- `performance:*` - All performance events
- `*` - All events (use with caution)

### Event Filtering

Subscribe to specific events for targeted updates:

```javascript
// Subscribe only to source activation/deactivation
socket.send(JSON.stringify({
  action: 'subscribe',
  events: ['sources:activate', 'sources:deactivate'],
}));

// Subscribe to scene changes and performance alerts
socket.send(JSON.stringify({
  action: 'subscribe',
  events: ['obs:sceneChanged', 'performance:alert'],
}));
```

---

## Connection Management

### Connection States

- `CONNECTING` - Initial connection attempt
- `OPEN` - Connection established and ready
- `CLOSING` - Connection is being closed
- `CLOSED` - Connection is closed

### Reconnection Strategy

Implement exponential backoff for reconnections:

```javascript
let reconnectAttempts = 0;
const maxReconnectAttempts = 5;
const baseDelay = 1000; // 1 second

function connect() {
  const socket = new WebSocket('ws://localhost:3000');

  socket.onopen = function () {
    reconnectAttempts = 0;
    console.log('Connected to Source Saver');
  };

  socket.onclose = function () {
    if (reconnectAttempts < maxReconnectAttempts) {
      const delay = baseDelay * Math.pow(2, reconnectAttempts);
      console.log(`Reconnecting in ${delay}ms...`);
      setTimeout(connect, delay);
      reconnectAttempts++;
    }
  };
}
```

### Heartbeat

Implement periodic ping to keep connection alive:

```javascript
setInterval(() => {
  if (socket.readyState === WebSocket.OPEN) {
    socket.send(JSON.stringify({
      action: 'ping',
      timestamp: new Date().toISOString(),
    }));
  }
}, 30000); // Every 30 seconds
```

---

## Error Handling

### Connection Errors

```json
{
  "event": "error",
  "data": {
    "type": "connection_error",
    "message": "WebSocket connection failed",
    "code": "CONNECTION_FAILED"
  }
}
```

### Subscription Errors

```json
{
  "event": "error",
  "data": {
    "type": "subscription_error",
    "message": "Invalid event pattern: invalid:*",
    "code": "INVALID_EVENT_PATTERN"
  }
}
```

### Rate Limiting

```json
{
  "event": "error",
  "data": {
    "type": "rate_limit_exceeded",
    "message": "Too many messages sent",
    "retryAfter": 5000
  }
}
```

---

## Best Practices

### Performance

- Subscribe only to events you need
- Use wildcard patterns judiciously
- Implement proper error handling and reconnection logic
- Buffer messages during disconnections if needed

### Security

- Validate all incoming messages
- Implement proper authentication if needed
- Use secure WebSocket (WSS) in production
- Rate limit client messages

### Reliability

- Implement heartbeat/ping-pong mechanism
- Handle connection drops gracefully
- Store critical state locally when possible
- Implement message acknowledgment for critical operations

---

**Back to**: [API Documentation Home](Home)
