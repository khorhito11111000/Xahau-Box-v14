# Xahau Box — hook v14

A hook for Xahau that locks coins (XAH), a token (URIToken), or both on the creator's account until a box is opened. The recipient does not have to be known in advance.

- **HookHash:** `3CEEAB2F96E71E3AAEEA0C7220834B5F3D4643E82B1CB74B89B88A22C9722207`
- **Tested on:** Xahau Testnet (NetworkID 21338), October 2026.
- **Mainnet:** needs the amendment `fix20261005`. It is enabled on Xahau mainnet (checked 2026-10-20, ledger 26655259).
- **Status:** tested on testnet only. No external audit yet.
- **Source:** the hook source is not published. The deployed hook can be checked only by its HookHash on chain.

## What a box is

A box holds:
- only XAH,
- only a token,
- or XAH and a token together.

The coins and the token stay on the creator's account. The hook locks them until the box is opened, or until the creator takes it back after the time limit. Nothing moves to a third account. There is no custodian.

## Two ways to pass the right to open

### 1. Key link (off-chain)

The right travels outside the chain: as a link, a QR, or a message. Whoever has the key opens the box.

A recipient address can be set. Then the creator keeps the key and gives out only a signature made for that one address. Only that address can open the box. Such a link can be shown to anyone.

### 2. Key token (on-chain)

The right travels inside the chain, as a URIToken. Whoever holds the token opens the box.

The token can be given, sold, passed on. The one who held it before cannot open the box anymore. The creator cannot either.

### Time limit

A time limit (a ledger number) can be added to both ways. After it, the recipient cannot open the box, and the creator can take the contents back.

## How it works

All commands are `Invoke` transactions with hook parameters. Examples are in [docs/](docs/).

1. **AUDIT.** The creator asks the hook to count the account's objects. This is needed before packing.
2. **PACK.** The creator packs a box. The hook records it and locks the XAH and/or token.
3. **CLAIM.** The recipient opens the box. The hook checks the key signature or the key token, then sends the contents to the recipient with its own Remit.
4. **OPEN.** The creator takes the box back after the time limit.

While a box is closed, the hook rejects any transaction from the creator's account that would move the locked value. Free XAH and other tokens move as usual.

## QR codes

QR is not part of the hook. The hook knows nothing about QR. It only checks the signed transaction.

A QR is just one way to carry the key link or the box number. A wallet, an xApp or a site can make and read it. Some ideas:
- a QR with the box number on the image of the key token: only the token holder can open;
- a QR on a gift token inside the box, with a link to an outside gift: a gift card, a subscription, a shop item. The recipient uses it after the box is opened.

A QR with the key inside is a secret. Share it only in private. A QR for a named box or a key-token box holds only the box number. It is safe to show.

More in [docs/QR.md](docs/QR.md).

## How it differs from vouchers

Xaman vouchers pass only as a link, off-chain. Here the right to open can be an on-chain asset (the key token), and opening can be limited to one address.

## Testing

All on Xahau Testnet, with the HookHash above.

- **Manual:** 49 tests in 94 steps, signed by hand in Xaman. 99 signed transactions and 13 payouts sent by the hook. All results matched the expectation.
- **Automated:** 687 checks, 687 passed, 653 transactions on chain. This includes escape attempts, races and multi-sign.

Details in [TESTS.md](TESTS.md).

## Limitations

Read [LIMITATIONS.md](LIMITATIONS.md) before you use it. In short:
- the creator can delete the hook while a box is closed;
- fees can push the balance below the locked XAH; then the hook answers `tgs: reserve` and waits for a top-up;
- a creator who knows the key or holds the key token can take a box back before the limit;
- a box with no time limit cannot be taken back with OPEN (except a key-token box whose key token was burned);
- if the recipient blocks the payout, the box is restored;
- v14 lets SetRemarks pass on a locked token, so if the box account itself issued the gift, it can rewrite the gift's Remarks while it is locked;
- it needs `fix20261005`;
- no external audit, testnet only.

## Interface

The interface is not part of this work. A wallet, an xApp, explorer support and good UX are a full product: it needs support, key safety, and care for users. That is work for a team. It is better if the wallets and explorers people already use (Xaman, xrplwin, Bithomp and others) can show boxes, pack them and open them.

The hook works and is tested on testnet. If the Xahau team or wallet developers see value in it, it can grow. If not, it stays as a working example of what hooks can do.

## Files

```
README.md        this file
TESTS.md         manual and automated tests
LIMITATIONS.md   what the hook cannot promise
SECURITY.md      how to report a problem
CHANGELOG.md     versions
docs/
  TRANSACTIONS.md  parameters and JSON examples
  SIGNING.md       how a key signature is made
  QR.md            QR ideas and formats
```

The repo has no hook source, no wasm, no keys and no seeds.

## Previous version

The first version of the hook (with source): [Xahau-Box-](https://github.com/khorhito11111000/Xahau-Box-)

Used @XahauNetwork, @XamanWallet, @XahauExplorer, @bithomp, @XRPLWin Hooks Builder, and AI.
