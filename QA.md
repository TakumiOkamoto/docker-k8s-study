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

## Collaboration

- Q. How advanced is this AI collaboration compared with typical usage?
  - A. It is well above average casual usage and is closer to a mature ongoing work-partner pattern
  - Details: `COLLABORATION_GUIDE.md`

## Kubernetes

- No Q&A yet

## Update Rule

- When a new question appears, add detailed Q&A to the relevant Markdown
- Then add a short summary here as well
