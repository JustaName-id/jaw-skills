---
name: jaw-cli
description: Best practices and usage guide for the JAW CLI (@jaw.id/cli). Use this skill when using the `jaw` command-line tool, scripting JAW wallet operations in shell, configuring the CLI, sending transactions or signing messages via the CLI, managing permissions from the terminal, setting up the JAW MCP server for AI agents, or when asked about JAW CLI commands, configuration, or patterns.
---

# JAW CLI

Guide for using the JAW CLI, a terminal interface and MCP server for interacting with JAW.id passkey-authenticated smart accounts from the command line or an AI agent.

## When to use

Reference these guidelines when:

- Installing or setting up `@jaw.id/cli`
- Configuring the CLI (API key, chain, paymaster, ENS)
- Sending ETH or ERC-20 tokens from the command line
- Sending batch transactions or checking transaction status
- Signing messages or typed data via the CLI
- Granting, revoking, or querying permissions
- Setting up the MCP server for AI agents (Claude Desktop, etc.)
- Scripting or automating wallet operations in shell
- Debugging CLI errors or unexpected behavior

## Key facts

- **Package:** `@jaw.id/cli` — binary: `jaw`
- **Daemon architecture:** First RPC call auto-spawns a background daemon and opens a browser. No manual start needed. Daemon shuts down automatically after 30 minutes of inactivity.
- **Browser required for signing:** `wallet_sendCalls`, `personal_sign`, `wallet_grantPermissions` and other state-changing methods require passkey confirmation in browser. Read-only methods (`eth_accounts`, `wallet_getAssets`, etc.) do not.
- **Config resolution order:** CLI flag → env var → `~/.jaw/config.json` → default
- **AI agent flags:** Always pass `-o json` for machine-readable output and `-y` to skip confirmations
- **Preferred transaction method:** Use `wallet_sendCalls` over `eth_sendTransaction` — supports batching, paymasters, and permission-based execution
- **MCP transport:** `jaw mcp` starts a stdio server — configured in `claude_desktop_config.json`, not a port or URL

## Command reference

### jaw rpc call method [params]

Execute any JAW.id RPC method. `params` is a JSON string.


| Flag        | Short | Default | Description                                                 |
| ----------- | ----- | ------- | ----------------------------------------------------------- |
| `--output`  | `-o`  | `human` | Output format: `json` or `human`. Use `json` for AI agents. |
| `--chain`   | `-c`  | config  | Chain ID (e.g. `8453` for Base, `1` for Ethereum)           |
| `--api-key` |       | config  | JAW API key                                                 |
| `--timeout` | `-t`  | `120`   | Request timeout in seconds                                  |
| `--yes`     | `-y`  | `false` | Skip confirmations (use in AI agent contexts)               |
| `--quiet`   | `-q`  | `false` | Suppress non-essential output                               |


### jaw config set [key=value ...]

Set one or more configuration values.

Valid keys: `apiKey`, `defaultChain`, `keysUrl`, `paymasterUrl`, `ens`

```bash
jaw config set apiKey=YOUR_KEY defaultChain=8453
```

### jaw config show

Display current configuration (API key is redacted).

```bash
jaw config show
jaw config show -o json
```

### jaw disconnect

Stop the background daemon and close the browser session.

### jaw mcp

Start the MCP server via stdio for AI agent integration.

## Rule index

### 1. Setup

- <rules/installation.md> — Installing the CLI and first-run setup
- <rules/configuration.md> — Configuration keys, environment variables, external prerequisites

### 2. Operations

- <rules/transactions.md> — Sending ETH, ERC-20 tokens, batch transactions, checking status
- <rules/signing.md> — Signing messages and typed data
- <rules/permissions.md> — Granting, revoking, and querying permissions

### 3. AI Agent Integration

- <rules/mcp.md> — MCP server setup, Claude Desktop config, available tools

### 4. Reference

- <rules/api-reference.md> — Full per-method reference with params schemas and examples

## How to use

Read individual rule files for detailed guidance. Each contains:
correct usage examples, common mistakes to avoid, and critical requirements.