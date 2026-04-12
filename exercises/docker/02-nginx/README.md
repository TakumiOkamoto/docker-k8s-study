# Docker Exercise 02

## Run

```bash
docker run --rm -p 8080:80 -v "$PWD/index.html:/usr/share/nginx/html/index.html:ro" nginx:alpine
```

Open <http://localhost:8080>.

## Learn

- Port publishing
- Bind mounts
- Reusing public images
