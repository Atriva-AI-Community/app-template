# Getting Started

This guide walks through cloning, building, and running the app-template locally.

## Prerequisites

- Git (2.x+)
- Docker (20.10+) and Docker Compose v2+
- ~8 GB free disk space (for Docker images and build layers)
- Optional: NVIDIA or Intel GPU drivers for hardware-accelerated inference

## Step 1 — Clone the template

```bash
git clone https://github.com/atriva-ai-community/app-template.git
cd app-template
```

## Step 2 — Clone the core service repos

The template uses the real Atriva service implementations. Clone them into the `app-template/` directory:

```bash
git clone https://github.com/atriva-ai/video-pipeline-ffmpeg-x86.git
git clone https://github.com/atriva-ai/ai-inference-ov.git
git clone https://github.com/atriva-ai/atriva-ai-platform-backend.git
```

> **Rockchip RK3588?** Substitute `ai-inference-rk3588` and `video-pipeline-ffmpeg-rk3588` instead.

## Step 3 — Configure environment

```bash
cp .env.example .env
```

Edit `.env` to set your own database credentials if needed (defaults work for local dev):

```
POSTGRES_USER=atriva
POSTGRES_PASSWORD=changeme
POSTGRES_DB=atriva_app
```

## Step 4 — Start the stack

```bash
docker-compose up --build
```

First build takes 5–15 minutes depending on your machine and internet speed (pulls base images,
installs Python dependencies, compiles OpenVINO models). Subsequent starts are fast.

## Step 5 — Verify the services

Open these URLs in your browser:

| Service | URL |
|---------|-----|
| Platform API (via nginx) | http://localhost/docs |
| AI Inference API | http://localhost:8001/docs |
| Video Pipeline API | http://localhost:8002/docs |
| Platform API (direct) | http://localhost:8000/docs |

Health check endpoints:
```bash
curl http://localhost:8002/api/v1/video-pipeline/health/
curl http://localhost:8001/models
```

## Step 6 — Run a test decode

Send a decode request using the sample video config in `config/cameras.yml`:

```bash
# Start decoding a demo video (adjust path to a real video file)
curl -X POST http://localhost:8002/api/v1/video-pipeline/decode/ \
  -H "Content-Type: application/json" \
  -d '{"camera_id": "cam-demo-1", "source": "/app/videos/sample.mp4", "fps": 5}'

# Check decode status
curl "http://localhost:8002/api/v1/video-pipeline/decode/status/?camera_id=cam-demo-1"

# Run inference on the latest frame
curl "http://localhost:8001/api/v1/ai/latest-frame/?camera_id=cam-demo-1"
```

## Stopping

```bash
docker-compose down
```

To also remove volumes (wipes the database and cached frames):
```bash
docker-compose down -v
```

## Troubleshooting

**Build fails on ai-inference-ov**: The service downloads OpenVINO models at build time. Ensure
you have a stable internet connection and at least 4 GB free memory during the build.

**Port conflict on 80**: Another service is using port 80. Change nginx's host port in
`docker-compose.yml`: `"8080:80"` and access via `http://localhost:8080`.

**Backend exits immediately**: The `db` healthcheck must pass before backend starts. Check
PostgreSQL logs: `docker-compose logs db`.

**Video frames not appearing**: Verify the shared volume is mounted correctly. Both
`video_pipeline` and `ai_inference` must mount `shared-frames-data` at `/app/frames`.
