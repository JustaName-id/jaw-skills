## Investor Messaging

Narratives, traction metrics, moat articulation, and market framing for fundraising conversations. This file covers _what to say_ to investors — not financial terms or asks.

### The investment narrative (three pillars)

Every investor conversation should touch these three pillars, in order:

**1. The category is validated.**
$200M+ has been invested in wallet and account abstraction infrastructure. Privy was acquired by Stripe. Dynamic by Fireblocks. ZeroDev by Offchain Labs. Web3Auth by Consensys. This is not a speculative category — the market has spoken.

**2. The incumbents are architecturally stuck.**
Every acquired company is built on KMS — key management systems that split and reassemble private keys. That architecture has a structural ceiling: the key still exists, still has unlimited authority, and still creates vendor dependency. Smart accounts with passkeys are the next generation, and you can't retrofit KMS into smart accounts — it's a rebuild.

**3. JAW.id is the infrastructure for what comes next.**
JAW.id is a unified SDK that replaces three separate integrations (wallet + account abstraction + identity) with one. It's built natively on ERC-4337 smart accounts, passkey signers, and ENS identity. The architecture is right, the timing is right, and the product is shipping.

### Market validation signals

Use these data points to establish that the market exists and is growing.

**Capital deployed:**

- $200M+ raised in wallet/AA infrastructure
- Privy acquired by Stripe
- Dynamic acquired by Fireblocks
- ZeroDev acquired by Offchain Labs
- Web3Auth acquired by Consensys
- Magic: $83M raised
- Biconomy: $10.5M raised

**Adoption (smart accounts):**

- 49M+ active smart accounts
- 820M+ UserOps executed
- 75M+ accounts created via Privy

**Identity / ENS:**

- 40M+ World App users
- 6M+ cb.id names (Coinbase)
- 2M+ uni.eth names (Uniswap)

> The market has validated identity wallets at scale. But only giants can build them. JustaLab makes it accessible for everyone else.

### Traction (JustaName — launched Q4 2024)

- Official ENS infrastructure provider (Q2 2025)
- $300K yearly ENS grant
- 7 live clients including Peanut (fastest-growing crypto wallet in Argentina)
- 30+ clients in pipeline

### Traction (JAW.id — launching Q1 2026)

- Final audit completed
- Beta launch phase
- 30+ promising leads from Web Summit (Doha) and Consensus (Hong Kong) in Feb 2026
- Lead profile: banks and telcos (Web Summit), Web3 companies (Consensus)

### Moat articulation

Investors will ask "what's your moat?" Here are the four defensible advantages:

**1. Architectural lead.**
JAW.id is built natively on smart accounts + passkeys. Competitors built on KMS cannot retrofit this architecture — it's a fundamental rebuild. Being native to the right architecture from day one is a structural advantage that grows with the ecosystem.

**2. ENS infrastructure position.**
JustaLab is an official ENS infrastructure provider with a $300K yearly grant. JustaName is the identity layer that every JAW.id account uses. No competitor has a comparable position in the decentralized naming ecosystem.

**3. Namespace sovereignty.**
JAW.id lets businesses issue identities from their own domain (`alice.yourapp.eth`). This creates switching costs — once a business has issued identities under their namespace, migrating away means changing every user's identity. This is a natural retention mechanism.

**4. Modular upgrade path.**
JAW.id doesn't require apps to rip out existing infrastructure. KMS signers from Privy, Dynamic, or Web3Auth can be used as signers on JAW.id smart accounts. This means every app with an embedded wallet is a potential upgrade customer, not a competitor's locked-in user. The upgrade path lowers adoption friction and turns competitor installed bases into distribution.

**5. Unified stack.**
Replacing three vendors (wallet + AA + identity) with one SDK creates integration depth. Once an app is built on JAW.id, switching back to a three-vendor assembly is a downgrade in developer experience. Consolidation is sticky.

### The JustaName → JAW.id evolution story

Investors want to understand the trajectory, not just the snapshot. Here's the narrative:

> JustaLab started with JustaName — ENS infrastructure for programmable digital identity. We became an official ENS infrastructure provider, earned a $300K yearly grant, and onboarded 7 clients with 30+ in pipeline.
>
> Through that work, we saw the same pattern repeatedly: every client who wanted to issue identities also needed to give users onchain accounts. And the available options — KMS wallets from Privy, Dynamic, or Web3Auth — were architecturally limited.
>
> So we built JAW.id: smart account infrastructure that includes JustaName's identity layer, plus passkey authentication, gasless transactions, programmable permissions, and everything else an app needs to give users onchain accounts. One SDK. No KMS. True self-custody.
>
> The identity business gave us distribution, credibility, and the ENS relationship. The smart account infrastructure is the product that scales.

### The team narrative

**Ryan Naayem, CEO**
Axiom Zen / Dapper Labs founding team — the people who invented NFTs (ERC-721) and built CryptoKitties. Co-founded ZenHub (used by Microsoft, Apple, Adobe, Docker). Co-founded KLKTN (acquired by Animoca Brands). 14+ years building at the intersection of developer tools and emerging technology.

**Ghadi Mhawej, CTO**
7+ years in blockchain development. Architected the UAE's largest crypto custody platform at FOO. Smart contract and identity systems expert. SEAL Steward (security alliance). Deep expertise in exactly the technical domain JAW.id operates in.

**Edwin Hage, CBO & COO**
CDO at Chedid Capital (50 countries, 1,700+ employees). Strategy& / Booz Allen consultant. PE investment manager (MENA growth funds). INSEAD MBA. 16+ years. Brings enterprise sales, financial partnerships, and operational scale-up experience.

Current team: 7 full-time, 3 part-time.

### TAM framing

Use these framings to contextualize the market opportunity without overpromising:

**Bottom-up (developer infrastructure):**

> Every app that wants to give users onchain accounts is a potential JAW.id customer. Smart accounts are growing from 49M to what the industry expects will be hundreds of millions. JAW.id captures value on every account created and every UserOp executed.

**Stablecoin rails:**

> Stablecoins are becoming the default for cross-border payments, freelance payroll, and instant settlement. Every stablecoin payment needs an account to send from and receive into. JAW.id is the infrastructure that makes those accounts easy to create and self-custodial.

**AI agent economy:**

> As AI agents become economic actors, they need onchain accounts with programmable permissions. This is an entirely new category of demand that didn't exist 18 months ago. JAW.id's permission system is purpose-built for it.

### Investor FAQ (messaging guidance)

**"How are you different from Privy?"**
Privy is KMS — they split and manage private keys. JAW.id uses smart accounts with passkeys. The key never leaves the user's device. The account has programmable permissions. It's a different architecture, not a different feature set. See `rules/competitive.md` for full positioning.

**"Privy has 75M accounts. How do you compete with that scale?"**
We don't compete with it — we upgrade it. JAW.id's stack is modular: apps already using Privy can keep their Privy signer and use it as one signer on a JAW.id smart account. They gain programmable permissions, batch transactions, gasless UX, and ENS identity without rebuilding their auth flow. So Privy's 75M accounts aren't a moat against us — they're a distribution channel. Every Privy app is a potential upgrade customer. Beyond that, Privy's acquisition by Stripe means they're now optimizing for Stripe's priorities, not for being the best general-purpose smart account SDK.

**"Why not just use ZeroDev?"**
ZeroDev gives you account abstraction primitives — the low-level building blocks. You still need to build onboarding, add a wallet UI, integrate an identity layer, set up gas sponsoring, and stitch it all together. JAW.id is the complete product. See `rules/competitive.md`.

**"What about Porto (Paradigm)?"**
Porto validates our thesis — Paradigm saw the same architectural shift from KMS to smart accounts. But Porto has no identity layer, nor namespace sovereignty. JAW.id is a full-stack product (smart accounts + permissions + ENS identity + UI components), not just a smart account primitive. Porto confirms the direction; JAW.id is the complete infrastructure.

**"What's your business model?"**
Integration fees, SLAs, and user operation fees — a small transaction fee on every UserOp executed through JAW.id accounts. The model scales with usage.

### Common mistakes

- Do NOT lead with technology in investor conversations — lead with market validation, then explain why the architecture matters.
- Do NOT be defensive about company size — "7 full-time, shipping product, with ENS backing and 30+ pipeline leads" is a strength at this stage, not a weakness.
- Do NOT compare valuations or speculate on competitor financials — stick to publicly known facts (acquisitions happened, amounts raised).
- Do NOT understate the ENS relationship — being an official infrastructure provider with a $300K yearly grant is a significant credibility signal and distribution advantage.
- Do NOT forget the evolution narrative — JustaName → JAW.id shows strategic vision and market-informed product development, not a pivot.
