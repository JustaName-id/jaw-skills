---
name: jaw-cli
description: Guide for using the JAW CLI and MCP server (@jaw.id/cli) — passkey-authenticated smart account operations for humans and AI agents. Use this skill when executing wallet RPC methods via the CLI, configuring the JAW CLI (API key, chain, keysUrl), using jaw_rpc MCP tool from an AI agent, sending transactions or signing messages through the CLI, managing permissions via CLI, debugging CLI browser auth flow, or setting up the MCP server for Claude Code or other AI tools.
---

# JAW CLI

Guide for using the JAW CLI and MCP server — passkey-authenticated smart account operations for humans and AI agents.

## When to use

Reference these guidelines when:

- Executing wallet RPC methods via `jaw rpc call`
- Setting up the MCP server for Claude Code or other AI agents
- Configuring the CLI (API key, default chain, keys URL)
- Sending transactions, signing messages, or managing permissions via CLI
- Using the `jaw_rpc` MCP tool from an AI agent
- Debugging the browser auth flow
- Building automation or scripts that interact with JAW smart accounts

## When NOT to use

- For SDK integration in apps (React, vanilla JS) → use **jaw-sdk-best-practices** instead
- For messaging, marketing, or positioning → use **jaw-messaging** instead

## Key facts

- **Package:** `@jaw.id/cli` on npm
- **API Key:** Required. Get one at [https://dashboard.jaw.id](https://dashboard.jaw.id)
- **Auth:** Browser-based passkey auth via keys.jaw.id — no private key management
- **Bridge:** Uses JAW SDK in AppSpecific mode (inline passkey UI, no popup)
- **MCP:** Single `jaw_rpc` tool supports all EIP-1193 methods
- **Config:** Stored at `~/.jaw/config.json`, session at `~/.jaw/session.json`

## Rule index

### 1. Setup & Configuration

- <rules/setup.md> - Installation, MCP config, first-time setup, environment variables

### 2. RPC Methods

- <rules/rpc-methods.md> - All supported methods, categories, params format, examples

### 3. MCP Server

- <rules/mcp-server.md> - MCP tools reference, agent usage patterns, jaw_rpc examples

### 4. Auth & Architecture

- <rules/auth-architecture.md> - Browser flow, AppSpecific mode, session management, security

### 5. Troubleshooting

- <rules/troubleshooting.md> - Common errors, debugging tips, popup blockers, timeout issues

## How to use

Read individual rule files for detailed guidance. Each contains:
correct usage examples, common mistakes to avoid, and critical requirements.
