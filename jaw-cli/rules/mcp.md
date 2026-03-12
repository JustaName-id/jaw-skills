## MCP Server

The JAW CLI includes an MCP (Model Context Protocol) server that exposes wallet operations as tools for AI agents. Use `jaw mcp` to start it.

### How it works

`jaw mcp` starts a stdio MCP server. The host application (e.g. Claude Desktop) launches the server as a subprocess and communicates over stdin/stdout. The server bridges all requests to the JAW.id browser backend using the same daemon architecture as the CLI.

### Configure in Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "jaw": {
      "command": "jaw",
      "args": ["mcp"],
      "env": {
        "JAW_API_KEY": "YOUR_API_KEY"
      }
    }
  }
}
```

Restart Claude Desktop after saving. The JAW tools will appear in the tool list.

### Available MCP tools

#### `jaw_rpc`

Execute any JAW.id wallet RPC method.

Parameters:
- `method` (string, required) — EIP-1193 method name
- `params` (any, optional) — Method parameters (same format as `jaw rpc call`)
- `chainId` (number, optional) — Override default chain

Example tool call:
```json
{
  "method": "wallet_sendCalls",
  "params": {"calls":[{"to":"0xRECIPIENT","value":"1000000000000000000"}]},
  "chainId": 8453
}
```

#### `jaw_config_show`

Show current CLI configuration (API key redacted). No parameters.

#### `jaw_config_set`

Set a single configuration value.

Parameters:
- `key` (string, required) — One of: `apiKey`, `defaultChain`, `keysUrl`, `paymasterUrl`, `ens`
- `value` (string, required) — The value to set

Example:
```json
{"key": "defaultChain", "value": "8453"}
```

#### `jaw_status`

Check the current status of the JAW.id relay bridge — whether a relay session exists, the bridge connection is active, and what configuration is in use. No parameters.

Example response:
```json
{
  "relay": { "session": true },
  "bridgeConnection": "disconnected",
  "config": { "apiKey": "redacted", "defaultChain": 84532 }
}
```

#### `jaw_disconnect`

Close the relay session and browser tab. Call this when done making wallet requests to clean up resources. No parameters.

### Using jaw_rpc in agent prompts

When instructing an AI agent to perform wallet operations, use natural language — the agent maps intent to the correct `jaw_rpc` call:

- "Send 10 USDC to 0x1234..." → `jaw_rpc` with `wallet_sendCalls` and ERC-20 transfer calldata
- "Check my ETH balance" → `jaw_rpc` with `wallet_getAssets`
- "Grant a 50 USDC/day spend permission expiring in 30 days" → `jaw_rpc` with `wallet_grantPermissions`

### Key rules

- You MUST set `JAW_API_KEY` in the MCP server environment — the MCP server reads it from `JAW_API_KEY` env var or the config file
- You MUST restart the host application after editing `claude_desktop_config.json`
- Do NOT run `jaw mcp` manually in a terminal alongside a running CLI daemon — the host application manages the process lifecycle
- The browser tab must remain open for signing operations — the same passkey flow applies to MCP tool calls
