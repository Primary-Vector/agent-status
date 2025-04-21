# AGENT_STATUS

## Overview

The `AGENT_STATUS` file is a simple, standardized format for AI agents to communicate their status to the development environment and optionally after a task is completed. This allows terminals, IDEs, editors, and other tools to display relevant information about AI agents working in a project.

It's meant to be simple, agent/tool-agnostic and file-based so it can be versioned, read by any tool on the system, or the current terminal or editor.

If desired, the file can be checked in with completed work for later review, parsing metrics or tags as part of CI pipeline. 

## Examples

Below are example `AGENT_STATUS` files for common use cases:

### 🛠 Example 1: Refactor Agent Working on Auth Module
```
AGENT_NAME=Auth Refactorer
AGENT_TYPE=Claude-3.5-Sonnet
STATUS=working
STATUS_TEXT=Refactoring login flow
STATUS_COLOR=#3498db
STATUS_EMOJI=🛠️
PROGRESS=40
TASK_TYPE=refactor
FILES_MODIFIED=src/auth.js,src/login.js
BRANCH=feature/refactor-auth
PROJECT_NAME=MyWebApp
```

### 🧪 Example 2: Testing Agent Waiting for Input
```
AGENT_NAME=TestBot
AGENT_TYPE=OpenAI-GPT-4
STATUS=waiting
WAITING_FOR=user_input
STATUS_TEXT=Waiting for test case approval
STATUS_COLOR=#f1c40f
STATUS_EMOJI=⏳
TASK_TYPE=code-gen
TASK_DESCRIPTION=Generate unit tests for payment module
INPUT_TYPE=code
PROJECT_NAME=PaymentsAPI
```

### ✅ Example 3: Completed Task Summary with Token Usage
```
AGENT_NAME=Bug Finder
AGENT_TYPE=Gemini-Pro
STATUS=complete
STATUS_TEXT=Finished static analysis
STATUS_DETAIL=Found 3 potential null dereferences
STATUS_COLOR=#2ecc71
STATUS_EMOJI=✅
PROGRESS=100
TASK_TYPE=review
TOKEN_USAGE=9321
API_CALLS=6
COST=$0.19
FILES_MODIFIED=src/utils.js
PROJECT_NAME=InventoryService
```

### Example Consumers
- AGENT_STATUS can be read by a terminal and shown within its tabs for developers managing multiple local agents in multiple tabs/windows
- For human interaction it can be piped into a chat window for quickly gaining context to an agent's current question or request for approval
- Can be checked in at the end of work to summarize the task, how many tokens were spent and give overall status
- Shell scripts could reference the file as it changes or periodically to post to an external service

## File Location

The `AGENT_STATUS` file should be placed in the root of the project or specific directory where an AI agent is operating. Tools should look for this file in the current working directory or parent directories.

## File Format

The file uses a simple `KEY=VALUE` format, with one key-value pair per line:

```
AGENT_NAME=Code Refactor Assistant
AGENT_TYPE=Claude-3.5-Sonnet
STATUS=working
STATUS_TEXT=Analyzing class dependencies
STATUS_COLOR=#3498db
PROGRESS=45
```

Comments can be added using the `#` character:

```
# This agent is working on the authentication module
AGENT_NAME=Auth Helper
```

## Standard Keys

Below is a comprehensive list of standardized keys that can be used in the `AGENT_STATUS` file. Applications should be designed to handle any subset of these keys.

### Basic Identification

| Key | Description | Example |
|-----|-------------|---------|
| `AGENT_NAME` | Friendly name for the agent | `AGENT_NAME=Bug Finder` |
| `AGENT_TYPE` | Type/model of agent | `AGENT_TYPE=Claude-3.5-Sonnet` |
| `AGENT_GROUP` | For grouping multiple agents | `GROUP=bug-fixers` |
| `AGENT_VERSION` | Specific version of the agent | `AGENT_VERSION=1.2.3` |
| `SESSION_ID` | Unique identifier for this session | `SESSION_ID=abc123def456` |

### Status Information

| Key | Description | Example |
|-----|-------------|---------|
| `STATUS` | Overall status | `STATUS=working` (idle, working, error, waiting, complete, abandoned) |
| `STATUS_TEXT` | Human-readable description | `STATUS_TEXT=Generating unit tests` |
| `STATUS_DETAIL` | More detailed information | `STATUS_DETAIL=Creating fixtures for AuthController` |
| `PROGRESS` | Numeric completion estimate | `PROGRESS=65` (0-100) |
| `START_TIME` | When the current task started | `START_TIME=2025-04-21T15:30:45Z` |
| `ESTIMATED_COMPLETION` | Expected completion time | `ESTIMATED_COMPLETION=2025-04-21T15:45:12Z` |

### Visual Indicators

| Key | Description | Example |
|-----|-------------|---------|
| `STATUS_COLOR` | Color code for visual indicators | `STATUS_COLOR=#FF5733` |
| `STATUS_EMOJI` | Emoji representation | `STATUS_EMOJI=🔍` |
| `PRIORITY` | Importance level | `PRIORITY=high` (low, medium, high) |

### Task Information

| Key | Description | Example |
|-----|-------------|---------|
| `TASK_TYPE` | Type of task | `TASK_TYPE=refactor` (code-gen, debug, refactor, review) |
| `TASK_TICKET` | Ticket number or ID of the task | `TASK_TICKET=APP-5421` |
| `TASK_URL` | Documentation URL if outside of the repo | `TASK_URL=https://myticketsystem.com/APP-5421` |
| `TASK_DESCRIPTION` | Brief description of current task | `TASK_DESCRIPTION=Improving error handling` |
| `FILES_MODIFIED` | Files being worked on | `FILES_MODIFIED=src/auth.js,src/login.js` |
| `BRANCH` | Git branch information | `BRANCH=feature/improved-auth` |
| `PR` | PR information | `PR=feature/improved-auth-pr` |

### Resource Usage

| Key | Description | Example |
|-----|-------------|---------|
| `TOKEN_USAGE` | Number of tokens used/remaining | `TOKEN_USAGE=4500/10000` |
| `API_CALLS` | Number of API calls made | `API_CALLS=12` |
| `COST` | Running cost information, if known | `COST=$0.42` |

### Interaction

| Key | Description | Example |
|-----|-------------|---------|
| `WAITING_FOR` | What the agent is waiting for | `WAITING_FOR=user_input` (user_input, api_response) |
| `INPUT_TYPE` | Type of input expected | `INPUT_TYPE=code` (text, code, file) |
| `LAST_INTERACTION` | Timestamp of last interaction | `LAST_INTERACTION=2025-04-21T15:22:30Z` |

### Project Context

| Key | Description | Example |
|-----|-------------|---------|
| `PROJECT_NAME` | Name of the project | `PROJECT_NAME=MyAwesomeApp` |
| `PROJECT_PATH` | Path to the project | `PROJECT_PATH=/Users/dev/projects/myapp` |
| `CONTEXT_SCOPE` | File/directory scope | `CONTEXT_SCOPE=src/auth/*` |

### Integration

| Key | Description | Example |
|-----|-------------|---------|
| `LOG_FILE` | Path to detailed log file | `LOG_FILE=.logs/agent-2025-04-21.log` |
| `CALLBACK_URL` | URL for status updates | `CALLBACK_URL=http://localhost:3000/agent-callback` |
| `COMMAND_PREFIX` | Command prefix for this agent | `COMMAND_PREFIX=/claude` |

## Tool Integration Examples

### Terminal Emulators (like Ghostty)

Terminals can enhance tab displays with agent information:
- Show status color in tab indicator
- Display agent emoji and name in tab title
- Show progress and status in tab details

### Code Editors/IDEs

IDEs can integrate agent status in various ways:
- Status bar integration showing current agent activities
- Sidebar panels for agent interactions
- Color coding of files being actively edited by agents
- Integration with notifications for completed tasks

### Git Clients

Git tools can incorporate agent information:
- Show which branches have active agents
- Indicate which files are being modified
- Display agent activity in commit history views

### Status Bar Applications

Standalone status monitors can:
- Show overall agent activity across projects
- Provide quick toggles for agent interaction
- Display estimated completion times

### Project Management Tools

Task trackers can leverage agent data:
- Track agent progress on assigned tasks
- Calculate time/cost savings from agent usage
- Show agent activity history

## Implementation Guidelines

1. **Reading the file**: Applications should read the file periodically (e.g., every 0.5-2 seconds) and check for modifications before parsing.

2. **File changes**: Agents should update the file atomically to avoid partial reads.

3. **Missing values**: Applications should handle missing keys gracefully and use reasonable defaults.

4. **Unknown keys**: Applications should ignore keys they don't recognize for forward compatibility.

5. **Security**: The file should only contain non-sensitive information. No credentials or private data.



---
