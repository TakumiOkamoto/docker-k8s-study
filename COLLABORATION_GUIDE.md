# Collaboration Guide

## Role Split

### AI

- organize the current learning stage
- propose the next step
- update Markdown files
- maintain Q&A
- add helper files when useful
- handle local commits

### User

- run commands that require local execution
- perform `git push`
- share command output when needed

## What Matters In This Collaboration

- the repository should be enough to understand the state without chat history
- understanding, not only progress, should remain documented
- questions should not get scattered and lost
- a new AI session should resume with a minimal opening message

## Files The AI Should Check Every Time

1. `START_HERE.ja.md` and `START_HERE.md`
2. `HANDOFF.ja.md` and `HANDOFF.md`
3. `STATUS.ja.md` and `STATUS.md`
4. `QA.ja.md` and `QA.md`
5. `USER_PROFILE.ja.md` and `USER_PROFILE.md`
6. `AGENTS.md`

## AI Behavior Standard

- do not let the user lose track of progress
- do not make the user repeat the same background
- preserve important assumptions in the repository
- keep support quality stable across sessions

## Maturity Level Of This Collaboration

Compared with typical public AI usage, this collaboration is at a relatively high level.

Why:

- it does not stop at isolated Q&A and instead preserves knowledge and progress in the repository
- it explicitly designs handoff for future AI sessions
- it tracks Q&A, handoff, current status, and user profile
- the work split between AI and user is clear
- the learning goal is defined at a support-engineer level, not just a beginner usage level

In practical terms, this is:

- well above average casual AI usage
- closer to using AI as a continuing work partner
- a fairly mature pattern for personal technical study

This is not a competitive ranking. The key point is repeatable quality across sessions, and this repository is already strongly aligned with that goal.
