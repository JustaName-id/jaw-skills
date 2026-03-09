# Setup & Configuration

## Installation

```bash
# Run directly with npx
npx @jaw.id/cli <command>

# Install globally
npm install -g @jaw.id/cli
```

## First-Time Setup

```bash
# Configure API key and default chain
jaw config init --api-key YOUR_API_KEY --chain 8453

# Connect your wallet (opens browser for passkey auth)
jaw rpc call wallet_connect
```

## MCP Server Configuration

### Claude Code (`~/.claude.json`)

```json
{
  "mcpServers": {
    "jaw": {
      "command": "npx",
      "args": ["@jaw.id/cli", "mcp"],
      "env": {
        "JAW_API_KEY": "your-api-key"
      }
    }
  }
}
```

## Configuration File

Stored at `~/.jaw/config.json`:

```json
{
  "apiKey": "your-api-key",
  "defaultChain": 8453,
  "keysUrl": "https://keys.jaw.id",
  "paymasterUrl": "https://paymaster.example.com"
}
```

### Config Commands

```bash
jaw config show                          # Display config (API key redacted)
jaw config show --output json            # JSON output
jaw config set apiKey your-api-key       # Set API key
jaw config set defaultChain 8453         # Set default chain
jaw config set keysUrl https://keys.jaw.id
jaw config set paymasterUrl https://paymaster.example.com
```

## Environment Variables

| Variable | Description |
|----------|-------------|
| `JAW_API_KEY` | JAW API key (overrides config file) |
| `JAW_CHAIN_ID` | Default chain ID |
| `JAW_OUTPUT` | Output format (`json` or `human`) |

## Common Flags

| Flag | Short | Description | Default |
|------|-------|-------------|---------|
| `--output` | `-o` | Output format: `json` or `human` | `human` |
| `--chain` | `-c` | Chain ID override | config default |
| `--api-key` | | JAW API key | config/env |
| `--timeout` | `-t` | Browser callback timeout (seconds) | `120` |
| `--yes` | `-y` | Skip confirmations | `false` |
| `--quiet` | `-q` | Suppress non-essential output | `false` |
