# Claude Task Manager - Development Specification

## Overview

A sequential task queuing system for AI code generation agents (Claude Code, Open Code, etc.) that automates the process of executing multiple discrete coding tasks one after another, with fresh context for each task.

## Problem Statement

When using AI coding assistants on larger projects, users face several challenges:

1. **Context Window Exhaustion**: Each task consumes context from the first token, limiting effectiveness on multi-task sessions
2. **Manual Task Handoff**: Users must manually load the next task after each completion, defeating the purpose of automation
3. **Parallel Worker Problems**: Spawning multiple workers (via Git worktrees or similar) causes:
   - Merge conflicts from simultaneous codebase changes
   - Rate limiting (both explicit and implicit from providers like Anthropic)
   - Coordination overhead

## Core Philosophy

### Sequential Over Parallel

The system explicitly rejects parallel task execution in favor of sequential queuing. This ensures:
- Clean context windows for each task
- No merge conflicts
- Predictable rate limit usage
- Higher reliability per task

### Agent-Agnostic Orchestration

Following the Open Code philosophy: the orchestration/management layer should be separated from the code generation agent. The task manager should work with:
- Claude Code
- Open Code
- Other AI coding tools (e.g., "GLM 4.7")

### YOLO Mode Support

The target use case is personal/hobbyist projects where:
- Code doesn't need to be production-perfect
- Auto-accept all changes is acceptable
- The agent should attempt to fix any breakage it causes
- Minimal human intervention is desired

## Functional Requirements

### 1. Task Ingestion

**Voice/Freeform Input Processing**
- Accept unstructured input (voice transcriptions, freeform notes)
- Parse input into discrete, actionable tasks
- Example input: "The emojis are still in the prompt stack. There's a UX issue with shading. TTS announcements aren't working."
- Expected output: Three separate tasks with clear definitions

### 2. Task Categorization

Automatically categorize tasks into types:
- **Debugging/Fixes**: Broken functionality that needs repair
- **UI/UX**: Visual and interaction improvements
- **CSS/Styling**: Appearance-only changes
- **Feature Additions**: New functionality

### 3. Task Prioritization

Apply intelligent ordering based on best practices:

1. **Fixes First**: Anything broken gets priority
2. **Stability Second**: CSS/styling that doesn't risk breaking functionality
3. **Features Last**: New additions only after existing code is stable

Rationale: Don't add new features on top of broken code.

### 4. Sequential Queue Execution

- Execute one task at a time
- Wait for task completion before starting next
- Start each new task with fresh context (new conversation/session)
- Automate the "load task → execute → complete → load next" cycle

### 5. Agent Interface

The system must interface with the underlying code generation agent to:
- Pass task definitions
- Monitor task completion status
- Trigger new agent sessions with fresh context
- Operate in autonomous/YOLO mode (auto-accept changes)

## Non-Functional Requirements

### Reliability
- Must reliably detect task completion
- Must properly reset context between tasks
- Should handle agent failures gracefully

### Simplicity
- Minimal configuration required
- Should work with existing project structures
- No complex Git worktree setups

### Transparency
- Log task execution status
- Show queue progress
- Report any failures or issues

## Example Workflow

1. User reviews their project (e.g., walks through a web app)
2. User records voice notes: "Menu on storage page is ugly, needs horizontal layout. Nested items not displaying right. Add export button to reports page."
3. System parses into tasks:
   - Task 1 (UI/UX): Redesign storage page menu with horizontal layout
   - Task 2 (Fix): Fix nested item display
   - Task 3 (Feature): Add export button to reports page
4. System reorders by priority:
   - Task 1: Fix nested item display (fix first)
   - Task 2: Redesign storage page menu (UI second)
   - Task 3: Add export button (feature last)
5. System executes sequentially:
   - Launches agent with Task 1 → waits for completion
   - Launches fresh agent session with Task 2 → waits for completion
   - Launches fresh agent session with Task 3 → waits for completion
6. User returns to find all tasks completed

## Technical Considerations

### Context Reset Mechanism
The critical technical challenge is ensuring each task starts with a fresh context window. Options:
- Spawn new agent process per task
- Use agent-specific session reset commands
- Leverage agent SDK/API if available

### Completion Detection
Need reliable way to detect when agent has finished a task:
- Monitor agent output for completion signals
- Check for agent process exit
- Use agent-specific completion hooks

### Rate Limit Awareness
- Implement delays between tasks if needed
- Monitor for rate limit errors
- Back off and retry appropriately

## Prior Art / Inspiration

- **Open Code**: Liked for its separation of orchestration from agent
- **Claude Code**: Primary target agent, but system should be agent-agnostic
- **Git Worktrees approach**: Explicitly rejected due to conflict and rate limit issues

## Success Criteria

The system is successful if:
1. User can dump a batch of tasks and walk away
2. Tasks execute sequentially with fresh context each time
3. No manual intervention required between tasks
4. Works reliably with Claude Code in YOLO/auto-accept mode
5. Fixes are prioritized over feature additions automatically

## Open Questions

1. What's the best interface for task input? (CLI, file-based, voice API?)
2. How to handle task dependencies? (e.g., "after adding X, update Y to use it")
3. Should there be a review step before execution, or pure YOLO?
4. How to handle partial task completion or agent confusion?
5. Integration with existing tools vs. standalone solution?
