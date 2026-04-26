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

Prerequisites: Docker and Docker Compose installed.

```bash
# 1. Clone this template
git clone https://github.com/atriva-ai-community/app-template.git
cd app-template

# 2. Clone the core service repos alongside it
git clone https://github.com/atriva-ai/video-pipeline-ffmpeg-x86.git
git clone https://github.com/atriva-ai/ai-inference-ov.git
git clone https://github.com/atriva-ai/atriva-ai-platform-backend.git

# 3. Start the full stack
docker-compose up --build
```

The stack will be available at:
- AI Inference API: http://localhost:8001/docs
- Video Pipeline API: http://localhost:8002/docs
- Platform API: http://localhost:8000/docs

---

## 📁 Template Structure

```
app-template/
├── docker-compose.yml     ← wires together the atriva-ai service containers
├── config/
│   └── cameras.yml        ← camera / stream configuration
├── examples/
│   ├── sample.mp4         ← test video for local development
│   └── test_image.jpg     ← test image for inference verification
└── docs/
    ├── architecture.md    ← how the services connect
    ├── getting-started.md ← full setup guide
    └── customizing.md     ← how to swap models, add cameras, extend the stack
```

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
