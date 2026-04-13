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

### Q. Can HTML show both the host-side port and the container-side port at the same time?

A. Yes, for learning purposes you can show both, but they are different kinds of information.

- Host-side port
  - can be read from the browser URL (`window.location.port`)
- Container-side port
  - is not directly discoverable by browser JavaScript in normal setups
  - should be shown as an expected configuration value (for this exercise, `80`)

So this is usually not "both values measured live".

- one is observed at runtime (host-side URL port)
- one is displayed from known container config

The exercise `index.html` now demonstrates this pattern and visualizes the mapping idea as `host 8080 -> container 80`.

### Q. Using the same idea, can I confirm which host-side and container-side files or directories are connected by `-v`?

A. Yes, but this is something you confirm with Docker CLI rather than from the browser.

The clearest source is `docker inspect`, especially the `Mounts` section.

Typical flow:

```bash
docker ps
docker inspect <container-id>
```

In `Mounts`, you can verify at least:

- the host-side path (`Source`)
- the container-side path (`Destination`)
- the mount type (`Type`)
- whether it is read-only or read-write (`RW`)

If you want a shorter view:

```bash
docker inspect <container-id> --format '{{range .Mounts}}{{println .Type .Source "->" .Destination "RW=" .RW}}{{end}}'
```

You can also confirm from inside the container:

```bash
docker exec <container-id> ls -l /usr/share/nginx/html
```

With a file mount, you will see where that single file appears in the container. With a directory mount, you will see the mounted directory contents.

However, browser-side HTML or JavaScript usually cannot discover the host-side source path. This is similar to the port example: the browser can see the served result, but Docker manages the host-to-container binding details.

In short:

- browser
  - can see content returned by the container
  - cannot normally see the host-side source path
- Docker CLI
  - can show the host/container mount mapping
- inside the container
  - you can see the mounted result as files or directories

### Q. How do I find the `container-id`?

A. The most basic way is `docker ps`.

```bash
docker ps
```

This shows running containers, including:

- `CONTAINER ID`
- `NAMES`
- `PORTS`

For the nginx exercise, you can usually identify the right row by the published port, such as `0.0.0.0:8080->80/tcp`, and then use that row's `CONTAINER ID`.

Example:

```bash
docker inspect 1a2b3c4d5e6f
docker exec 1a2b3c4d5e6f ls -l /usr/share/nginx/html
```

You can also use the container name instead of the ID.

```bash
docker exec <container-name> ls -l /usr/share/nginx/html
```

If you want an easier name, assign one at startup.

```bash
docker run --rm --name study-nginx -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine
```

Then you can use `study-nginx` with `docker inspect` or `docker exec`.

If you only want raw IDs, this is also useful:

```bash
docker ps -q
```

But if multiple containers are running, `docker ps` is safer because it also shows names and ports.

### Q. `IMAGE` and `NAMES` looked like the same kind of information. Is that correct?

A. No. They can look similar in simple examples, but they mean different things.

- `IMAGE`
  - which image the container was created from
  - example: `nginx:alpine`
- `NAMES`
  - the name of the running container instance itself
  - example: `study-nginx` or an auto-generated Docker name

So:

- `IMAGE` is closer to the blueprint or source
- `NAMES` is the identity of the running instance

You can start multiple containers from the same image, so `IMAGE` can be the same while `NAMES` differs.

Example:

```bash
docker run --rm --name web-a -p 8080:80 nginx:alpine
docker run --rm --name web-b -p 8081:80 nginx:alpine
```

In that case:

- `IMAGE` is `nginx:alpine` for both
- `NAMES` is `web-a` and `web-b`

When targeting a specific running container with `docker exec` or `docker inspect`, you normally use `NAMES` or `CONTAINER ID`. `IMAGE` is mainly there to tell you what kind of container it is.

### Q. Besides `docker ps`, what other standard Docker commands do people use all the time?

A. Yes. These are the main day-to-day commands worth learning first.

State and listing:

- `docker ps`
  - show running containers
- `docker ps -a`
  - show all containers, including stopped ones
- `docker images`
  - show local images

Inspecting behavior and contents:

- `docker logs <name-or-id>`
  - show container stdout and stderr
- `docker inspect <name-or-id>`
  - show detailed config such as ports, mounts, and networks
- `docker exec -it <name-or-id> sh`
  - open a shell inside the container

Lifecycle:

- `docker run ...`
  - start a new container
- `docker stop <name-or-id>`
  - stop a running container
- `docker rm <name-or-id>`
  - remove a stopped container

Image operations:

- `docker build -t <image-name> .`
  - build an image from a Dockerfile
- `docker pull <image>`
  - download an image

For this exercise, the first six to internalize are:

```bash
docker ps
docker logs <name-or-id>
docker inspect <name-or-id>
docker exec -it <name-or-id> sh
docker images
docker run ...
```

The mental model is:

- `run`
  - create and start
- `ps`
  - see what is running now
- `logs`
  - read startup messages and errors
- `inspect`
  - verify ports, mounts, and networks as facts
- `exec`
  - look from inside the container
- `images`
  - see the source images

If these six become natural, you can already handle a large part of basic Docker work.

### Q. In `docker images`, does `In Use` or `U` mean that the image is currently running?

A. Not necessarily. It is safer to read `In Use` as: some container still references that image.

That means:

- it may be used by a running container
- or it may still be referenced by a stopped container

On this Mac, `hello-world` was marked `In Use`, but it was not running. The check showed this:

```bash
docker ps -a --filter ancestor=hello-world:latest
```

Result:

- there were three `hello-world` containers left
- all were `Exited (0)`
- so the image was still referenced, but not actively running

By contrast, `nginx:alpine` had one container in `Up` state, so that image really was in active use by a running container.

The practical reading pattern is:

- `docker images`
  - broad view of whether an image is in use
- `docker ps`
  - check what is running right now
- `docker ps -a`
  - check which running or stopped containers still reference the image

If you want to clean up stopped `hello-world` containers, for example:

```bash
docker ps -a --filter ancestor=hello-world:latest
docker rm <container-id>
```

If there are several, you can remove them after confirming the targets.

### Q. How do I actually remove the stopped `hello-world` containers?

A. The safest way is to list them first and then remove them with `docker rm`.

Check first:

```bash
docker ps -a --filter ancestor=hello-world:latest
```

On this Mac, the current stopped containers are:

- `70b6ede64a52`
- `2ab3e021c407`
- `537bd7d759a7`

To remove them one by one:

```bash
docker rm 70b6ede64a52
docker rm 2ab3e021c407
docker rm 537bd7d759a7
```

To remove all matching stopped containers at once:

```bash
docker rm $(docker ps -aq --filter ancestor=hello-world:latest)
```

Then verify:

```bash
docker ps -a --filter ancestor=hello-world:latest
docker images
```

Important distinction:

- `docker rm`
  - removes containers
- `docker rmi`
  - removes images

For learning, it is useful to remove the containers first and then observe how the `In Use` status for `hello-world` changes.

Note:

- `docker rm` is for stopped containers
- if a container is still running, stop it first with `docker stop <name-or-id>`
