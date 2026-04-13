# Docker Exercise 03

## Purpose

Write your own Dockerfile, build a custom nginx image, and understand the relationship between `docker build` and `docker run`.

Experience the difference from Exercise 02 (bind mount).

## How It's Built

- Uses `nginx:alpine` as the base image
- Embeds the local `index.html` file into the image during build
- Creates a custom image `my-custom-nginx`
- Run the custom image to serve content

## Build

```bash
docker build -t my-custom-nginx:latest .
```

Once built, the image appears in `docker images`:

```bash
docker images | grep my-custom-nginx
```

## Run

```bash
docker run --rm -p 8080:80 my-custom-nginx:latest
```

Open <http://localhost:8080> in your browser.

## Before Running

- Are you in `exercises/docker/03-custom-nginx`?
- Do `Dockerfile` and `index.html` exist in the same directory?
- Is port `8080` on the host already in use?

Verify with:

```bash
pwd
ls -l Dockerfile index.html
```

## Build Configuration Explained

### Dockerfile

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
```

- `FROM nginx:alpine`
  - Uses `nginx:alpine` as the base image
  - Means we pull this public image and build on top of it
- `COPY index.html /usr/share/nginx/html/`
  - Copies your local `index.html` into the container's nginx serving directory
  - Executed at build time (not runtime)
  - File is embedded in the image

### docker build Command

```bash
docker build -t my-custom-nginx:latest .
```

- `-t my-custom-nginx:latest`
  - The name (tag) for the resulting image
  - `my-custom-nginx` is the image name
  - `latest` is the version tag
- `.`
  - The Dockerfile location (current directory = build context)

After build completes, verify with `docker images`.

### docker run Command

```bash
docker run --rm -p 8080:80 my-custom-nginx:latest
```

- `my-custom-nginx:latest`
  - The image to run
  - In Exercise 02, this was `nginx:alpine` (a public image)
  - Here, it's the custom image you just built

## What Should You Master

- Understand the difference between `docker build` and `docker run`
- Know when to use build vs bind mount
- Recognize the trade-offs: build time vs reproducibility

## Comparison with Exercise 02

### Exercise 02: Bind Mount (Development)

```bash
docker run --rm -p 8080:80 \
  -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" \
  nginx:alpine
```

Characteristics:

- Uses pre-built `nginx:alpine` as-is
- `-v` references host files at runtime
- Host file changes reflect immediately in the container
- No image build required

### Exercise 03: Build (Production)

```bash
# 1. Write Dockerfile
# FROM nginx:alpine
# COPY index.html /usr/share/nginx/html/

# 2. Build image
docker build -t my-custom-nginx:latest .

# 3. Run image
docker run --rm -p 8080:80 my-custom-nginx:latest
```

Characteristics:

- Build a custom image
- Files are embedded in the image (at build time)
- Image can be pulled on other machines to recreate the same setup
- Content changes require rebuild
- Ideal for production and team sharing

### Decision Matrix

| Use Case | Recommended | Reason |
|----------|-------------|--------|
| Active development, frequent file changes | Exercise 02 (bind mount) | Changes visible immediately, fast iteration |
| Production, stability required | Exercise 03 (build) | Configuration is fixed, high reproducibility |
| Team sharing, environment parity | Exercise 03 (build) | Same image → identical environment everywhere |
| Multiple versions in parallel | Exercise 03 (build) | Image tags provide version control |

## Next Steps to Explore

### 1. Check Image Size

```bash
docker images my-custom-nginx
```

Output:

```
REPOSITORY          TAG       IMAGE ID     CREATED        SIZE
my-custom-nginx     latest    abc123def    2 minutes ago   95MB
nginx:alpine        latest    xyz789uvw    2 weeks ago     91MB
```

`my-custom-nginx` is slightly larger than `nginx:alpine` (due to the added `index.html`).

### 2. Inspect Image Details

```bash
docker image inspect my-custom-nginx:latest
```

Displays JSON with image configuration, layers, environment variables, etc.

### 3. Review Build Output

```bash
docker build -t my-custom-nginx:v2 --progress=plain .
```

Shows each build step, execution time, and cache usage.

### 4. Extend the Dockerfile

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
COPY css/ /usr/share/nginx/html/css/
COPY js/ /usr/share/nginx/html/js/
```

Add multiple files or directories and rebuild.

### 5. Add Custom nginx Config

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
COPY nginx.conf /etc/nginx/nginx.conf
```

Customize and COPY a custom nginx.conf.

## Q&A

### Q. In `docker build -t my-custom-nginx:latest .`, what does the dot `.` mean?

A. `.` means the current directory, and in Docker it is used as the **build context**.

Three key implications:

- Files under the directory specified by `.` are sent to Docker for build
- If `-f` is omitted, Docker uses `./Dockerfile` by default
- `COPY` can only reference files inside the build context

So if you run this inside `exercises/docker/03-custom-nginx`:

```bash
docker build -t my-custom-nginx:latest .
```

the whole directory becomes the context, and `COPY index.html ...` reads from that scope.

Notes:

- Large contexts make builds slower
- Exclude unnecessary files with `.dockerignore`
- You can replace `.` with another path to change the context

Examples:

```bash
# use parent directory as context
docker build -t my-custom-nginx:latest ..

# explicitly specify Dockerfile while keeping current directory as context
docker build -f Dockerfile -t my-custom-nginx:latest .
```

Learning points:

- `.` is not only where you run the command, but also the file scope sent to Docker
- Many `COPY` errors are caused by using the wrong context

### Q. Why does `docker build -t my-custom-nginx:latest -f ../Dockerfile` fail with `requires 1 argument`?

A. Because `-f` only points to the Dockerfile location. You still must pass a separate **build context** argument (PATH | URL | -).

Your command is missing the final context, so buildx reports `requires 1 argument`.

Incorrect (missing context):

```bash
docker build -t my-custom-nginx:latest -f ../Dockerfile
```

Correct forms (add context at the end):

```bash
# Dockerfile is ../Dockerfile, context is current directory
docker build -t my-custom-nginx:latest -f ../Dockerfile .

# Dockerfile and context are both parent directory
docker build -t my-custom-nginx:latest -f ../Dockerfile ..
```

Rule to remember:

```bash
docker build [OPTIONS] <CONTEXT>
```

- `-f` does not replace `<CONTEXT>`
- `COPY` can only read files inside `<CONTEXT>`
- Run `pwd` first to verify your current location before building

### Q. What's the difference between `docker build` and `docker run`?

A. They are fundamentally different stages:

- `docker build`
  - Creates an image from a Dockerfile
  - Files are embedded into the image
  - A creation/assembly phase
  - Result: image file (OCI image format)

- `docker run`
  - Creates and starts a container from an image
  - Image remains unchanged; a new container instance is created
  - An execution phase
  - Result: a running container process

Conceptually:

```
Dockerfile
    ↓ (docker build)
image (template, filesystem snapshot)
    ↓ (docker run)
container (running process + isolated filesystem)
```

### Q. Why do image and container sizes differ?

A. An image is a filesystem snapshot; a container adds an execution layer and runtime state.

- Size in `docker images`
  - Compressed image size (sum of layers)
  - Offline, not running

- Size in `docker ps` or statistics
  - Container's write layer (changes) + image reference
  - Runtime memory is shown separately in `docker stats`

### Q. Can I run multiple containers from the same image?

A. Absolutely.

```bash
# Three containers from the same image
docker run --rm -p 8080:80 my-custom-nginx:latest
docker run --rm -p 8081:80 my-custom-nginx:latest
docker run --rm -p 8082:80 my-custom-nginx:latest
```

Run all three in separate terminals; access them at 8080, 8081, 8082 respectively.

### Q. How do I delete an image?

A. Use `docker rmi`:

```bash
docker rmi my-custom-nginx:latest
```

Note:

- Cannot delete if running containers reference the image
- Must stop and remove containers first:

```bash
docker ps | grep my-custom-nginx
docker stop <container-id>
docker rmi my-custom-nginx:latest
```

### Q. Why does Docker use cache during builds?

A. Build optimization. Each Dockerfile line (RUN, COPY, etc.) becomes a layer that Docker caches. Unchanged lines are skipped in subsequent builds.

Example:

```dockerfile
FROM nginx:alpine         # ← layer 1 (usually cache hit)
RUN apk add --no-cache... # ← layer 2 (usually cache hit)
COPY index.html ...       # ← layer 3 (hit if file unchanged)
```

Skip cache entirely:

```bash
docker build --no-cache -t my-custom-nginx:latest .
```

### Q. What is a tag?

A. A version label for an image. The same repository can have multiple versions.

```bash
docker build -t my-custom-nginx:v1 .
docker build -t my-custom-nginx:v2 .

docker images | grep my-custom-nginx

# Output
# my-custom-nginx   v1    abc123
# my-custom-nginx   v2    def456
```

Run specific versions:

```bash
docker run --rm -p 8080:80 my-custom-nginx:v1
docker run --rm -p 8081:80 my-custom-nginx:v2
```

Convention:

- `latest` = newest version (development)
- `v1.0`, `v1.1` ... = release versions (pinned by tag)

### Q. Can I upload a built image to Docker Hub?

A. Yes, using `docker push`:

```bash
# 1. Have a Docker Hub account and be logged in
docker login

# 2. Build with Hub-style naming
docker build -t <username>/my-custom-nginx:latest .

# 3. Push
docker push <username>/my-custom-nginx:latest

# 4. On another machine, pull and run
docker run --rm -p 8080:80 <username>/my-custom-nginx:latest
```

The identical configuration is reproduced on any machine. This is Docker's key strength: `docker build` + `docker push/pull` enables environment sharing.

This exercise doesn't require going this far, but image sharing is how Docker is typically used in teams.
