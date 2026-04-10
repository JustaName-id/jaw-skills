## Auto Mode

Auto mode lets AI agents operate a JAW smart account without a browser or passkey for every transaction. A human approves once during setup, granting scoped on-chain permissions to a locally-stored session key. From then on, `--session` enables browserless execution.

### How it works

1. **Setup (one-time, human required):** `jaw session setup` generates a session key, opens browser for passkey approval of `wallet_grantPermissions`, saves encrypted keystore + session config locally.
2. **Execution (autonomous):** `jaw rpc call ... --session` decrypts the key, signs locally, sends through PermissionManager. No browser, no human.
3. **Teardown (human required):** `jaw session revoke` opens browser, revokes on-chain permission, deletes local files.

### Prerequisites

Before setting up auto mode, your `~/.jaw/config.json` must have:
- `apiKey` — required
- `defaultChain` or use `--chain` flag
- `permissions` — the scope for the session key
- `paymasters` (optional) — for gas sponsorship

Set up the full config:

```bash
jaw config write '{"apiKey":"YOUR_KEY","defaultChain":84532,"paymasters":{"84532":{"url":"https://api.pimlico.io/v2/84532/rpc?apikey=YOUR_PM_KEY"}},"sessionExpiry":7,"permissions":{"calls":[{"target":"0x3232323232323232323232323232323232323232","selector":"0xe0e0e0e0"}],"spends":[{"token":"0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE","allowance":"0x16345785d8a0000","unit":"day","multiplier":1}]}}'
```

### Session setup

```bash
jaw session setup --chain 84532
```

Opens browser once for passkey. Creates `~/.jaw/keystore.json` (encrypted session key) and `~/.jaw/session-config.json` (permissionId, addresses, expiry).

Optional overrides:

```bash
# Override permissions for this session
jaw session setup --permissions '{"calls":[...],"spends":[...]}'

# Override expiry (default: config.sessionExpiry or 7 days)
jaw session setup --expiry 14

# Skip overwrite confirmation
jaw session setup --chain 84532 --yes
```

### Using --session

Add `--session` (or `-s`) to any `jaw rpc call`:

```bash
# Get session account address
jaw rpc call eth_requestAccounts --session -o json -y
# Returns the session key's smart account address, NOT the owner's

# Send transaction (no browser)
jaw rpc call wallet_sendCalls '{"calls":[{"to":"0xRECIPIENT","value":"0x5af3107a4000","data":"0x"}]}' --session -o json -y

# Sign message (no browser)
jaw rpc call personal_sign '"Hello from agent"' --session -o json -y

# Check transaction status
jaw rpc call wallet_getCallsStatus '"0xBATCH_ID"' --session -o json -y
```

Or set via environment variable:

```bash
export JAW_SESSION=true
jaw rpc call eth_requestAccounts -o json -y
```

### Supported methods in session mode

| Method                  | Behavior                                                    |
| ----------------------- | ----------------------------------------------------------- |
| `eth_requestAccounts`   | Returns session key's smart account address                 |
| `eth_accounts`          | Same as above                                               |
| `wallet_sendCalls`      | Signs locally, auto-injects permissionId, sends via PermissionManager |
| `wallet_getCallsStatus` | Queries transaction status                                  |
| `personal_sign`         | Signs message with session key                              |
| `eth_signTypedData_v4`  | Signs typed data with session key                           |

Blocked methods (require browser):
- `wallet_grantPermissions` — "Requires browser — run jaw session setup"
- `wallet_revokePermissions` — "Requires browser — run jaw session revoke"
- Any other method — "Method X is not supported in auto mode"

### Session management

```bash
# Check session status
jaw session status
jaw session status -o json

# Revoke session (opens browser if not expired, skips browser if expired)
jaw session revoke
```

### Identity model

The session key operates as its **own smart account**:
- `eth_requestAccounts` with `--session` returns the session key's address (the agent's identity)
- Transactions execute on the owner's wallet via PermissionManager
- The owner's address is in `~/.jaw/session-config.json` as `ownerAddress`

### Security model

Even if the session key is compromised, damage is bounded by **on-chain enforcement**:
- **Call restrictions** — only whitelisted contract + function selector pairs
- **Spend limits** — capped token spend per time period
- **Time bound** — permission expires automatically
- **Instant revocation** — owner can revoke anytime via passkey

The keystore is encrypted with AES-256-GCM using the API key. It is useless without the API key. The on-chain PermissionManager is the primary security boundary.

### Typical agent workflow

```bash
# 1. Human configures (once)
jaw config write '{"apiKey":"...","defaultChain":84532,"paymasters":{...},"permissions":{...}}'

# 2. Human sets up session (once, opens browser)
jaw session setup --chain 84532

# 3. Agent operates autonomously
jaw rpc call eth_requestAccounts --session -o json -y
jaw rpc call wallet_sendCalls '{"calls":[{"to":"0x...","data":"0x..."}]}' --session -o json -y
jaw rpc call wallet_getCallsStatus '"0xBatchId"' --session -o json -y

# 4. Human tears down when done
jaw session revoke
```

### Error handling

| Scenario                        | Error message                                                           |
| ------------------------------- | ----------------------------------------------------------------------- |
| No keystore                     | "No session configured. Run jaw session setup first."                   |
| Session expired                 | "Session expired on DATE. Run jaw session setup to create a new session." |
| Unsupported method              | "Method NAME is not supported in auto mode."                            |
| Browser-only method             | "Requires browser — run jaw session setup/revoke."                      |
| API key changed                 | "Failed to decrypt keystore. API key may have changed."                 |
| Transaction exceeds spend limit | On-chain revert from PermissionManager                                  |
| Unauthorized contract call      | On-chain revert from PermissionManager                                  |

### Key rules

- You MUST set `permissions` in config before running `jaw session setup` (or pass `--permissions` flag)
- You MUST run `jaw session setup` once (requires browser) before using `--session`
- You MUST pass `--session` (or `-s` or `JAW_SESSION=true`) to use auto mode — it is never auto-detected
- You MUST pass `-o json -y` for AI agent contexts
- `wallet_sendCalls` with `--session` auto-injects the `permissionId` — do NOT pass it manually
- `eth_requestAccounts` with `--session` returns the session key's address, NOT the owner's address
- Session files are stored in `~/.jaw/keystore.json` and `~/.jaw/session-config.json`
- To change permission scope: `jaw session revoke` then `jaw session setup` with new permissions
- Editing `permissions` in config does NOT affect an active session — permissions are enforced on-chain
- Do NOT share `~/.jaw/keystore.json` — it contains the encrypted session private key
