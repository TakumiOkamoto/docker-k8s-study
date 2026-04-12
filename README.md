# Docker + Kubernetes Support Study Repo

Private study repository for learning Docker and Kubernetes from a support-engineer perspective.

This repository is not only for "running commands that work". It is for understanding:

- how the system is structured
- what each configuration file means
- what symptoms map to which layer
- how to inspect and explain behavior when something breaks

## Start Here

- Minimal session entry: `START_HERE.ja.md`
- Current stage: `STATUS.ja.md`
- Japanese overview: `README.ja.md`
- Handoff summary: `HANDOFF.ja.md`
- Repository policy: `REPOSITORY_POLICY.ja.md`
- AI collaborator instructions: `AGENTS.md`

## Learning Principle

Every exercise should answer these questions:

1. What is this component?
2. Where does it run?
3. Which configuration controls it?
4. How do I verify it?
5. What usually breaks here?

## Repository Layout

- `journal/`: daily progress logs
- `notes/`: conceptual notes in English and Japanese
- `exercises/`: small reproducible exercises
- `bin/`: helper scripts

## Current Focus

The current stage is:

1. Docker Desktop is working
2. Basic Docker image build is confirmed
3. Next step is nginx with port publishing and bind mounts
4. Then move to a local Kubernetes cluster with `kind`

## Commit Policy

Use Conventional Commits in English.

Examples:

- `docs: explain deployment selectors and service routing`
- `feat: add compose troubleshooting example`
- `fix: correct Docker bind mount note`
