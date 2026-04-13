# Current Status

## Current Stage

Docker exercises in progress:
- Exercise 01: `hello-world` for basic concepts
- Exercise 02: `nginx:alpine` for bind mounts and port publishing
- Exercise 03: **New** Writing Dockerfile and building custom images

Next: understanding build vs bind mount trade-offs, then Kubernetes fundamentals.

## Completed

### Docker Exercises

- `exercises/docker/01-hello/` - base concepts with Alpine image
- `exercises/docker/02-nginx/` - image customization via bind mount
  - port publishing (`-p 8080:80`)
  - file mounting (`-v`)
  - separating host / container / port-forwarding layers
- **`exercises/docker/03-custom-nginx/`** - 🆕 Writing and building custom Dockerfile
  - Dockerfile creation and structure
  - `docker build` image generation
  - build vs bind mount trade-offs
  - image and container relationship

### Reference & Documentation

- `notes/nginx-dockerfile.ja.md / .md`
  - Official nginx:alpine Dockerfile explanation
  - each line's purpose, daemon off necessity
  - image sizes, custom Dockerfile template
