# Claude Code: Subagents & Agent Teams

## Overview

Claude Code provides two mechanisms for multi-agent workflows:

| Feature | Status | Use in Production? |
|---------|--------|--------------------|
| **Subagents** | Stable | Yes |
| **Agent Teams** | Experimental | Not yet recommended |

---

## Subagents (Stable)

Subagents are specialized agents defined as Markdown files with YAML frontmatter. They run as child processes within a Claude Code session, each with their own system prompt, tool restrictions, and permission modes.

### Built-in Subagents

Claude Code ships with several built-in subagents:

- **Explore** - Fast codebase exploration (file search, keyword search, structural questions)
- **Plan** - Software architecture and implementation planning

### How They Work

1. The parent Claude Code session spawns a subagent via the `Agent` tool
2. The subagent runs in its own context window with a focused task
3. It returns a single result message when complete
4. The parent session incorporates the result and continues

### Defining a Custom Subagent

Create a `.md` file with YAML frontmatter specifying the agent's configuration:

```markdown
---
name: api-developer
description: Handles API endpoint development following team conventions
tools:
  - Read
  - Edit
  - Write
  - Bash
  - Grep
  - Glob
model: sonnet
---

You are a specialized API developer agent. Follow these conventions:

- All endpoints go in `src/api/`
- Use the repository's established error handling patterns
- Write OpenAPI annotations for every new endpoint
- Include input validation at the boundary layer
```

### Key Properties

- **Isolated context**: Each subagent gets its own context window, protecting the parent from information overload
- **Tool restrictions**: You can limit which tools a subagent can access
- **Model selection**: Subagents can use a different model than the parent (e.g., use `haiku` for simple lookups, `opus` for complex reasoning)
- **Parallel execution**: Multiple subagents can run concurrently for independent tasks

### When to Use Subagents

- **Research tasks** that would flood the parent context with search results
- **Focused subtasks** that benefit from a specialized system prompt
- **Parallel work** on independent parts of a problem
- **Repetitive operations** across multiple files or modules

### Example: Spawning a Subagent

```
Use the Agent tool with:
  subagent_type: "Explore"
  prompt: "Find all REST API endpoint definitions in this codebase and list their HTTP methods, paths, and handler files."
```

---

## Agent Teams (Experimental)

Agent Teams let one Claude Code session act as a "team lead" coordinating multiple "teammates," each running in their own context window with the ability to communicate directly with each other.

### Enabling Agent Teams

Set the environment variable:

```bash
export CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1
```

### How They Work

1. A lead session creates teammates and assigns tasks via a shared task list
2. Each teammate runs in its own context window
3. Teammates can communicate with each other directly (not just through the lead)
4. The lead coordinates overall progress

### Example Prompt

> "Create an agent team to refactor the payment module. Spawn three teammates: one for the API layer, one for the database migrations, one for test coverage."

### When Agent Teams Make Sense

- Large refactors spanning multiple system layers
- Tasks where different specialists need to coordinate
- Work where parallel agents need to share intermediate findings

### Current Limitations

- Experimental status - the API and behavior may change
- Not recommended for production workflows yet
- Requires the environment variable flag to enable

---

## Guidance Summary

| Scenario | Recommendation |
|----------|---------------|
| Focused subtask (search, plan, review) | Use a **subagent** |
| Parallel independent work | Use multiple **subagents** concurrently |
| Complex multi-agent coordination | Consider **Agent Teams** (experimental) |
| Production automation | **Subagents** only |
| Third-party agent frameworks | Evaluate case-by-case |

### Quick Decision

- **Subagents**: Ready for production. Use them.
- **Agent Teams**: Promising but experimental. Test internally, don't ship to users yet.
- **Third-party frameworks** (LangGraph, CrewAI, etc.): Evaluate on a case-by-case basis against what Claude Code provides natively.
