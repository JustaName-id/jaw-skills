## Installation

Install the JAW CLI globally using your preferred package manager.

### Install

```bash
npm install -g @jaw.id/cli
# or
yarn global add @jaw.id/cli
# or
pnpm add -g @jaw.id/cli
# or
bun add -g @jaw.id/cli
```

### Requirements

- Node.js 18 or later
- A modern browser (Chrome, Firefox, Safari, or Edge) — required for passkey authentication

### Verify installation

```bash
jaw --version
```

### First-run behavior

The first time you run any RPC command, the CLI automatically:

1. Spawns a background daemon on a local port
2. Opens `https://keys.jaw.id` in your default browser
3. Waits for you to authenticate with your passkey

The daemon stays alive across commands and shuts down after 30 minutes of inactivity.
You do not need to run `jaw start` or any other setup command.

### Stopping the daemon

```bash
jaw disconnect
```

This stops the daemon and closes the browser session. The next RPC call will re-open the browser.

### Key rules

- You MUST have Node.js 18+ installed
- You MUST have a modern browser available — passkey authentication cannot happen without one
- You MUST set `apiKey` and `defaultChain` before making RPC calls (see <rules/configuration.md>)
- Do NOT close the browser tab that opens — it is the signing backend for all CLI requests
