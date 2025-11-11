# Usage Guide

This document explains how to run the demo, switch models, and test.

## Running the inference loop

1. Ensure containers are up: `docker-compose up`
2. The inference container will process frames and publish JSON results to the backend (via HTTP or gRPC depending on config)

## Switching to a real model

The template uses `inference_stub` by default. To replace with a real model:

1. Build your model container that exposes the same inference API (input: frame, output: list of detections).
2. Update `docker/docker-compose.yml` to point to your inference image.
3. Rebuild: `docker-compose up --build`

## Debugging tips

- Attach to a container shell: `docker exec -it <container> /bin/bash`
- Inspect logs: `docker-compose logs -f --tail=200`

## Contributing modules

- Use the `src/` layout to add modules
- Keep module API consistent: `POST /infer` or process frames from stdin as documented


