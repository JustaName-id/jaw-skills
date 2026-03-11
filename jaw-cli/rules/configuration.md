## Configuration

The CLI stores configuration in `~/.jaw/config.json` (mode 0600). Use `jaw config set` to update values and `jaw config show` to inspect them.

### Set configuration values

```bash
# Set individual values
jaw config set apiKey=YOUR_API_KEY
jaw config set defaultChain=8453

# Set multiple values at once
jaw config set apiKey=YOUR_API_KEY defaultChain=8453

# Set optional features
jaw config set paymasterUrl=https://paymaster.example.com
jaw config set ens=yourname.eth
```

### Show current configuration

```bash
jaw config show
jaw config show -o json
```

The API key is always redacted in output.

### Configuration keys


| Key            | Required | Description                                                                                  |
| -------------- | -------- | -------------------------------------------------------------------------------------------- |
| `apiKey`       | Yes      | JAW API key. Obtain from `dashboard.jaw.id`.                                                 |
| `defaultChain` | Yes      | Default chain ID. E.g. `1` (Ethereum), `8453` (Base), `84532` (Base Sepolia).                |
| `keysUrl`      | No       | JAW keys URL. Default: `https://keys.jaw.id`. Only change for self-hosted deployments.       |
| `paymasterUrl` | No       | Paymaster URL for gas sponsorship. Obtain from your paymaster provider.                      |
| `ens`          | No       | ENS domain for on-chain identity. Must be configured in the JAW dashboard before using here. |


### Environment variables

Environment variables override the config file.


| Variable       | Equivalent config key                                         |
| -------------- | ------------------------------------------------------------- |
| `JAW_API_KEY`  | `apiKey`                                                      |
| `JAW_CHAIN_ID` | `defaultChain`                                                |
| `JAW_OUTPUT`   | Output format (`json` or `human`) — no config file equivalent |


### External prerequisites

Three config values require setup outside the CLI before use:

- `**apiKey**` — Obtain from `dashboard.jaw.id`. Required for all RPC calls.
- `**paymasterUrl**` — Obtain from any paymaster provider. Required only if sponsoring gas.
- `**ens**` — Must be configured and verified in the JAW dashboard before setting it here.

### Key rules

- You MUST set `apiKey` before making any RPC call
- You MUST set `defaultChain` or pass `-c <chainId>` with every `jaw rpc call` command
- Do NOT put `apiKey` directly in shell scripts — use the `JAW_API_KEY` environment variable instead
- You MUST NOT set `keysUrl` to a non-jaw.id domain — the CLI validates and rejects untrusted URLs
- Do NOT share or commit `~/.jaw/config.json` — it contains your API key

