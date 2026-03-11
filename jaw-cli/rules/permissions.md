## Permissions

Grant, revoke, and query ERC-7715 permissions on smart accounts. Permissions allow third-party execution of specific operations (spend limits, contract calls) without per-transaction approval.

The `wallet_grantPermissions` params object takes:
- `expiry` — Unix timestamp (seconds) when the permission expires
- `spender` — address of the entity being granted execution rights (your app's backend/signer)
- `permissions.spends` — array of spend limits (native ETH or ERC-20)
- `permissions.calls` — array of contract call permissions

### Grant spend + call permissions

Always include both `spends` and `calls` in the `permissions` object. Providing only one may cause the request to fail.

**ERC-20 spend (100 USDC/day) + call permission:**

```bash
jaw rpc call wallet_grantPermissions \
  '{"expiry":1893456000,"spender":"0xSPENDER_ADDRESS","permissions":{"spends":[{"token":"0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913","allowance":"100000000","unit":"day","multiplier":1}],"calls":[{"target":"0xCONTRACT","selector":"0xSELECTOR"}]}}' \
  -o json -y
```

`allowance` is in the token's smallest unit as a decimal string (USDC has 6 decimals: 100 USDC = `"100000000"`).
`expiry` is a Unix timestamp in seconds.

Spend `unit` options: `minute` | `hour` | `day` | `week` | `month` | `year` | `forever`

**Native ETH spend (0.1 ETH/day) + call permission:**

Use `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE` (ERC-7528 native token address) for ETH:

```bash
jaw rpc call wallet_grantPermissions \
  '{"expiry":1893456000,"spender":"0xSPENDER_ADDRESS","permissions":{"spends":[{"token":"0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE","allowance":"100000000000000000","unit":"day","multiplier":1}],"calls":[{"target":"0xCONTRACT","selector":"0xSELECTOR"}]}}' \
  -o json -y
```

### Grant multiple spend permissions + call permissions

```bash
jaw rpc call wallet_grantPermissions \
  '{"expiry":1893456000,"spender":"0xSPENDER_ADDRESS","permissions":{"spends":[{"token":"0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE","allowance":"1000000000000000000","unit":"week","multiplier":1},{"token":"0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913","allowance":"100000000","unit":"day","multiplier":1}],"calls":[{"target":"0xCONTRACT","selector":"0xSELECTOR"}]}}' \
  -o json -y
```

Returns an object including a `permissionId` that can be used to revoke the permission later.

### Query active permissions

```bash
jaw rpc call wallet_getPermissions -o json
```

Returns an array of active permission objects, each with a `permissionId`, spender, expiry, and permission details.

### Revoke a permission

Use the `id` field (the `permissionId` returned by `wallet_grantPermissions`):

```bash
jaw rpc call wallet_revokePermissions \
  '[{"id":"0xPERMISSION_ID"}]' \
  -o json -y
```

### Key rules

- You MUST provide `spender` — the address of the entity being granted execution rights
- You MUST provide `expiry` as a Unix timestamp in seconds (not milliseconds)
- You MUST encode token amounts in the token's smallest unit (USDC: 6 decimals, ETH: 18 decimals)
- You MUST include both `spends` and `calls` in the `permissions` object — providing only one may cause the request to fail
- You MUST include `multiplier` in each spend object (e.g. `"multiplier": 1`)
- Use `unit` (not `period`) for the spend period: `minute | hour | day | week | month | year | forever`
- For native ETH spend, set `token` to `"0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE"` (ERC-7528 native token address)
- You MUST store the returned `permissionId` to revoke the permission later
- For `wallet_revokePermissions`, pass `id` (not `permissionId`) in the revoke object
- You MUST pass `-y` in AI agent contexts to skip confirmation prompts
