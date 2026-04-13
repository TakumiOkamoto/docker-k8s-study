# nginx:alpine Dockerfile

## Official Dockerfile Structure

The `nginx:alpine` image Dockerfile is very simple.

```dockerfile
FROM alpine:latest

RUN apk add --no-cache nginx

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

References:
- Official image: https://hub.docker.com/_/nginx
- Official repository: https://github.com/nginxinc/docker-nginx

## Line-by-Line Explanation

### `FROM alpine:latest`

- Uses `alpine:latest` as the base image
- Alpine Linux is a lightweight Linux distribution
- Results in smaller Docker image sizes (around 8-10 MB)

### `RUN apk add --no-cache nginx`

- `apk` = Alpine Linux package manager (similar to apt or yum)
- `add` = install package
- `--no-cache` = remove cache after installation to further reduce image size
- This means package manager cache metadata is not included in the final image

### `EXPOSE 80`

- "Declares" that the container listens on port 80
- Acts as documentation and a hint for `-p` flag in `docker run`
- Important: `EXPOSE` alone does not expose the port to the host; `-p` is required at runtime

### `CMD ["nginx", "-g", "daemon off;"]`

- Default command executed when the container starts
- `nginx` = start the nginx server process
- `-g "daemon off;"` = run nginx in foreground instead of background daemon mode
- Reason: container exits when its main process (PID 1) terminates. If nginx runs as a daemon, the main process would exit immediately, killing the container

## Default Configuration Already Included

`nginx:alpine` comes with these pre-configured paths and settings:

- nginx configuration file: `/etc/nginx/nginx.conf`
- document root (served files): `/usr/share/nginx/html`
- listening port: `80`
- running user: `nginx`

## Relationship to This Exercise

### Using the pre-built image as-is (Exercise 02)

```bash
docker run --rm -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine
```

- Use `nginx:alpine` Dockerfile unchanged
- Replace only the served content via bind mount

### Building your own custom image

```dockerfile
# Your own Dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/
COPY css/ /usr/share/nginx/html/css/
```

- Extend `nginx:alpine` as the base
- Add your own files with `COPY`
- Result: a new custom image

This pattern—using `FROM nginx:alpine` and extending it with your content—is the standard way to write production Dockerfiles.

## Image Size Comparison

| Image | Size | Reason |
|-------|------|--------|
| `nginx:alpine` | ~50-100 MB | lightweight Linux + minimal nginx |
| `nginx:latest` (Debian-based) | ~180-250 MB | Debian + extra tools included |
| `nginx:1.27-alpine` | ~50-100 MB | specific version + lightweight |

Alpine-based images are genuinely minimal.

## Important Note: Why `daemon off;`?

On a typical Linux system, nginx runs as `daemon on;` in the background:

```bash
# Normal Linux
systemctl start nginx
# nginx starts in background
```

But containers are different. When the container's main process (PID 1) exits, the entire container shuts down:

```
CMD ["nginx", "-g", "daemon off;"]
^
This command becomes PID 1

If we used daemon on;...
→ nginx would fork to background
→ CMD would complete and exit
→ No PID 1 → container would exit (not what we want)
```

For this reason, the nginx Dockerfile for containers always uses `daemon off;` to keep nginx in the foreground.

## Reference: Template for Custom Dockerfile

```dockerfile
FROM nginx:alpine

# If you have a custom nginx config
COPY nginx.conf /etc/nginx/nginx.conf

# Your content
COPY html/ /usr/share/nginx/html/

# Optional: explicitly set runtime user
USER nginx

# Optional: health check
HEALTHCHECK --interval=10s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --quiet --tries=1 --spider http://localhost/health || exit 1
```

This is roughly how you would customize and build your own image.
