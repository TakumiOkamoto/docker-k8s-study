# Docker Exercise 04: Docker Compose

## Purpose

Instead of typing long options (`-p 8080:80 -v ...`) every time with `docker run`, learn the basics of Infrastructure as Code (IaC) by describing the configuration in a `compose.yaml` file.

## What is compose.yaml?

Docker Compose is a tool for defining and running multi-container applications (now integrated into the Docker CLI as `docker compose`).
Even for running a single container, it is extremely useful because it allows you to save and share the startup parameters as a file.

## First Compose

In this exercise, we will recreate the nginx startup with bind mount from Exercise 02 using `compose.yaml`.

### 1. Preparation

Run the following command in this directory to confirm that `index.html` and `compose.yaml` exist.

```bash
ls -l
```

### 2. Startup (Up)

Start the container with the following command (`-d` means detached mode for background execution).

```bash
docker compose up -d
```

Open <http://localhost:8080> in your browser.

### 3. Verification

Check the list of running containers.

```bash
docker compose ps
```

### 4. Teardown (Down)

Stop and remove the started containers together.

```bash
docker compose down
```

## What Should You Master

You should be able to answer:

- What is the difference between `docker run` and `docker compose`?
- What are the benefits of using `compose.yaml`?
- Which `docker run` options do `ports` and `volumes` in `compose.yaml` correspond to?

## Next Steps

1. Move to the exercise directory: `cd ../04-compose`
2. Execute steps 1 through 4 of "First Compose" above on your local terminal
3. After execution, summarize your thoughts on what felt easier compared to `docker run`

## Q&A

### Q. Does `compose.yaml` need to be in the current directory when running `docker compose up`?
A. No. You can specify the file location using the `-f` (or `--file`) option.

```bash
docker compose -f /path/to/my-project/compose.yaml up -d
```

### Q. If I use `-f` from a different directory, how are relative paths in `volumes` (e.g., `./index.html`) resolved?
A. Relative paths are resolved **relative to the directory containing the `compose.yaml` file**, not the directory where you ran the command.

This is a brilliant design for Infrastructure as Code (IaC). Because the path `./index.html` is resolved relative to the compose file, it guarantees that "the index.html sitting next to the compose file" is mounted regardless of where the user runs the `docker compose -f` command from. It ensures **environment reproducibility independent of the working directory**.
