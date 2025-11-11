# 🧩 Atriva AI — App Template

A minimal, runnable reference application demonstrating how to compose an Edge Vision AI product using containerized modules (video pipeline → inference → tracker → UI).  
This repository is intended as the community-facing template to help developers and recent graduates learn product-oriented Edge AI development.

> Note: this version is derived from the Atriva AI internal WIP Docker app and has been sanitized for public release. See `docs/` for architecture, setup, and contribution guidelines.

---

## 🔍 What this repo contains

- `docker/` — Dockerfiles and `docker-compose.yml` for a minimal pipeline (local testing)
- `src/` — Example modules (inference_stub, tracker_stub, ui)
- `data/` — Small sample video/images for quick testing
- `docs/` — Architecture, setup guides, usage, and licensing information
- `.github/` — Issue & PR templates, workflows (CI placeholder)
- `LICENSE` — License for this public repo

---

## 🚀 Quick start (local, Docker)

> Prerequisites: Docker and Docker Compose installed

```bash
# clone
git clone https://github.com/atriva-ai-community/app-template.git
cd app-template

# build & run (simple demo)
docker-compose up --build
```

Visit the UI at: http://localhost:8000 (see docker/docker-compose.yml for ports)

## 🧭 Project goals

Provide a clear, runnable example of an Edge AI product skeleton

Demonstrate how components connect (video input → inference → results → dashboard)

Serve as a sandbox for contributors to add hardware backends, model integrations, and production improvements

## 📚 Docs & next steps

docs/architecture.md — system architecture and component responsibilities

docs/getting-started.md — full environment setup and troubleshooting

docs/usage.md — how to run pipelines, switch models, and test with sample data

docs/licensing.md — license choices and guidance for contributors

## 🧩 Contribution

We welcome contributions — small or large. See docs/getting-started.md and docs/licensing.md for how to prepare pull requests. If you’re new, search issues labelled good first issue.

## ⚠️ Security / privacy note

This repo has been sanitized for public release. Sensitive configuration, credentials, and enterprise-only modules are intentionally excluded. See docs/licensing.md for more.

Contacts

Community hub: https://github.com/atriva-ai-community/community

Contact: contact@atriva.ai

