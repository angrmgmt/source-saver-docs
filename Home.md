# Source Saver API Documentation

Welcome to the Source Saver API documentation! Source Saver is a browser source manager for OBS Studio that combines
multiple browser sources into a single, optimized source to save memory, CPU, and GPU resources.

## Quick Start

- **Base URL**: `http://localhost:3000`
- **Content Type**: `application/json`
- **Authentication**: None (localhost only)

## API Overview

Source Saver provides a RESTful API with the following main endpoints:

### 🎯 [OBS Integration](OBS-API)

Manage OBS Studio connection and scenes

- Connection management
- Scene operations
- Statistics and monitoring

### 📺 [Source Management](Sources-API)

Create, update, and manage browser sources

- CRUD operations for sources
- Source activation/deactivation
- Performance monitoring

### 🎬 [Scene Management](Scenes-API)

Manage sources within OBS scenes

- Add/remove sources from scenes
- Scene-specific source configuration

### 🔧 [System Endpoints](System-API)

Health checks and system information

- Health monitoring
- Configuration retrieval
- Performance metrics

## Features

- **Memory Optimization**: Intelligent source management to reduce resource usage
- **Real-time Updates**: WebSocket-based communication for live updates
- **Performance Monitoring**: Built-in metrics and monitoring
- **OBS Integration**: Seamless integration with OBS Studio via WebSocket

## Rate Limiting

API endpoints are rate-limited to prevent abuse:

- **Standard endpoints**: 100 requests per 15 minutes
- **Bulk operations**: Applied per IP address in production

## Error Handling

All endpoints return consistent error responses:

```json
{
  "error": "Error message describing what went wrong"
}
```

Common HTTP status codes:

- `200` - Success
- `400` - Bad Request (invalid parameters)
- `404` - Not Found (resource doesn't exist)
- `500` - Internal Server Error

## Getting Started

1. Ensure OBS Studio is running with WebSocket server enabled
2. Start Source Saver: `deno run --allow-all src/start.ts`
3. Access the API at `http://localhost:3000/api`
4. Use the web interface at `http://localhost:3000`

## WebSocket Support

Source Saver also provides real-time WebSocket communication for live updates. Connect to `ws://localhost:3000` for
real-time events.

## Examples

### Create a new source

```bash
curl -X POST http://localhost:3000/api/sources \
  -H "Content-Type: application/json" \
  -d '{
    "name": "My Stream Alert",
    "url": "https://streamlabs.com/widgets/alertbox",
    "width": 800,
    "height": 600
  }'
```

### Get all sources

```bash
curl http://localhost:3000/api/sources
```

### Check OBS connection status

```bash
curl http://localhost:3000/api/obs/status
```

---

## Project Policies

- [Issues Definition and Accuracy Policy](../policies/issues-definition-and-accuracy-policy.md)

**Next**: Start with the [OBS API](OBS-API) to learn about OBS integration, or jump to [Sources API](Sources-API) for
source management.
