# Repository Policy

This is a private repository for continuous Docker and Kubernetes study.

## Goals

- Keep the current learning stage obvious
- Make the work reproducible later
- Work with AI in a way that makes the next step clear
- Build understanding suitable for someone who may later support Docker or Kubernetes

## Documentation Policy

- Every Markdown document must exist as both an English `.md` file and a Japanese `.ja.md` file
- Major concept explanations should be maintained in both languages
- The current stage summary lives in `STATUS.ja.md` and `STATUS.md`
- Daily detail lives in `journal/YYYY-MM-DD.md` and `journal/YYYY-MM-DD.ja.md`
- Topic notes live in `notes/`
- Small runnable examples live in `exercises/`
- Configuration files and manifests must be explained in Markdown

## Progress Visibility

The repository should remain understandable by checking:

1. `README.ja.md` and `README.md`
2. `STATUS.ja.md` and `STATUS.md`
3. The latest `journal/` entry in both languages

## Commit Policy

- Commit messages must follow Conventional Commits
- Commit messages must be written in English
- Prefer one clear intent per commit

## AI Collaboration Policy

- The AI should act as a strong study partner and make the next step concrete
- The AI should proactively update progress notes, summaries, and helper files
- The AI handles work up to just before `git push`
- The user shares local command output when needed
