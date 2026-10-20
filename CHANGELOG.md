# Changelog

## v14 — 2026-10

HookHash `3CEEAB2F96E71E3AAEEA0C7220834B5F3D4643E82B1CB74B89B88A22C9722207`.

- Two ways to pass the right to open: key link (off-chain, optional named address) and key token (on-chain).
- The key signature is bound to the opener, the hook, the NetworkID, EXP, TID, AMT and a NONCE. An old signature does not work after a repack.
- New PACK checks: no token with an open sell offer, no burnable token in the box or as the key, the key must be a URIToken, the key must not be the content or already locked, no unknown MODE, no time limit in the past, no empty box.
- OPEN without a time limit is refused, except for a key-token box whose key token was burned.
- Box records no longer clash with the hook's own records.
- Testnet: manual 49 tests in 94 steps (99 signed tx, 13 hook Remits), all as expected. Automated 687/687 checks, 653 tx on chain.
- Needs `fix20261005` (enabled on testnet and on mainnet; checked 2026-10-20, ledger 26655259).
- No external audit yet. Source not published.

## Earlier

- Hook 70A3CE48…F037 (earlier draft): token mode, named QR, gift code in the token, protection of the locked token. With no time limit the owner could OPEN at any time.
- Hook C3C5FD66…5083 (tgshook_03): first publication. https://github.com/khorhito11111000/Xahau-Box-
