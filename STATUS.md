# Current Status

## Current Stage

Docker exercise progress:
- Exercise 01: `hello-world` basics (completed)
- Exercise 02: `nginx:alpine` bind mount and port publishing (completed)
- Exercise 03: writing Dockerfile and building custom images (started)

Current phase is Exercise 03. Next is to verify build-vs-bind-mount behavior hands-on, then move into Kubernetes fundamentals.

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

## Next Actions (Exercise 03)

1. Run `docker build -t my-custom-nginx:latest .` in `exercises/docker/03-custom-nginx`
2. Run `docker run --rm -p 8080:80 my-custom-nginx:latest`
3. Verify the page at `localhost:8080`
4. Explain the difference from Exercise 02: bind mount updates immediately, build requires rebuild
5. Optionally inspect with `docker image inspect my-custom-nginx:latest`

## Completion Criteria (at Exercise 03 start)

- Verified page is served from the built image
- Can explain Exercise 02 vs 03 in three lines
- New questions are captured in `03-custom-nginx` README and `QA.md`
