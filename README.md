# 🧩 Atriva AI — App Template

A minimal, runnable reference application showing how to wire Atriva's Edge AI platform components together using Docker Compose.

> This template is the **orchestration layer** — it connects the real services from [github.com/atriva-ai](https://github.com/atriva-ai) rather than containing its own inference or video code.

---

## 🏗️ What This Template Wires Together

```
[Camera / RTSP Stream]
        │
        ▼
video-pipeline-ffmpeg-x86   ← decodes frames → shared volume
        │  (shared volume)
        ▼
ai-inference-ov             ← runs OpenVINO inference on frames
        │
        ▼
atriva-ai-platform-backend  ← stores results, exposes analytics API
        │
        ▼
Your app / dashboard / agent
```

---

## 🚀 Quick Start

Prerequisites: Docker (20.10+) and Git.

```bash
# 1. Clone this template
git clone https://github.com/atriva-ai-community/app-template.git
cd app-template

# 2. Clone the three core service repos into this directory
git clone https://github.com/atriva-ai/video-pipeline-ffmpeg-x86.git
git clone https://github.com/atriva-ai/ai-inference-ov.git
git clone https://github.com/atriva-ai/atriva-ai-platform-backend.git

# 3. Copy environment config (edit credentials if needed)
cp .env.example .env

# 4. Build and start the full stack
docker-compose up --build
```

The stack will be available at:

| Service | URL | Notes |
|---------|-----|-------|
| Platform API (via nginx) | http://localhost/docs | Swagger UI |
| AI Inference API | http://localhost:8001/docs | OpenVINO models |
| Video Pipeline API | http://localhost:8002/docs | FFmpeg frame extraction |
| Platform API (direct) | http://localhost:8000/docs | Bypass nginx |

> **Tip:** Send a test decode request to start extracting frames:
> ```bash
> curl -X POST http://localhost:8002/api/v1/video-pipeline/decode/ \
>   -H "Content-Type: application/json" \
>   -d '{"camera_id": "cam-demo-1", "source": "/app/videos/sample.mp4", "fps": 5}'
> ```

---

## 📁 Template Structure

```
app-template/
├── docker-compose.yml        ← wires together the atriva-ai service containers
├── .env.example              ← copy to .env before starting
├── nginx/
│   ├── nginx.conf            ← nginx base config
│   └── conf.d/default.conf   ← reverse proxy routing rules
├── config/
│   └── cameras.yml           ← camera / RTSP stream configuration
└── docs/
    ├── architecture.md       ← how the services connect
    ├── getting-started.md    ← full setup guide
    └── usage.md              ← API usage examples
```

After running the quickstart, cloned service repos (`video-pipeline-ffmpeg-x86/`, `ai-inference-ov/`,
`atriva-ai-platform-backend/`) will appear alongside these files — they are gitignored intentionally.

---

## 🧩 Core Service Repos

| Service | Repo | Docs |
|---------|------|------|
| AI Inference (OpenVINO) | [atriva-ai/ai-inference-ov](https://github.com/atriva-ai/ai-inference-ov) | [atriva.ai/docs/ai-inference/openvino](https://atriva.ai/docs/ai-inference/openvino) |
| Video Pipeline (x86) | [atriva-ai/video-pipeline-ffmpeg-x86](https://github.com/atriva-ai/video-pipeline-ffmpeg-x86) | [atriva.ai/docs/video-pipeline/ffmpeg-x86](https://atriva.ai/docs/video-pipeline/ffmpeg-x86) |
| Platform Backend | [atriva-ai/atriva-ai-platform-backend](https://github.com/atriva-ai/atriva-ai-platform-backend) | [atriva.ai/docs/core-api](https://atriva.ai/docs/core-api) |

For Rockchip RK3588, substitute:
- [atriva-ai/ai-inference-rk3588](https://github.com/atriva-ai/ai-inference-rk3588)
- [atriva-ai/video-pipeline-ffmpeg-rk3588](https://github.com/atriva-ai/video-pipeline-ffmpeg-rk3588)

---

## 🤝 Contributing

New to Atriva? See the [contribution guide](https://github.com/atriva-ai-community/community/blob/main/CONTRIBUTING.md) for how to get started and where to submit PRs.

Search issues labelled [`good first issue`](https://github.com/atriva-ai/ai-inference-ov/labels/good%20first%20issue) in the core repos to find something to work on.

**Community hub:** https://github.com/atriva-ai-community/community  
**Full repo directory:** https://github.com/atriva-ai-community/community/blob/main/REPOS.md  
**Contact:** contact@atriva.ai
