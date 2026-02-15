# Scenes Management API

The Scenes API provides endpoints for managing browser sources within OBS scenes.

## Base URL

All scene endpoints are prefixed with `/api/scenes`

---

## Scene Source Management

### GET /api/scenes/:sceneName/sources

Get all sources associated with a specific OBS scene.

**Path Parameters:**

- `sceneName` - The name of the OBS scene

**Response:**

```json
{
  "sceneName": "Main Scene",
  "sources": [
    {
      "id": "source-123-abc",
      "name": "StreamLabs Donations",
      "url": "https://streamlabs.com/widgets/donations",
      "isActive": true,
      "sceneConfig": {
        "x": 100,
        "y": 50,
        "width": 800,
        "height": 600,
        "scaleX": 1.0,
        "scaleY": 1.0,
        "rotation": 0,
        "visible": true
      },
      "addedAt": "2024-01-15T10:30:00Z"
    },
    {
      "id": "source-456-def",
      "name": "Chat Widget",
      "url": "https://streamlabs.com/widgets/chat",
      "isActive": true,
      "sceneConfig": {
        "x": 1200,
        "y": 100,
        "width": 400,
        "height": 800,
        "scaleX": 0.8,
        "scaleY": 0.8,
        "rotation": 0,
        "visible": true
      },
      "addedAt": "2024-01-15T11:00:00Z"
    }
  ],
  "totalSources": 2
}
```

**Status Codes:**

- `200` - Success
- `404` - Scene not found
- `503` - OBS not connected

---

### POST /api/scenes/:sceneName/sources/:sourceId

Add a browser source to an OBS scene with optional positioning configuration.

**Path Parameters:**

- `sceneName` - The name of the OBS scene
- `sourceId` - The unique identifier of the source to add

**Request Body (Optional):**

```json
{
  "config": {
    "x": 100,
    "y": 50,
    "width": 800,
    "height": 600,
    "scaleX": 1.0,
    "scaleY": 1.0,
    "rotation": 0,
    "visible": true
  }
}
```

**Configuration Fields:**

- `x` - X position in the scene (pixels)
- `y` - Y position in the scene (pixels)
- `width` - Width override for the scene (pixels)
- `height` - Height override for the scene (pixels)
- `scaleX` - Horizontal scaling factor (0.1 - 10.0)
- `scaleY` - Vertical scaling factor (0.1 - 10.0)
- `rotation` - Rotation in degrees (0 - 360)
- `visible` - Whether the source is visible in the scene

**Response:**

```json
{
  "success": true,
  "message": "Source added to scene successfully",
  "sceneSource": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "config": {
      "x": 100,
      "y": 50,
      "width": 800,
      "height": 600,
      "scaleX": 1.0,
      "scaleY": 1.0,
      "rotation": 0,
      "visible": true
    },
    "addedAt": "2024-01-15T16:30:00Z"
  }
}
```

**Status Codes:**

- `200` - Source added to scene successfully
- `400` - Invalid configuration parameters
- `404` - Scene or source not found
- `409` - Source already exists in scene
- `503` - OBS not connected

---

### DELETE /api/scenes/:sceneName/sources/:sourceId

Remove a browser source from an OBS scene.

**Path Parameters:**

- `sceneName` - The name of the OBS scene
- `sourceId` - The unique identifier of the source to remove

**Response:**

```json
{
  "success": true,
  "message": "Source removed from scene successfully",
  "removedSource": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "removedAt": "2024-01-15T16:35:00Z"
  }
}
```

**Status Codes:**

- `200` - Source removed from scene successfully
- `404` - Scene or source not found
- `503` - OBS not connected

---

## Scene Configuration

### PUT /api/scenes/:sceneName/sources/:sourceId/config

Update the configuration of a source within a specific scene.

**Path Parameters:**

- `sceneName` - The name of the OBS scene
- `sourceId` - The unique identifier of the source

**Request Body:**

```json
{
  "x": 200,
  "y": 100,
  "width": 1000,
  "height": 700,
  "scaleX": 1.2,
  "scaleY": 1.2,
  "rotation": 15,
  "visible": false
}
```

**Response:**

```json
{
  "success": true,
  "message": "Source configuration updated successfully",
  "sceneSource": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "config": {
      "x": 200,
      "y": 100,
      "width": 1000,
      "height": 700,
      "scaleX": 1.2,
      "scaleY": 1.2,
      "rotation": 15,
      "visible": false
    },
    "updatedAt": "2024-01-15T16:40:00Z"
  }
}
```

**Status Codes:**

- `200` - Configuration updated successfully
- `400` - Invalid configuration parameters
- `404` - Scene or source not found
- `503` - OBS not connected

---

## Bulk Operations

### POST /api/scenes/:sceneName/sources/bulk

Add multiple sources to a scene at once.

**Path Parameters:**

- `sceneName` - The name of the OBS scene

**Request Body:**

```json
{
  "sources": [
    {
      "sourceId": "source-123-abc",
      "config": {
        "x": 100,
        "y": 50,
        "visible": true
      }
    },
    {
      "sourceId": "source-456-def",
      "config": {
        "x": 1200,
        "y": 100,
        "scaleX": 0.8,
        "scaleY": 0.8,
        "visible": true
      }
    }
  ]
}
```

**Response:**

```json
{
  "success": true,
  "message": "2 sources added to scene successfully",
  "results": [
    {
      "sourceId": "source-123-abc",
      "success": true,
      "message": "Added successfully"
    },
    {
      "sourceId": "source-456-def",
      "success": true,
      "message": "Added successfully"
    }
  ],
  "addedCount": 2,
  "failedCount": 0
}
```

**Status Codes:**

- `200` - All sources processed (check individual results)
- `400` - Invalid request format
- `404` - Scene not found
- `503` - OBS not connected

---

### DELETE /api/scenes/:sceneName/sources/bulk

Remove multiple sources from a scene at once.

**Path Parameters:**

- `sceneName` - The name of the OBS scene

**Request Body:**

```json
{
  "sourceIds": ["source-123-abc", "source-456-def"]
}
```

**Response:**

```json
{
  "success": true,
  "message": "2 sources removed from scene successfully",
  "results": [
    {
      "sourceId": "source-123-abc",
      "success": true,
      "message": "Removed successfully"
    },
    {
      "sourceId": "source-456-def",
      "success": true,
      "message": "Removed successfully"
    }
  ],
  "removedCount": 2,
  "failedCount": 0
}
```

**Status Codes:**

- `200` - All sources processed (check individual results)
- `400` - Invalid request format
- `404` - Scene not found
- `503` - OBS not connected

---

## Error Responses

### Scene Not Found

```json
{
  "error": "Scene 'NonExistent Scene' not found in OBS"
}
```

### Source Not Found

```json
{
  "error": "Source not found"
}
```

### Source Already in Scene

```json
{
  "error": "Source is already present in this scene"
}
```

### Invalid Configuration

```json
{
  "error": "Invalid configuration: scaleX must be between 0.1 and 10.0"
}
```

### OBS Not Connected

```json
{
  "error": "OBS is not connected. Please connect to OBS first."
}
```

---

## WebSocket Events

When connected via WebSocket, you'll receive real-time events for scene changes:

### Source Added to Scene

```json
{
  "event": "scenes:sourceAdded",
  "data": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "config": {
      "x": 100,
      "y": 50,
      "visible": true
    },
    "timestamp": "2024-01-15T16:45:00Z"
  }
}
```

### Source Removed from Scene

```json
{
  "event": "scenes:sourceRemoved",
  "data": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "timestamp": "2024-01-15T16:50:00Z"
  }
}
```

### Source Configuration Updated

```json
{
  "event": "scenes:sourceConfigUpdated",
  "data": {
    "sceneName": "Main Scene",
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "config": {
      "x": 200,
      "y": 100,
      "visible": false
    },
    "changes": ["x", "y", "visible"],
    "timestamp": "2024-01-15T16:55:00Z"
  }
}
```

---

## Best Practices

### Scene Management

- Always check if OBS is connected before making scene operations
- Use bulk operations when adding/removing multiple sources
- Keep source configurations reasonable (avoid extreme scaling or positioning)

### Performance Considerations

- Limit the number of active sources per scene to maintain performance
- Use the performance monitoring endpoints to track resource usage
- Consider deactivating sources when not in use

### Error Handling

- Always handle the case where OBS is not connected
- Check for scene existence before attempting operations
- Validate configuration parameters before sending requests

---

**Next**: Learn about [System API](System-API) for health checks and system information.
