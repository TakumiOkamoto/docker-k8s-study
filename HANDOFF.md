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
- `docker info` run successfully
- `docker run hello-world` succeeded
- `kubectl` and `kind` available
- Dockerfile exercise `01-hello` completed
- Q&A about Alpine meaning and pronunciation recorded
- README, notes, and exercise docs updated for a support-engineer perspective

## Current Stage

The next step is `exercises/docker/02-nginx`. The focus is:

- showing a host file inside a container
- understanding host and container port mapping
- learning which layer to inspect when a problem occurs

To make resuming easier, `exercises/docker/02-nginx/README.md` now includes:

- pre-run checks
- post-run verification
- a layer-by-layer troubleshooting order

This sandbox cannot access the Docker socket, so live container verification is still pending here. The next session should resume by running the `02-nginx` `docker run` command on the user's machine and checking `localhost:8080`.

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
