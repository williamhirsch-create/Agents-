# Agent Team

A team of 6 specialized agents you can spawn to handle different tasks.

## The Team

| Agent | Model | Specialty |
|-------|-------|-----------|
| **researcher** | Sonnet | Deep research — codebases, docs, web searches |
| **coder** | Sonnet | Writing, editing, and refactoring code |
| **reviewer** | Sonnet | Code review — bugs, security, performance |
| **tester** | Sonnet | Writing and running tests |
| **devops** | Sonnet | CI/CD, Docker, builds, infrastructure |
| **architect** | Opus | System design, planning, technical decisions |

## How to Use

Ask Claude Code to spawn any agent by name:

- *"Use the researcher to find all API endpoints in this project"*
- *"Have the coder implement a login form"*
- *"Get the reviewer to check my latest changes"*
- *"Ask the tester to write unit tests for the auth module"*
- *"Have devops set up a Dockerfile for this project"*
- *"Ask the architect to design a caching layer"*

## Running Agents in Parallel

You can run multiple agents at the same time for independent tasks:

- *"Run the coder and tester in parallel — coder builds the feature, tester writes the tests"*
- *"Have the researcher investigate the bug while the architect plans the fix"*

## Agent Capabilities

Each agent has access to specific tools suited to its role:

- **Read-only agents** (researcher, reviewer, architect): Can search and read but not modify files
- **Read-write agents** (coder, tester, devops): Can create and edit files, run commands
- **Web access** (researcher): Can search the web and fetch URLs
