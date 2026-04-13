# Docker Exercise 02

## Goal

Understand port publishing and bind mounts by serving a local HTML file through an nginx container.

## How It Is Structured

- `nginx:alpine` is the prebuilt web server image
- `index.html` is a local file on the host
- `-v` mounts that file into the container
- `-p 8080:80` maps a host port to a container port

## Run

```bash
docker run --rm -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine
```

Open <http://localhost:8080>.

## What To Check Before Running

- confirm you are in `exercises/docker/02-nginx`
- confirm the source file `index.html` exists there
- confirm host port `8080` is not already in use

Quick checks:

```bash
pwd
ls -l index.html
```

## What This Configuration Means

- `nginx:alpine`
  - Reuses an existing nginx image instead of building one first
- `-p 8080:80`
  - Host port `8080` forwards traffic to container port `80`
- `-v "$PWD/index.html:/usr/share/nginx/html/index.html:ro"`
  - The local file replaces nginx's default page inside the container
  - `:ro` means the mount is read-only
- `--rm`
  - The container is removed after exit

## Support Perspective

Questions this exercise should answer:

- Is the issue on the host side, the container side, or the port mapping layer?
- If the page is wrong, is the mount path correct?
- If the page does not open, is nginx listening and is the port published correctly?

## Where Each Part Lives

- Host
  - owns the local `index.html`
  - sends browser traffic to `localhost:8080`
- Container
  - runs nginx and listens on port `80`
  - serves `/usr/share/nginx/html/index.html`
- Docker port publishing
  - forwards host port `8080` to container port `80`

This single command is useful because it touches all three layers at once.

## What To Verify After Running

1. Check that nginx did not print startup errors in the terminal.
2. Open <http://localhost:8080>.
3. Confirm the page content matches `index.html`.

If you want a second terminal for checks:

```bash
docker ps
docker logs <container-id>
```

## Troubleshooting Order

### 1. The page does not open

- confirm `docker run` is still running
- confirm `-p 8080:80` was included
- confirm host port `8080` is not already occupied

### 2. nginx opens, but the page is not your HTML

- confirm `"$PWD/index.html"` points to the right file
- confirm the mount target `/usr/share/nginx/html/index.html` is correct
- confirm you launched the command from the intended directory

### 3. The container runs, but updates do not appear

- confirm the mount is the file you are editing
- confirm you are not seeing a browser cache issue
- confirm you changed the host file, not a copy elsewhere

## Done When

- the browser shows the content from `index.html`
- you can explain `-p 8080:80` in your own words
- you can explain that a bind mount exposes a host file rather than copying it into the image
- you can describe the order for checking host, container, and port-publishing issues

## Q&A

### Q. What is `nginx`?

A. `nginx` is web server software. It receives HTTP requests from a browser and returns content such as HTML files.

In this exercise, `nginx:alpine` means the container uses the published nginx image directly from Docker Hub.

Key points in this context:

- `nginx` is the web server running inside the container
- it listens on port `80`
- it serves files from `/usr/share/nginx/html/` by default
- in this exercise, that `index.html` is replaced through a bind mount

So this is not an exercise about building nginx itself. It is an exercise about using a ready-made web server to understand port publishing and mounts.

### Q. How should `nginx` be pronounced in Japanese?

A. In Japanese technical conversation, `Engine-X` (`エンジンエックス`) is the natural pronunciation.

Notes:

- it is written as `N-G-I-N-X`
- saying `エンジンエックス` is clearer than trying to read the letters literally
- it is not usually pronounced like `ンギンクス` in Japanese conversation

### Q. Why was the standard output different on the first run versus later runs?

A. The main reason is that the first run usually has to download the `nginx:alpine` image.

`docker run nginx:alpine` has two broad phases internally:

1. check whether the image already exists locally
2. start a container from that image

On the first run, if `nginx:alpine` is not present locally, Docker pulls it from the registry. That is why you often see output such as:

- `Unable to find image ... locally`
- `Pulling from library/nginx`
- layer download and extract progress

On later runs, if the image is already cached locally, Docker skips the pull step. The output is then mostly just from the container startup phase.

What to understand from this exercise:

- the first run is often "image download + container start"
- later runs are often just "container start"
- the output difference usually does not mean nginx changed, only that the earlier image-fetch phase was no longer needed

Useful checks:

```bash
docker images
docker image inspect nginx:alpine
```

### Q. What does "local" mean here? Where is the image actually downloaded, and can I control it?

A. On Docker Desktop for macOS, images are not usually unpacked directly into a normal host folder. They are stored inside the large disk image file used by Docker Desktop's Linux VM.

On this Mac, the visible host-side file is:

```text
/Users/tapacchi/Library/Containers/com.docker.docker/Data/vms/0/data/Docker.raw
```

So `nginx:alpine` is effectively stored inside that `Docker.raw` file.

Control differs by environment:

- Docker Desktop on macOS
  - you normally do not choose a separate folder per image
  - instead, you can move the overall disk image using Docker Desktop's `Disk image location` setting
- Docker Engine on Linux
  - you can change the daemon storage directory with `data-root`
  - the common default is `/var/lib/docker`

The important idea is that on Docker Desktop for macOS, you usually manage the location of the whole Docker disk image, not the storage path of each individual image.

### Q. What does it mean that the image is "inside `Docker.raw`"?

A. From macOS, `Docker.raw` looks like one large file. From Docker Desktop's Linux VM, it is used as a virtual disk.

Inside that virtual disk is a Linux filesystem, and that filesystem stores Docker image layers, container writable layers, and volumes.

Conceptually:

```text
macOS
└─ Docker.raw
   └─ virtual disk used by Docker Desktop
      └─ Linux filesystem
         ├─ image layers
         ├─ container writable layers
         └─ volumes
```

So `nginx:alpine` is not visible as a normal host folder. It lives inside the Docker-managed Linux storage used by Docker Desktop.

### Q. Is the Linux VM always created for Docker Desktop, and who decides its specs?

A. On Docker Desktop for macOS, Docker Desktop uses a Linux VM to run Linux containers.

The reason is simple: Linux containers expect a Linux kernel, but the host here is macOS. Docker Desktop therefore provides a Linux environment as a VM, and Docker Engine plus containers run inside it.

The simplified flow looks like this:

```text
macOS terminal
-> docker CLI
-> Docker Desktop
-> Docker Desktop managed Linux VM
-> Docker Engine
-> containers
```

About sizing:

- Docker Desktop chooses the initial defaults
- the user can adjust CPU, memory, swap, and disk in Docker Desktop Resources settings

On this Mac, `docker context ls` shows `desktop-linux`, which indicates the Docker Desktop managed Linux environment is in use.

### Q. Is `-v` only for passing one file? What if HTML uses CSS and other files?

A. `-v` can mount either a single file or a whole directory. In this exercise, the command uses a file mount, so only `index.html` is injected.

If `index.html` references `style.css` or `app.js`, you need one of these approaches:

- mount each additional file explicitly
- mount the entire content directory

For static websites, mounting the whole directory is usually simpler.

Example:

```bash
docker run --rm -p 8080:80 -v "$PWD:/usr/share/nginx/html:ro" nginx:alpine
```

With this pattern, relative paths from `index.html` (such as `./style.css`) resolve naturally.

In short:

- file mount is good when you want to replace exactly one file
- directory mount is better when you want to serve multiple related assets together
