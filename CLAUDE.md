# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Development Commands

```bash
# Development
bun run dev              # Run CLI in development mode
bun run dev:debug        # Run with debugger on port 9229

# Build
bun run build            # Build to dist/ directory

# Type checking
bunx tsc --noEmit        # Type check without emitting

# Linting (Biome)
bunx biome check .       # Run linter
bunx biome check --write .  # Auto-fix lint issues
bunx biome format --write . # Format code
```

## Architecture Overview

This is Claude Code CLI - a terminal-based AI coding assistant built with Bun, TypeScript, and React (via Ink for terminal rendering).

### Core Entry Points

- `src/entrypoints/cli.tsx` - Main CLI entry point with commander.js argument parsing
- `src/main.tsx` - Application bootstrap, command registration, and session setup
- `src/query.ts` - Core query/message loop for AI interactions
- `src/entrypoints/init.ts` - Initialization and telemetry setup

### Key Directories

- **`src/commands/`** - Slash commands (e.g., `/commit`, `/review`, `/mcp`). Each command is a directory with `index.ts` exporting a `Command` object.
- **`src/tools/`** - AI-callable tools (BashTool, FileEditTool, AgentTool, etc.). Each tool exports a `Tool` object with schema, handler, and permission logic.
- **`src/services/`** - Core services:
  - `mcp/` - MCP (Model Context Protocol) client implementation
  - `api/` - Claude API communication (`claude.ts` is the main API client)
  - `analytics/` - GrowthBook feature flags and event logging
- **`src/components/`** - React/Ink UI components for terminal rendering
- **`src/ink/`** - Fork/customization of Ink framework for terminal React rendering
- **`src/hooks/`** - React hooks for state management, keybindings, IDE integration
- **`src/state/`** - Global state management via `AppStateStore.ts`
- **`src/bridge/`** - Remote session support (cloud/desktop connectivity)
- **`src/utils/`** - Utilities for auth, permissions, settings, files, etc.
- **`src/skills/`** - Skills system (bundled and user-defined)
- **`src/plugins/`** - Plugin architecture

### Tool System

Tools are defined in `src/tools/` and registered via `src/tools.ts`. Each tool:
1. Extends the `Tool` class/interface from `src/Tool.ts`
2. Defines a JSON schema for input validation
3. Implements a `handler` function for execution
4. Optionally defines permission prompts for user approval

### Command System

Commands are defined in `src/commands/` and registered in `src/commands.ts`. Each command exports a `Command` object with:
- `name` - Command name (e.g., "commit")
- `description` - Help text
- `action` - Handler function

### Message Flow

1. User input → `PromptInput` component
2. Message created via `createUserMessage()` in `src/utils/messages.ts`
3. Query loop in `src/query.ts` processes messages
4. API call via `src/services/api/claude.ts`
5. Tool execution via `StreamingToolExecutor` and `runTools()`
6. UI updates via React state in `AppStateStore`

### MCP Integration

MCP servers are configured via settings and managed through `src/services/mcp/client.ts`. MCP tools appear alongside built-in tools and follow the same permission system.

### State Management

Global state lives in `src/state/AppStateStore.ts` using a simple store pattern with `createStore()`. State includes:
- Messages
- Tool permissions
- Model settings
- Session metadata

### Remote Sessions (Bridge)

The `src/bridge/` module handles remote sessions for Claude.ai integration:
- `bridgeMain.ts` - Main bridge logic
- `bridgeApi.ts` - API client for remote communication
- `sessionRunner.ts` - Spawning and managing remote sessions

### Feature Flags

Features are gated via `feature()` from `bun:bundle` (compile-time) or GrowthBook (runtime). Check `src/services/analytics/growthbook.ts` for feature flag utilities.

### Monorepo Structure

Workspace packages in `packages/`:
- `@ant/computer-use-mcp` - Computer use MCP server
- `@ant/claude-for-chrome-mcp` - Chrome integration MCP
- `audio-capture-napi`, `color-diff-napi`, etc. - Native addon packages

### Code Patterns

- **Dead Code Elimination**: Use `feature('FEATURE_NAME')` for compile-time feature gating
- **Lazy imports**: Use `require()` for conditional imports to avoid circular deps
- **Path aliases**: `src/*` maps to `./src/*` via tsconfig paths
- **React Compiler**: Some components use React Compiler (`useCompilerRuntime`)
- **ES Modules**: Project uses ESM (`"type": "module"`)

### Important Files

- `src/Tool.ts` - Tool type definitions and interfaces
- `src/types/message.ts` - Message type definitions
- `src/types/permissions.ts` - Permission system types
- `src/utils/auth.ts` - Authentication logic
- `src/utils/permissions/` - Permission handling
- `src/constants/prompts.ts` - System prompts for the AI
- `src/bootstrap/state.ts` - Global bootstrap state
