# Claude Task Manager

Planning repository for a sequential task queuing system for Claude Code and other agentic AI coding tools.

## Status

This is a placeholder repository for gathering notes, requirements, and specifications as I explore building (or finding) tooling to solve a specific workflow problem.

## The Problem

When using AI coding assistants on multi-task projects, context window exhaustion and manual task handoff create friction. Parallel worker approaches (Git worktrees, multiple agents) introduce merge conflicts and rate limiting issues.

## The Goal

A sequential task queue that:
- Accepts freeform/voice input and parses it into discrete tasks
- Prioritizes fixes over features automatically
- Executes tasks one at a time with fresh context per task
- Works with Claude Code, Open Code, or other agents

See [SPEC.md](SPEC.md) for the full development specification.
