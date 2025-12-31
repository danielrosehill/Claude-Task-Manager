[![Claude Code Project](https://img.shields.io/badge/Claude%20Code-Project-blue?style=flat-square&logo=anthropic)](https://github.com/danielrosehill/Claude-Code-Repos-Index)

# Claude Task Manager

Planning repository for a sequential task queuing system for Claude Code and other agentic AI coding tools.

## Status

This is a placeholder repository for gathering notes, requirements, and specifications as I explore building (or finding) tooling to solve a specific workflow problem.

## Planning Materials

The [planning/](planning/) folder contains:
- **[notes.mp3](planning/notes.mp3)** - Original voice notes capturing the vision and requirements
- **[verbatim-transcript.md](planning/verbatim-transcript.md)** - Raw transcription of the audio
- **[cleaned-transcript.md](planning/cleaned-transcript.md)** - Edited transcript with structure

I deliberately commit audio alongside transcripts as part of my workflow - anyone can listen to the original thinking and then read the derived specification.

## The Problem

When using AI coding assistants on multi-task projects, context window exhaustion and manual task handoff create friction. Parallel worker approaches (Git worktrees, multiple agents) introduce merge conflicts and rate limiting issues.

## The Goal

A sequential task queue that:
- Accepts freeform/voice input and parses it into discrete tasks
- Prioritizes fixes over features automatically
- Executes tasks one at a time with fresh context per task
- Works with Claude Code, Open Code, or other agents

See [SPEC.md](SPEC.md) for the full development specification.

---

*For more Claude Code projects, visit my [Claude Code Repos Index](https://github.com/danielrosehill/Claude-Code-Repos-Index).*
