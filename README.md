# Cellium Wallet for Hermes

Connect Hermes to one Cellium agent and use its available tools for wallet balances, transaction history, signing, transfers, swaps, bridges, and TWAP or limit orders. Aave, Morpho, and Hyperliquid tools appear for agents with access. Cellium discovers the tools for each agent at connection time.

## Connect an agent

1. Sign in to [Cellium](https://app.cellium.xyz) with an invited account. Choose an agent and copy its credential from **Agent Settings → Manual MCP credential**. Cellium grants wallet signing separately.
2. Install the plugin in Hermes 0.23.1 or later.

   ```sh
   hermes plugins install xjubiliance/cellium-hermes-plugin --enable
   ```

3. Set the **Agent credential** secret in the plugin's Hermes settings. If you use the CLI, set `CELLIUM_MCP_TOKEN` in the active Hermes profile's private `.env` or export it from a hidden terminal prompt before launching Hermes. For Bash or Zsh, run:

   ```sh
   read -rs CELLIUM_MCP_TOKEN && export CELLIUM_MCP_TOKEN
   hermes chat
   ```

   Paste the credential at the hidden prompt and press Enter. For a remote Hermes gateway, set the secret in that gateway's active profile and reload the connection.
4. Start a new chat or run `/reload-mcp`. Ask Hermes to use the **Cellium Wallet** plugin to show the connected agent's portfolio. Then try an exact transfer fee estimate or a swap quote for review.

The plugin's server is named `cellium_wallet`. A manual Hermes server with that same name takes precedence over the plugin connection. Use a fresh profile for the demo if you already have multiple Cellium connections.

## Approvals

Hermes treats the packaged connection as untrusted and prompts before calling write-capable tools. When reviewing an action, approve the specific call. The bundled skill asks for exact terms and approval before signing, submitting, or scheduling a trade. Cellium also checks the agent's signing permission and the operation terms before executing.

## Catalog review

Cellium is invite-only. Maintainers can request an individual reviewer invite on the catalog PR, then provide their sign-in email privately. An invited reviewer can create a dedicated agent and verify portfolio discovery without funding it. A funded test agent is useful for demonstrating a live fee estimate or quote.

To verify the package without a Cellium account, clone this repository and run:

```sh
hermes plugins validate . --install-deps
```

## Disclosure

Hermes sends the credential and tool requests to the hosted Cellium MCP at `https://app.cellium.xyz/mcp`. Calls can include wallet addresses, transaction details and order parameters. Cellium uses chain and venue providers to fulfill requests. The agent credential stays in the active Hermes profile or session environment. Wallet signing is a separate Cellium approval.

The hosted server supplies its tool list and schemas at runtime. With active signing access, those tools can sign transactions, move funds or schedule future execution. The package contains a connection manifest and a workflow skill. It runs no local executable, stores no credentials itself, and adds no background work or telemetry.
