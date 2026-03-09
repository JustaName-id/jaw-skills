# Auth & Architecture

## How It Works

```
AI Agent / User
       |
       v
  jaw_rpc({ method, params })
       |
       v
  CLI classifies method
       |
  +----+----+----+
  |         |    |
  v         v    v
Read-only  Local  Signing/Session
(API)     (disk)  (browser)
  |         |    |
  v         v    v
Direct    Update  CLICommunicator
API call  config  1. Start HTTP server on 127.0.0.1
          /session 2. Open browser -> keys.jaw.id/cli-bridge
                  3. Bridge runs JAW SDK (AppSpecific mode)
                  4. User signs with passkey inline
                  5. Bridge POSTs result to localhost
                  6. CLI returns result
```

## Browser Auth Flow

When a signing or session method is called:

1. **CLI starts local HTTP server** on `127.0.0.1` (random port, loopback only)
2. **CLI opens browser** to `keys.jaw.id/cli-bridge` with callback URL, method, params, chainId
3. **Bridge page initializes JAW SDK** in AppSpecific mode with `ReactUIHandler`
4. **User signs with passkey** — the signing UI renders inline on the bridge page (no popup)
5. **Bridge POSTs result** to CLI's localhost callback
6. **CLI receives result** and closes the HTTP server

Since the bridge page runs on `keys.jaw.id` (same origin as the CrossPlatform popup), passkeys and accounts are shared — no separate registration needed.

## Session Management

- After `wallet_connect`, the session (address + chainId) is saved to `~/.jaw/session.json`
- Sessions expire after 24 hours
- `eth_accounts` returns the cached address without re-authenticating
- `wallet_disconnect` clears the local session
- `wallet_switchEthereumChain` updates the chainId in the session file locally

## Security

- **API key**: Sent via `x-api-key` header for API calls; URL fragment (`#apiKey=...`) for browser (never sent to server)
- **keysUrl**: Validated against `*.jaw.id` / localhost allowlist at set time and bridge time
- **HTTPS**: Enforced for non-localhost hosts
- **CORS**: Restricted to exact `keysUrl` origin on callback server
- **Callback URL**: Validated as localhost-only on the bridge page
- **requestId**: UUID validated on callback to prevent replay
- **Body size**: Limited to 1MB on callback server
- **Config directory**: `~/.jaw/` mode `0o700`, files mode `0o600`
- **Session validation**: Address format, chainId, and expiry dates checked on load
