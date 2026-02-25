## ENS Identity

ENS subname issuance, programmatic subname creation, and profile resolution. JAW SDK (`@jaw.id/core`) handles subname issuance during wallet connect, while JustaName SDK (`@justaname.id/sdk`) is a **separate SDK** for full programmatic subname management. Both SDKs use the same API key from `dashboard.jaw.id`.

### JustaName SDK vs JAW SDK

- **JAW SDK** (`@jaw.id/core`, `@jaw.id/wagmi`) — smart account wallet with passkey authentication. Handles ENS subname issuance automatically during account creation. Use this when you want users to claim a subname as part of the connect flow.
- **JustaName SDK** (`@justaname.id/sdk`, `@justaname.id/react`) — dedicated ENS subname management. Create, reserve, accept, revoke, and resolve subnames programmatically. Use this when you need full control over subname lifecycle outside the wallet connect flow.

The **same API key** from `dashboard.jaw.id` works for both SDKs — you do NOT need a separate key for JustaName SDK.

They are complementary: JAW auto-issues subnames on connect; JustaName gives full programmatic control over subname creation and management.

### ENS configuration

You MUST configure your ENS domain in the JAW Dashboard (dashboard.jaw.id) before adding it to your SDK config. The dashboard registers your domain for subname issuance -- without this step, subname creation will fail silently.

```typescript
import { jaw } from '@jaw.id/wagmi';

const connector = jaw({
  apiKey: 'your-api-key',
  ens: 'myapp.eth', // Must be registered in JAW Dashboard first
});
```

Or with core provider:

```typescript
import { JAW } from '@jaw.id/core';

const jaw = JAW.create({
  apiKey: 'your-api-key',
  ens: 'myapp.eth',
});
```

### Subname issuance during connect

When `ens` is configured, new users are automatically prompted to claim a subname during account creation (e.g., `alice.myapp.eth`). Returning users who already have a subname skip this step.

You MUST NOT expect subname prompts for existing accounts -- subnames are only issued when a user creates a new account.

### Attaching text records during connect

You MUST use the `subnameTextRecords` capability inside `wallet_connect` (or `useConnect` from `@jaw.id/wagmi`) to attach text records to the user's subname during connection. Text records are only attached if `ens` is configured and the user creates a new account.

```tsx
import { useConnect } from '@jaw.id/wagmi';
import { config } from './config';

function ConnectWithProfile() {
  const { mutate: connect } = useConnect();

  const handleConnect = () => {
    connect({
      connector: config.connectors[0],
      capabilities: {
        subnameTextRecords: [
          { key: 'avatar', value: 'https://myapp.com/avatars/default.png' },
          { key: 'description', value: 'A myapp.eth user' },
          { key: 'url', value: 'https://myapp.com' },
          { key: 'com.twitter', value: '@myapp' },
        ],
      },
    });
  };

  return <button onClick={handleConnect}>Connect & Claim Subname</button>;
}
```

### Subname creation (JustaName SDK)

For programmatic subname creation outside the wallet connect flow, use `@justaname.id/sdk` or `@justaname.id/react`.

```bash
npm install @justaname.id/sdk
# or for React
npm install @justaname.id/react
```

#### Initialize with ensDomains config

Subname creation requires the full config with `ensDomains` and `apiKey`. Resolution-only use cases only need `networks`.

```typescript
import { JustaName } from '@justaname.id/sdk';

const justaName = JustaName.init({
  networks: [{ chainId: 1, providerUrl: 'https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY' }],
  ensDomains: [{ chainId: 1, domain: 'myapp.eth', apiKey: 'your-api-key' }],
  config: { domain: 'yourdapp.com', origin: 'https://yourdapp.com' },
});
```

#### Core SDK methods

- `justaName.subnames.addSubname()` — create a new subname (requires SIWE auth + API key)
- `justaName.subnames.reserveSubname()` — reserve a subname for future claiming
- `justaName.subnames.acceptSubname()` — accept a reserved/invited subname
- `justaName.subnames.revokeSubname()` — revoke an existing subname
- `justaName.subnames.isSubnameAvailable()` — check availability

#### React hooks (`@justaname.id/react`)

- `useAddSubname` — claim/create subnames off-chain
- `useAcceptSubname` — accept invitations
- `useIsSubnameAvailable` — availability check with debouncing
- `useRevokeSubname` — revoke subnames
- `useUpdateSubname` — update text records and addresses

### Profile resolution (JustaName SDK)

Profile resolution uses `@justaname.id/sdk`. For resolution-only use cases, you only need `networks` in the init config — `ensDomains` with `apiKey` is only required when creating subnames.

```bash
npm install @justaname.id/sdk
```

#### Initialize the JustaName SDK

```typescript
import { JustaName } from '@justaname.id/sdk';

const justaName = JustaName.init({
  networks: [{
    chainId: 1,
    providerUrl: 'https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY',
  }],
});
```

#### Resolve by ENS name

```typescript
const profile = await justaName.subnames.getRecords({
  ens: 'alice.myapp.eth',
});

console.log(profile.records.texts);  // [{ key: 'avatar', value: '...' }, ...]
console.log(profile.records.coins);  // [{ id: 60, name: 'ETH', value: '0x...' }]
```

#### Reverse resolve by address

```typescript
const result = await justaName.subnames.reverseResolve({
  address: '0x1234...abcd',
  chainId: 1,
});

console.log(result.ens); // 'alice.myapp.eth'
```

### Key rules

- You MUST configure the ENS domain in the JAW Dashboard before adding `ens` to your SDK config -- the dashboard registers the domain for subname issuance.
- You MUST use `subnameTextRecords` inside `wallet_connect` (or `useConnect` capabilities) to attach text records -- there is no separate method.
- You MUST install `@justaname.id/sdk` separately for profile resolution -- it is NOT included in `@jaw.id/core` or `@jaw.id/wagmi`.
- You MUST NOT expect subname prompts for returning users -- subnames are only issued during new account creation.
- You MUST NOT use `eth_requestAccounts` when requesting subname capabilities -- use `wallet_connect` (or `useConnect` from `@jaw.id/wagmi`) instead.
- You MUST NOT request `subnameTextRecords` on a connector that lacks `ens` configuration -- text records require an ENS domain to be set.
- Text records are only attached if `ens` is configured AND the user creates a new account during the connection flow.
- You MUST use `@justaname.id/sdk` (not `@jaw.id/core`) for programmatic subname creation -- JAW SDK only issues subnames during wallet connect.
- You MUST provide `ensDomains` with `apiKey` in JustaName SDK init when creating subnames -- resolution-only use cases only need `networks`.
- The API key from `dashboard.jaw.id` works for both JAW SDK and JustaName SDK -- you do NOT need a separate key.
- You MUST NOT confuse `@justaname.id/sdk` with `@jaw.id/core` -- they are separate SDKs serving different purposes.

### Common mistakes

Do NOT try to issue subnames without dashboard setup:

```typescript
// Wrong -- ens domain not registered in dashboard, will fail
jaw({ apiKey: 'key', ens: 'unregistered.eth' })
```

Do NOT confuse the JustaName SDK with the JAW SDK:

```typescript
// Wrong -- getRecords is not on the JAW SDK
import { JAW } from '@jaw.id/core';
const jaw = JAW.create({ apiKey: 'key' });
jaw.getRecords({ ens: 'alice.myapp.eth' }); // Does not exist

// Correct -- use @justaname.id/sdk for resolution
import { JustaName } from '@justaname.id/sdk';
const justaName = JustaName.init({ networks: [{ chainId: 1, providerUrl: 'https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY' }] });
const profile = await justaName.subnames.getRecords({ ens: 'alice.myapp.eth' });
```

Do NOT try to create subnames with the JAW SDK:

```typescript
// Wrong -- JAW SDK does not have subname creation methods
import { JAW } from '@jaw.id/core';
const jaw = JAW.create({ apiKey: 'key', ens: 'myapp.eth' });
jaw.subnames.addSubname({ subname: 'alice.myapp.eth' }); // Does not exist

// Correct -- use @justaname.id/sdk for programmatic subname creation
import { JustaName } from '@justaname.id/sdk';
const justaName = JustaName.init({
  networks: [{ chainId: 1, providerUrl: 'https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY' }],
  ensDomains: [{ chainId: 1, domain: 'myapp.eth', apiKey: 'your-api-key' }],
  config: { domain: 'yourdapp.com', origin: 'https://yourdapp.com' },
});
await justaName.subnames.addSubname({ subname: 'alice.myapp.eth' });
```

Do NOT initialize JustaName SDK without `ensDomains` when creating subnames:

```typescript
// Wrong -- missing ensDomains, subname creation will fail
const justaName = JustaName.init({
  networks: [{ chainId: 1, providerUrl: 'https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY' }],
});
await justaName.subnames.addSubname({ subname: 'alice.myapp.eth' }); // Fails

// Correct -- include ensDomains with apiKey for subname creation
const justaName = JustaName.init({
  networks: [{ chainId: 1, providerUrl: 'https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY' }],
  ensDomains: [{ chainId: 1, domain: 'myapp.eth', apiKey: 'your-api-key' }],
  config: { domain: 'yourdapp.com', origin: 'https://yourdapp.com' },
});
```

Do NOT use a different API key for JustaName SDK:

```typescript
// Wrong -- using a separate key for JustaName SDK
ensDomains: [{ chainId: 1, domain: 'myapp.eth', apiKey: 'some-other-key' }]

// Correct -- same API key from dashboard.jaw.id works for both SDKs
ensDomains: [{ chainId: 1, domain: 'myapp.eth', apiKey: 'your-jaw-api-key' }]
```
