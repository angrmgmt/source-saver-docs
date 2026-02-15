# Sources Management API

The Sources API provides endpoints for creating, managing, and controlling browser sources.

## Base URL

All source endpoints are prefixed with `/api/sources`

---

## Source CRUD Operations

### GET /api/sources

Get all browser sources.

**Query Parameters:**

- `active` (optional): Filter by active status (`true`/`false`)
- `limit` (optional): Limit number of results (default: 100)
- `offset` (optional): Offset for pagination (default: 0)

**Response:**

```json
{
  "sources": [
    {
      "id": "source-123-abc",
      "name": "StreamLabs Donations",
      "url": "https://streamlabs.com/widgets/donations",
      "width": 800,
      "height": 600,
      "refreshRate": 30,
      "isActive": true,
      "cssCustom": "body { background: transparent; }",
      "jsCustom": "console.log('Source loaded');",
      "createdAt": "2024-01-15T10:30:00Z",
      "updatedAt": "2024-01-15T12:45:00Z"
    }
  ],
  "total": 1,
  "limit": 100,
  "offset": 0
}
```

**Status Codes:**

- `200` - Success

---

### GET /api/sources/:sourceId

Get a specific browser source by ID.

**Path Parameters:**

- `sourceId` - The unique identifier of the source

**Response:**

```json
{
  "id": "source-123-abc",
  "name": "StreamLabs Donations",
  "url": "https://streamlabs.com/widgets/donations",
  "width": 800,
  "height": 600,
  "refreshRate": 30,
  "isActive": true,
  "cssCustom": "body { background: transparent; }",
  "jsCustom": "console.log('Source loaded');",
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-15T12:45:00Z",
  "performanceMetrics": {
    "cpuUsage": 5.2,
    "memoryUsage": 45.8,
    "renderTime": 12.3,
    "lastUpdated": "2024-01-15T12:45:00Z"
  }
}
```

**Status Codes:**

- `200` - Success
- `404` - Source not found

---

### POST /api/sources

Create a new browser source.

**Request Body:**

```json
{
  "name": "StreamLabs Donations",
  "url": "https://streamlabs.com/widgets/donations",
  "width": 800,
  "height": 600,
  "refreshRate": 30,
  "cssCustom": "body { background: transparent; }",
  "jsCustom": "console.log('Source loaded');"
}
```

**Required Fields:**

- `name` - Human-readable name (1-255 characters)
- `url` - Valid URL for the browser source
- `width` - Width in pixels (1-7680)
- `height` - Height in pixels (1-4320)

**Optional Fields:**

- `refreshRate` - Refresh rate in seconds (1-60)
- `cssCustom` - Custom CSS to apply
- `jsCustom` - Custom JavaScript to apply

**Response:**

```json
{
  "id": "source-456-def",
  "name": "StreamLabs Donations",
  "url": "https://streamlabs.com/widgets/donations",
  "width": 800,
  "height": 600,
  "refreshRate": 30,
  "isActive": false,
  "cssCustom": "body { background: transparent; }",
  "jsCustom": "console.log('Source loaded');",
  "createdAt": "2024-01-15T14:30:00Z",
  "updatedAt": "2024-01-15T14:30:00Z"
}
```

**Status Codes:**

- `201` - Source created successfully
- `400` - Invalid request data
- `409` - Source with same name already exists

---

### PUT /api/sources/:sourceId

Update an existing browser source.

**Path Parameters:**

- `sourceId` - The unique identifier of the source

**Request Body:**

```json
{
  "name": "Updated StreamLabs Donations",
  "url": "https://streamlabs.com/widgets/donations",
  "width": 1200,
  "height": 800,
  "refreshRate": 15,
  "isActive": true,
  "cssCustom": "body { background: transparent; font-size: 14px; }",
  "jsCustom": "console.log('Source updated');"
}
```

**Response:**

```json
{
  "id": "source-123-abc",
  "name": "Updated StreamLabs Donations",
  "url": "https://streamlabs.com/widgets/donations",
  "width": 1200,
  "height": 800,
  "refreshRate": 15,
  "isActive": true,
  "cssCustom": "body { background: transparent; font-size: 14px; }",
  "jsCustom": "console.log('Source updated');",
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-15T15:00:00Z"
}
```

**Status Codes:**

- `200` - Source updated successfully
- `400` - Invalid request data
- `404` - Source not found

---

### DELETE /api/sources/:sourceId

Delete a browser source.

**Path Parameters:**

- `sourceId` - The unique identifier of the source

**Response:**

```json
{
  "success": true,
  "message": "Source deleted successfully",
  "deletedSource": {
    "id": "source-123-abc",
    "name": "StreamLabs Donations"
  }
}
```

**Status Codes:**

- `200` - Source deleted successfully
- `404` - Source not found
- `409` - Cannot delete active source (deactivate first)

---

## Source Control

### POST /api/sources/:sourceId/activate

Activate a browser source (make it visible and start loading content).

**Path Parameters:**

- `sourceId` - The unique identifier of the source

**Response:**

```json
{
  "id": "source-123-abc",
  "name": "StreamLabs Donations",
  "isActive": true,
  "activatedAt": "2024-01-15T15:30:00Z",
  "status": "loading"
}
```

**Status Codes:**

- `200` - Source activated successfully
- `404` - Source not found
- `409` - Source already active

---

### POST /api/sources/:sourceId/deactivate

Deactivate a browser source (hide it and stop loading content).

**Path Parameters:**

- `sourceId` - The unique identifier of the source

**Response:**

```json
{
  "success": true,
  "message": "Source deactivated successfully",
  "source": {
    "id": "source-123-abc",
    "name": "StreamLabs Donations",
    "isActive": false,
    "deactivatedAt": "2024-01-15T15:35:00Z"
  }
}
```

**Status Codes:**

- `200` - Source deactivated successfully
- `404` - Source not found
- `409` - Source already inactive

---

## Performance Monitoring

### GET /api/sources/:sourceId/performance

Get performance metrics for a specific source.

**Path Parameters:**

- `sourceId` - The unique identifier of the source

**Query Parameters:**

- `hours` (optional): Number of hours of history to retrieve (default: 24, max: 168)

**Response:**

```json
{
  "sourceId": "source-123-abc",
  "sourceName": "StreamLabs Donations",
  "metrics": [
    {
      "timestamp": "2024-01-15T15:30:00Z",
      "cpuUsage": 5.2,
      "memoryUsage": 45.8,
      "renderTime": 12.3,
      "fps": 60.0
    },
    {
      "timestamp": "2024-01-15T15:25:00Z",
      "cpuUsage": 4.8,
      "memoryUsage": 44.2,
      "renderTime": 11.8,
      "fps": 60.0
    }
  ],
  "summary": {
    "averageCpuUsage": 5.0,
    "averageMemoryUsage": 45.0,
    "averageRenderTime": 12.0,
    "averageFps": 60.0,
    "dataPoints": 48
  }
}
```

**Status Codes:**

- `200` - Success
- `404` - Source not found

---

## Error Responses

### Validation Errors

```json
{
  "error": "Validation failed: width must be between 1 and 7680 pixels"
}
```

### Source Not Found

```json
{
  "error": "Source not found"
}
```

### Source Already Exists

```json
{
  "error": "A source with this name already exists"
}
```

### Cannot Delete Active Source

```json
{
  "error": "Cannot delete an active source. Please deactivate it first."
}
```

---

## WebSocket Events

When connected via WebSocket, you'll receive real-time events for source changes:

### Source Created

```json
{
  "event": "sources:create",
  "data": {
    "source": {
      "id": "source-789-ghi",
      "name": "New Source",
      "isActive": false
    },
    "timestamp": "2024-01-15T16:00:00Z"
  }
}
```

### Source Updated

```json
{
  "event": "sources:update",
  "data": {
    "source": {
      "id": "source-123-abc",
      "name": "Updated Source",
      "isActive": true
    },
    "changes": ["name", "width", "height"],
    "timestamp": "2024-01-15T16:05:00Z"
  }
}
```

### Source Activated/Deactivated

```json
{
  "event": "sources:activate",
  "data": {
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "isActive": true,
    "timestamp": "2024-01-15T16:10:00Z"
  }
}
```

### Source Deleted

```json
{
  "event": "sources:delete",
  "data": {
    "sourceId": "source-123-abc",
    "sourceName": "StreamLabs Donations",
    "timestamp": "2024-01-15T16:15:00Z"
  }
}
```

---

**Next**: Learn about [Scenes API](Scenes-API) for managing sources within OBS scenes.
