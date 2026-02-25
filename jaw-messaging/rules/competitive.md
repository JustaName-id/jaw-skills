## Competitive Positioning

How JAW.id differentiates against named competitors. Use this for sales conversations, partnership pitches, conference applications, and any content that requires positioning against alternatives.

### The framing: architectural generation, not feature comparison

The strongest competitive argument is not that JAW.id has more features — it's that JAW.id is a **different architectural generation**.

> KMS-based providers are managing the private key. JAW.id eliminates the need to manage it at all.

This is not a marginal improvement. It's the difference between optimizing horse-drawn carriages and building a car. Always lead with the architectural distinction before comparing features.

### The upgrade path: KMS as a signer, not a replacement

JAW.id's stack is modular. Apps that already use a KMS provider (Privy, Dynamic, Web3Auth) don't need to rip it out — they can use their existing KMS signer as one signer on a JAW.id smart account. The KMS key becomes a constrained signer with programmable permissions, instead of the account itself.

This reframes the competitive conversation entirely:

> JAW.id doesn't replace your KMS provider. It upgrades it. Your existing signer becomes one key on a smart account — with spending limits, session expiration, and recovery options it never had before.

**Why this matters:**

- **Eliminates migration friction.** Teams with Privy/Dynamic already integrated can adopt JAW.id without rebuilding their auth flow.
- **Turns competitors into components.** KMS providers become one signer among many, not the account itself. This is architecturally correct — a signer should be replaceable, not the identity.
- **Lowers the decision threshold.** "Upgrade your existing stack" is easier to say yes to than "replace your existing stack."

### The three-in-one replacement

JAW.id replaces three separate integrations that competitors require apps to assemble independently — or upgrades existing ones into a unified stack:

| Layer                     | What You'd Otherwise Need                | JAW.id                                                     |
| ------------------------- | ---------------------------------------- | ---------------------------------------------------------- |
| **Wallet infrastructure** | Privy, Dynamic, or Web3Auth              | Built-in: passkey smart accounts                           |
| **Account abstraction**   | ZeroDev, Biconomy, Pimlico, or Etherspot | Built-in: ERC-4337 (EntryPoint v0.8), paymasters, bundlers |
| **Identity service**      | Namespace or custom ENS integration      | Built-in: JustaName ENS infrastructure                     |

> One SDK. One integration. One dashboard. Instead of stitching three vendors together.

### KMS vs Smart Accounts (the structural comparison)

Use this table when the conversation turns to "how are you different from Privy/Dynamic/Web3Auth?"

|                        | KMS Providers (Privy, Dynamic, Web3Auth)                                                                             | JAW.id                                                                                                     |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Key architecture**   | ECDSA key split across device, provider servers, and recovery. Shares reassembled or MPC-signed at transaction time. | Passkey in device secure enclave. No key to split, reassemble, or manage.                                  |
| **Custody model**      | Provider holds key shares. Technically "non-custodial" but provider is needed for signing.                           | True self-custody. The device is the signer. JustaLab never touches key material.                          |
| **Root of trust**      | Auth token (OTP, OAuth session). Phishable.                                                                          | Biometric (Face ID, fingerprint). Hardware-bound.                                                          |
| **Service dependency** | Provider must be online for signing. If provider shuts down, access requires key recovery.                           | Account works forever. Signers are replaceable without changing the account.                               |
| **Permission control** | None. The key has unlimited authority. A compromised session signs anything.                                         | Programmable. Spending limits, whitelisted contracts, session expiration, cosignatures — enforced onchain. |
| **Transaction model**  | Single transactions, gas in ETH only.                                                                                | Batch transactions, gasless, pay gas in any ERC-20 token.                                                  |
| **Recovery**           | Provider-dependent recovery flows.                                                                                   | Account-level: add backup passkeys, guardians, social recovery — programmable and composable.              |

### Head-to-head: named competitors

#### Privy (acquired by Stripe)

- **What they do well:** Largest installed base (75M+ accounts). Strong developer experience. Now part of Stripe's distribution.
- **Structural limitation:** KMS architecture — keys on company servers. Auth token as root of trust. No onchain permission layer. Since Stripe acquisition, heavily tied to Arbitrum ecosystem.
- **JAW.id advantage:** True self-custody. Programmable permissions. Chain-agnostic. ENS identity layer. Independent — not locked into a single L2 ecosystem.
- **Upgrade angle:** Apps already using Privy can keep their Privy signer and upgrade to JAW.id smart accounts — gaining permissions, batch transactions, and ENS identity without rebuilding auth.
- **Messaging angle:** "Privy onboarded millions to KMS wallets. JAW.id upgrades them to smart accounts they actually own — and you don't even have to rip out Privy to do it."

#### Dynamic (acquired by Fireblocks)

- **What they do well:** Multi-wallet aggregation. Strong enterprise positioning via Fireblocks.
- **Structural limitation:** Same KMS model. Fireblocks acquisition signals institutional custody focus, not consumer self-custody.
- **JAW.id advantage:** Self-custody native. Not an aggregator — a purpose-built smart account SDK with identity.
- **Upgrade angle:** Dynamic's KMS signer can be used as a signer on a JAW.id smart account — teams keep their existing auth, gain smart account capabilities.
- **Messaging angle:** "Dynamic connects to existing wallets. JAW.id creates smart accounts that don't need a separate wallet — or upgrades the ones Dynamic already manages."

#### ZeroDev (acquired by Offchain Labs)

- **What they do well:** Deep account abstraction expertise. Good developer tooling for ERC-4337.
- **Structural limitation:** Very low-level — developers must assemble their own stack. No identity layer. No UI components. No turnkey onboarding. Now tied to Arbitrum/Offchain Labs.
- **JAW.id advantage:** Full-stack solution: onboarding UX + smart accounts + permissions + ENS identity + UI components. One integration, not a toolkit assembly project.
- **Messaging angle:** "ZeroDev gives you account abstraction primitives. JAW.id gives you a complete smart account product."

#### Porto (Paradigm)

- **What they do well:** Similar unified vision (smart accounts + permissions). Paradigm brand and technical credibility.
- **Current status:** Porto's team has been redeployed to the Tempo blockchain. Self-custody smart wallet development has stalled for 3+ months.
- **JAW.id advantage:** Actively shipping. Full SDK available. ENS identity layer (Porto has none). Mobile-native support. ERC-4337 + EIP-7702 support.
- **Messaging angle:** "Porto had the right thesis. JAW.id is actually building it."

#### Coinbase Smart Account

- **What they do well:** Massive distribution through Coinbase ecosystem. Brand trust.
- **Structural limitation:** Tightly coupled to Coinbase ecosystem. No namespace sovereignty — you get `*.base.eth`, not your own domain. Limited developer customization.
- **JAW.id advantage:** Namespace sovereignty (issue identities from any domain). Both app-specific and cross-platform modes. Not locked into any ecosystem. Full SDK personalization.
- **Messaging angle:** "Coinbase gives your users a Coinbase account. JAW.id gives your users _your_ account."

### Feature comparison matrix

Use this when a visual comparison is needed (decks, landing pages, competitive one-pagers).

| Feature                         | JAW.id | Privy   | ZeroDev | Porto | Coinbase SA          |
| ------------------------------- | ------ | ------- | ------- | ----- | -------------------- |
| App-specific mode               | Yes    | No      | No      | Yes   | No                   |
| Cross-platform mode             | Yes    | Yes     | No      | Yes   | Yes                  |
| Full mobile native              | Yes    | Partial | No      | No    | Partial              |
| 2-line wagmi connector          | Yes    | No      | No      | No    | No                   |
| EIP-7702 support                | Yes    | No      | No      | Yes   | No                   |
| ERC-4337 (EP v0.8)              | Yes    | No      | Yes     | No    | Yes                  |
| ENS identity layer              | Yes    | No      | No      | No    | Partial (cb.id only) |
| Namespace sovereignty           | Yes    | No      | No      | No    | No                   |
| Multi-level SDK personalization | Yes    | Limited | No      | No    | No                   |
| True self-custody (no KMS)      | Yes    | No      | No      | Yes   | Partial              |

### Rules of engagement

- **Never trash competitors by name in public content.** Describe the architectural difference. Let the audience draw the conclusion.
- **In private conversations (sales, investor, BD):** direct comparison is fine and expected. Use the head-to-head sections above.
- **Always acknowledge what competitors got right** before explaining the limitation. "Privy onboarded millions — that's a real achievement. The question is what architecture those accounts are built on."
- **Lead with architecture, not features.** Features can be copied. The KMS-to-smart-account shift cannot be retrofitted — it's a rebuild.
- **Update this document as competitors evolve.** Competitive positioning is perishable. Review quarterly.

### Common mistakes

- Do NOT say "Privy is insecure" — say "KMS architecture has a structural limitation: the key still has unlimited authority." The difference is precision, not diplomacy.
- Do NOT compare features that JAW.id doesn't have yet. Only claim what's shipped.
- Do NOT ignore Porto — it's the closest architectural match. Address it directly (their team is redeployed, development stalled) rather than hoping the audience doesn't ask.
- Do NOT forget the three-in-one framing — it's the simplest way to explain why JAW.id replaces multiple vendors, not just one.
- Do NOT overweight the acquisition narrative — the fact that Privy was acquired by Stripe is a signal of category validation, not a weakness. Frame it as: "The market validated the category. JAW.id is the next-generation approach."
