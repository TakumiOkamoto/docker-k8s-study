# Handoff

## About This Repository

This is a private study repository for learning Docker and Kubernetes in a way that builds support-oriented understanding.

## User Intent

- Understand structure and configuration meaning, not only procedures
- Learn from a perspective closer to supporting systems than simply using them
- Track daily progress in Markdown
- Allow a different AI session to resume naturally tomorrow

## Expected AI Role

- Act as a trustworthy study partner
- Make the next step concrete
- Preserve progress notes, key points, and Q&A in Markdown
- Explain configuration files and manifests, not only commands
- Handle edits and local commits up to just before `git push`

## Important Rules

- Japanese docs use `*.ja.md`
- Every Markdown document must have both `.md` and `.ja.md`
- Major explanations should exist in both English and Japanese
- Learning questions should be added as Q&A to the relevant Markdown
- Commit messages must follow Conventional Commits in English
- The current stage must remain obvious through `STATUS.ja.md` and `STATUS.md`

## Progress So Far

- Study repository created
- GitHub remote configured
- Docker Desktop confirmed working
- Client-server architecture confirmed via `docker info` connection error troubleshooting
- `kubectl` and `kind` available
- Dockerfile exercise `01-hello` completed
- Docker bind mount exercise `02-nginx` completed
- Dockerfile build exercise `03-custom-nginx` completed
- Docker Compose basics exercise `04-compose` completed
- Advanced concepts like `k8s` vs `k3s` and DinD (Docker in Docker) documented in Q&A

## Current Stage

The Docker fundamentals and Docker Compose phases are complete. The next phase is "Kubernetes Fundamentals (Day 5)".

The next step is reading `notes/10-k8s-basics.md` and using `kind` to build a local cluster. The focus is:

- Understanding why Docker Compose has limits and why Kubernetes is necessary
- Grasping how `kind` (Kubernetes in Docker) uses DinD technology to simulate a cluster
- Recognizing the differences between Kubernetes distributions (`k8s`, `k3s`, `kind`, etc.)

The next session should resume with the user reading `notes/10-k8s-basics.md` or starting the `kind` cluster setup. There are no blocking errors.

## Files To Read First

1. `STATUS.ja.md` and `STATUS.md`
2. `README.ja.md` and `README.md`
3. `REPOSITORY_POLICY.ja.md` and `REPOSITORY_POLICY.md`
4. `AGENTS.md`
5. the latest `journal/` entries
6. `QA.ja.md` and `QA.md`
7. `USER_PROFILE.ja.md` and `USER_PROFILE.md`
8. `COLLABORATION_GUIDE.ja.md` and `COLLABORATION_GUIDE.md`
9. `ASKING_GUIDE.ja.md` and `ASKING_GUIDE.md`
10. `ENGINEER_GROWTH.ja.md` and `ENGINEER_GROWTH.md`
11. `CONVERSATION_HISTORY.ja.md` and `CONVERSATION_HISTORY.md`
