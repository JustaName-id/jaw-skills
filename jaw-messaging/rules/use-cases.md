## Use Cases

Concrete scenarios and examples for how JAW.id is used across verticals. Use these to make abstract product capabilities tangible for specific audiences.

### How to use this file

Each use case follows the same structure: the problem the vertical faces, how JAW.id solves it, which JAW.id features are most relevant, and an example narrative. Pick the use case that matches your audience — don't list all of them unless you're building a comprehensive overview.

---

### 1. Neobanks & Fintechs

**The problem:** Neobanks want to offer onchain capabilities (stablecoin accounts, DeFi yield, cross-border payments) but building wallet infrastructure from scratch requires 12–18 months and a dedicated blockchain team. KMS-based solutions introduce custody risk and vendor dependency.

**How JAW.id solves it:**
- One-tap onboarding: users sign up with biometrics, receive a self-custodial smart account instantly.
- Human-readable identity: `alice.neobank.eth` instead of `0x7a3b...`.
- Stablecoin payments: send, receive, and hold USDC/USDT with gasless transactions.
- Regulatory alignment: true self-custody means the neobank is not a custodian — reducing regulatory burden.

**Key features:** One-tap onboarding, true self-custody, gasless ENS identity, pay gas in stablecoins, multi-chain support.

**Example narrative:**
> A digital bank in Southeast Asia wants to offer stablecoin savings accounts. With JAW.id, their users sign up with a fingerprint, get a `username.bank.eth` identity, and can receive USDC from anywhere in the world. The bank never touches private keys. The user's account works even if the bank's servers go down.

---

### 2. TradFi & Enterprise

**The problem:** Traditional financial institutions want programmable, 24/7 settlement without the limitations of SWIFT and legacy clearing systems. They need onchain rails but cannot compromise on custody, compliance, or operational control.

**How JAW.id solves it:**
- Self-custodial accounts for institutional and end-user participants.
- Programmable permissions: spending limits, whitelisted counterparties, multi-sig thresholds for high-value transactions.
- Batch transactions: multiple settlement operations in a single atomic execution.
- Multi-chain: operate across EVM networks without separate integrations per chain.

**Key features:** Programmable permissions, atomic batch transactions, true self-custody, multi-chain support.

**Example narrative:**
> An asset manager runs a tokenized fund. With JAW.id, each investor has a smart account with their own ENS identity. Distributions are batched into single transactions. Redemptions are governed by onchain permissions with approval thresholds. Settlement is 24/7, instant, and auditable.

---

### 3. AI Agents

**The problem:** AI agents are becoming autonomous economic actors. They need to transact — pay for services, manage portfolios, execute strategies. But agents cannot do KYC, cannot hold bank accounts, and humans should not give unlimited payment credentials to autonomous software.

**How JAW.id solves it:**
- A human creates a smart account and grants the AI agent a constrained permission (ERC-7715).
- The permission defines: which contracts the agent can call, how much it can spend per period, when the permission expires.
- The agent operates as a delegated signer within those boundaries. The account enforces the rules onchain.
- The human can revoke the permission at any time.

**Key features:** AI agent delegation (ERC-7715 permissions), programmable permissions, headless Account API, true self-custody.

**Example narrative — DCA bot:**
> A user wants an AI agent to dollar-cost-average $50/week into ETH. They grant the agent a permission: call the DEX router, spend up to $50 USDC per week, valid for 90 days. The agent executes weekly. If the user wants to stop, they revoke the permission. The agent never had full account access — only the specific authority it was granted.

**Example narrative — Trading copilot:**
> A DeFi protocol offers an AI copilot that rebalances user portfolios. Each user grants the copilot a permission scoped to specific pool contracts with a daily spending cap. The copilot operates autonomously within bounds. No seed phrases shared. No unlimited access. Fully auditable onchain.

**Example narrative — Service payments:**
> An AI coding assistant needs to pay for compute, API calls, and cloud resources. Its owner grants a permission: up to $100/month, only to whitelisted service provider contracts. The agent pays for what it uses. The owner sees every transaction onchain.

---

### 4. Subscription Services & SaaS

**The problem:** Crypto-native subscription billing doesn't exist. Current options are either centralized payment processors (Stripe, which requires KYC and bank accounts) or repeated manual wallet approvals (terrible UX).

**How JAW.id solves it:**
- Users approve a subscription once via an ERC-7715 permission: amount, frequency, recipient.
- The service charges on schedule within the approved limits.
- No repeated wallet popups. No stored payment cards. No chargebacks.
- Users can revoke at any time — no "cancel my subscription" dark patterns.

**Key features:** Recurring payments & subscriptions (ERC-7715), programmable permissions, gasless transactions.

**Example narrative:**
> A content platform charges $9.99/month in USDC. Users approve the subscription with a fingerprint scan. The platform charges monthly, automatically, within the approved limit. The user can revoke anytime — the permission is theirs, not the platform's. No payment processor. No chargebacks. No intermediaries.

---

### 5. dApps & Web3-Native Products

**The problem:** Existing dApps use wagmi + MetaMask/WalletConnect, which limits users to those who already have a wallet. They want to onboard non-crypto users without rebuilding their entire stack.

**How JAW.id solves it:**
- JAW.id's wagmi connector is a drop-in addition. Existing hooks (`useBalance`, `useSendTransaction`, `useSignMessage`) work unchanged.
- Non-crypto users sign up with passkeys and get a smart account. Crypto-native users can still connect their existing wallets.
- No stack changes required — just add the JAW connector alongside WalletConnect.

**Key features:** 2-line wagmi connector, portable or app-native modes, gasless transactions, ENS identity.

**Example narrative:**
> A gaming studio has 50,000 MetaMask users and wants to onboard the next million. They add the JAW.id wagmi connector — two lines of code. New users sign up with Face ID, get a `username.game.eth` identity, and start playing with gasless transactions. Existing MetaMask users are unaffected. The studio didn't change a single existing hook.

---

### 6. Cross-Border Remittances

**The problem:** Remittance corridors (US→Philippines, EU→Africa, Gulf→South Asia) are expensive (5–7% average fees), slow (1–3 days), and require banking infrastructure that many recipients don't have.

**How JAW.id solves it:**
- Sender and receiver each have a JAW.id smart account with a human-readable name.
- Send USDC directly — instant settlement, near-zero cost.
- Receiver onboards with a fingerprint, no bank account needed.
- True self-custody means no remittance company controls the user's funds.

**Key features:** One-tap onboarding, gasless ENS identity, stablecoin payments, true self-custody, multi-chain support.

**Example narrative:**
> A remittance app in the Gulf serves migrant workers sending money to South Asia. Workers sign up with a fingerprint, get a `name.remit.eth` identity, and send USDC to family members who onboard the same way. Settlement is instant. Fees are a fraction of Western Union. Neither party needs a bank account.

---

### Selecting use cases for your content

| Audience | Lead Use Case | Supporting Use Case |
|---|---|---|
| Investors | AI Agents (growth narrative) | Neobanks (market size) |
| Enterprise / TradFi | TradFi & Enterprise | Subscription Services |
| Developer conference | dApps & Web3-Native | AI Agents |
| Fintech partnership | Neobanks & Fintechs | Cross-Border Remittances |
| General media | Cross-Border Remittances (human story) | AI Agents (novelty) |

### Common mistakes

- Do NOT list all six use cases in a single piece of content — pick 1–2 that match the audience.
- Do NOT lead with AI agents for audiences that don't understand the agent economy yet — it's a powerful narrative for tech-forward audiences but confusing for mainstream ones.
- Do NOT describe use cases as hypothetical if clients are already using JAW.id for them — check current traction and cite real examples when possible.
- Do NOT forget the "two lines of code" angle for the dApp use case — it's the most powerful proof point for developer audiences.
- Do NOT over-promise on remittance use cases — on-ramp/off-ramp integrations (fiat-to-stablecoin) are roadmap items. JAW.id handles the onchain account and transfer; the fiat bridge is a partner integration.
