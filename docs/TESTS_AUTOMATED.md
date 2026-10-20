# Automated tests: full list

Hook v14 on Xahau Testnet (NetworkID 21338). HookHash `3CEEAB2F96E71E3AAEEA0C7220834B5F3D4643E82B1CB74B89B88A22C9722207`.

Date: 2026-10-17. Node.js scripts on fresh testnet faucet accounts. The user's own accounts were not used. The scripts are not in this repo.

**687 checks, 687 passed, 0 failed.**

A check is one expected result: a hook answer, a payout, a balance or a ledger state. Rows that were only notes (not checks) are left out. Rows marked "limitation" or "by design" show a known limitation; they pass when the chain behaves as documented in [LIMITATIONS.md](../LIMITATIONS.md).

Each section lists the checks in the order they ran. The ID is the scenario group used by the script.

## Summary

| Suite | Checks | Passed |
|---|---|---|
| [run_tests](#run_tests--167-checks) | 167 | 167 |
| [escape](#escape--29-checks) | 29 | 29 |
| [escape2](#escape2--14-checks) | 14 | 14 |
| [gaps](#gaps--26-checks) | 26 | 26 |
| [gaps2](#gaps2--35-checks) | 35 | 35 |
| [race](#race--85-checks) | 85 | 85 |
| [multisign](#multisign--13-checks) | 13 | 13 |
| [new_tests](#new_tests--93-checks) | 93 | 93 |
| [audit](#audit--99-checks) | 99 | 99 |
| [auditb](#auditb--11-checks) | 11 | 11 |
| [extra](#extra--115-checks) | 115 | 115 |
| **Total** | **687** | **687** |

Not counted: an extra emitfail probe. A real EmitFailure could not be made from a normal account, see [TESTS.md](../TESTS.md).

## run_tests — 167 checks

Main scenarios: the manual tests, QR boxes of all four kinds, many boxes at once.

Accounts:

- creator (hook account): [r9zGR7cSQtxpUkrB1HiWhCusGHz2cHeqW6](https://test.xahauexplorer.com/account/r9zGR7cSQtxpUkrB1HiWhCusGHz2cHeqW6)
- R1: [rrCbU2CFBGCDUfSHzWPcbW46bed4JpTzC](https://test.xahauexplorer.com/account/rrCbU2CFBGCDUfSHzWPcbW46bed4JpTzC)
- R2: [r4wVh3TExVY2uhcdbu3QvHMnjy8YvMDtBR](https://test.xahauexplorer.com/account/r4wVh3TExVY2uhcdbu3QvHMnjy8YvMDtBR)
- R3: [rDVrxoDKH3HM2sdN52Vh7BQksewumw6KVR](https://test.xahauexplorer.com/account/rDVrxoDKH3HM2sdN52Vh7BQksewumw6KVR)
- R4 (stranger): [rJeSkUUXY5D3gJzfQjg6DRx47KPdcuBpqe](https://test.xahauexplorer.com/account/rJeSkUUXY5D3gJzfQjg6DRx47KPdcuBpqe)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | 0 | Install the hook by HookHash (no wasm) | tesSUCCESS | PASS |
| 2 | 0 | Separate SetHook: HookOn | tesSUCCESS | PASS |
| 3 | 0 | Hook on the account: HookHash matches | HookHash 3CEEAB2F | PASS |
| 4 | 1 | PACK after 10 mints, without AUDIT | tgs: audit | PASS |
| 5 | 1 | AUDIT | tgs: audit ok/done | PASS |
| 6 | 1 | Box 1 (key link, XAH + token): PACK | tgs: packed | PASS |
| 7 | 3 | Box 2 (key link, token only): PACK | tgs: packed | PASS |
| 8 | 4 | Box 3 (key link, XAH only): PACK | tgs: packed | PASS |
| 9 | 5 | PACK again with the token already in box 2 | tgs: locked | PASS |
| 10 | 6 | PACK again with the box number of box 3 | tgs: exists | PASS |
| 11 | 7 | CLAIM box 2 from R1 with a signature made for R2 | tgs: bad sig | PASS |
| 12 | 14 | Creator tries to move a locked token with Remit | tgs: locked | PASS |
| 13 | 23 | Sell offer on a locked token | tgs: locked | PASS |
| 14 | 15a | Payment of 1 XAH from free XAH | tesSUCCESS | PASS |
| 15 | 15b | Payment of almost the whole balance (eats locked XAH) | tgs: reserve | PASS |
| 16 | 18 | PACK for more XAH than available | tgs: not enough | PASS |
| 17 | 19 | OfferCreate that sells XAH | tgs: offer | PASS |
| 18 | 20 | CheckCreate for XAH | tgs: check | PASS |
| 19 | 2 | Box 1 (key link, XAH + token): CLAIM (R1) | tgs: claiming | PASS |
| 20 | 2 | Box 1 (key link, XAH + token): hook sends the payout Remit | tesSUCCESS | PASS |
| 21 | 2 | Box 1 (key link, XAH + token): XAH arrived (balance change incl. fee) | 3200000 drops | PASS |
| 22 | 2 | Box 1 (key link, XAH + token): token now owned by R1 | R1 | PASS |
| 23 | 3 | Box 2 (key link, token only): CLAIM (R2) | tgs: claiming | PASS |
| 24 | 3 | Box 2 (key link, token only): hook sends the payout Remit | tesSUCCESS | PASS |
| 25 | 3 | Box 2 (key link, token only): XAH arrived (balance change incl. fee) | 200000 drops | PASS |
| 26 | 3 | Box 2 (key link, token only): token now owned by R2 | R2 | PASS |
| 27 | 4 | Box 3 (key link, XAH only): CLAIM (R3) | tgs: claiming | PASS |
| 28 | 4 | Box 3 (key link, XAH only): hook sends the payout Remit | tesSUCCESS | PASS |
| 29 | 4 | Box 3 (key link, XAH only): XAH arrived (balance change incl. fee) | 2000000 drops | PASS |
| 30 | 8 | Second CLAIM: Box 1 (key link, XAH + token) | tgs: unknown | PASS |
| 31 | 8 | Second CLAIM: Box 3 (key link, XAH only) | tgs: unknown | PASS |
| 32 | 9 | Box 4 (key token, token + 3 XAH): PACK | tgs: packed | PASS |
| 33 | 13 | CLAIM box 4 without the key token (R4) | tgs: not holder | PASS |
| 34 | 9 | Key token of box 4 sent to R1 | tesSUCCESS | PASS |
| 35 | 9 | Box 4 (key token, token + 3 XAH): CLAIM by the key token holder (R1) | tgs: claiming | PASS |
| 36 | 9 | Box 4 (key token, token + 3 XAH): hook sends the payout Remit | tesSUCCESS | PASS |
| 37 | 9 | Box 4 (key token, token + 3 XAH): XAH arrived (balance change incl. fee) | 3200000 drops | PASS |
| 38 | 9 | Box 4 (key token, token + 3 XAH): token now owned by R1 | R1 | PASS |
| 39 | 10 | Box 5 (key link, with time limit): PACK | tgs: packed | PASS |
| 40 | 10 | OPEN box 5 before the time limit | tgs: not expired | PASS |
| 41 | 11 | CLAIM box 5 after the time limit | tgs: expired | PASS |
| 42 | 12 | OPEN box 5 after the time limit | tgs: opened | PASS |
| 43 | 12 | Token of box 5 is back with the creator | creator | PASS |
| 44 | 21 | Second OPEN of box 5 | tgs: unknown | PASS |
| 45 | 16 | Box 6 (key token, passed hand to hand): PACK | tgs: packed | PASS |
| 46 | 16 | Key token of box 6: creator → R1 | tesSUCCESS | PASS |
| 47 | 16 | Key token of box 6: R1 → R2 | tesSUCCESS | PASS |
| 48 | 17 | Former holder R1 tries to open box 6 | tgs: not holder | PASS |
| 49 | 16 | Box 6 (key token, passed hand to hand): CLAIM by the key token holder (R2) | tgs: claiming | PASS |
| 50 | 16 | Box 6 (key token, passed hand to hand): hook sends the payout Remit | tesSUCCESS | PASS |
| 51 | 16 | Box 6 (key token, passed hand to hand): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 52 | 16 | Box 6 (key token, passed hand to hand): token now owned by R2 | R2 | PASS |
| 53 | 24 | Box 7 (key token, key sold): PACK | tgs: packed | PASS |
| 54 | 24 | Sell offer of the key token for 5 XAH to R3 | tesSUCCESS | PASS |
| 55 | 24 | R3 buys the key token (URITokenBuy) | tesSUCCESS | PASS |
| 56 | 24 | Box 7 (key token, key sold): CLAIM by the key token holder (R3) | tgs: claiming | PASS |
| 57 | 24 | Box 7 (key token, key sold): hook sends the payout Remit | tesSUCCESS | PASS |
| 58 | 24 | Box 7 (key token, key sold): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 59 | 24 | Box 7 (key token, key sold): token now owned by R3 | R3 | PASS |
| 60 | 22 | Multi 1 (key link): PACK | tgs: packed | PASS |
| 61 | 22 | Multi 2 (key token): PACK | tgs: packed | PASS |
| 62 | 22 | Multi 3 (key link): PACK | tgs: packed | PASS |
| 63 | 22 | Multi 4 (key token): PACK | tgs: packed | PASS |
| 64 | 22 | Multi 5 (key link): PACK | tgs: packed | PASS |
| 65 | 22 | Multi 6 (key token): PACK | tgs: packed | PASS |
| 66 | 22 | Multi 7 (key link): PACK | tgs: packed | PASS |
| 67 | 22 | Multi 8 (key token): PACK | tgs: packed | PASS |
| 68 | 22 | Multi 3 (key link): CLAIM (R3) | tgs: claiming | PASS |
| 69 | 22 | Multi 3 (key link): hook sends the payout Remit | tesSUCCESS | PASS |
| 70 | 22 | Multi 3 (key link): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 71 | 22 | Multi 3 (key link): token now owned by R3 | R3 | PASS |
| 72 | 22 | Multi 2 (key token): CLAIM by the key token holder (R2) | tgs: claiming | PASS |
| 73 | 22 | Multi 2 (key token): hook sends the payout Remit | tesSUCCESS | PASS |
| 74 | 22 | Multi 2 (key token): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 75 | 22 | Multi 2 (key token): token now owned by R2 | R2 | PASS |
| 76 | 22 | Multi 1 (key link): CLAIM (R1) | tgs: claiming | PASS |
| 77 | 22 | Multi 1 (key link): hook sends the payout Remit | tesSUCCESS | PASS |
| 78 | 22 | Multi 1 (key link): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 79 | 22 | Multi 1 (key link): token now owned by R1 | R1 | PASS |
| 80 | 22 | Multi 8 (key token): CLAIM by the key token holder (R2) | tgs: claiming | PASS |
| 81 | 22 | Multi 8 (key token): hook sends the payout Remit | tesSUCCESS | PASS |
| 82 | 22 | Multi 8 (key token): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 83 | 22 | Multi 8 (key token): token now owned by R2 | R2 | PASS |
| 84 | 22 | Multi 6 (key token): CLAIM by the key token holder (R3) | tgs: claiming | PASS |
| 85 | 22 | Multi 6 (key token): hook sends the payout Remit | tesSUCCESS | PASS |
| 86 | 22 | Multi 6 (key token): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 87 | 22 | Multi 6 (key token): token now owned by R3 | R3 | PASS |
| 88 | 22 | Multi 5 (key link): CLAIM (R2) | tgs: claiming | PASS |
| 89 | 22 | Multi 5 (key link): hook sends the payout Remit | tesSUCCESS | PASS |
| 90 | 22 | Multi 5 (key link): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 91 | 22 | Multi 5 (key link): token now owned by R2 | R2 | PASS |
| 92 | 22 | Multi 4 (key token): CLAIM by the key token holder (R1) | tgs: claiming | PASS |
| 93 | 22 | Multi 4 (key token): hook sends the payout Remit | tesSUCCESS | PASS |
| 94 | 22 | Multi 4 (key token): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 95 | 22 | Multi 4 (key token): token now owned by R1 | R1 | PASS |
| 96 | 22 | Multi 7 (key link): CLAIM (R1) | tgs: claiming | PASS |
| 97 | 22 | Multi 7 (key link): hook sends the payout Remit | tesSUCCESS | PASS |
| 98 | 22 | Multi 7 (key link): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 99 | 22 | Multi 7 (key link): token now owned by R1 | R1 | PASS |
| 100 | QR-A | QR-A1 bearer: PACK | tgs: packed | PASS |
| 101 | QR-A | QR-A2 bearer: PACK | tgs: packed | PASS |
| 102 | QR-B | QR-B1 with gift code: PACK | tgs: packed | PASS |
| 103 | QR-B | QR-B2 with gift code: PACK | tgs: packed | PASS |
| 104 | QR-C | QR-C1 named: PACK | tgs: packed | PASS |
| 105 | QR-C | QR-C2 named: PACK | tgs: packed | PASS |
| 106 | QR-D | QR-D1 key token: PACK | tgs: packed | PASS |
| 107 | QR-D | QR-D2 key token: PACK | tgs: packed | PASS |
| 108 | QR-X | QR-X with time limit: PACK | tgs: packed | PASS |
| 109 | QR-A | QR-A1 bearer: PNG decoded with jsQR matches | matches | PASS |
| 110 | QR-A | QR-A1 bearer: intercepted signature sent from another account R4 | tgs: bad sig | PASS |
| 111 | QR-A | QR-A1 bearer: CLAIM from the QR | tgs: claiming | PASS |
| 112 | QR-A | QR-A1 bearer: hook sends the payout Remit | tesSUCCESS | PASS |
| 113 | QR-A | QR-A1 bearer: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 114 | QR-A | QR-A1 bearer: token now owned by R1 | R1 | PASS |
| 115 | QR-A | QR-A1 bearer: scan again | tgs: unknown | PASS |
| 116 | QR-A | QR-A2 bearer: PNG decoded with jsQR matches | matches | PASS |
| 117 | QR-A | QR-A2 bearer: intercepted signature sent from another account R4 | tgs: bad sig | PASS |
| 118 | QR-A | QR-A2 bearer: CLAIM from the QR | tgs: claiming | PASS |
| 119 | QR-A | QR-A2 bearer: hook sends the payout Remit | tesSUCCESS | PASS |
| 120 | QR-A | QR-A2 bearer: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 121 | QR-A | QR-A2 bearer: token now owned by R2 | R2 | PASS |
| 122 | QR-A | QR-A2 bearer: scan again | tgs: unknown | PASS |
| 123 | QR-B | QR-B1 with gift code: PNG decoded with jsQR matches | matches | PASS |
| 124 | QR-B | QR-B1 with gift code: CLAIM from the QR | tgs: claiming | PASS |
| 125 | QR-B | QR-B1 with gift code: hook sends the payout Remit | tesSUCCESS | PASS |
| 126 | QR-B | QR-B1 with gift code: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 127 | QR-B | QR-B1 with gift code: token now owned by R1 | R1 | PASS |
| 128 | QR-B | QR-B1 with gift code: gift code from the ledger (tgsE1:Aej00i3r…) decrypted | TEST-GIFT-329A92 | PASS |
| 129 | QR-B | QR-B1 with gift code: scan again | tgs: unknown | PASS |
| 130 | QR-B | QR-B2 with gift code: PNG decoded with jsQR matches | matches | PASS |
| 131 | QR-B | QR-B2 with gift code: CLAIM from the QR | tgs: claiming | PASS |
| 132 | QR-B | QR-B2 with gift code: hook sends the payout Remit | tesSUCCESS | PASS |
| 133 | QR-B | QR-B2 with gift code: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 134 | QR-B | QR-B2 with gift code: token now owned by R3 | R3 | PASS |
| 135 | QR-B | QR-B2 with gift code: gift code from the ledger (tgsE1:AUgjOaag…) decrypted | TEST-GIFT-F122AA | PASS |
| 136 | QR-B | QR-B2 with gift code: scan again | tgs: unknown | PASS |
| 137 | QR-C | QR-C1 named: PNG decoded with jsQR matches | matches | PASS |
| 138 | QR-C | QR-C1 named: the QR holds no secret | no key field | PASS |
| 139 | QR-C | QR-C1 named: another account R4 uses the named QR | tgs: bad sig | PASS |
| 140 | QR-C | QR-C1 named: CLAIM by the named recipient | tgs: claiming | PASS |
| 141 | QR-C | QR-C1 named: hook sends the payout Remit | tesSUCCESS | PASS |
| 142 | QR-C | QR-C1 named: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 143 | QR-C | QR-C1 named: token now owned by R2 | R2 | PASS |
| 144 | QR-C | QR-C2 named: PNG decoded with jsQR matches | matches | PASS |
| 145 | QR-C | QR-C2 named: the QR holds no secret | no key field | PASS |
| 146 | QR-C | QR-C2 named: another account R4 uses the named QR | tgs: bad sig | PASS |
| 147 | QR-C | QR-C2 named: CLAIM by the named recipient | tgs: claiming | PASS |
| 148 | QR-C | QR-C2 named: hook sends the payout Remit | tesSUCCESS | PASS |
| 149 | QR-C | QR-C2 named: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 150 | QR-C | QR-C2 named: token now owned by R3 | R3 | PASS |
| 151 | QR-D | QR-D1 key token: PNG decoded with jsQR matches | matches | PASS |
| 152 | QR-D | QR-D1 key token: R4 without the key token uses the QR | tgs: not holder | PASS |
| 153 | QR-D | QR-D1 key token: CLAIM by the key token holder (R1) | tgs: claiming | PASS |
| 154 | QR-D | QR-D1 key token: hook sends the payout Remit | tesSUCCESS | PASS |
| 155 | QR-D | QR-D1 key token: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 156 | QR-D | QR-D1 key token: token now owned by R1 | R1 | PASS |
| 157 | QR-D | QR-D1 key token: repeat | tgs: unknown | PASS |
| 158 | QR-D | QR-D2 key token: PNG decoded with jsQR matches | matches | PASS |
| 159 | QR-D | QR-D2 key token: R4 without the key token uses the QR | tgs: not holder | PASS |
| 160 | QR-D | QR-D2 key token: CLAIM by the key token holder (R2) | tgs: claiming | PASS |
| 161 | QR-D | QR-D2 key token: hook sends the payout Remit | tesSUCCESS | PASS |
| 162 | QR-D | QR-D2 key token: XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 163 | QR-D | QR-D2 key token: token now owned by R2 | R2 | PASS |
| 164 | QR-D | QR-D2 key token: repeat | tgs: unknown | PASS |
| 165 | QR-X | QR-X: PNG decoded with jsQR matches | matches | PASS |
| 166 | QR-X | QR-X: CLAIM after the time limit | tgs: expired | PASS |
| 167 | QR-X | QR-X: OPEN by the creator after the time limit | tgs: opened | PASS |

## escape — 29 checks

Escrow, payment channels, offers, regular key, signer list and other ways to move locked value.

Accounts:

- creator (hook account): [rpFCaDhmkahnfPMVbRpP3x5e8bpA3zvQ29](https://test.xahauexplorer.com/account/rpFCaDhmkahnfPMVbRpP3x5e8bpA3zvQ29)
- R1: [r33ayY9tgFDvwnZUwehboNwUp6JruMgPSi](https://test.xahauexplorer.com/account/r33ayY9tgFDvwnZUwehboNwUp6JruMgPSi)
- R2: [rpzgCEzovVyKthfS4eEtik7mhERNDrjf7x](https://test.xahauexplorer.com/account/rpzgCEzovVyKthfS4eEtik7mhERNDrjf7x)
- R3: [rH4mx6s9ynXw2bmfiXpZfuoiwtJtJeuGY6](https://test.xahauexplorer.com/account/rH4mx6s9ynXw2bmfiXpZfuoiwtJtJeuGY6)
- R4 (stranger): [rtjqipu2pJMBVK3Ng6K9PcVsUcFksckHj](https://test.xahauexplorer.com/account/rtjqipu2pJMBVK3Ng6K9PcVsUcFksckHj)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | ESC | Box Z (key link, token + 100 XAH, time limit): PACK | tgs: packed | PASS |
| 2 | ESC | EscrowCreate for more than the free XAH | refused, tgs: reserve | PASS |
| 3 | ESC | EscrowCreate of 1 XAH from free XAH (with FinishAfter) | allowed (tgs: out) | PASS |
| 4 | ESC | EscrowCreate of 1 XAH (to cancel later) | allowed | PASS |
| 5 | ESC | PaymentChannelCreate for more than the free XAH | refused, tgs: reserve | PASS |
| 6 | ESC | PaymentChannelCreate of 1 XAH | allowed | PASS |
| 7 | ESC | PaymentChannelFund for more than the free XAH | refused, tgs: reserve | PASS |
| 8 | ESC | CheckCash of an incoming check (money comes in) | allowed | PASS |
| 9 | ESC | Sell offer of a locked token to R1 for 0 | refused, tgs: locked | PASS |
| 10 | ESC | URITokenBurn of a locked token | refused, tgs: locked | PASS |
| 11 | ESC | AccountDelete of the creator (all XAH would go to R1) | refused (by hook or network) | PASS |
| 12 | ESC | SetRegularKey (a second signing key) | allowed: txs signed with it still pass through the hook | PASS |
| 13 | ESC | URITokenBurn of a locked token, signed with the regular key | refused, tgs: locked | PASS |
| 14 | ESC | SetRegularKey removed | allowed | PASS |
| 15 | ESC | SignerListSet (multi-sign with R1) | allowed: multi-signed txs still pass through the hook | PASS |
| 16 | ESC | SignerListSet removed | allowed | PASS |
| 17 | ESC | TrustSet (no XAH leaves, only reserve grows) | allowed, reserve is checked | PASS |
| 18 | ESC | ClaimReward (XAH only comes in): no loss beyond the fee | loss 0 | PASS |
| 19 | ESC | Payment for more than the free XAH (control) | refused, tgs: reserve | PASS |
| 20 | ESC | EscrowFinish by R1 (only the 1 XAH in escrow leaves) | allowed | PASS |
| 21 | ESC | EscrowCancel (1 XAH back to the creator) | allowed | PASS |
| 22 | ESC | Result: token of box Z is still with the creator and locked | creator | PASS |
| 23 | ESC-after | OPEN box Z after the time limit | tgs: opened | PASS |
| 24 | ESC-after | EscrowCreate of the same large amount | allowed | PASS |
| 25 | ESC-after | Sell offer of token Z to R1 | allowed | PASS |
| 26 | ESC-after | URITokenBurn of token Z | allowed | PASS |
| 27 | ESC-after | EscrowCancel: the large amount is back | allowed | PASS |
| 28 | OWNER | Box Y (key link, token + 1 XAH): PACK | tgs: packed | PASS |
| 29 | OWNER | [limitation] Creator removes hook and state in one SetHook (Flags 3) while box Y is closed | info: the creator must be trusted | PASS |

## escape2 — 14 checks

More escape tries after OPEN; hook removal (a known limitation).

Accounts:

- creator (hook account): [rG85ptyadjUxPwXPdKKr6upozHazLsT4F9](https://test.xahauexplorer.com/account/rG85ptyadjUxPwXPdKKr6upozHazLsT4F9)
- R1: [rNseWaj2JtVGcmxGkF6kb5eAjXMF7KXgFp](https://test.xahauexplorer.com/account/rNseWaj2JtVGcmxGkF6kb5eAjXMF7KXgFp)
- R2: [rENuiPVW16tV1B3R9Vy8rFdjdZFh14wRZS](https://test.xahauexplorer.com/account/rENuiPVW16tV1B3R9Vy8rFdjdZFh14wRZS)
- R3: [rGE3Nso1VQr8fqnBPopUc4CHc2dQeYViDD](https://test.xahauexplorer.com/account/rGE3Nso1VQr8fqnBPopUc4CHc2dQeYViDD)
- R4 (stranger): [raubEzoYY8GUgfPNNUidxPyTAovQEtyoeA](https://test.xahauexplorer.com/account/raubEzoYY8GUgfPNNUidxPyTAovQEtyoeA)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | OWNER | Box Y2 (key link, token + 1 XAH): PACK | tgs: packed | PASS |
| 2 | OWNER | [limitation] Creator removes the hook while a box is closed | SetHook does not pass through the hook, so removal is possible (trust in the creator) | PASS |
| 3 | OWNER | [limitation] Remit of a closed box token after the hook is removed | tesSUCCESS: the token left (no hook, no protection) | PASS |
| 4 | CLEANUP | Hook installed again by HookHash (with HookOn) | tesSUCCESS | PASS |
| 5 | CLEANUP | State removed (hsfHook state cleared (NSDelete)): records 4 → 0 | tesSUCCESS, 0 records | PASS |
| 6 | ESC2 | Box W (key link, token + 100 XAH, time limit): PACK | tgs: packed | PASS |
| 7 | ESC2 | EscrowCreate for more than the free XAH, box closed | refused, tgs: reserve | PASS |
| 8 | ESC2 | EscrowCreate of 1 XAH from free XAH | allowed | PASS |
| 9 | ESC2 | EscrowFinish by R1: only the 1 XAH in escrow leaves | allowed | PASS |
| 10 | ESC2 | OPEN box W after the time limit | tgs: opened | PASS |
| 11 | ESC2 | The same large amount in EscrowCreate after OPEN | allowed | PASS |
| 12 | ESC2 | EscrowCancel: the large amount is back | allowed | PASS |
| 13 | ESC2 | Large PaymentChannelCreate after OPEN | allowed | PASS |
| 14 | ESC2 | Channel closed, XAH came back | tesSUCCESS | PASS |

## gaps — 26 checks

Recipient blocks Remit; unrelated tokens move freely.

Accounts:

- creator (hook account): [rECFEsvupGGxnm5sv5yRoZrmDxBQd6osfW](https://test.xahauexplorer.com/account/rECFEsvupGGxnm5sv5yRoZrmDxBQd6osfW)
- R1: [rwiMQKEC7wrGTqtkRcYt6jyNHmXDH7gbd9](https://test.xahauexplorer.com/account/rwiMQKEC7wrGTqtkRcYt6jyNHmXDH7gbd9)
- R2: [rMkc6dVpqyasiWA1N4xaGPSpifWmTAbaEt](https://test.xahauexplorer.com/account/rMkc6dVpqyasiWA1N4xaGPSpifWmTAbaEt)
- R3: [r4YajaEKfSN2sMifQqDTLakxXrDYp9zGJT](https://test.xahauexplorer.com/account/r4YajaEKfSN2sMifQqDTLakxXrDYp9zGJT)
- R4 (stranger): [rH7F8LUaHscyYEzWBf5kNHdzkkzYtjxQHm](https://test.xahauexplorer.com/account/rH7F8LUaHscyYEzWBf5kNHdzkkzYtjxQHm)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | G2 | R3 blocks incoming Remit (asfDisallowIncomingRemit) | tesSUCCESS | PASS |
| 2 | G2 | Box G2 (key link, token + 2 XAH, time limit): PACK | tgs: packed | PASS |
| 3 | G2 | CLAIM by R3 while R3 blocks Remit | tgs: claiming (the hook cannot know in advance) | PASS |
| 4 | G2 | Payout Remit to the recipient | fails (tecNO_PERMISSION or similar) | PASS |
| 5 | G2 | Box token after the failed payout | stays with the creator | PASS |
| 6 | G2 | No XAH reached R3 (balance change without fee) | 0 | PASS |
| 7 | G2 | Box XAH still locked (large Payment) | refused, tgs: reserve | PASS |
| 8 | G2 | Box token still locked (Remit) | refused, tgs: locked | PASS |
| 9 | G2 | R3 removes the block | tesSUCCESS | PASS |
| 10 | G2 | R3 CLAIMs again with the same QR and signature | tgs: claiming | PASS |
| 11 | G2 | Box G2 (key link, token + 2 XAH, time limit): hook sends the payout Remit | tesSUCCESS | PASS |
| 12 | G2 | Box G2 (key link, token + 2 XAH, time limit): XAH arrived (balance change incl. fee) | 2200000 drops | PASS |
| 13 | G2 | Box G2 (key link, token + 2 XAH, time limit): token now owned by R3 | R3 | PASS |
| 14 | G2 | Third CLAIM after the payout | tgs: unknown | PASS |
| 15 | G2 | State records: 4 before CLAIM, 4 after the failure, 2 after the payout | after the failure the box is still there | PASS |
| 16 | G3 | Box G3 (key link, token + 2 XAH, time limit): PACK | tgs: packed | PASS |
| 17 | G3 | Remit of an unrelated token to R1 | allowed | PASS |
| 18 | G3 | Sell offer of an unrelated token to R2 | allowed | PASS |
| 19 | G3 | R2 buys the unrelated token | tesSUCCESS | PASS |
| 20 | G3 | Burn of an unrelated token | allowed | PASS |
| 21 | G3 | Payment of 1 XAH from free XAH | allowed | PASS |
| 22 | G3 | Remit of 1 XAH from free XAH | allowed | PASS |
| 23 | G3 | Remit of an unrelated and a locked token in one tx | refused, tgs: locked | PASS |
| 24 | G3 | Control: Remit of the locked token | refused, tgs: locked | PASS |
| 25 | CLEANUP | OPEN box G3 after the time limit | tgs: opened | PASS |
| 26 | CLEANUP | Hook state cleared (2 → 0) | 0 records | PASS |

## gaps2 — 35 checks

CLAIM or OPEN that never reach a ledger (old LastLedgerSequence, low fee); EXP left out or zero.

Accounts:

- creator (hook account): [rnib9UVcZhk9RTswdXi8wtq4LErrZePndY](https://test.xahauexplorer.com/account/rnib9UVcZhk9RTswdXi8wtq4LErrZePndY)
- R1: [rUmKaLGz6UCfRU1aV1sQE3LBQDDRn8CpmP](https://test.xahauexplorer.com/account/rUmKaLGz6UCfRU1aV1sQE3LBQDDRn8CpmP)
- R2: [rEKCZQczoG9rr2gamc1mVQqKbe9NBw7nU8](https://test.xahauexplorer.com/account/rEKCZQczoG9rr2gamc1mVQqKbe9NBw7nU8)
- R3: [rMLmmop2i3iwdimP5DeHDaSnGm47tnGEZW](https://test.xahauexplorer.com/account/rMLmmop2i3iwdimP5DeHDaSnGm47tnGEZW)
- R4 (stranger): [rsXgyBRzQQcm294TDWUVExQ1BwqRu6t4D1](https://test.xahauexplorer.com/account/rsXgyBRzQQcm294TDWUVExQ1BwqRu6t4D1)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | N2 | Box N2 (key link, 1 XAH, time limit): PACK | tgs: packed | PASS |
| 2 | N2 | CLAIM with LastLedgerSequence in the past | tefMAX_LEDGER, not in a ledger | PASS |
| 3 | N2 | CLAIM with a fee of 1 drop | telINSUF_FEE_P, not in a ledger | PASS |
| 4 | N2 | Box record did not change | byte for byte as after PACK | PASS |
| 5 | N2 | The same CLAIM with normal LastLedgerSequence and fee | tgs: claiming | PASS |
| 6 | N2 | Box N2 (key link, 1 XAH, time limit): hook sends the payout Remit | tesSUCCESS | PASS |
| 7 | N2 | Box N2 (key link, 1 XAH, time limit): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 8 | N1 | Box N1 (key link, 1 XAH, time limit): PACK | tgs: packed | PASS |
| 9 | N1 | OPEN before the time limit | tgs: not expired | PASS |
| 10 | N1 | OPEN after the time limit, LastLedgerSequence in the past | tefMAX_LEDGER, not in a ledger | PASS |
| 11 | N1 | OPEN with a fee of 1 drop | tel*, not in a ledger | PASS |
| 12 | N1 | OPEN by a non-owner (R1) after the time limit | tgs: owner | PASS |
| 13 | N1 | CLAIM after the time limit | tgs: expired | PASS |
| 14 | N1 | Box still closed after the failures | record unchanged | PASS |
| 15 | N1 | OPEN again by the owner | tgs: opened | PASS |
| 16 | N1 | OPEN emits nothing (assets are already on the account) | 0 payouts | PASS |
| 17 | N1 | Box record removed | gone | PASS |
| 18 | N1 | Third OPEN | tgs: unknown | PASS |
| 19 | N3 | PACK with EXP=00000000 | no fixed answer; the stored EXP is checked next (got tgs: packed) | PASS |
| 20 | N3 | EXP bytes stored in the box record | 00000000 | PASS |
| 21 | N3 | OPEN by the owner right away (EXP=00000000 means no limit) | tgs: not expired | PASS |
| 22 | N3 | PACK no. 2 with EXP=00000000 | tgs: packed | PASS |
| 23 | N3 | CLAIM by R2 (signature with exp=0) | tgs: claiming | PASS |
| 24 | N3 | Box N3b (EXP=00000000): hook sends the payout Remit | tesSUCCESS | PASS |
| 25 | N3 | Box N3b (EXP=00000000): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 26 | N3 | Box N3c (no EXP given): PACK | tgs: packed | PASS |
| 27 | N3 | EXP bytes stored in the box record, no EXP given | 00000000 | PASS |
| 28 | N3 | OPEN by the owner right away (no EXP given) | tgs: not expired | PASS |
| 29 | N3-cleanup | Box N3a (EXP=00000000): CLAIM (R1) | tgs: claiming | PASS |
| 30 | N3-cleanup | Box N3a (EXP=00000000): hook sends the payout Remit | tesSUCCESS | PASS |
| 31 | N3-cleanup | Box N3a (EXP=00000000): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 32 | N3-cleanup | Box N3c (no EXP given): CLAIM (R1) | tgs: claiming | PASS |
| 33 | N3-cleanup | Box N3c (no EXP given): hook sends the payout Remit | tesSUCCESS | PASS |
| 34 | N3-cleanup | Box N3c (no EXP given): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 35 | CLEANUP | Hook state cleared (2 → 0) | 0 | PASS |

## race — 85 checks

3 rounds × 3 boxes, 4 CLAIMs on one box in one ledger (9 races).

Accounts:

- creator (hook account): [rsYxhQHVgZ2RrV79emg4gVM6zdo5Wcg8AK](https://test.xahauexplorer.com/account/rsYxhQHVgZ2RrV79emg4gVM6zdo5Wcg8AK)
- R1: [rELPp5qMroa2cjuykSocgB5UQEgD1dPZV4](https://test.xahauexplorer.com/account/rELPp5qMroa2cjuykSocgB5UQEgD1dPZV4)
- R2: [rEnhHWwAjxHWYZsx3c28sfBRkgfHR5Scud](https://test.xahauexplorer.com/account/rEnhHWwAjxHWYZsx3c28sfBRkgfHR5Scud)
- R3: [rGoJRACbfvDBdsLsEiLopcfJwycu9i8T9p](https://test.xahauexplorer.com/account/rGoJRACbfvDBdsLsEiLopcfJwycu9i8T9p)
- R4 (stranger): [rLF9iGGfoDxS6715aAgwZHEcEFUAD63mwi](https://test.xahauexplorer.com/account/rLF9iGGfoDxS6715aAgwZHEcEFUAD63mwi)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | RACE | Round 1, box A (bearer): PACK | tgs: packed | PASS |
| 2 | RACE | Round 1, box C (named for R1): PACK | tgs: packed | PASS |
| 3 | RACE | Round 1, box D (key token, held by R2): PACK | tgs: packed | PASS |
| 4 | RACE-A | Round 1, box A: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 5 | RACE-A | Round 1, box A: exactly one winner, and it is a rightful one | 1, one of R1/R3 | PASS |
| 6 | RACE-A | Round 1, box A: payout Remit | tesSUCCESS | PASS |
| 7 | RACE-A | Round 1, box A: number of payouts | 1 | PASS |
| 8 | RACE-A | Round 1, box A: winner R3 got the amount plus reserve | 2200000 | PASS |
| 9 | RACE-A | Round 1, box A: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 10 | RACE-A | Round 1, box A: the token is with the winner | R3 | PASS |
| 11 | RACE-A | Round 1, box A: loser R1 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 12 | RACE-C | Round 1, box C: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 13 | RACE-C | Round 1, box C: exactly one winner, and it is a rightful one | 1, one of R1 | PASS |
| 14 | RACE-C | Round 1, box C: payout Remit | tesSUCCESS | PASS |
| 15 | RACE-C | Round 1, box C: number of payouts | 1 | PASS |
| 16 | RACE-C | Round 1, box C: winner R1 got the amount plus reserve | 2200000 | PASS |
| 17 | RACE-C | Round 1, box C: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 18 | RACE-C | Round 1, box C: the token is with the winner | R1 | PASS |
| 19 | RACE-C | Round 1, box C: loser R2 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 20 | RACE-D | Round 1, box D: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 21 | RACE-D | Round 1, box D: exactly one winner, and it is a rightful one | 1, one of R2 | PASS |
| 22 | RACE-D | Round 1, box D: payout Remit | tesSUCCESS | PASS |
| 23 | RACE-D | Round 1, box D: number of payouts | 1 | PASS |
| 24 | RACE-D | Round 1, box D: winner R2 got the amount plus reserve | 2200000 | PASS |
| 25 | RACE-D | Round 1, box D: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 26 | RACE-D | Round 1, box D: the token is with the winner | R2 | PASS |
| 27 | RACE-D | Round 1, box D: loser R1 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 28 | RACE | Round 1: after the round 0 box records and 0 token locks remain (4 internal records) | 0 and 0 | PASS |
| 29 | RACE | Round 2, box A (bearer): PACK | tgs: packed | PASS |
| 30 | RACE | Round 2, box C (named for R1): PACK | tgs: packed | PASS |
| 31 | RACE | Round 2, box D (key token, held by R2): PACK | tgs: packed | PASS |
| 32 | RACE-A | Round 2, box A: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 33 | RACE-A | Round 2, box A: exactly one winner, and it is a rightful one | 1, one of R1/R3 | PASS |
| 34 | RACE-A | Round 2, box A: payout Remit | tesSUCCESS | PASS |
| 35 | RACE-A | Round 2, box A: number of payouts | 1 | PASS |
| 36 | RACE-A | Round 2, box A: winner R3 got the amount plus reserve | 2200000 | PASS |
| 37 | RACE-A | Round 2, box A: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 38 | RACE-A | Round 2, box A: the token is with the winner | R3 | PASS |
| 39 | RACE-A | Round 2, box A: loser R1 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 40 | RACE-C | Round 2, box C: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 41 | RACE-C | Round 2, box C: exactly one winner, and it is a rightful one | 1, one of R1 | PASS |
| 42 | RACE-C | Round 2, box C: payout Remit | tesSUCCESS | PASS |
| 43 | RACE-C | Round 2, box C: number of payouts | 1 | PASS |
| 44 | RACE-C | Round 2, box C: winner R1 got the amount plus reserve | 2200000 | PASS |
| 45 | RACE-C | Round 2, box C: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 46 | RACE-C | Round 2, box C: the token is with the winner | R1 | PASS |
| 47 | RACE-C | Round 2, box C: loser R2 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 48 | RACE-D | Round 2, box D: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 49 | RACE-D | Round 2, box D: exactly one winner, and it is a rightful one | 1, one of R2 | PASS |
| 50 | RACE-D | Round 2, box D: payout Remit | tesSUCCESS | PASS |
| 51 | RACE-D | Round 2, box D: number of payouts | 1 | PASS |
| 52 | RACE-D | Round 2, box D: winner R2 got the amount plus reserve | 2200000 | PASS |
| 53 | RACE-D | Round 2, box D: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 54 | RACE-D | Round 2, box D: the token is with the winner | R2 | PASS |
| 55 | RACE-D | Round 2, box D: loser R1 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 56 | RACE | Round 2: after the round 0 box records and 0 token locks remain (4 internal records) | 0 and 0 | PASS |
| 57 | RACE | Round 3, box A (bearer): PACK | tgs: packed | PASS |
| 58 | RACE | Round 3, box C (named for R1): PACK | tgs: packed | PASS |
| 59 | RACE | Round 3, box D (key token, held by R2): PACK | tgs: packed | PASS |
| 60 | RACE-A | Round 3, box A: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 61 | RACE-A | Round 3, box A: exactly one winner, and it is a rightful one | 1, one of R1/R3 | PASS |
| 62 | RACE-A | Round 3, box A: payout Remit | tesSUCCESS | PASS |
| 63 | RACE-A | Round 3, box A: number of payouts | 1 | PASS |
| 64 | RACE-A | Round 3, box A: winner R1 got the amount plus reserve | 2200000 | PASS |
| 65 | RACE-A | Round 3, box A: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 66 | RACE-A | Round 3, box A: the token is with the winner | R1 | PASS |
| 67 | RACE-A | Round 3, box A: loser R3 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 68 | RACE-C | Round 3, box C: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 69 | RACE-C | Round 3, box C: exactly one winner, and it is a rightful one | 1, one of R1 | PASS |
| 70 | RACE-C | Round 3, box C: payout Remit | tesSUCCESS | PASS |
| 71 | RACE-C | Round 3, box C: number of payouts | 1 | PASS |
| 72 | RACE-C | Round 3, box C: winner R1 got the amount plus reserve | 2200000 | PASS |
| 73 | RACE-C | Round 3, box C: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 74 | RACE-C | Round 3, box C: the token is with the winner | R1 | PASS |
| 75 | RACE-C | Round 3, box C: loser R2 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 76 | RACE-D | Round 3, box D: all 4 CLAIMs in one ledger | 1 ledger | PASS |
| 77 | RACE-D | Round 3, box D: exactly one winner, and it is a rightful one | 1, one of R2 | PASS |
| 78 | RACE-D | Round 3, box D: payout Remit | tesSUCCESS | PASS |
| 79 | RACE-D | Round 3, box D: number of payouts | 1 | PASS |
| 80 | RACE-D | Round 3, box D: winner R2 got the amount plus reserve | 2200000 | PASS |
| 81 | RACE-D | Round 3, box D: only one account got XAH (balance change without fees) | 1 account with a balance gain | PASS |
| 82 | RACE-D | Round 3, box D: the token is with the winner | R2 | PASS |
| 83 | RACE-D | Round 3, box D: loser R1 tries again afterwards | refused (unknown / bad sig / not holder) | PASS |
| 84 | RACE | Round 3: after the round 0 box records and 0 token locks remain (4 internal records) | 0 and 0 | PASS |
| 85 | CLEANUP | Hook state cleared (4 → 0) | 0 | PASS |

## multisign — 13 checks

Multi-signed transactions do not bypass the hook.

Accounts:

- creator (hook account): [rKkaeDUsV4mgwAfdgeoEEphybaNpGdj3Kq](https://test.xahauexplorer.com/account/rKkaeDUsV4mgwAfdgeoEEphybaNpGdj3Kq)
- R1: [rPDeB4LZwYLM6g167CLrtTkGE8daHMWHNh](https://test.xahauexplorer.com/account/rPDeB4LZwYLM6g167CLrtTkGE8daHMWHNh)
- R2: [rKSBKorWWh7Pj8kHKY2i8g4PfD3BivyErK](https://test.xahauexplorer.com/account/rKSBKorWWh7Pj8kHKY2i8g4PfD3BivyErK)
- R3: [rUajrA7gKYK36zkstYSLe9vrc25JTkg5m7](https://test.xahauexplorer.com/account/rUajrA7gKYK36zkstYSLe9vrc25JTkg5m7)
- R4 (stranger): [rUhv18XkC523smuqgWwi6tu2W5zUGceV4Y](https://test.xahauexplorer.com/account/rUhv18XkC523smuqgWwi6tu2W5zUGceV4Y)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | MULTI | SignerListSet R1 + R2, quorum 2 | tesSUCCESS | PASS |
| 2 | MULTI | Box M (key link, token + 5 XAH, time limit): PACK | tgs: packed | PASS |
| 3 | MULTI | Control: a multi-signed AccountSet passes | tesSUCCESS (hook was called) | PASS |
| 4 | MULTI | Multi-signed: Remit of a locked token to R1 | refused, tgs: locked | PASS |
| 5 | MULTI | Multi-signed: URITokenBurn of a locked token | refused, tgs: locked | PASS |
| 6 | MULTI | Multi-signed: sell offer of a locked token to R1 for 0 | refused, tgs: locked | PASS |
| 7 | MULTI | Multi-signed: Payment of almost the whole balance | refused, tgs: reserve | PASS |
| 8 | MULTI | Multi-signed: EscrowCreate of almost the whole balance | refused, tgs: reserve | PASS |
| 9 | MULTI | Control: one signature with quorum 2 | refused by the network (tefBAD_QUORUM) | PASS |
| 10 | MULTI | OPEN box M after the time limit | tgs: opened | PASS |
| 11 | MULTI | After OPEN: multi-signed URITokenBurn of token M | allowed | PASS |
| 12 | CLEANUP | Signer list removed | tesSUCCESS | PASS |
| 13 | CLEANUP | Hook state cleared (4 → 0), hook still installed | tesSUCCESS, 0 records | PASS |

## new_tests — 93 checks

New v14 rules: offers, burnable tokens, key token checks, box numbers, NONCE.

Accounts:

- creator (hook account): [rayptvGyuatJckMB5L98AzKNugCAruyKYH](https://test.xahauexplorer.com/account/rayptvGyuatJckMB5L98AzKNugCAruyKYH)
- R1: [rf5HE2oj7Ua8GjR9cgV1HY8mNLUiVvZTKe](https://test.xahauexplorer.com/account/rf5HE2oj7Ua8GjR9cgV1HY8mNLUiVvZTKe)
- R2: [rsv5zy2UrBy5hyBGh9P6abZirvjKr6Tegx](https://test.xahauexplorer.com/account/rsv5zy2UrBy5hyBGh9P6abZirvjKr6Tegx)
- R3: [rfgiNQDpkAVvmdoQgXknVUszB7NwGyqGJE](https://test.xahauexplorer.com/account/rfgiNQDpkAVvmdoQgXknVUszB7NwGyqGJE)
- R4 (stranger): [rntqVwJU92RWSuw4VC6fB7oiukonuKdGop](https://test.xahauexplorer.com/account/rntqVwJU92RWSuw4VC6fB7oiukonuKdGop)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | 0 | Hook v14 is on the account | tesSUCCESS | PASS |
| 2 | N1 | PACK a token with an open offer (no Destination) | tgs: token offer | PASS |
| 3 | N1 | PACK a token with an offer to a second account | tgs: token offer | PASS |
| 4 | N1 | Offer cancelled | tesSUCCESS | PASS |
| 5 | N1 | N1c: PACK | tgs: packed | PASS |
| 6 | N1 | URITokenBuy of a locked token (the offer is gone) | not tesSUCCESS | PASS |
| 7 | N1 | New offer on a locked token | tgs: locked | PASS |
| 8 | N1 | N1c: CLAIM (R1) | tgs: claiming | PASS |
| 9 | N1 | N1c: hook sends the payout Remit | tesSUCCESS | PASS |
| 10 | N1 | N1c: XAH arrived (balance change incl. fee) | 1200000 drops | PASS |
| 11 | N1 | N1c: token now owned by R1 | R1 | PASS |
| 12 | N1 | Buying a token that is NOT locked (hook lets it pass) | tesSUCCESS | PASS |
| 13 | N2 | N2a: PACK | tgs: packed | PASS |
| 14 | N2 | OPEN a box with no time limit | tgs: not expired | PASS |
| 15 | N2 | PACK with EXP=00000000 | tgs: packed | PASS |
| 16 | N2 | OPEN a box with EXP=00000000 | tgs: not expired | PASS |
| 17 | N2 | PACK with EXP=0000 (2 bytes) | tgs: exp | PASS |
| 18 | N2 | N2c: PACK | tgs: packed | PASS |
| 19 | N2 | OPEN before the time limit | tgs: not expired | PASS |
| 20 | N2 | OPEN after the time limit | tgs: opened | PASS |
| 21 | N2 | N2a: CLAIM (R1) | tgs: claiming | PASS |
| 22 | N2 | N2a: hook sends the payout Remit | tesSUCCESS | PASS |
| 23 | N2 | N2a: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 24 | N2 | N2b: CLAIM (R2) | tgs: claiming | PASS |
| 25 | N2 | N2b: hook sends the payout Remit | tesSUCCESS | PASS |
| 26 | N2 | N2b: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 27 | N3 | PACK a key-token box with box number = token ID | tgs: packed | PASS |
| 28 | N3 | N3b: PACK | tgs: packed | PASS |
| 29 | N3 | N3a: CLAIM by the key token holder (R1) | tgs: claiming | PASS |
| 30 | N3 | N3a: hook sends the payout Remit | tesSUCCESS | PASS |
| 31 | N3 | N3a: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 32 | N3 | N3b: CLAIM (R2) | tgs: claiming | PASS |
| 33 | N3 | N3b: hook sends the payout Remit | tesSUCCESS | PASS |
| 34 | N3 | N3b: XAH arrived (balance change incl. fee) | 1200000 drops | PASS |
| 35 | N3 | N3b: token now owned by R2 | R2 | PASS |
| 36 | N3 | PACK with box number 55…55 | tgs: packed | PASS |
| 37 | N3 | Owner XAH offer after PACK with box number 55… | tesSUCCESS | PASS |
| 38 | N3 | N3-55: CLAIM by the key token holder (R3) | tgs: claiming | PASS |
| 39 | N3 | N3-55: hook sends the payout Remit | tesSUCCESS | PASS |
| 40 | N3 | N3-55: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 41 | N3 | PACK with box number 66…66 | tgs: packed | PASS |
| 42 | N3 | N3-66: CLAIM by the key token holder (R3) | tgs: claiming | PASS |
| 43 | N3 | N3-66: hook sends the payout Remit | tesSUCCESS | PASS |
| 44 | N3 | N3-66: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 45 | N3 | PACK with box number 11…11 | tgs: packed | PASS |
| 46 | N3 | N3-11: CLAIM by the key token holder (R3) | tgs: claiming | PASS |
| 47 | N3 | N3-11: hook sends the payout Remit | tesSUCCESS | PASS |
| 48 | N3 | N3-11: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 49 | N3 | PACK with box number 33…33 | tgs: packed | PASS |
| 50 | N3 | N3-33: CLAIM by the key token holder (R3) | tgs: claiming | PASS |
| 51 | N3 | N3-33: hook sends the payout Remit | tesSUCCESS | PASS |
| 52 | N3 | N3-33: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 53 | N3 | PACK with box number 44…44 | tgs: packed | PASS |
| 54 | N3 | N3-44: CLAIM by the key token holder (R3) | tgs: claiming | PASS |
| 55 | N3 | N3-44: hook sends the payout Remit | tesSUCCESS | PASS |
| 56 | N3 | N3-44: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 57 | N4 | PACK a burnable token (issued by R1) | tgs: burnable | PASS |
| 58 | N4 | PACK own burnable token | tgs: burnable | PASS |
| 59 | N4 | PACK in key-token mode with a burnable key token | tgs: burnable | PASS |
| 60 | N4 | N4d: PACK | tgs: packed | PASS |
| 61 | N4 | Creator tries to burn a non-burnable token in the box | not tesSUCCESS | PASS |
| 62 | N4 | N4d: CLAIM (R2) | tgs: claiming | PASS |
| 63 | N4 | N4d: hook sends the payout Remit | tesSUCCESS | PASS |
| 64 | N4 | N4d: XAH arrived (balance change incl. fee) | 200000 drops | PASS |
| 65 | N4 | N4d: token now owned by R2 | R2 | PASS |
| 66 | N5 | Key token ID = an AccountRoot | tgs: ktid | PASS |
| 67 | N5 | Key token ID that does not exist | tgs: ktid | PASS |
| 68 | N5 | N5c: PACK | tgs: packed | PASS |
| 69 | N5 | OPEN a key-token box with no time limit, key token exists | tgs: not expired | PASS |
| 70 | N5 | Holder burned the key token | tesSUCCESS | PASS |
| 71 | N5 | CLAIM after the key token is burned | tgs: not holder | PASS |
| 72 | N5 | OPEN by the owner, the key token is gone | tgs: opened | PASS |
| 73 | N6 | N6a: PACK | tgs: packed | PASS |
| 74 | N6 | Pack counter (NONCE) in the box record | not zero | PASS |
| 75 | N6 | CLAIM box 1 | tgs: claiming | PASS |
| 76 | N6 | N6b: PACK | tgs: packed | PASS |
| 77 | N6 | Counter grew on repack | greater than before | PASS |
| 78 | N6 | CLAIM the new box with the OLD signature | tgs: bad sig | PASS |
| 79 | N6 | Signature with a wrong AMT | tgs: bad sig | PASS |
| 80 | N6 | CLAIM with a signature from the signing tool | tgs: claiming | PASS |
| 81 | N6 | N6b: hook sends the payout Remit | tesSUCCESS | PASS |
| 82 | N6 | N6b: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 83 | N6 | N6c: PACK | tgs: packed | PASS |
| 84 | N6 | R2 presents a signature made for R1 | tgs: bad sig | PASS |
| 85 | N6 | N6c: CLAIM (R1) | tgs: claiming | PASS |
| 86 | N6 | N6c: hook sends the payout Remit | tesSUCCESS | PASS |
| 87 | N6 | N6c: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 88 | N7 | N7: PACK | tgs: packed | PASS |
| 89 | N7 | The hook cannot undo a fee charge (known network limitation) | 3 XAH charged | PASS |
| 90 | N7 | N7: CLAIM (R1) | tgs: claiming | PASS |
| 91 | N7 | N7: hook sends the payout Remit | tesSUCCESS | PASS |
| 92 | N7 | N7: XAH arrived (balance change incl. fee) | 2000000 drops | PASS |
| 93 | CLEANUP | Hook state cleared (NSDelete) | tesSUCCESS | PASS |

## audit — 99 checks

A1–A14: wrong CLAIMs next to right ones, reused keys, internal records, fee burn, RequireDest, AccountDelete.

Accounts:

- creator (hook account): [r4x4diyMetgwaTqXWWWNBBvLw99C9AuQdz](https://test.xahauexplorer.com/account/r4x4diyMetgwaTqXWWWNBBvLw99C9AuQdz)
- R1: [rpfCjF9yLonbBJmzscAwP5M3LMrDxVg2m2](https://test.xahauexplorer.com/account/rpfCjF9yLonbBJmzscAwP5M3LMrDxVg2m2)
- R2: [rswFWMbVNkcL1YcyahfMHbNrtiKqat4261](https://test.xahauexplorer.com/account/rswFWMbVNkcL1YcyahfMHbNrtiKqat4261)
- R3: [rP83yENwMjHZ9MfWem1Zj41dwzmdRSE8fB](https://test.xahauexplorer.com/account/rP83yENwMjHZ9MfWem1Zj41dwzmdRSE8fB)
- R4 (stranger): [rGq9cTiyEnvJaehPA2mubNGQTL71x3RDx3](https://test.xahauexplorer.com/account/rGq9cTiyEnvJaehPA2mubNGQTL71x3RDx3)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | 0 | Install the hook on H2 by HookHash | tesSUCCESS | PASS |
| 2 | A1 | A1-0: PACK | tgs: packed | PASS |
| 3 | A1 | Round 0: wrong CLAIMs 13260241#6,13260241#5, right CLAIM 13260241#4 (same ledger) | wrong ones: bad sig (or busy if after the right one); right one: claiming | PASS |
| 4 | A1 | Round 0: payout | tesSUCCESS | PASS |
| 5 | A1 | A1-1: PACK | tgs: packed | PASS |
| 6 | A1 | Round 1: wrong CLAIMs 13260246#7,13260246#5, right CLAIM 13260246#4 (same ledger) | wrong ones: bad sig (or busy if after the right one); right one: claiming | PASS |
| 7 | A1 | Round 1: payout | tesSUCCESS | PASS |
| 8 | A1 | A1-2: PACK | tgs: packed | PASS |
| 9 | A1 | Round 2: wrong CLAIMs 13260251#5,13260251#6, right CLAIM 13260251#4 (same ledger) | wrong ones: bad sig (or busy if after the right one); right one: claiming | PASS |
| 10 | A1 | Round 2: payout | tesSUCCESS | PASS |
| 11 | A1 | A1-3: PACK | tgs: packed | PASS |
| 12 | A1 | Round 3: wrong CLAIMs 13260256#8,13260256#6, right CLAIM 13260256#5 (same ledger) | wrong ones: bad sig (or busy if after the right one); right one: claiming | PASS |
| 13 | A1 | Round 3: payout | tesSUCCESS | PASS |
| 14 | A1 | A1-4: PACK | tgs: packed | PASS |
| 15 | A1 | Round 4: wrong CLAIMs 13260261#7,13260261#6, right CLAIM 13260261#5 (same ledger) | wrong ones: bad sig (or busy if after the right one); right one: claiming | PASS |
| 16 | A1 | Round 4: payout | tesSUCCESS | PASS |
| 17 | A1 | A1-5: PACK | tgs: packed | PASS |
| 18 | A1 | Round 5: wrong CLAIMs 13260266#8,13260266#6, right CLAIM 13260266#4 (same ledger) | wrong ones: bad sig (or busy if after the right one); right one: claiming | PASS |
| 19 | A1 | Round 5: payout | tesSUCCESS | PASS |
| 20 | A2 | A2a: PACK | tgs: packed | PASS |
| 21 | A2 | CLAIM box 1 with a signature for R1 | tgs: claiming | PASS |
| 22 | A2 | A2b (same key, 2 XAH): PACK | tgs: packed | PASS |
| 23 | A2 | Old R1 signature tries to open a NEW box with the same key | tgs: bad sig | PASS |
| 24 | A3 | A3: PACK | tgs: packed | PASS |
| 25 | A3 | OPEN before the time limit | tgs: not expired | PASS |
| 26 | A3 | [by design] The owner knows the key and takes the box from a 2nd account before the limit | possible: the owner makes the key | PASS |
| 27 | A4 | A4a (box number = ID of token X, key-token mode): PACK | tgs: packed | PASS |
| 28 | A4 | PACK a second box whose token ID equals the first box number (box records and token locks do not collide) | tgs: packed | PASS |
| 29 | A4 | Box record A4a is intact (status 00) | intact, status 00 | PASS |
| 30 | A4 | Lock on token X is set | set | PASS |
| 31 | A4 | A4a (box number = ID of token X, key-token mode): CLAIM by the key token holder (R1) | tgs: claiming | PASS |
| 32 | A4 | A4a (box number = ID of token X, key-token mode): hook sends the payout Remit | tesSUCCESS | PASS |
| 33 | A4 | A4a (box number = ID of token X, key-token mode): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 34 | A4 | A4b (token ID = X): CLAIM (R2) | tgs: claiming | PASS |
| 35 | A4 | A4b (token ID = X): hook sends the payout Remit | tesSUCCESS | PASS |
| 36 | A4 | A4b (token ID = X): XAH arrived (balance change incl. fee) | 200000 drops | PASS |
| 37 | A4 | A4b (token ID = X): token now owned by R2 | R2 | PASS |
| 38 | A4 | Locked XAH total is back to the value before A4 | 2100000 | PASS |
| 39 | A5 | PACK with box number = internal record name TRK (the internal record is not touched) | tgs: packed | PASS |
| 40 | A5 | Internal record TRK after PACK | same length as before | PASS |
| 41 | A5 | PACK with box number = internal record name OCK (the internal record is not touched) | tgs: packed | PASS |
| 42 | A5 | Internal record OCK after PACK | same length as before | PASS |
| 43 | A5 | PACK with box number = internal record name AUD (the internal record is not touched) | tgs: packed | PASS |
| 44 | A5 | Internal record AUD after PACK | same length as before | PASS |
| 45 | A5 | PACK with box number = internal record name META (the internal record is not touched) | tgs: packed | PASS |
| 46 | A5 | Internal record META after PACK | same length as before | PASS |
| 47 | A5 | PACK with box number = internal record name NCK (the internal record is not touched) | tgs: packed | PASS |
| 48 | A5 | Internal record NCK after PACK | same length as before | PASS |
| 49 | A5 | OfferCreate of 1 XAH (updates an internal record) | tesSUCCESS | PASS |
| 50 | A5 | Internal record after the offer is fine, box with that number intact | both intact | PASS |
| 51 | A5 | AUDIT (updates internal records) | tgs: audited | PASS |
| 52 | A5 | All 5 boxes with internal-name numbers intact after AUDIT | 5 | PASS |
| 53 | A5 | OPEN box A5 (number TRK) after the time limit | tgs: opened | PASS |
| 54 | A5 | OPEN box A5 (number OCK) after the time limit | tgs: opened | PASS |
| 55 | A5 | OPEN box A5 (number AUD) after the time limit | tgs: opened | PASS |
| 56 | A5 | OPEN box A5 (number META) after the time limit | tgs: opened | PASS |
| 57 | A5 | OPEN box A5 (number NCK) after the time limit | tgs: opened | PASS |
| 58 | A6 | A6 with no time limit: PACK | tgs: packed | PASS |
| 59 | A6 | Holder burned the key token | tesSUCCESS | PASS |
| 60 | A6 | CLAIM after the key token is burned | tgs: not holder | PASS |
| 61 | A6 | OPEN right away (no time limit) | tgs: opened (key token burned, so the owner may open) | PASS |
| 62 | A6 | A6 with time limit: PACK | tgs: packed | PASS |
| 63 | A6 | Holder burned the key token | tesSUCCESS | PASS |
| 64 | A6 | CLAIM after the key token is burned | tgs: not holder | PASS |
| 65 | A6 | OPEN right away (time limit set) | tgs: not expired | PASS |
| 66 | A6 | OPEN after the time limit | tgs: opened | PASS |
| 67 | A7 | PACK with key token ID = an AccountRoot ID (not a token) | tgs: ktid | PASS |
| 68 | A8 | A8, key token kept by the owner: PACK | tgs: packed | PASS |
| 69 | A8 | [by design] The owner sent the key token to a 2nd own account and took the box before the limit | possible: the key token is the owner's until given away | PASS |
| 70 | A9 | Sell offer before PACK (Destination R2) | tesSUCCESS | PASS |
| 71 | A9 | PACK a token with an open offer | tgs: token offer | PASS |
| 72 | A9 | No box was made and the token is not locked | no / no | PASS |
| 73 | A9 | Sell offer before PACK (no Destination) | tesSUCCESS | PASS |
| 74 | A9 | PACK a token with an open offer | tgs: token offer | PASS |
| 75 | A9 | No box was made and the token is not locked | no / no | PASS |
| 76 | A10 | PACK 2921124818 drops (everything except ~60 XAH) | tgs: packed | PASS |
| 77 | A10 | AccountSet with Fee=64695701 (balance 2983020519, locked 2923324818): hook refuses | tecHOOK_REJECTED / tgs: reserve | PASS |
| 78 | A10 | [network limitation] The fee is charged even when the hook refuses, so the balance falls below the locked amount | how the network works: a hook cannot refund a fee | PASS |
| 79 | A10 | CLAIM when the account has less XAH than the box: hook does not pay a part | tgs: reserve | PASS |
| 80 | A10 | CLAIM after a top-up | claiming | PASS |
| 81 | A11 | A11-0: PACK | tgs: packed | PASS |
| 82 | A11 | exp=13260595: OPEN in ledger 13260596, CLAIM in ledger 13260596 | in a ledger ≤ exp: OPEN not expired, CLAIM claiming/busy | PASS |
| 83 | A11 | A11-1: PACK | tgs: packed | PASS |
| 84 | A11 | exp=13260605: OPEN in ledger 13260606, CLAIM in ledger 13260606 | in a ledger ≤ exp: OPEN not expired, CLAIM claiming/busy | PASS |
| 85 | A11 | A11-2: PACK | tgs: packed | PASS |
| 86 | A11 | exp=13260615: OPEN in ledger 13260616, CLAIM in ledger 13260616 | in a ledger ≤ exp: OPEN not expired, CLAIM claiming/busy | PASS |
| 87 | A12 | R4 turned on RequireDest | tesSUCCESS | PASS |
| 88 | A12 | A12: PACK | tgs: packed | PASS |
| 89 | A12 | [limitation] The owner pays payout fees when the recipient blocks the payout | fees are charged (only the holder of the right signature can grief) | PASS |
| 90 | A12 | Box after the blocked payouts | still closed | PASS |
| 91 | A12 | CLAIM by R1 after the blocked ones (not stuck in busy) | claiming | PASS |
| 92 | A12 | Payout to R1 | tesSUCCESS | PASS |
| 93 | A13 | PACK a burnable token from another issuer (R1 could burn it inside the box) | tgs: burnable | PASS |
| 94 | A14 | A14: PACK | tgs: packed | PASS |
| 95 | A14 | AccountDelete while a box is closed | refused | PASS |
| 96 | A14 | A14: CLAIM (R1) | tgs: claiming | PASS |
| 97 | A14 | A14: hook sends the payout Remit | tesSUCCESS | PASS |
| 98 | A14 | A14: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 99 | CLEANUP | Hook state cleared (6 → 0) | 0 | PASS |

## auditb — 11 checks

A1b: a wrong CLAIM before a right one in the same ledger.

Accounts:

- creator (hook account): [rLTSiscxKPSy22k6ZFuqhhxkBfA3r4F27z](https://test.xahauexplorer.com/account/rLTSiscxKPSy22k6ZFuqhhxkBfA3r4F27z)
- R1: [rLBJM3bAZ4NZmQvjhQNSjY5tSKDe8iUCEA](https://test.xahauexplorer.com/account/rLBJM3bAZ4NZmQvjhQNSjY5tSKDe8iUCEA)
- R2: [raho6ZQ2ALt3ycJP17xu81mzdzVnh5WtFp](https://test.xahauexplorer.com/account/raho6ZQ2ALt3ycJP17xu81mzdzVnh5WtFp)
- R3: [r9jNRouxsk29JS8rCcPg5c5Zekn8bdeLuM](https://test.xahauexplorer.com/account/r9jNRouxsk29JS8rCcPg5c5Zekn8bdeLuM)
- R4 (stranger): [rPKrgouKpL2XiEEasGe5X4JQb9SHtNC1g5](https://test.xahauexplorer.com/account/rPKrgouKpL2XiEEasGe5X4JQb9SHtNC1g5)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | A1b | A1b-0: PACK | tgs: packed | PASS |
| 2 | A1b | Round 0: wrong CLAIM 13260060#7, right CLAIM 13260060#8 (one ledger, wrong first) | wrong one: bad sig, then right one: claiming | PASS |
| 3 | A1b | Round 0: payout | tesSUCCESS | PASS |
| 4 | A1b | A1b-1: PACK | tgs: packed | PASS |
| 5 | A1b | Round 1: wrong CLAIM 13260065#5, right CLAIM 13260065#6 (one ledger, wrong first) | wrong one: bad sig, then right one: claiming | PASS |
| 6 | A1b | Round 1: payout | tesSUCCESS | PASS |
| 7 | A1b | A1b-2: PACK | tgs: packed | PASS |
| 8 | A1b | Round 2: wrong CLAIM 13260070#6, right CLAIM 13260070#7 (one ledger, wrong first) | wrong one: bad sig, then right one: claiming | PASS |
| 9 | A1b | Round 2: payout | tesSUCCESS | PASS |
| 10 | A1b | Rounds with "wrong first in the same ledger" and the right outcome | >=1 | PASS |
| 11 | CLEANUP | Hook state cleared (2 → 0) | 0 | PASS |

## extra — 115 checks

X1–X8: bad parameters, signature bound to every field, replay on another hook, repack, double CLAIM, CLAIM vs OPEN at the limit, reserve tricks.

X7 round 4 failed in the first run because the test set the time limit too close. Those 2 rows are replaced by the rerun (marked "rerun"), which passed.

Accounts:

- creator (hook account): [rMexHqP4FvJhroArS8BCoNCFMu2DrVA4tQ](https://test.xahauexplorer.com/account/rMexHqP4FvJhroArS8BCoNCFMu2DrVA4tQ)
- R1: [rpb8STzHkJyaeKnySzd9aC2yAZ3sxS2qUv](https://test.xahauexplorer.com/account/rpb8STzHkJyaeKnySzd9aC2yAZ3sxS2qUv)
- R2: [rpsmmR2wcq34V9fpD424hh5gdfCuZpB3SB](https://test.xahauexplorer.com/account/rpsmmR2wcq34V9fpD424hh5gdfCuZpB3SB)
- H2 (R3, second hook): [rw8WFhVmBYx2oNx3uGtPRhjVHFUR3m9ZHR](https://test.xahauexplorer.com/account/rw8WFhVmBYx2oNx3uGtPRhjVHFUR3m9ZHR)
- R4 (stranger): [rfCZArs6iJywbkhMjgeUvGzrYzoDa8UUt](https://test.xahauexplorer.com/account/rfCZArs6iJywbkhMjgeUvGzrYzoDa8UUt)
- X7 round 4 rerun used its own accounts:
  - creator (hook account): [rMiVr8HkWpNL59YrDe4qznYT4eeYKNYZWD](https://test.xahauexplorer.com/account/rMiVr8HkWpNL59YrDe4qznYT4eeYKNYZWD)
  - R1: [rU4UL8zypHK2MSLLoNbcj6YdTEeSC6cz2k](https://test.xahauexplorer.com/account/rU4UL8zypHK2MSLLoNbcj6YdTEeSC6cz2k)
  - R2: [r9kbsws47e42oFmq82euSmKU8t7Qi4hfQi](https://test.xahauexplorer.com/account/r9kbsws47e42oFmq82euSmKU8t7Qi4hfQi)
  - R3: [rM1hXQfzsJUdpTSdzYNrKJEKZBCWydEgDh](https://test.xahauexplorer.com/account/rM1hXQfzsJUdpTSdzYNrKJEKZBCWydEgDh)
  - R4 (stranger): [r38NCRfSBWid7v12ycPykY6YbUJw8ii8qD](https://test.xahauexplorer.com/account/r38NCRfSBWid7v12ycPykY6YbUJw8ii8qD)

| # | ID | Check | Expected | Result |
|---|---|---|---|---|
| 1 | X1 | X1 base box (key link, token + 1 XAH): PACK | tgs: packed | PASS |
| 2 | X1 | EXP in the past (validated ledger) | tgs: exp past | PASS |
| 3 | X1 | EXP of 3 bytes | tgs: exp | PASS |
| 4 | X1 | AMT of 7 bytes | tgs: amt | PASS |
| 5 | X1 | AMT with flag 0x40 in the top byte | tgs: amt | PASS |
| 6 | X1 | AMT above 10^17 drops | tgs: amt | PASS |
| 7 | X1 | Empty box (no token, AMT=0) | tgs: empty | PASS |
| 8 | X1 | Box number = 32 zero bytes | tgs: vc | PASS |
| 9 | X1 | Box number = reserved internal key (22…22) | tgs: vc | PASS |
| 10 | X1 | Box number of 31 bytes | tgs: vc | PASS |
| 11 | X1 | MODE=02 | tgs: mode | PASS |
| 12 | X1 | MODE of 2 bytes | tgs: mode | PASS |
| 13 | X1 | Token ID that does not exist | tgs: token | PASS |
| 14 | X1 | Token ID of a token owned by R1 | tgs: token | PASS |
| 15 | X1 | Key-token mode: key token = content token | tgs: ktid=tid | PASS |
| 16 | X1 | Key-token mode: key token is locked in another box | tgs: ktid locked | PASS |
| 17 | X1 | Key-token mode: key token ID = zeros | tgs: ktid | PASS |
| 18 | X1 | PACK from another account (R1) | tgs: owner | PASS |
| 19 | X1 | AUDIT from another account (R1) | tgs: owner | PASS |
| 20 | X1 | Unknown command XXXX | tgs: cmd | PASS |
| 21 | X1 | Invoke with no CMD | tgs: cmd | PASS |
| 22 | X1 | After all refusals no records were added (only the base box) | 1 box | PASS |
| 23 | X1 | X1 base box (key link, token + 1 XAH): CLAIM (R2) | tgs: claiming | PASS |
| 24 | X1 | X1 base box (key link, token + 1 XAH): hook sends the payout Remit | tesSUCCESS | PASS |
| 25 | X1 | X1 base box (key link, token + 1 XAH): XAH arrived (balance change incl. fee) | 1200000 drops | PASS |
| 26 | X1 | X1 base box (key link, token + 1 XAH): token now owned by R2 | R2 | PASS |
| 27 | X2 | X2 key-link box: PACK | tgs: packed | PASS |
| 28 | X2 | PK of 32 bytes | tgs: pk | PASS |
| 29 | X2 | SIG shorter than 64 bytes | tgs: sig | PASS |
| 30 | X2 | SIG longer than 72 bytes | tgs: sig | PASS |
| 31 | X2 | Key-link box claimed like a key-token box (box number, no PK) | tgs: key box | PASS |
| 32 | X2 | CLAIM with the number of a box that does not exist | tgs: unknown | PASS |
| 33 | X2 | X2 key-token box with box number = hash(PK): PACK | tgs: packed | PASS |
| 34 | X2 | Key-token box claimed with a signature (PK/SIG) | tgs: token box | PASS |
| 35 | X2 | SYNC with no box number, nothing pending | tgs: idle | PASS |
| 36 | X2 | SYNC with the number of a closed box | tgs: idle | PASS |
| 37 | X2 | X2 key-link box: CLAIM (R1) | tgs: claiming | PASS |
| 38 | X2 | X2 key-link box: hook sends the payout Remit | tesSUCCESS | PASS |
| 39 | X2 | X2 key-link box: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 40 | X2 | X2 key-token box with box number = hash(PK): CLAIM by the key token holder (R2) | tgs: claiming | PASS |
| 41 | X2 | X2 key-token box with box number = hash(PK): hook sends the payout Remit | tesSUCCESS | PASS |
| 42 | X2 | X2 key-token box with box number = hash(PK): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 43 | X3 | X3 (key link, token + 1.5 XAH, time limit): PACK | tgs: packed | PASS |
| 44 | X3 | Signature with a wrong field: other NetworkID 21337 (mainnet-like) | tgs: bad sig | PASS |
| 45 | X3 | Signature with a wrong field: NetworkID 0 | tgs: bad sig | PASS |
| 46 | X3 | Signature with a wrong field: other hook account (H2) | tgs: bad sig | PASS |
| 47 | X3 | Signature with a wrong field: EXP+1 | tgs: bad sig | PASS |
| 48 | X3 | Signature with a wrong field: EXP=0 instead of the real one | tgs: bad sig | PASS |
| 49 | X3 | Signature with a wrong field: another token ID | tgs: bad sig | PASS |
| 50 | X3 | Signature with a wrong field: token ID = zeros (as if XAH-only box) | tgs: bad sig | PASS |
| 51 | X3 | Signature with a wrong field: AMT + 1 drop | tgs: bad sig | PASS |
| 52 | X3 | Signature with a wrong field: NONCE+1 | tgs: bad sig | PASS |
| 53 | X3 | Signature with a wrong field: NONCE-1 | tgs: bad sig | PASS |
| 54 | X3 | Signature with a wrong field: recipient R2, sent by R1 | tgs: bad sig | PASS |
| 55 | X3 | Box PK plus a signature by another key over the right message | tgs: bad sig | PASS |
| 56 | X3 | Box X3 untouched after 12 wrong signatures | status 00 | PASS |
| 57 | X3 | X3 (key link, token + 1.5 XAH, time limit): CLAIM (R1) | tgs: claiming | PASS |
| 58 | X3 | X3 (key link, token + 1.5 XAH, time limit): hook sends the payout Remit | tesSUCCESS | PASS |
| 59 | X3 | X3 (key link, token + 1.5 XAH, time limit): XAH arrived (balance change incl. fee) | 1700000 drops | PASS |
| 60 | X3 | X3 (key link, token + 1.5 XAH, time limit): token now owned by R1 | R1 | PASS |
| 61 | X4 | Second hook H2 (on R3) installed by HookHash | HookHash 3CEEAB2F | PASS |
| 62 | X4 | X4 on hook H1: PACK | tgs: packed | PASS |
| 63 | X4 | PACK on H2 with the same key and amount | tgs: packed | PASS |
| 64 | X4 | Signature for H1 presented on H2 | tgs: bad sig | PASS |
| 65 | X4 | Signature for H2 but with the NONCE of H1 (they differ) | tgs: bad sig | PASS |
| 66 | X4 | X4 on hook H1: CLAIM (R1) | tgs: claiming | PASS |
| 67 | X4 | X4 on hook H1: hook sends the payout Remit | tesSUCCESS | PASS |
| 68 | X4 | X4 on hook H1: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 69 | X4 | Rightful CLAIM on H2 | tgs: claiming | PASS |
| 70 | X4 | R2 got 1 XAH from H2 | 1000000 | PASS |
| 71 | X5 | X5a: PACK | tgs: packed | PASS |
| 72 | X5 | X5a: CLAIM (R1) | tgs: claiming | PASS |
| 73 | X5 | X5a: hook sends the payout Remit | tesSUCCESS | PASS |
| 74 | X5 | X5a: XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 75 | X5 | X5b (same key, same amount): PACK | tgs: packed | PASS |
| 76 | X5 | NONCE changed | not 00000006 | PASS |
| 77 | X5 | Old R1 signature on a new, identical box | tgs: bad sig | PASS |
| 78 | X5 | New signature but with the old NONCE | tgs: bad sig | PASS |
| 79 | X5 | X5b (same key, same amount): CLAIM (R2) | tgs: claiming | PASS |
| 80 | X5 | X5b (same key, same amount): hook sends the payout Remit | tesSUCCESS | PASS |
| 81 | X5 | X5b (same key, same amount): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 82 | X5 | X5c (time limit, then OPEN): PACK | tgs: packed | PASS |
| 83 | X5 | OPEN X5c after the time limit | tgs: opened | PASS |
| 84 | X5 | X5d (after OPEN): PACK | tgs: packed | PASS |
| 85 | X5 | Signature from X5c used on X5d | tgs: bad sig | PASS |
| 86 | X5 | X5d (after OPEN): CLAIM (R1) | tgs: claiming | PASS |
| 87 | X5 | X5d (after OPEN): hook sends the payout Remit | tesSUCCESS | PASS |
| 88 | X5 | X5d (after OPEN): XAH arrived (balance change incl. fee) | 1000000 drops | PASS |
| 89 | X6 | X6-0 (key link, token + 2 XAH): PACK | tgs: packed | PASS |
| 90 | X6 | Round 0: two CLAIMs in one ledger: #1 13260572#4 "tgs: claiming", #2 13260572#6 "tgs: busy" | exactly 1 claiming, the other busy/claimed/unknown | PASS |
| 91 | X6 | Round 0: paid out exactly once | 2200000 | PASS |
| 92 | X6 | X6-1 (key link, token + 2 XAH): PACK | tgs: packed | PASS |
| 93 | X6 | Round 1: two CLAIMs in one ledger: #1 13260579#5 "tgs: claiming", #2 13260579#6 "tgs: busy" | exactly 1 claiming, the other busy/claimed/unknown | PASS |
| 94 | X6 | Round 1: paid out exactly once | 2200000 | PASS |
| 95 | X7 | X7-0 (key link, token + 1 XAH, time limit): PACK | tgs: packed | PASS |
| 96 | X7 | Round 0: EXP=13260588; OPEN in 13260588#7 "tgs: busy"; CLAIM in 13260588#6 "tgs: claiming" | at most one winner; assets go to the winner; time limit rule kept; with no winner the box is intact | PASS |
| 97 | X7 | X7-1 (key link, token + 1 XAH, time limit): PACK | tgs: packed | PASS |
| 98 | X7 | Round 1: EXP=13260598; OPEN in 13260599#5 "tgs: opened"; CLAIM in 13260599#6 "tgs: unknown" | at most one winner; assets go to the winner; time limit rule kept; with no winner the box is intact | PASS |
| 99 | X7 | X7-2 (key link, token + 1 XAH, time limit): PACK | tgs: packed | PASS |
| 100 | X7 | Round 2: EXP=13260609; OPEN in 13260609#5 "tgs: busy"; CLAIM in 13260609#4 "tgs: claiming" | at most one winner; assets go to the winner; time limit rule kept; with no winner the box is intact | PASS |
| 101 | X7 | X7-3 (key link, token + 1 XAH, time limit): PACK | tgs: packed | PASS |
| 102 | X7 | Round 3: EXP=13260620; OPEN in 13260621#4 "tgs: opened"; CLAIM in 13260621#5 "tgs: unknown" | at most one winner; assets go to the winner; time limit rule kept; with no winner the box is intact | PASS |
| 103 | X7 | X7-5 (key link, token + 1 XAH, time limit): PACK | tgs: packed | PASS |
| 104 | X7 | Round 5: EXP=13260643; OPEN in 13260644#4 "tgs: opened"; CLAIM in 13260644#5 "tgs: unknown" | at most one winner; assets go to the winner; time limit rule kept; with no winner the box is intact | PASS |
| 105 | X8 | X8 big box (XAH, time limit): PACK | tgs: packed | PASS |
| 106 | X8 | TicketCreate 200 (reserve 40 XAH with 30 free) | tgs: reserve | PASS |
| 107 | X8 | TicketCreate 2 (enough free XAH) | tgs: out | PASS |
| 108 | X8 | Remit of 29 XAH (more than free) | tgs: reserve | PASS |
| 109 | X8 | Payment of 29 XAH to a new account (+1 XAH reserve for it) | tgs: reserve | PASS |
| 110 | X8 | OPEN the big box after the time limit | tgs: opened | PASS |
| 111 | FINAL | No boxes left on H1 | 0 | PASS |
| 112 | FINAL | No token locks left on H1 | 0 | PASS |
| 113 | FINAL | Locked XAH and lock count are 0 | 0/0 | PASS |
| 114 | X7 | X7-4 (key link, token + 1 XAH, time limit): PACK (rerun) | tgs: packed | PASS |
| 115 | X7 | Round 4: EXP=13260971; OPEN in 13260971#5 "tgs: busy"; CLAIM in 13260971#4 "tgs: claiming" (rerun) | at most one winner; assets go to the winner; time limit rule kept; with no winner the box is intact | PASS |
