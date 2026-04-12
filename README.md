# Docker + Kubernetes Study Repo

Private study repository for learning Docker and Kubernetes with daily notes, small exercises, and repeatable commands.

## Goal

Build hands-on understanding in this order:

1. Docker basics
2. Docker Compose
3. Kubernetes basics
4. Local cluster workflows

## Repo Layout

- `journal/`: daily progress logs
- `notes/`: topic notes and checklists
- `exercises/docker/`: Docker exercises
- `exercises/k8s/`: Kubernetes exercises
- `bin/`: helper scripts

## First Week

### Day 1

- Install Docker Desktop
- Run `docker run hello-world`
- Read `notes/00-setup.md`
- Write one entry in `journal/2026-04-12.md`

### Day 2

- Build the sample app in `exercises/docker/01-hello`
- Learn `docker build`, `docker run`, `docker logs`

### Day 3

- Run the nginx exercise in `exercises/docker/02-nginx`
- Learn port mapping and bind mounts

### Day 4

- Learn `docker compose`
- Add your own compose example

### Day 5

- Install `kubectl` and `kind`
- Read `notes/10-k8s-basics.md`

### Day 6

- Create a local cluster
- Apply `exercises/k8s/01-deployment/deployment.yaml`

### Day 7

- Review what was unclear
- Write a short weekly summary in `journal/`

## Commands

### Start here

```bash
cd ~/Develop/docker-k8s-study
```

### Docker check

```bash
docker --version
docker run hello-world
```

### Kubernetes check

```bash
kubectl version --client
kind version
```

## Daily Workflow

1. Do one exercise or read one note.
2. Write what you learned in `journal/YYYY-MM-DD.md`.
3. Commit with a small message.

## Suggested Commit Style

```bash
git add .
git commit -m "study: Docker hello-world and notes"
```

## Push To GitHub

After creating a private GitHub repository:

```bash
git remote add origin git@github.com:<your-account>/docker-k8s-study.git
git branch -M main
git push -u origin main
```

# User input
  - Opened Docker Desktop
  - Verified `docker info`
  - Ran `docker run hello-world`
  - Confirmed Docker is working
  - Verified `kubectl` is installed
  - Verified `kind` is installed
