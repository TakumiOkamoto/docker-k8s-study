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
