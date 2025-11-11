# Architecture — App Template

This document describes the high-level architecture for the app-template and responsibilities of each module.

## Overview

The app-template shows a small, modular Edge AI product:

Camera/Input -> Video Pipeline -> Inference -> Tracker/Aggregator -> Backend/API -> UI/Dashboard

### Main components

- **Video Pipeline**
  - Responsibilities: capture (file/local camera), decode, resize, pre-process frames.
  - Typical tech: GStreamer or OpenCV, containerized.

- **Inference**
  - Responsibilities: load model, run inference per frame, return detections.
  - For this template: a simplified inference stub is provided; replace with RKNN/TensorRT/ONNX modules as needed.

- **Tracker / Aggregator**
  - Responsibilities: link object detections across frames, aggregate metrics, generate events.

- **Backend / API**
  - Responsibilities: expose REST/WebSocket endpoints for metadata, persist events (optional).

- **UI / Dashboard**
  - Responsibilities: display live video stream + overlay, show analytics and historical data.

### Containerization

Each major component runs in its own container to keep the template modular and extendable. See `docker/docker-compose.yml`.

### Extensibility points

- Replace `inference_stub` with hardware-optimized inference container (RKNN, TensorRT).
- Add GPU support by updating container runtime and base images.
- Add authentication and persistent storage (Postgres, S3) in production variants.

