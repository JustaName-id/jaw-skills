## Integration — Developer & Technical Buyer Messaging

How to talk about JAW.id's integration story with engineering teams, CTOs, and technical decision-makers evaluating or adopting JAW. This is devrel-voiced messaging — practical, developer-empathetic, and grounded in what teams actually experience.

### When to use

- Developer-facing conversations and technical demos
- CTO and engineering lead pitches
- Partnership and integration calls with technical stakeholders
- Technical blog posts and documentation landing pages
- Evaluation guides and "getting started" narratives
- Any context where the audience asks "what does integrating this actually look like?"

### The integration story

The strongest version of this story is concrete: what developers actually do vs. what they'd normally need to build.

**What your team actually does:**

> One package. One API key. Familiar wagmi hooks. Your existing `useBalance`, `useSendTransaction`, and `useSignMessage` hooks work unchanged — the only difference is what's underneath: a smart account instead of an EOA.

```tsx
import { jawConnector } from "@jaw.id/wagmi";

const connector = jawConnector({ apiKey: "YOUR_API_KEY" });
// Add this connector to your wagmi config — that's the integration
```

**What your team would normally need to build:**

- Wallet SDK or KMS provider (Privy, Dynamic, Web3Auth)
- Account abstraction bundler (ZeroDev, Biconomy, Pimlico)
- Paymaster service for gas sponsoring
- Identity provider or ENS integration
- Passkey server and WebAuthn implementation
- Recovery flow and key management infrastructure

> JAW.id replaces that entire list with a single SDK. Not by hiding the complexity behind a black box — by eliminating the layers that shouldn't exist in the first place. Smart accounts don't need KMS key management. Passkeys don't need a server. Identity is built into the account.

---

### Before & after: the stack JAW replaces

Use this table when engineering teams want to understand what they're removing from their architecture.

| Infrastructure component    | Without JAW.id                             | With JAW.id                                                                                |
| --------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------------ |
| **Wallet / key management** | Privy, Dynamic, or Web3Auth integration    | Built-in: passkey signers in device secure enclave                                         |
| **Account abstraction**     | ZeroDev, Biconomy, Pimlico, or Etherspot   | Built-in: ERC-4337 (EntryPoint v0.8)                                                       |
| **Bundler**                 | Self-hosted or third-party bundler service | Managed by JAW — no configuration needed                                                   |
| **Paymaster**               | Separate paymaster contract + funding      | Native stablecoin gas payments + sponsoring paymaster configurable in config or per-action |
| **Identity / naming**       | Custom ENS integration                     | Built-in: JustaName ENS infrastructure                                                     |
| **Passkey server**          | WebAuthn RP server + credential storage    | Built-in: passkeys are native account signers                                              |
| **Gas estimation**          | Custom logic per chain                     | Handled automatically                                                                      |
| **Account deployment**      | Deploy factory + initCode management       | Counterfactual — deployed on first transaction                                             |
| **Multi-chain support**     | Per-chain configuration and bridging logic | Automatic chain detection, one account identity                                            |
| **Recovery infrastructure** | Custom recovery flows, seed phrase backup  | Programmable: guardian recovery, backup passkeys                                           |

---

### What developers don't need to worry about

Use this when the audience is evaluating build-vs-buy or comparing integration complexity.

> Here's what goes away when you integrate JAW.id:

- **Bundlers.** You don't run one, configure one, or think about one. UserOperations are bundled and submitted automatically.
- **Paymasters.** Stablecoin gas payments are native — no paymaster contract deployment. Sponsoring paymaster is configurable in your config or on a per-action basis.
- **Node services.** No Alchemy, Infura, or QuickNode dependency for basic account operations. JAW handles RPC routing.
- **Account deployment.** Accounts are counterfactual — they exist before the first transaction. No deploy step, no initCode management, no factory contracts to maintain.
- **Passkey servers.** Passkeys are native smart account signers. No WebAuthn relying party server, no credential database, no attestation handling.
- **Gas estimation.** Handled per-chain, automatically. No custom gas logic, no EIP-1559 fee calculations, no L2-specific gas oracle integrations.
- **ENS resolution and subname issuance.** Built-in. Users get a `username.yourapp.eth` name at onboarding. No separate ENS integration.

---

### What "just works"

Use this to show the capabilities teams get without additional integration effort.

| Capability                   | What it means for developers                                                                                                      |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Gasless transactions**     | Users never see gas. App sponsors, or users pay in ERC-20 tokens. No paymaster code.                                              |
| **Batch operations**         | Multiple contract calls in one user approval. Approve + swap, mint + transfer. One `useSendCalls` action.                         |
| **Multi-chain**              | Same account, same identity, all EVM chains. No per-chain configuration.                                                          |
| **ENS subnames**             | Every user gets `name.yourapp.eth` at signup. Gasless issuance. No ENS contract interaction needed.                               |
| **Session caching**          | Passkey sessions reduce re-authentication prompts. Configurable expiration.                                                       |
| **ERC-20 gas payments**      | Users pay gas in USDC, USDT, or any supported token. No ETH required anywhere in the flow.                                        |
| **Programmable permissions** | ERC-7715 permissions for spending limits, whitelisted contracts, time-bounded sessions, AI agent delegation.                      |
| **Portable or app-native**   | Choose CrossPlatform (re-usable wallets across dApps) or AppSpecific (Passkey operations stay within your dApp). One config flag. |

---

### Talking points by audience

#### Engineering teams

> "Here's what your integration looks like: add the JAW wagmi connector, configure your API key in the dashboard, and your existing hooks work unchanged. Your users get passkey-authenticated smart accounts with gasless transactions. You don't set up a bundler, deploy a paymaster, or run a passkey server. That entire layer is handled."

Key angles:

- **Familiar stack.** wagmi hooks, EIP-1193 provider, React components. Nothing new to learn.
- **Two lines to start.** Full smart account infrastructure from a connector import and an API key.
- **Progressive complexity.** Start with basic accounts, add permissions, batch operations, and ENS customization as needed.

#### CTOs and technical buyers

> "JAW.id replaces three vendor integrations — wallet SDK, account abstraction provider, and identity service — with one. Your team ships smart accounts in an afternoon instead of building wallet infrastructure for 12–18 months. And if you already have a KMS provider, you don't rip it out — you upgrade it."

Key angles:

- **Build-vs-buy clarity.** One SDK vs. three vendors + 6 months of glue code.
- **Operational simplicity.** One dashboard, one billing relationship, one support channel.
- **The upgrade path.** Existing KMS signers (Privy, Dynamic) become constrained signers on a smart account. No migration, no rebuild.

#### Existing wagmi users

> "If you're already using wagmi, integrating JAW.id is adding a connector. Your `useBalance`, `useSendTransaction`, `useSignMessage` — all of them work. The difference is what's underneath: a smart account with gasless transactions, batch operations, and programmable permissions instead of an EOA."

Key angles:

- **Zero learning curve.** Same hooks, same patterns, same mental model.
- **Immediate capability upgrade.** Gasless, batch, permissions — without changing application code.

#### Teams migrating from KMS providers

> "You don't need to rip out Privy or Dynamic. Your existing KMS signer becomes one key on a JAW.id smart account — with spending limits, session expiration, and recovery options it never had before. It's an upgrade, not a migration."

Key angles:

- **No rebuild.** Keep existing auth flow. Add smart account capabilities on top.
- **Immediate gains.** Programmable permissions, batch transactions, ENS identity — from day one.
- **Reduced vendor risk.** The account works independently of any single signer. If the KMS provider has downtime, backup passkeys maintain access.

---

### Common mistakes

- **Don't oversimplify into "no code needed."** JAW.id is genuinely easy to integrate, but it's still a developer product. "Two lines of code" is accurate for the connector — but a production integration involves dashboard configuration, gas sponsoring setup, and UX decisions. Be honest about the path, not just the starting line.
- **Don't position as "another wallet SDK."** JAW.id replaces the wallet SDK _and_ the account abstraction provider _and_ the identity service. If you describe it as a wallet SDK, you're underselling by two-thirds. Use the three-in-one framing.
- **Don't forget the upgrade path for KMS teams.** Many prospects already have Privy or Dynamic integrated. Leading with "replace your stack" creates friction. Leading with "upgrade your stack" creates opportunity. Always mention that existing KMS signers can be used as constrained signers on a JAW.id smart account.
- **Don't list infrastructure components without the "what goes away" framing.** Saying "we handle bundlers, paymasters, and gas estimation" is less compelling than "you never configure a bundler, deploy a paymaster, or write gas estimation logic." Frame it from the developer's perspective — what they stop doing, not what JAW does.
- **Don't confuse this with SDK documentation.** This rule covers _how to talk about_ the integration experience. For actual implementation guidance, code examples, and API reference, use **jaw-sdk-best-practices**.
