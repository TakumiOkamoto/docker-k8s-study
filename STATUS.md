# Current Status

## Current Stage

Docker exercise progress:
- Exercise 01: `hello-world` basics (completed)
- Exercise 02: `nginx:alpine` bind mount and port publishing (completed)
- Exercise 03: writing Dockerfile and building custom images (completed)
- Exercise 04: managing multiple containers with Docker Compose (completed)
- Phase transition: Kubernetes Fundamentals (started)

Current phase is transitioning to Kubernetes fundamentals (Day 5). Connecting the structural learning from Docker to the Kubernetes ecosystem.

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
- `exercises/docker/04-compose/` - Docker Compose basics (completed)
  - `compose.yaml` structure and meaning

### Reference & Documentation

- `notes/nginx-dockerfile.ja.md / .md`
  - Official nginx:alpine Dockerfile explanation
  - each line's purpose, daemon off necessity
  - image sizes, custom Dockerfile template

## Next Actions (Kubernetes Fundamentals)

1. Read `notes/10-k8s-basics.md` to grasp the big picture and terminology of Kubernetes
2. Understand the differences between K8s distributions like k8s, k3s, and kind
3. Verify the installation status and versions of `kubectl` and `kind`
4. Create local cluster with `kind create cluster --name study` (completed)
5. Apply first deployment: `kubectl apply -f exercises/k8s/01-deployment/deployment.yaml`

## Completion Criteria (at Kubernetes Fundamentals start)

- Understand the problem K8s solves (why Docker Compose isn't always enough)
- Can stand up a local cluster using `kind`
