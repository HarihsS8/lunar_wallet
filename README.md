# Lunar Agent Wallet

A local, non-custodial wallet interface for Base mainnet. It reads ETH and canonical USDC balances from a connected injected EVM wallet. ETH portfolio values use a Coinbase spot quote; values are estimates, not accounting data.

The AI assistant can draft a direct ETH or USDC transfer, but it cannot sign, submit, or execute transactions. The app never requests or stores a seed phrase or private key. This is an experimental prototype, not an audited wallet. Base mainnet transactions use real funds and require ETH for gas.

## Run locally

Requires Node.js 18 or newer and an injected browser wallet such as MetaMask. No npm packages need to be installed.

1. Copy the example environment file:

	```sh
	cp .env.example .env
	```

2. To enable AI drafts, add your OpenAI API key to `.env`. The key stays on the local server and is never sent to the browser. Wallet balances and manual transfers work without an AI key.
3. Start the app and open it in a browser:

	```sh
	npm start
	```

	Open `http://127.0.0.1:4173`, connect your wallet, and approve the switch to Base if prompted.

`OPENAI_MODEL` defaults to `gpt-4o-mini`; `PORT` defaults to `4173`. Set either in `.env` to override. Never commit `.env`, share its contents, or enter a recovery phrase or private key into the app or AI prompt.

## Transfer flow

1. Connect an injected wallet. The app reads native ETH and USDC from Base mainnet.
2. Enter a transfer manually or ask the AI for an explicit ETH or USDC transfer draft to a recipient address. The agent does not support swaps, token approvals, or arbitrary contract calls.
3. Review the draft in the transfer dialog. The app checks the recipient format and refreshes the on-chain balance.
4. Continue to your wallet, inspect the transaction and gas fee there, then approve or reject it. The app cannot bypass this wallet confirmation.

## Scope

- Network: Base mainnet (chain ID 8453).
- Assets: native ETH and canonical USDC (`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`).
- Signing: the browser wallet signs each transaction; there is no unattended or autonomous signing.
- AI: OpenAI Chat Completions is used server-side to produce constrained transfer drafts. The user's prompt, public wallet address, and displayed balances are sent to OpenAI when requesting an AI response. Drafts are validated by the server and must still be reviewed and signed by the connected wallet.
- Pricing: ETH uses a cached Coinbase spot quote; USDC is estimated at $1. USD totals are approximate.
- Transaction history: this prototype lists transactions submitted during the current browser session; it does not index past wallet activity.

## Security notes

- The local server binds to `127.0.0.1`; it is intended for local development and should not be exposed publicly.
- The wallet extension holds the signing keys. The app never asks for a seed phrase, private key, or wallet password.
- The AI service receives prompts and public wallet context, but has no signing capability. Do not put secrets in prompts.
- This prototype has not been audited. Verify the network, recipient, amount, and gas fee in your wallet before approving any mainnet transaction.

## License

This project is licensed under the [MIT License](LICENSE).
