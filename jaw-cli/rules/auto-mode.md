## Auto Mode

Auto mode lets AI agents operate a JAW smart account without a browser or passkey for every transaction. A human approves once during setup, granting scoped on-chain permissions to a locally-stored session key. From then on, `--session` enables browserless execution.

### How it works

1. **Setup (one-time, human required):** `jaw session setup` generates a session key, opens browser for passkey approval of `wallet_grantPermissions`, saves the keystore + session config to `~/.jaw/` (file mode `0o600`).
2. **Execution (autonomous):** `jaw rpc call ... --session` loads the session key from disk, signs locally, sends through PermissionManager. No browser, no human.
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

Opens browser once for passkey. Creates `~/.jaw/keystore.json` (session private key, file mode `0o600`) and `~/.jaw/session-config.json` (permissionId, addresses, expiry).

Optional overrides:

```bash
# Override permissions for this session
jaw session setup --permissions '{"calls":[...],"spends":[...]}'

# Override expiry (default: config.sessionExpiry or 7 days)
jaw session setup --expiry 14

# Skip overwrite confirmation
jaw session setup --chain 84532 --yes
```

### Funding the session smart account

The session key acts as its own smart account at the `Session address` shown by `jaw session status`. That smart account needs gas to execute transactions. Pick **one** of:

1. **Configure a paymaster for the chain.** Set `paymasters[<chainId>].url` in `~/.jaw/config.json` (e.g. Pimlico). The session's UserOps are sponsored — no native ETH required on the session address.
2. **Fund the session smart account directly.** Send a small amount of native gas to the `Session address`. The address is deterministic per session key, so once funded it persists for the session's lifetime.

If neither is in place, the first `wallet_sendCalls --session` fails with:

```
UserOperationExecutionError: Smart Account does not have sufficient funds
to execute the User Operation.
…
Details: FailedOp(0,"AA21 didn't pay prefund")
```

This is the ERC-4337 entry point rejecting a UserOp it can't prefund — not a CLI bug. Add gas or a paymaster and retry.

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

Blocked methods (any method not in the table above):

The CLI rejects with `Error: Method <name> is not supported in session mode. Use without --session to route through the browser bridge.` This includes `wallet_grantPermissions` (use `jaw session setup` instead) and `wallet_revokePermissions` (use `jaw session revoke` instead), as well as everything else not listed.

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

The keystore stores the session private key as plaintext hex in `~/.jaw/keystore.json` with file mode `0o600` (read/write only by the owning user). The parent directory `~/.jaw/` is mode `0o700`. There is no application-level encryption — the on-chain PermissionManager is the primary security boundary. The granted permission scope (calls + spends + expiry) caps the worst case if the file is exfiltrated, and the owner can `jaw session revoke` at any time to neutralize the key.

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

| Scenario | Error / behavior |
| --- | --- |
| No keystore | `Error: No session configured. Run \`jaw session setup\` first.` |
| Session expired (local timestamp passed) | `Error: Session expired on <ISO_DATE>. Run \`jaw session setup\` to create a new session.` |
| Method not supported in session mode (anything outside `eth_accounts` / `eth_requestAccounts` / `wallet_sendCalls` / `wallet_getCallsStatus` / `personal_sign` / `eth_signTypedData_v4` — including `wallet_grantPermissions` and `wallet_revokePermissions`) | `Error: Method <name> is not supported in session mode. Use without --session to route through the browser bridge.` |
| Chain mismatch (`--session --chain X` while session is on chain Y) | `Error: Session was created for chain <Y>, but --chain <X> was requested. Run \`jaw session setup --chain <X>\` to create a session for that chain.` |
| Keystore or session-config corrupted on disk | `Error: Keystore at <path> is corrupted. Run \`jaw session setup\` to recreate it.` (or the session-config equivalent) |
| `jaw session setup` over an existing session with non-TTY stdin and no `--yes` | `Error: Existing session found, but stdin is not a terminal (piped, redirected, or running in CI). Re-run with --yes to overwrite the existing session non-interactively.` |
| `jaw session setup` failed after the inner revoke had already succeeded (e.g. bad permissions file, network error) | Stderr: `Old permission was revoked on-chain but setup did not complete. Local session-config still references the revoked permission; run \`jaw session setup\` again to create a new session.` — printed above the underlying error. |
| Session smart account has no native gas and no paymaster configured (first `wallet_sendCalls --session`) | `UserOperationExecutionError: Smart Account does not have sufficient funds … FailedOp(0,"AA21 didn't pay prefund")`. Fund the address shown by `jaw session status`, or configure `paymasters[<chainId>]` in `~/.jaw/config.json`. |
| Transaction violates permission scope (out-of-scope target/selector, or spend limit exceeded) | On-chain revert from PermissionManager. The CLI currently surfaces a generic `UserOperationExecutionError: Execution reverted for an unknown reason` — the typed revert reason is not yet parsed and translated. |

All errors exit non-zero. Pattern-match on the substring shown rather than the full message — exact wording may evolve.

### Key rules

- You MUST set `permissions` in config before running `jaw session setup` (or pass `--permissions` flag)
- You MUST run `jaw session setup` once (requires browser) before using `--session`
- You MUST pass `--session` (or `-s` or `JAW_SESSION=true`) to use auto mode — it is never auto-detected
- You MUST pass `-o json -y` for AI agent contexts
- `jaw session setup` requires `--yes` when run with non-TTY stdin (pipes, heredocs, CI) AND a session already exists. The CLI rejects upfront with an actionable error rather than risk a readline race that could mutate on-chain state mid-flow. Always pass `--yes` for AI-agent and CI invocations.
- `wallet_sendCalls` with `--session` auto-injects the `permissionId` — do NOT pass it manually
- `eth_requestAccounts` with `--session` returns the session key's address, NOT the owner's address
- Session files are stored in `~/.jaw/keystore.json` and `~/.jaw/session-config.json`
- To change permission scope: `jaw session revoke` then `jaw session setup` with new permissions
- Editing `permissions` in config does NOT affect an active session — permissions are enforced on-chain
- Do NOT share `~/.jaw/keystore.json` — it contains the session private key in plaintext (file mode `0o600`); anyone who reads the file can spend within the on-chain permission scope until it expires or is revoked
