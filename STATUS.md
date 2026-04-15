# Current Status

## Current Stage

Docker exercise progress:
- Exercise 01: `hello-world` basics (completed)
- Exercise 02: `nginx:alpine` bind mount and port publishing (completed)
- Exercise 03: writing Dockerfile and building custom images (completed)
- Exercise 04: managing multiple containers with Docker Compose (started)

Current phase is Exercise 04. Learning the basics of Infrastructure as Code (IaC) using Docker Compose.

## Completed

### Docker Exercises

- `exercises/docker/01-hello/` - base concepts with Alpine image
- `exercises/docker/02-nginx/` - image customization via bind mount
  - port publishing (`-p 8080:80`)
  - file mounting (`-v`)
  - separating host / container / port-forwarding layers
- `exercises/docker/03-custom-nginx/` - Writing and building custom Dockerfile
  - Dockerfile creation and structure
  - `docker build` image generation
  - build vs bind mount trade-offs
  - image and container relationship
- **`exercises/docker/04-compose/`** - 🆕 Docker Compose basics
  - `compose.yaml` structure and meaning

### Reference & Documentation

- `notes/nginx-dockerfile.ja.md / .md`
  - Official nginx:alpine Dockerfile explanation
  - each line's purpose, daemon off necessity
  - image sizes, custom Dockerfile template

## Next Actions (Exercise 04)

1. Create `exercises/docker/04-compose` directory
2. Create your first `compose.yaml`
3. Experience `docker compose up` and understand why it is useful compared to `docker run`

## Completion Criteria (at Exercise 04 start)

- Understand difference and relationship between `docker run` and `docker compose`
- New questions are captured in `04-compose` README and `QA.md`
