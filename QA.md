# Q&A

This file is the repository-wide entrypoint for learning questions and answers.

## How To Use This File

- Detailed Q&A should live near the relevant exercise or note
- This file should keep a short summary and a reference
- New AI sessions should read this file to understand question history

## Docker

### `01-hello`

- Q. What is `alpine`?
  - A. A lightweight Linux distribution commonly used as a Docker base image
  - Details: `exercises/docker/01-hello/README.md`

- Q. How should `alpine` be pronounced in Japanese?
  - A. `Arupain` (`アルパイン`)
  - Details: `exercises/docker/01-hello/README.md`

### `02-nginx`

- Q. What is `nginx`?
  - A. Web server software that receives HTTP requests and returns content such as HTML
  - Details: `exercises/docker/02-nginx/README.md`

- Q. How should `nginx` be pronounced in Japanese?
  - A. `Engine-X` (`エンジンエックス`)
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Why was the standard output different on the first run versus later runs?
  - A. The first run often pulls `nginx:alpine`, while later runs reuse the local cached image
  - Details: `exercises/docker/02-nginx/README.md`

- Q. What does "local" mean here, and can I control where the image is stored?
  - A. On Docker Desktop for macOS, images live inside the Docker Desktop disk image, and you typically control the disk image location rather than a per-image folder
  - Details: `exercises/docker/02-nginx/README.md`

- Q. What does it mean that the image is "inside `Docker.raw`"?
  - A. `Docker.raw` looks like one file from macOS, but Docker Desktop uses it as a virtual disk that contains the Linux filesystem holding Docker data
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Is the Linux VM always created for Docker Desktop, and who decides its specs?
  - A. On Docker Desktop for macOS, Docker Desktop uses its own Linux VM for Linux containers, and CPU, memory, and disk are managed through Docker Desktop resource settings
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Is `-v` only for one file? What should I do when HTML depends on CSS and other files?
  - A. `-v` can mount either a file or a directory, and for multi-asset static content you typically mount the whole content directory
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Can HTML show both host-side and container-side ports at the same time?
  - A. The host-side URL port can be detected in the browser, while the container-side port is usually shown as known config because browser JS cannot directly inspect it
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Can I confirm which host-side and container-side files or directories are connected by `-v`?
  - A. Yes. `docker inspect` shows source and destination mount paths, and `docker exec` can confirm the mounted result from inside the container
  - Details: `exercises/docker/02-nginx/README.md`

- Q. How do I find the container ID?
  - A. Use `docker ps` first, and identify the right container by its published ports or name; in many cases the name is easier to reuse than the raw ID
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Are `IMAGE` and `NAMES` the same kind of information?
  - A. No. `IMAGE` tells you which image the container came from, while `NAMES` identifies the running container instance
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Besides `docker ps`, what Docker commands are standard for day-to-day work?
  - A. Start with `docker logs`, `docker inspect`, `docker exec`, `docker images`, `docker run`, and `docker stop` as the most common basics
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Does `In Use` in `docker images` mean the image is running right now?
  - A. Not necessarily; an image can be marked in use because a stopped container still references it
  - Details: `exercises/docker/02-nginx/README.md`

- Q. How do I remove the stopped `hello-world` containers?
  - A. First confirm them with `docker ps -a --filter ancestor=hello-world:latest`, then use either `docker rm <container-id>` or `docker rm $(docker ps -aq --filter ancestor=hello-world:latest)`
  - Details: `exercises/docker/02-nginx/README.md`

- Q. What does `--filter ancestor=` mean? How is it pronounced?
  - A. A Docker filter that narrows down to containers created from a specific image. `ancestor` means "parent" or "predecessor"; the image is the ancestor generation of the container
  - Details: `exercises/docker/02-nginx/README.md`

- Q. The nginx container is set up to serve `/usr/share/nginx/html/index.html` as the top-level HTML and start a Web server. Is my understanding correct that the container comes pre-built this way?
  - A. Your understanding is correct. `nginx:alpine` is a pre-built image that already has nginx installed and configured (via Dockerfile) to listen on port 80 and serve from `/usr/share/nginx/html/`
  - Details: `exercises/docker/02-nginx/README.md`

- Q. What if I want to create an nginx container that serves index.html from a different directory?
  - A. Two approaches: 1) bind mount to replace files dynamically (good for development), 2) write a Dockerfile and build a custom image (good for production, files embedded in image)
  - Details: `exercises/docker/02-nginx/README.md`

- Q. In `docker build -t my-custom-nginx:latest .`, what does the dot `.` mean?
  - A. It is the current directory used as build context, i.e., the file scope sent to Docker; `COPY` can only read within that scope
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. Why does `docker build -t my-custom-nginx:latest -f ../Dockerfile` fail with `requires 1 argument`?
  - A. `-f` only specifies Dockerfile path; a final build context argument (for example `.`) is still required. Use `docker build [OPTIONS] <CONTEXT>`
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. Why does `COPY index.html ...` fail with `"/index.html": not found`?
  - A. Because `index.html` is not in the selected build context. `-f` finds Dockerfile, but `COPY` can only read files inside context
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. In `transferring context: 2B`, does `2B` mean size?
  - A. Yes. It means 2 bytes of build-context data were sent to Docker, usually indicating an almost-empty context
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. What if files needed for build are scattered across multiple directories?
  - A. Temporary staging directory build is valid. In practice prefer repo-root context + `.dockerignore`, or explicit staging copies; advanced option is BuildKit `--build-context`. Copy-based staging is generally more reproducible than symlink-based assembly
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. Can files from one context be placed at different paths inside the container during build?
  - A. Yes. Use multiple `COPY <src> <dest>` lines; one context with multiple destination paths is the standard Dockerfile pattern
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. Can I control file permissions during build, or only copy the original permissions?
  - A. You can control permissions via `COPY --chown` (for ownership) or `RUN chmod`/`RUN chown` (for permissions); host permissions are not automatically preserved
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. What should I do if I see `ERROR: failed to connect to the docker API at unix:///Users/.../docker.sock`?
  - A. The Docker daemon (server) is not running. Start Docker Desktop so the "client" command can connect to it
  - Details: `exercises/docker/03-custom-nginx/README.md`

- Q. Is there a way to check the status of the Docker daemon from the terminal?
  - A. Run `docker version` to see if both `Client` and `Server` sections appear, or run `docker info` to check for detailed output
  - Details: `exercises/docker/03-custom-nginx/README.md`

## Docker References

- nginx:alpine Dockerfile structure and design
  - Details: `notes/nginx-dockerfile.md`
  - Official image structure, line-by-line explanation, daemon management reasoning, custom Dockerfile template

- Q. How do I see the default file state of a container?
  - A. Use `docker exec <container-id> ls /path` to see container directories. Use `docker inspect` for mount info, `docker stat` for file metadata
  - Details: `exercises/docker/02-nginx/README.md`

- Q. Does `docker exec` require the container to be running? And what does a bind-mounted file look like when viewed with `docker exec`?
  - A. Exactly. `docker exec` only works on running containers. Bind-mounted files show the current host-side content (changes propagate immediately because they are the same file)
  - Details: `exercises/docker/02-nginx/README.md`

## Collaboration

- Q. How advanced is this AI collaboration compared with typical usage?
  - A. It is well above average casual usage and is closer to a mature ongoing work-partner pattern
  - Details: `COLLABORATION_GUIDE.md`

## Kubernetes

- No Q&A yet

## Update Rule

- When a new question appears, add detailed Q&A to the relevant Markdown
- Then add a short summary here as well
