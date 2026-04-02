# Open-Multi-Agent Source Directory Architecture

This document provides a high-level overview of the `src` directory in the `open-multi-agent` project. The framework is designed to orchestrate multi-agent interactions, providing abstractions for Agents, Teams, Tasks, Memory, Tools, and LLM providers.

## Core Files
- `index.ts`: The primary entry point. Exports the public API, including the Orchestrator, Agent, Team, and other primitives.
- `types.ts`: Core type definitions for the entire framework (e.g., `AgentConfig`, `Task`, `ToolDefinition`). It keeps the dependency graph acyclic by exporting all public interfaces from a single module.

## Subdirectories Overview

### `orchestrator/`
The primary control layer of the framework. It manages the execution, coordination, and runs.
- **`orchestrator.ts`**: The main `OpenMultiAgent` controller logic orchestrating agents and teams.
- **`scheduler.ts`**: Handles the scheduling strategies (`SchedulingStrategy`) for async operations.

### `agent/`
Manages single-agent execution, lifecycles, and concurrency.
- **`agent.ts`**: Definition and execution logic for a single `Agent`.
- **`pool.ts`**: Provides `AgentPool` and `Semaphore` for managing multiple active agents concurrently.
- **`runner.ts`**: Manages the loop for evaluating LLM responses and handling iteration turn limits and tool calls.

### `team/`
Facilitates multi-agent cooperation and shared context.
- **`team.ts`**: Defines the `Team` abstraction for coordinating a group of predefined agents.
- **`messaging.ts`**: Implementations like `MessageBus` for interconnecting agents, allowing message passing, broadcasts, and direct communications.

### `task/`
Handles work breakdown, allocation, and execution ordering based on dependencies.
- **`task.ts`**: Defines standard tasks, checking readiness (`isTaskReady`), dependency order (`getTaskDependencyOrder`), and validation (`validateTaskDependencies`).
- **`queue.ts`**: Provides `TaskQueue` for sequencing operations asynchronously.

### `tool/`
The extensible function-calling tool framework.
- **`framework.ts`**: Includes `ToolRegistry`, `defineTool`, and handles Zod-to-JSON Schema conversions.
- **`executor.ts`**: `ToolExecutor` that injects execution context (`ToolUseContext`), manages batch calls, and handles abort controllers.
- **`built-in/`**: Contains pre-made foundational tools (e.g., `bashTool`, `fileReadTool`, `fileWriteTool`, `grepTool`).

### `llm/`
Provider-agnostic interface adapters for accessing various language models.
- **`adapter.ts`**: Base `LLMAdapter` definition standardizing chats and streams.
- **`anthropic.ts`**: Concrete adapter for Anthropic models.
- **`openai.ts`**: Concrete adapter for OpenAI models.

### `memory/`
Manages state persistence and short/long-term memory.
- **`store.ts`**: Core abstractions and implementations like `InMemoryStore`.
- **`shared.ts`**: `SharedMemory` implementation for cross-agent contextual alignment within teams out of the box.

### `utils/`
- **`semaphore.ts`**: A concurrency limiting and general-purpose coordination utility safe for lock acquisition among agents.
