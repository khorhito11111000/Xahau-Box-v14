# Manual tests: full list

Hook v14 on Xahau Testnet (NetworkID 21338). HookHash `3CEEAB2F96E71E3AAEEA0C7220834B5F3D4643E82B1CB74B89B88A22C9722207`.

Date: 2026-10-17. Every transaction was signed by hand in Xaman, sent as a raw transaction through xrplwin.

## Accounts

| Role | Address |
|---|---|
| A — creator, the hook is on this account | [rNHBiY7RQhmAviL8cj56maSiHMyYPJvzW6](https://test.xahauexplorer.com/account/rNHBiY7RQhmAviL8cj56maSiHMyYPJvzW6) |
| B — recipient 1 | [rn3FvKwmgvwu4HtZt8RpGhSqiFiwQa9WoV](https://test.xahauexplorer.com/account/rn3FvKwmgvwu4HtZt8RpGhSqiFiwQa9WoV) |
| C — recipient 2 | [r9PyoseZW532HCvfwcdCRc1VKSzA5GKpRN](https://test.xahauexplorer.com/account/r9PyoseZW532HCvfwcdCRc1VKSzA5GKpRN) |

## Result

**49 tests in 94 steps. All 94 steps passed. Every result matched the expectation.**

Tests 1–31 are the earlier tests, run again on v14. Tests 32–49 are new in v14.

"Success" means tesSUCCESS. The text after it is the hook answer (HookReturnString). "Rejected" means the hook refused the transaction with tecHOOK_REJECTED, as planned. After each `tgs: claiming` the hook sends the payout itself, as a separate Remit from A. These 13 payout Remits are not steps in the list.

## Tests

| # | Test | What is checked | Expected | Steps |
|---|---|---|---|---|
| 1 | AUDIT first | PACK is refused until AUDIT is done. After AUDIT it works. | `tgs: audit`, then `tgs: audited`, `tgs: packed` | 13, 14, 15 |
| 2 | Key link: token + XAH | A key-link box with a token and XAH opens with the right signature. | `tgs: claiming`, payout Remit to B | 15, 30 |
| 3 | Key link: token only | A key-link box with only a token opens. | `tgs: claiming`, token goes to C | 24, 32 |
| 4 | Key link: XAH only | A key-link box with only XAH opens. | `tgs: claiming`, XAH goes to B | 25, 33 |
| 5 | One token, one box | A token already in a box cannot go into a second box. | `tgs: locked` | 26 |
| 6 | Unique box number | The same box number cannot be used twice. | `tgs: exists` | 27 |
| 7 | Signature for another address | A signature made for C does not work from B. | `tgs: bad sig` | 31 |
| 8 | No second CLAIM | An opened box cannot be opened again. | `tgs: unknown` | 34, 35, 36 |
| 9 | Key token: full cycle | A packs, sends the key token to B, B opens. | `tgs: packed`, `tgs: out`, `tgs: claiming` | 37, 38, 39 |
| 10 | Time limit: too early | The creator cannot take the box back before the limit. | `tgs: not expired` | 91, 92 |
| 11 | Time limit: too late to open | After the limit the recipient cannot open. | `tgs: expired` | 93 |
| 12 | Time limit: take back | After the limit the creator takes the contents back. | `tgs: opened` | 94 |
| 13 | No key token, no box | An account without the key token cannot open a key-token box. | `tgs: not holder` | 40, 41 |
| 14 | Locked token cannot be sent | The creator cannot Remit a token that is in a box. The rightful recipient still opens it. | `tgs: locked`, then `tgs: claiming` | 46, 47, 49 |
| 15 | Locked XAH is safe | A Payment that eats into box XAH is refused. A Payment from free XAH passes. The big box still opens. | `tgs: reserve`, `tgs: out`, `tgs: claiming` | 16, 18, 19, 22, 23 |
| 16 | Key token as a gift | A gives the key token to B for free, the key passes on, the holder opens. | `tgs: out`, `tgs: claiming` | 54, 55, 60 |
| 17 | Former holder cannot open | B sold the key token and can no longer open. | `tgs: not holder` | 58 |
| 18 | Not more than you have | A box for more XAH than the account has is refused. | `tgs: not enough` | 16, 17 |
| 19 | No DEX escape | An offer that sells locked XAH is refused. | `tgs: offer` | 20 |
| 20 | No check escape | A check for locked XAH is refused. | `tgs: check` | 21 |
| 21 | No second OPEN | An OPEN on a box that is already open is refused. | `tgs: unknown` | 45 |
| 22 | Many boxes at once | Four boxes of both modes are open at the same time. | `tgs: packed` for all four | 62, 63, 64, 65 |
| 23 | Locked token cannot be sold | A sell offer on a locked token is refused. | `tgs: locked` | 51 |
| 24 | Key token is sold | B sells the key token to C for 5 XAH. | tesSUCCESS | 56, 57 |
| 25 | Bearer QR | A box opens from a bearer QR. | `tgs: claiming` | 62, 66 |
| 26 | Bearer QR: no repeat | The same QR cannot open the box twice. | `tgs: unknown` | 67 |
| 27 | QR with a gift code | A box opens from a QR with an encrypted gift code. The code is decrypted after opening. | `tgs: claiming` | 63, 68 |
| 28 | Named QR: wrong account | C cannot use the QR made for B. | `tgs: bad sig` | 64, 69 |
| 29 | Named QR: right account | The named recipient B opens the box. | `tgs: claiming` | 70 |
| 30 | Key-token QR without the key | C scans the QR but has no key token. | `tgs: not holder` | 65, 71 |
| 31 | Key-token QR with the key | C gets the key token and opens from the same QR. | `tgs: out`, `tgs: claiming` | 72, 73 |
| 32 | Main story A → B → C | A voucher token and 10 XAH in a box. A gives the key token to B, B sells it to C, only C opens. A second CLAIM fails. | `tgs: claiming` for C only | 50, 54, 55, 56, 57, 60, 61 |
| 33 | No OPEN without a time limit | The creator cannot take back a box with no time limit while the key exists. | `tgs: not expired` | 29, 42, 52 |
| 34 | Burned key token | If the key token is burned, the creator can take the box back. | `tgs: out`, then `tgs: opened` | 43, 44 |
| 35 | Token with an open offer | A token with an open sell offer cannot be packed. | `tgs: token offer` | 74, 75, 76 |
| 36 | Burnable content | A burnable token cannot go into a box. | `tgs: burnable` | 77 |
| 37 | Burnable key token | A burnable token cannot be the key token. | `tgs: burnable` | 78 |
| 38 | Key must be a URIToken | The key token must be a real URIToken. | `tgs: ktid` | 79 |
| 39 | Key is not the content | The key token cannot be the token inside the box. | `tgs: ktid=tid` | 80 |
| 40 | Locked token as key | A token that is locked in a box cannot be the key token. | `tgs: ktid locked` | 53 |
| 41 | Old signature after repack | After the same key is packed again, the old signature fails and a new one works. | `tgs: bad sig` for the old, `tgs: claiming` for the new | 86, 87, 88, 89, 90 |
| 42 | Unknown mode | PACK with an unknown MODE is refused. | `tgs: mode` | 81 |
| 43 | Box number is not enough | A key-link box does not open with only the public box number. | `tgs: key box` | 28 |
| 44 | Creator cannot open a key-token box | The creator does not hold the key token and cannot open. | `tgs: not holder` | 59 |
| 45 | Locked token cannot be burned | The creator cannot burn a token that is in a box. | `tgs: locked` | 48 |
| 46 | Time limit in the past | PACK with a time limit in the past is refused. | `tgs: exp past` | 82 |
| 47 | Empty box | PACK with no token and no XAH is refused. | `tgs: empty` | 83 |
| 48 | Stranger cannot PACK | Another account cannot pack a box on someone else's hook. | `tgs: owner` | 84 |
| 49 | Stranger cannot OPEN | Another account cannot OPEN on someone else's hook. | `tgs: owner` | 85 |

## Steps

Who signs: A, B or C (see Accounts). All steps passed.

| Step | Signs | Transaction | What it does | Expected result | Tests |
|---|---|---|---|---|---|
| 1 | A | URITokenMint | Mint token T01 (not burnable) | Success: tesSUCCESS | — |
| 2 | A | URITokenMint | Mint token T02 (not burnable) | Success: tesSUCCESS | — |
| 3 | A | URITokenMint | Mint token K04 (not burnable) | Success: tesSUCCESS | — |
| 4 | A | URITokenMint | Mint token T04 (not burnable) | Success: tesSUCCESS | — |
| 5 | A | URITokenMint | Mint token K05 (not burnable) | Success: tesSUCCESS | — |
| 6 | A | URITokenMint | Mint token T08 (not burnable) | Success: tesSUCCESS | — |
| 7 | A | URITokenMint | Mint token VOUCHER (not burnable) | Success: tesSUCCESS | — |
| 8 | A | URITokenMint | Mint token KEY1 (not burnable) | Success: tesSUCCESS | — |
| 9 | A | URITokenMint | Mint token GIFT (not burnable) | Success: tesSUCCESS | — |
| 10 | A | URITokenMint | Mint token K13 (not burnable) | Success: tesSUCCESS | — |
| 11 | A | URITokenMint | Mint token T35 (not burnable) | Success: tesSUCCESS | — |
| 12 | A | URITokenMint | Mint token T36 (burnable) | Success: tesSUCCESS | — |
| 13 | A | Invoke (PACK) | PACK box 1 before AUDIT (refusal expected) | Rejected: tecHOOK_REJECTED, `tgs: audit` | 1 |
| 14 | A | Invoke (AUDIT) | AUDIT | Success: tesSUCCESS, `tgs: audited` | 1 |
| 15 | A | Invoke (PACK) | PACK box 1: key link, T01 + 10 XAH | Success: tesSUCCESS, `tgs: packed` | 1, 2 |
| 16 | A | Invoke (PACK) | PACK box 8: key link, 2900 XAH (big box) | Success: tesSUCCESS, `tgs: packed` | 15, 18 |
| 17 | A | Invoke (PACK) | PACK for 5000 XAH (more than the account has) | Rejected: tecHOOK_REJECTED, `tgs: not enough` | 18 |
| 18 | A | Payment | Payment 500 XAH A → B (eats into box XAH) | Rejected: tecHOOK_REJECTED, `tgs: reserve` | 15 |
| 19 | A | Payment | Payment 1 XAH A → B (free XAH) | Success: tesSUCCESS, `tgs: out` | 15 |
| 20 | A | OfferCreate | OfferCreate: sell 500 XAH on the DEX | Rejected: tecHOOK_REJECTED, `tgs: offer` | 19 |
| 21 | A | CheckCreate | CheckCreate: check for 500 XAH | Rejected: tecHOOK_REJECTED, `tgs: check` | 20 |
| 22 | B | Invoke (CLAIM) | CLAIM box 8 (B, signature) | Success: tesSUCCESS, `tgs: claiming` | 15 |
| 23 | B | Payment | B returns 2900 XAH to the creator | Success: tesSUCCESS | 15 |
| 24 | A | Invoke (PACK) | PACK box 2: key link, only T02 | Success: tesSUCCESS, `tgs: packed` | 3 |
| 25 | A | Invoke (PACK) | PACK box 3: key link, only 5 XAH | Success: tesSUCCESS, `tgs: packed` | 4 |
| 26 | A | Invoke (PACK) | PACK a new box with the same T02 | Rejected: tecHOOK_REJECTED, `tgs: locked` | 5 |
| 27 | A | Invoke (PACK) | PACK with the same box number as box 3 | Rejected: tecHOOK_REJECTED, `tgs: exists` | 6 |
| 28 | B | Invoke (CLAIM) | CLAIM box 1 with the box number only, no signature (the number is public in PACK) | Rejected: tecHOOK_REJECTED, `tgs: key box` | 43 |
| 29 | A | Invoke (OPEN) | OPEN box 3 with no time limit (key link) | Rejected: tecHOOK_REJECTED, `tgs: not expired` | 33 |
| 30 | B | Invoke (CLAIM) | CLAIM box 1 (B, signature) | Success: tesSUCCESS, `tgs: claiming` | 2 |
| 31 | B | Invoke (CLAIM) | CLAIM box 2 from B's address with a signature made for C | Rejected: tecHOOK_REJECTED, `tgs: bad sig` | 7 |
| 32 | C | Invoke (CLAIM) | CLAIM box 2 (C, own signature) | Success: tesSUCCESS, `tgs: claiming` | 3 |
| 33 | B | Invoke (CLAIM) | CLAIM box 3 (B) | Success: tesSUCCESS, `tgs: claiming` | 4 |
| 34 | B | Invoke (CLAIM) | Repeat: CLAIM box 1 (B, signature) | Rejected: tecHOOK_REJECTED, `tgs: unknown` | 8 |
| 35 | C | Invoke (CLAIM) | Repeat: CLAIM box 2 (C, own signature) | Rejected: tecHOOK_REJECTED, `tgs: unknown` | 8 |
| 36 | B | Invoke (CLAIM) | Repeat: CLAIM box 3 (B) | Rejected: tecHOOK_REJECTED, `tgs: unknown` | 8 |
| 37 | A | Invoke (PACK) | PACK box 4: key token K04, inside T04 + 3 XAH | Success: tesSUCCESS, `tgs: packed` | 9 |
| 38 | A | Remit | Remit key token K04 → B | Success: tesSUCCESS, `tgs: out` | 9 |
| 39 | B | Invoke (CLAIM) | CLAIM box 4 (B holds the key token) | Success: tesSUCCESS, `tgs: claiming` | 9 |
| 40 | A | Invoke (PACK) | PACK box 5: key token K05 (stays with A), 1 XAH | Success: tesSUCCESS, `tgs: packed` | 13 |
| 41 | C | Invoke (CLAIM) | CLAIM box 5 from C's address without the key token | Rejected: tecHOOK_REJECTED, `tgs: not holder` | 13 |
| 42 | A | Invoke (OPEN) | OPEN box 5 with no time limit, key token still exists | Rejected: tecHOOK_REJECTED, `tgs: not expired` | 33 |
| 43 | A | URITokenBurn | URITokenBurn of key token K05 (A owns it) | Success: tesSUCCESS, `tgs: out` | 34 |
| 44 | A | Invoke (OPEN) | OPEN box 5 after the key token is burned | Success: tesSUCCESS, `tgs: opened` | 34 |
| 45 | A | Invoke (OPEN) | OPEN box 5 again | Rejected: tecHOOK_REJECTED, `tgs: unknown` | 21 |
| 46 | A | Invoke (PACK) | PACK box 7: key link, T08 + 2 XAH | Success: tesSUCCESS, `tgs: packed` | 14 |
| 47 | A | Remit | Remit locked T08 → B | Rejected: tecHOOK_REJECTED, `tgs: locked` | 14 |
| 48 | A | URITokenBurn | URITokenBurn of locked T08 | Rejected: tecHOOK_REJECTED, `tgs: locked` | 45 |
| 49 | C | Invoke (CLAIM) | CLAIM box 7 (C) | Success: tesSUCCESS, `tgs: claiming` | 14 |
| 50 | A | Invoke (PACK) | PACK box 9: key token KEY1, inside VOUCHER + 10 XAH | Success: tesSUCCESS, `tgs: packed` | 32 |
| 51 | A | URITokenCreateSellOffer | Sell offer of locked VOUCHER to B for 0 | Rejected: tecHOOK_REJECTED, `tgs: locked` | 23 |
| 52 | A | Invoke (OPEN) | OPEN box 9 by the creator (no time limit, key token exists) | Rejected: tecHOOK_REJECTED, `tgs: not expired` | 33 |
| 53 | A | Invoke (PACK) | PACK a box whose key token is the locked VOUCHER | Rejected: tecHOOK_REJECTED, `tgs: ktid locked` | 40 |
| 54 | A | URITokenCreateSellOffer | A gives key token KEY1 to B: sell offer for 0, only for B | Success: tesSUCCESS, `tgs: out` | 16, 32 |
| 55 | B | URITokenBuy | B accepts the key token (URITokenBuy for 0) | Success: tesSUCCESS | 16, 32 |
| 56 | B | URITokenCreateSellOffer | B sells the key token to C for 5 XAH (sell offer only for C) | Success: tesSUCCESS | 24, 32 |
| 57 | C | URITokenBuy | C buys the key token for 5 XAH | Success: tesSUCCESS | 24, 32 |
| 58 | B | Invoke (CLAIM) | CLAIM box 9 by the former holder B | Rejected: tecHOOK_REJECTED, `tgs: not holder` | 17 |
| 59 | A | Invoke (CLAIM) | CLAIM box 9 by the creator A | Rejected: tecHOOK_REJECTED, `tgs: not holder` | 44 |
| 60 | C | Invoke (CLAIM) | CLAIM box 9 (C holds the key token) | Success: tesSUCCESS, `tgs: claiming` | 16, 32 |
| 61 | C | Invoke (CLAIM) | CLAIM box 9 again | Rejected: tecHOOK_REJECTED, `tgs: unknown` | 32 |
| 62 | A | Invoke (PACK) | PACK box 10: key link, 2 XAH (bearer QR) | Success: tesSUCCESS, `tgs: packed` | 22, 25 |
| 63 | A | Invoke (PACK) | PACK box 11: key link, token with an encrypted gift code + 1 XAH | Success: tesSUCCESS, `tgs: packed` | 22, 27 |
| 64 | A | Invoke (PACK) | PACK box 12: key link, 2 XAH (QR only for B) | Success: tesSUCCESS, `tgs: packed` | 22, 28 |
| 65 | A | Invoke (PACK) | PACK box 13: key token K13, 2 XAH | Success: tesSUCCESS, `tgs: packed` | 22, 30 |
| 66 | B | Invoke (CLAIM) | CLAIM box 10 from the QR (B) | Success: tesSUCCESS, `tgs: claiming` | 25 |
| 67 | B | Invoke (CLAIM) | Repeat the same CLAIM from the QR of box 10 | Rejected: tecHOOK_REJECTED, `tgs: unknown` | 26 |
| 68 | B | Invoke (CLAIM) | CLAIM box 11 from the QR with the gift code (B) | Success: tesSUCCESS, `tgs: claiming` | 27 |
| 69 | C | Invoke (CLAIM) | C tries to open box 12 with the signature from B's QR | Rejected: tecHOOK_REJECTED, `tgs: bad sig` | 28 |
| 70 | B | Invoke (CLAIM) | CLAIM box 12 by the named recipient B | Success: tesSUCCESS, `tgs: claiming` | 29 |
| 71 | C | Invoke (CLAIM) | CLAIM box 13 from the QR, C has no key token | Rejected: tecHOOK_REJECTED, `tgs: not holder` | 30 |
| 72 | A | Remit | Remit key token K13 → C | Success: tesSUCCESS, `tgs: out` | 31 |
| 73 | C | Invoke (CLAIM) | CLAIM box 13 from the QR (C holds the key token) | Success: tesSUCCESS, `tgs: claiming` | 31 |
| 74 | A | URITokenCreateSellOffer | Sell offer of T35 to B for 1 XAH (setup) | Success: tesSUCCESS, `tgs: out` | 35 |
| 75 | A | Invoke (PACK) | PACK token T35 that has an open sell offer | Rejected: tecHOOK_REJECTED, `tgs: token offer` | 35 |
| 76 | A | URITokenCancelSellOffer | Cancel the sell offer on T35 | Success: tesSUCCESS, `tgs: out` | 35 |
| 77 | A | Invoke (PACK) | PACK burnable token T36 | Rejected: tecHOOK_REJECTED, `tgs: burnable` | 36 |
| 78 | A | Invoke (PACK) | PACK key-token box, key = burnable T36 | Rejected: tecHOOK_REJECTED, `tgs: burnable` | 37 |
| 79 | A | Invoke (PACK) | PACK key-token box, key = A's account record (not a token) | Rejected: tecHOOK_REJECTED, `tgs: ktid` | 38 |
| 80 | A | Invoke (PACK) | PACK key-token box, key = the content (T35) | Rejected: tecHOOK_REJECTED, `tgs: ktid=tid` | 39 |
| 81 | A | Invoke (PACK) | PACK with MODE=02 (no such mode) | Rejected: tecHOOK_REJECTED, `tgs: mode` | 42 |
| 82 | A | Invoke (PACK) | PACK with a time limit in the past (EXP=1) | Rejected: tecHOOK_REJECTED, `tgs: exp past` | 46 |
| 83 | A | Invoke (PACK) | PACK an empty box (no token, no XAH) | Rejected: tecHOOK_REJECTED, `tgs: empty` | 47 |
| 84 | B | Invoke (PACK) | B tries to pack a box on A's hook | Rejected: tecHOOK_REJECTED, `tgs: owner` | 48 |
| 85 | B | Invoke (OPEN) | B tries to OPEN | Rejected: tecHOOK_REJECTED, `tgs: owner` | 49 |
| 86 | A | Invoke (PACK) | PACK box 14: key link, 1 XAH | Success: tesSUCCESS, `tgs: packed` | 41 |
| 87 | B | Invoke (CLAIM) | CLAIM box 14 (B, signature no. 1) | Success: tesSUCCESS, `tgs: claiming` | 41 |
| 88 | A | Invoke (PACK) | Repack: same key for box 14, 1 XAH again | Success: tesSUCCESS, `tgs: packed` | 41 |
| 89 | B | Invoke (CLAIM) | B repeats old signature no. 1 | Rejected: tecHOOK_REJECTED, `tgs: bad sig` | 41 |
| 90 | B | Invoke (CLAIM) | CLAIM box 14 with new signature no. 2 | Success: tesSUCCESS, `tgs: claiming` | 41 |
| 91 | A | Invoke (PACK) | PACK box 6: key link, 2 XAH, time limit EXP | Success: tesSUCCESS, `tgs: packed` | 10 |
| 92 | A | Invoke (OPEN) | OPEN box 6 before the time limit | Rejected: tecHOOK_REJECTED, `tgs: not expired` | 10 |
| 93 | B | Invoke (CLAIM) | CLAIM box 6 after the time limit (B) | Rejected: tecHOOK_REJECTED, `tgs: expired` | 11 |
| 94 | A | Invoke (OPEN) | OPEN box 6 after the time limit | Success: tesSUCCESS, `tgs: opened` | 12 |

## Test tokens

All tokens were minted by A in steps 1–12. URIToken IDs are public.

| Name | URI (text) | URIToken ID | Role |
|---|---|---|---|
| T01 | TGS14-T01 | `389C7F5A3CE3ACC32F3276CF647A75AA826B92A98280363B4E4AEF6EC157CA76` | content of box 1 |
| T02 | TGS14-T02 | `74C1BAC0B0964B9BAD582905794F1D53C7F928F4971E14FA40ABD633D6D9A47F` | content of box 2 |
| K04 | TGS14-K04 | `F360000E467FAA55D2C86673FACBA449E44F6EC92BF80F818DBB570587891C91` | key token of box 4 |
| T04 | TGS14-T04 | `E71DB2A7E9DA392F24385F32E1DE4D87C3F7514EC01F384F7439F7CB698DD8C4` | content of box 4 |
| K05 | TGS14-K05 | `91DB4E826F08B1D01D29A61A0FBBA1D3EFDC25E958438A76F00D18A6C965BDD9` | key token of box 5 (burned in step 43) |
| T08 | TGS14-T08 | `C3CCB159B5EBDC0AF88ACD7DD34F1256275F980FE300550C514C2956EA1D2C5B` | content of box 7 |
| VOUCHER | VOUCHER-TEST-1 | `9F1A9E08FDE9AF304891A970EED8DCD1B66313535A027EDF91447923C720D5B4` | content of box 9 (the voucher) |
| KEY1 | TGSBOX-KEY-1 | `8E25B763E868E2B141D219E312A77AAC26912D727D2E3DD1C87E3C587FC98A98` | key token of box 9, path A → B → C |
| GIFT | tgsE1:… (encrypted gift code) | `BA3D02E802FDD37735B6152E9CDC05713B33020F7048992A50BC36B51D06FEEE` | content of box 11 |
| K13 | TGS14-K13 | `390D44EC7D6D15941E533DCA569B0F69A942BC1B055EA59129B3150D2A868A16` | key token of box 13 |
| T35 | TGS14-T35 | `26A7D19054B295BA896A46134690C537F0CCA7F640508BE93885F039CDED2724` | refusal tests 35, 39 |
| T36 | TGS14-T36-BURNABLE (Flags 1) | `B01888710604BCCCEE14FD905D5A19726B40539F846ACE01791A4FA3C2ACAF8B` | burnable, refusal tests 36, 37 |

The key token KEY1 can be seen in the explorer: [TGSBOX-KEY-1](https://test.xahauexplorer.com/explorer/8E25B763E868E2B141D219E312A77AAC26912D727D2E3DD1C87E3C587FC98A98). Its history shows the path A → B → C.
