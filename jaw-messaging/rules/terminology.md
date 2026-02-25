## Language & Terminology

Single source of truth for how we talk about JAW.id. Every other messaging rule file defers to this one for naming and language conventions.

### Product names

| Canonical Name | Usage | Never Say |
|---|---|---|
| **JAW.id** | The product. Always styled `JAW.id` with uppercase JAW and lowercase `.id`. | Jaw, jaw.id, JAW.ID (all-caps ID), JAW Id, JawID |
| **JustaName** | The ENS identity service. One word, camelCase N. | Justa Name, JustaName.id (unless referring to the website), JUSTANAME |
| **JustaLab** | The parent company. One word, camelCase L. | Justa Lab, JustaLabs (no plural), JUSTALAB |
| **JAW SDK** | The developer toolkit. Refers to `@jaw.id/wagmi`, `@jaw.id/core`, and `@jaw.id/ui` collectively. | JAW.id SDK, Jaw SDK |
| **JAW Dashboard** | The developer portal at dashboard.jaw.id. | JAW console, JAW admin |

### Core concepts — approved terms

| Use This | Not This | Why |
|---|---|---|
| smart accounts | smart wallets | "Wallet" implies a container for money. JAW.id provides programmable onchain accounts — identity, permissions, and signing, not just asset storage. |
| passkey signers | passkey auth, passkey login | The passkey is a *signer on the account*, not an authentication mechanism in the traditional sense. This distinction is architecturally important. |
| identity-first | identity-based, identity-centric | "Identity-first" is our positioning adjective. It signals that the account starts with a human-readable name, not a hex address. |
| programmable permissions | access control, authorization rules | "Programmable permissions" communicates that the rules are flexible, onchain, and developer-defined. |
| self-custody | non-custodial, decentralized custody | "Self-custody" is clearer for non-technical audiences. "Non-custodial" defines by negation. |
| true self-custody | self-custody (when contrasting with KMS) | Use "true self-custody" specifically when differentiating from KMS providers that claim non-custodial status but still hold key material. |
| onchain | on-chain, on chain | One word, no hyphen. Consistent with ENS and broader ecosystem convention. |
| gasless transactions | free transactions, zero-gas | "Gasless" means the user doesn't pay gas — someone (the app, a paymaster) still does. "Free" is misleading. |
| gas sponsoring | gas subsidies, gas abstraction | "Sponsoring" communicates that someone is paying on the user's behalf. |
| human-readable names | usernames, handles | ENS names are more than usernames — they're portable onchain identities. "Human-readable names" is the neutral, accurate description. |
| namespace sovereignty | white-label ENS, custom domains | "Namespace sovereignty" means the business owns and controls its identity namespace (e.g., `alice.yourapp.eth`). |
| AI agent delegation | AI agent wallets, bot wallets | "Delegation" emphasizes constrained, revocable authority — not giving an agent its own wallet. |
| account abstraction | AA | Spell out on first use in any document. Abbreviation is fine after that. |
| ERC-4337 | ERC4337, EIP-4337 | Use the ERC prefix (it's a finalized standard). Hyphenated. |
| batch transactions | bundled transactions, multi-call | "Batch" is the standard term in the ERC-4337 ecosystem. |

### Describing what JAW.id is

**One-liner (10 words):**
> Identity-first smart accounts with passkeys and programmable permissions.

**Short description (one sentence):**
> JAW.id is smart account infrastructure that lets any app give users self-custodial onchain accounts with passkey authentication, gasless transactions, and programmable permissions — in two lines of code.

**Medium description (two sentences):**
> JAW.id replaces 12–18 months of wallet infrastructure development with a single SDK integration. It gives every user a passkey-secured smart account with a human-readable ENS name, gasless transactions, batch operations, AI agent delegation, and a programmable permission layer — all in a familiar, one-tap onboarding experience.

**What JAW.id is NOT:**
- Not a wallet app (it's infrastructure for other apps to build on)
- Not a custodian (keys live in the user's device secure enclave)
- Not an identity provider (it leverages ENS, the existing decentralized naming system)
- Not only for crypto-native users (it's designed for mainstream onboarding)

### Audience-specific language adjustments

| Audience | Lean Into | Avoid |
|---|---|---|
| Developers | SDK specifics, code examples, ERC/EIP numbers, wagmi compatibility | Marketing superlatives, vague promises |
| Business decision-makers | ROI, time-to-market, competitive advantage, compliance readiness | Raw ERC numbers, implementation details |
| Investors | Market size, traction, competitive moat, team credibility | Deep technical architecture, code |
| Conference organizers | Timeliness, thought leadership, audience relevance | Internal metrics, pricing |
| General / press | Plain language, analogies, user impact | Jargon, acronyms, Ethereum-specific terms |

### Common mistakes

- Do NOT say "JAW wallet" — JAW.id provides smart *accounts*, not a wallet app.
- Do NOT say "we store keys" or "we manage keys" — the passkey private key lives in the user's device secure enclave. JustaLab never touches it.
- Do NOT say "blockchain login" — say "passkey onboarding" or "one-tap onboarding."
- Do NOT say "Web3 wallet" when describing JAW.id to non-crypto audiences — say "digital account" or "smart account."
- Do NOT use "decentralized" as a selling point in isolation — explain *what* is decentralized and *why it matters* (e.g., "your account works forever, even if we disappear").
- Do NOT say "replace MetaMask" — JAW.id is infrastructure for apps, not a consumer-facing wallet replacement.
- Do NOT describe JustaName as "just" a naming service — it's a portable onchain identity layer with ENS subname management, profile resolution, and interoperable credentials.
