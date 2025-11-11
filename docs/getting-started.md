# Getting Started

This guide walks through cloning, building, and running the app-template locally.

## Prerequisites

- Git
- Docker (20.10+)
- Docker Compose (v2+)
- Node.js (only if you plan to develop the UI locally)

## Clone the repo

```bash
git clone https://github.com/atriva-ai-community/app-template.git
cd app-template
```

## Build & Run (quick demo)

```bash
cd app-template
docker-compose up --build
```

Open http://localhost:8000 for the UI (port depends on docker/docker-compose.yml)

## Run with sample data

The repo includes data/sample.mp4. The pipeline defaults to use this file when no camera is attached.

## Stopping

```bash
docker-compose down
```

## Troubleshooting

Container build fails: check Dockerfile base image compatibility and available resources.

UI 500 error: inspect backend container logs (docker-compose logs <container-name>).

