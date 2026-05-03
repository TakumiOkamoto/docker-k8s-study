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

## Completed (Kubernetes Fundamentals Phase)

- Read `notes/10-k8s-basics.md` - grasped terminology and mental model
- Understood K8s distributions (`k8s`, `k3s`, `kind`)
- Created local cluster with `kind create cluster --name study`
- Applied first deployment: `kubectl apply -f exercises/k8s/01-deployment/deployment.yaml`
- Verified Pod creation and Service configuration
- Explored logging architecture: `kubectl logs`, `kubectl describe pod`
- Understood three log layers: app logs, Pod events, node OS logs
- Learned kubeconfig structure: cluster, user, context

## Current Understanding

- Deployment declaratively manages Pod replicas
- Service provides stable ClusterIP for Pod groups
- Pod names are generated with hash suffixes (ReplicaSet ID + random)
- Logs come from container stdout/stderr, not files
- Pod events show lifecycle and status changes

## Next Actions (Continued Kubernetes Learning)

1. Explore Pod deletion and Deployment recreation behavior
2. Test Service networking from another Pod
3. Scale Deployment replicas and observe behavior
4. Create multiple Deployments and Services to understand routing
5. Move to multi-namespace exercises
