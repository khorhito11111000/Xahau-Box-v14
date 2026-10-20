# Security

## What is in this repo

No hook source. No wasm. No private keys, seeds or secrets. Only documents, the HookHash, test addresses and transaction hashes.

The JSON in `docs/` uses placeholders like `<HOOK_ACCOUNT>`. Never put a seed or a private key into a transaction, an issue or a chat.

## Check the hook

The deployed hook is identified only by its HookHash:
`3CEEAB2F96E71E3AAEEA0C7220834B5F3D4643E82B1CB74B89B88A22C9722207`

Look up the hook account in an explorer and compare the HookHash before you trust a box.

## What the hook protects

While a box is closed, the creator's account cannot move the locked value:
- Payment, Remit, Escrow, PaymentChannel above the free balance: `tgs: reserve`.
- DEX offer or Check on locked XAH: `tgs: offer`, `tgs: check`.
- Sending, burning or selling a locked token: `tgs: locked`.
- Multi-signed transactions do not bypass the hook.

When opening:
- A key signature is bound to the opener's address, the hook, the network, the box contents and the box NONCE. A copied CLAIM from another address fails (`tgs: bad sig`).
- A key-token box opens only for the current holder (`tgs: not holder`).
- Many CLAIMs in one ledger give one winner and one payout.

## Phishing and bots

- A named-box QR or a key-token QR gives nothing to a stranger. It can be public.
- A bearer QR gives the box to whoever uses it first. Keep it private.
- Check what you sign in your wallet.

## Known limits

See [LIMITATIONS.md](LIMITATIONS.md). Most important: the creator can delete the hook while a box is closed.

## Report a problem

Open an issue in this repository, or write to the author directly. Describe the steps and the transaction hashes. Do not put keys, seeds or bearer QR codes in the report. For a serious problem, write privately first.
