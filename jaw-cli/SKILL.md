---
name: jaw-cli
description: Best practices and usage guide for the JAW CLI (@jaw.id/cli). Use this skill when using the `jaw` command-line tool, scripting JAW wallet operations in shell, configuring the CLI, sending transactions or signing messages via the CLI, managing permissions from the terminal, setting up the JAW MCP server for AI agents, setting up auto mode for autonomous AI agent operation, or when asked about JAW CLI commands, configuration, or patterns.
---

# JAW CLI

Guide for using the JAW CLI, a terminal interface and MCP server for interacting with JAW.id passkey-authenticated smart accounts from the command line or an AI agent.

## When to use

Reference these guidelines when:

- Installing or setting up `@jaw.id/cli`
- Configuring the CLI (API key, chain, paymaster, ENS, permissions)
- Writing a full config with `jaw config write`
- Resolving ENS names (`.eth`, `.id`, `.com`, etc.) to addresses before sending
- Sending ETH or ERC-20 tokens from the command line
- Sending batch transactions or checking transaction status
- Signing messages or typed data via the CLI
- Granting, revoking, or querying permissions
- Setting up auto mode for autonomous AI agents (session keys + scoped permissions)
- Using `--session` flag for browserless operation
- Managing session lifecycle (setup, status, revoke)
- Setting up the MCP server for AI agents (Claude Desktop, etc.)
- Scripting or automating wallet operations in shell
- Debugging CLI errors or unexpected behavior

## Key facts

- **Package:** `@jaw.id/cli` — binary: `jaw`
- **Two modes:** Default mode (browser + passkey per operation) and Auto mode (`--session` flag, local session key, no browser)
- **Browser required for signing (default mode):** `wallet_sendCalls`, `personal_sign`, `wallet_grantPermissions` and other state-changing methods require passkey confirmation in browser. Read-only methods (`eth_accounts`, `wallet_getAssets`, etc.) do not.
- **Auto mode:** One-time `jaw session setup` (browser), then `--session` flag for all subsequent calls. Session key is scoped by on-chain PermissionManager (call restrictions, spend limits, expiry).
- **Config file:** `~/.jaw/config.json` — supports flat keys via `jaw config set`, full JSON via `jaw config write`, nested objects (paymasters, permissions) via direct file editing or `config write`.
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
| `--session` | `-s`  | `false` | Use local session key (auto mode — no browser)              |

### jaw config set [key=value ...]

Set one or more flat configuration values.

Valid keys: `apiKey`, `defaultChain`, `keysUrl`, `ens`, `relayUrl`, `sessionExpiry`

```bash
jaw config set apiKey=YOUR_KEY defaultChain=8453 sessionExpiry=7
```

### jaw config write <json-or-filepath>

Write full config from inline JSON or a file path. Handles nested objects (paymasters, permissions).

```bash
jaw config write '{"apiKey":"...","defaultChain":84532,"paymasters":{"84532":{"url":"..."}},"permissions":{...}}'
jaw config write ./my-config.json
```

### jaw config show

Display current configuration (API key is redacted).

```bash
jaw config show
jaw config show -o json
```

### jaw session setup

Generate a session key and grant scoped on-chain permissions. Opens browser once for passkey approval.

```bash
jaw session setup --chain 84532
jaw session setup --chain 84532 --permissions ./perms.json --expiry 14 --yes
```

### jaw session status

Show current session status (address, permissionId, expiry, valid/expired).

```bash
jaw session status
jaw session status -o json
```

### jaw session revoke

Revoke on-chain permission and delete local session files. Opens browser (skipped if expired).

```bash
jaw session revoke
```

### jaw disconnect

Close the relay session and browser tab. Cleans up the session stored in `~/.jaw/relay.json`.

### jaw mcp

Start the MCP server via stdio for AI agent integration.

```bash
jaw mcp
```

## Rule index

### 1. Setup

- <rules/installation.md> — Installing the CLI and first-run setup
- <rules/configuration.md> — Configuration keys, environment variables, config write, external prerequisites

### 2. Operations

- <rules/ens-resolution.md> — Resolving ENS names to addresses before transacting
- <rules/transactions.md> — Sending ETH, ERC-20 tokens, batch transactions, checking status
- <rules/signing.md> — Signing messages and typed data
- <rules/permissions.md> — Granting, revoking, and querying permissions

### 3. Auto Mode

- <rules/auto-mode.md> — Session setup, `--session` flag, lifecycle, security model, error handling

### 4. AI Agent Integration

- <rules/mcp.md> — MCP server setup, Claude Desktop config, available tools

### 5. Reference

- <rules/api-reference.md> — Full per-method reference with params schemas and examples

## How to use

Read individual rule files for detailed guidance. Each contains:
correct usage examples, common mistakes to avoid, and critical requirements.
