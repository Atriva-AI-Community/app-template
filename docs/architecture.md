# Architecture — App Template

## Overview

The app-template wires five Docker services into a complete Edge AI platform stack:

```
[Camera / RTSP / File]
        │
        ▼
┌─────────────────────────┐
│  video_pipeline (8002)  │  FFmpeg-based frame extractor
│  video-pipeline-ffmpeg  │  Decodes video → JPEG frames
└───────────┬─────────────┘
            │  shared Docker volume
            │  /app/frames/{camera_id}/frame_XXXX.jpg
            ▼
┌─────────────────────────┐
│  ai_inference (8001)    │  OpenVINO inference engine
│  ai-inference-ov        │  Object detection, LPR, tracking
└───────────┬─────────────┘
            │  REST API results
            ▼
┌─────────────────────────┐
│  backend (8000)         │  FastAPI platform backend
│  atriva-ai-platform     │  Stores results in PostgreSQL
└───────────┬─────────────┘
            │
            ▼
┌─────────────────────────┐
│  db (5432)              │  PostgreSQL 15
│                         │  Persistent analytics storage
└─────────────────────────┘

All services reachable via:
┌─────────────────────────┐
│  nginx (80)             │  Reverse proxy
│                         │  Routes /api/v1/ai/ → ai_inference
│                         │  Routes /api/v1/video-pipeline/ → video_pipeline
│                         │  Routes / → backend
└─────────────────────────┘
```

## Shared Volume Design

The frame exchange between `video_pipeline` and `ai_inference` uses a Docker named volume
(`shared-frames-data`) mounted at `/app/frames` in both containers. This avoids API overhead
for high-frequency frame data.

```
/app/frames/
├── cam-entrance/
│   ├── frame_0001.jpg
│   ├── frame_0002.jpg
│   └── ...
└── cam-demo-1/
    ├── frame_0001.jpg
    └── ...
```

A second shared volume (`shared-temp-data` at `/app/shared`) holds temporary processing data.

## Service Responsibilities

| Service | Port | Repo | Role |
|---------|------|------|------|
| `video_pipeline` | 8002 | video-pipeline-ffmpeg-x86 | Decode video, extract frames |
| `ai_inference` | 8001 | ai-inference-ov | Run OpenVINO models on frames |
| `backend` | 8000 | atriva-ai-platform-backend | Store results, expose analytics API |
| `db` | 5432 | postgres:15.5 | Persistent storage |
| `nginx` | 80 | nginx:1.25.3-alpine | Reverse proxy |

## Extensibility

- **Different hardware**: Swap `ai-inference-ov` for `ai-inference-rk3588` on Rockchip boards,
  or `deepstream-api-jetson` on NVIDIA Jetson.
- **Add a frontend**: Clone [retail-analytics-frontend](https://github.com/atriva-ai/retail-analytics-frontend)
  into this directory and add a `frontend` service to `docker-compose.yml` pointing to it.
- **Add hardware acceleration**: For x86 with VAAPI, add `--device /dev/dri:/dev/dri` to the
  `video_pipeline` service in `docker-compose.yml`.
- **Production deployment**: Replace the `build:` directives with pre-built image references
  from your container registry. Add TLS to the nginx config.
