# OBS Integration API

The OBS API provides endpoints for managing the connection to OBS Studio and controlling scenes.

## Base URL

All OBS endpoints are prefixed with `/api/obs`

---

## Connection Management

### GET /api/obs/status

Get the current OBS WebSocket connection status.

**Response:**

```json
{
  "connected": true,
  "connectionId": "obs-connection-123",
  "obsVersion": "30.0.0",
  "websocketVersion": "5.0.0",
  "lastConnected": "2024-01-15T10:30:00Z"
}
```

**Status Codes:**

- `200` - Success
- `503` - OBS Manager not initialized

---

### POST /api/obs/connect

Establish connection to OBS Studio WebSocket server.

**Request Body:**

```json
{
  "host": "localhost",
  "port": 4455,
  "password": "optional-password"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Connected to OBS successfully",
  "connectionDetails": {
    "host": "localhost",
    "port": 4455,
    "connected": true
  }
}
```

**Status Codes:**

- `200` - Successfully connected
- `400` - Invalid connection parameters
- `500` - Connection failed

---

### POST /api/obs/disconnect

Disconnect from OBS Studio WebSocket server.

**Response:**

```json
{
  "success": true,
  "message": "Disconnected from OBS successfully"
}
```

**Status Codes:**

- `200` - Successfully disconnected
- `500` - Disconnection failed

---

### POST /api/obs/reconnect

Reconnect to OBS Studio WebSocket server.

**Response:**

```json
{
  "success": true,
  "message": "Reconnected to OBS successfully",
  "connectionDetails": {
    "host": "localhost",
    "port": 4455,
    "connected": true
  }
}
```

**Status Codes:**

- `200` - Successfully reconnected
- `500` - Reconnection failed

---

## Scene Management

### GET /api/obs/scenes

Get all available scenes from OBS Studio.

**Response:**

```json
{
  "scenes": [
    {
      "sceneName": "Main Scene",
      "sceneIndex": 0,
      "sources": [
        {
          "sourceName": "Browser Source 1",
          "sourceType": "browser_source",
          "visible": true
        }
      ]
    },
    {
      "sceneName": "BRB Scene",
      "sceneIndex": 1,
      "sources": []
    }
  ],
  "currentScene": "Main Scene"
}
```

**Status Codes:**

- `200` - Success
- `503` - OBS not connected

---

### GET /api/obs/current-scene

Get the currently active scene in OBS Studio.

**Response:**

```json
{
  "sceneName": "Main Scene",
  "sceneIndex": 0,
  "sources": [
    {
      "sourceName": "Browser Source 1",
      "sourceType": "browser_source",
      "visible": true,
      "transform": {
        "x": 0,
        "y": 0,
        "width": 1920,
        "height": 1080,
        "scaleX": 1.0,
        "scaleY": 1.0,
        "rotation": 0
      }
    }
  ]
}
```

**Status Codes:**

- `200` - Success
- `503` - OBS not connected

---

### POST /api/obs/current-scene

Set the current active scene in OBS Studio.

**Request Body:**

```json
{
  "sceneName": "BRB Scene"
}
```

**Response:**

```json
{
  "success": true,
  "message": "Scene changed successfully",
  "previousScene": "Main Scene",
  "currentScene": "BRB Scene"
}
```

**Status Codes:**

- `200` - Scene changed successfully
- `400` - Invalid scene name
- `404` - Scene not found
- `503` - OBS not connected

---

## Statistics

### GET /api/obs/stats

Get OBS Studio performance statistics.

**Response:**

```json
{
  "stats": {
    "cpuUsage": 15.2,
    "memoryUsage": 512.5,
    "fps": 60.0,
    "renderTime": 8.3,
    "outputSkippedFrames": 0,
    "outputTotalFrames": 3600,
    "webRtcStats": {
      "bytesReceived": 1024000,
      "bytesSent": 2048000
    }
  },
  "timestamp": "2024-01-15T10:30:00Z"
}
```

**Status Codes:**

- `200` - Success
- `503` - OBS not connected

---

## Error Responses

All OBS API endpoints may return these common errors:

### OBS Not Connected

```json
{
  "error": "OBS is not connected. Please connect to OBS first."
}
```

### OBS Manager Not Initialized

```json
{
  "error": "OBS Manager not initialized"
}
```

### Connection Failed

```json
{
  "error": "Failed to connect to OBS: Connection refused"
}
```

---

## WebSocket Events

When connected via WebSocket, you'll receive real-time events for OBS changes:

### Scene Changed

```json
{
  "event": "obs:sceneChanged",
  "data": {
    "previousScene": "Main Scene",
    "currentScene": "BRB Scene",
    "timestamp": "2024-01-15T10:30:00Z"
  }
}
```

### Connection Status Changed

```json
{
  "event": "obs:connectionChanged",
  "data": {
    "connected": true,
    "timestamp": "2024-01-15T10:30:00Z"
  }
}
```

---

**Next**: Learn about [Sources API](Sources-API) for managing browser sources.
