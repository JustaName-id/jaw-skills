## Product & Marketing

Core product descriptions, feature messaging, taglines, and copy blocks segmented by audience. Always defer to `rules/terminology.md` for naming and language conventions.

### Taglines and one-liners

**Primary tagline:**

> Web2 simplicity. Web3 sovereignty.

**Product-focused one-liners:**

| Context             | One-liner                                                                                |
| ------------------- | ---------------------------------------------------------------------------------------- |
| General             | Identity-first smart accounts with passkeys and programmable permissions.                |
| Developer pitch     | Two lines of code. Full smart account infrastructure.                                    |
| Business pitch      | Give every user a self-custodial onchain account without building wallet infrastructure. |
| Speed emphasis      | From zero to smart accounts in an afternoon.                                             |
| Replacement framing | Replace 12–18 months of wallet development with one SDK.                                 |

### The nine feature pillars

Use these as the canonical feature descriptions across all marketing materials. Each pillar has a headline, a one-sentence description, and the key proof point.

**1. One-Tap Onboarding**
Users sign up with Face ID, fingerprint, or device PIN. No seed phrases, no browser extensions, no app downloads. The experience is indistinguishable from any modern app.

**2. True Self-Custody**
The passkey private key lives in the user's device secure enclave — hardware that even the device's operating system cannot access. No custodians, no key servers, no KMS providers holding shares of your key.

**3. Gasless ENS Identity**
Every user gets a human-readable name (e.g., `alice.yourapp.eth`) at onboarding, issued gaslessly. No hex addresses. Portable across the entire ENS ecosystem.

**4. Pay Gas in Stablecoins**
Apps can sponsor gas entirely, or let users pay in USDC, USDT, or any ERC-20 token. No one needs to hold ETH to use the app.

**5. AI Agent Delegation**
Grant constrained permissions to AI agents: spending limits, whitelisted contracts, time-bounded sessions. Agents operate within defined boundaries — the account enforces the rules onchain.

**6. Recurring Payments & Subscriptions**
Users approve a payment schedule once. The app charges on schedule within the approved limits. No repeated wallet popups. Built on ERC-7715 permissions.

**7. Atomic Batch Transactions**
Multiple operations in a single user approval. Approve + swap, mint + transfer, any combination. Either all succeed or all revert — no partial states.

**8. Multi-Chain Support**
All EVM-compatible networks. Automatic chain detection. One account identity across chains.

**9. Portable or App-Native**
Two modes: **CrossPlatform** (one account across all JAW-integrated apps, like MetaMask) or **AppSpecific** (app-scoped accounts, invisible infrastructure). The app chooses what fits its product.

**10. Modular — Upgrade, Don't Replace**
Already using Privy, Dynamic, or Web3Auth? Keep your existing KMS signer and upgrade it to a smart account with JAW.id. The KMS key becomes one constrained signer on the account — gaining programmable permissions, batch transactions, and ENS identity without rebuilding your auth flow. JAW.id's stack is composable: use passkeys as the primary signer, or bring your own.

### Copy blocks by audience

#### For developers

> JAW.id is a wagmi-compatible smart account SDK. Add our connector, configure your API key, and your users get passkey-authenticated ERC-4337 accounts with gasless transactions, batch operations, and ERC-7715 permissions — using the same wagmi hooks you already know.
>
> It's an EIP-1193 provider. Every existing wagmi hook works out of the box. `useBalance`, `useSendTransaction`, `useSignMessage` — all of them. The only difference is what's underneath: a smart account instead of an EOA.

#### For business decision-makers

> JAW.id lets you offer your users self-custodial onchain accounts without building wallet infrastructure from scratch. Your users sign up with a fingerprint, get a human-readable identity, and can send, receive, and authorize transactions — all within your app's branded experience.
>
> You don't need a blockchain team. You don't need to integrate three separate providers for wallets, account abstraction, and identity. JAW.id is one SDK that replaces all three.

#### For partnership outreach

> JAW.id provides smart account infrastructure for apps that want to give users onchain capabilities without the complexity. We handle passkey authentication, account abstraction (ERC-4337), gas sponsoring, ENS identity, and programmable permissions — all through a single SDK integration.
>
> Our SDK is wagmi-compatible, supports all EVM chains, and offers both cross-platform and app-specific account modes. Current clients include Web3-native companies, with fintech and enterprise partnerships in active development.

### JustaName messaging

JustaName is JAW.id's identity layer, but it also operates as a standalone product.

**Standalone positioning:**

> JustaName is ENS infrastructure for programmable digital identity. Issue human-readable names under your own namespace, resolve profiles across chains, and give users a portable onchain identity — all through a simple API.

**Within JAW.id context:**

> Every JAW.id account comes with a human-readable ENS name, powered by JustaName. Users are identified by `alice.yourapp.eth`, not `0x7a3b...`. The name travels with them across any ENS-compatible app.

**Key proof points:**

- Official ENS infrastructure provider (Q2 2025)
- Funded by ENS ($300K yearly grant)
- 7 live clients including Peanut (fastest-growing crypto wallet in Argentina)
- 30+ clients in pipeline

### The "two lines of code" claim

This is one of our strongest hooks. Use it when the audience values speed-to-market. Always be prepared to back it up:

```tsx
import { jawConnector } from "@jaw.id/wagmi";

const connector = jawConnector({ apiKey: "YOUR_API_KEY" });
// That's it — add this connector to your wagmi config
```

The claim is accurate for the basic integration. A full-featured setup (ENS, gas sponsoring, permissions, custom UI) requires additional configuration, but the onboarding path is genuinely two lines.

### Passkey syncing and backup

Use this when explaining that "lose your device, lose your account" is not a real risk with passkeys. This is a common concern from non-technical audiences, investors, and enterprise partners.

**How it works — Apple (iCloud Keychain):**

- Devices form a syncing circle using asymmetric elliptic keys (P-384). Each passkey is encrypted so it can only be decrypted by devices within the user's circle of trust.
- Items transfer through Apple servers but are end-to-end encrypted — Apple cannot read the contents.
- Escrow recovery is encrypted using a hardware-bound keypair stored in an HSM, with the key inaccessible to Apple. Recovery requires iCloud authentication, SMS verification, _and_ the device passcode, with only 10 attempts allowed.

**How it works — Google (Password Manager):**

- Passkey private keys are uploaded only in encrypted form, using an encryption key accessible only on the user's own devices. Google itself cannot decrypt passkey material.
- Recovery on a new device requires either the Google Password Manager PIN or the screen lock of another existing device. Verification runs in secure hardware enclaves with a maximum of 10 attempts, even against internal attacks.

**The messaging takeaway:**

> Your passkey syncs automatically across your devices, end-to-end encrypted. Neither Apple nor Google can access the key material. If you get a new phone, your account comes with you — no recovery flow, no seed phrase backup, no provider dependency.

**Two layers of protection:**

1. **Passkey sync:** Handles the common case — lost or replaced device. Automatic, transparent, end-to-end encrypted.
2. **Smart account recovery:** Handles the edge case — total platform lockout. Guardian-based, social recovery, backup signers. Programmable at the account level.

Users can also register multiple passkeys on different devices as an additional backup strategy.

### Common mistakes

- Do NOT lead with technology when talking to business audiences — lead with the outcome (faster onboarding, lower development cost, self-custody without complexity).
- Do NOT list all nine features in every piece of content — pick the 2–3 most relevant to the audience and context.
- Do NOT use "Web3" in headlines for mainstream audiences — it's a filter that loses people. Use "digital accounts," "smart accounts," or describe the capability directly.
- Do NOT promise "free transactions" — say "gasless" and clarify that gas is sponsored by the app or paid in stablecoins.
- Do NOT describe JAW.id as "like Privy but better" — position it as a different architectural generation (smart accounts vs KMS), not a slightly improved version of the same thing. When relevant, emphasize that existing KMS setups can be _upgraded_ through JAW.id rather than replaced.
