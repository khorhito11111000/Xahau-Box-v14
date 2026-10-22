# Tests

All tests ran on Xahau Testnet (NetworkID 21338) against HookHash `3CEEAB2F96E71E3AAEEA0C7220834B5F3D4643E82B1CB74B89B88A22C9722207`. The hook was installed by HookHash only.

There was no external audit.

**Full lists:**
- [docs/TESTS_MANUAL.md](docs/TESTS_MANUAL.md): test accounts, all 49 manual tests, all 94 steps, token IDs.
- [docs/TESTS_AUTOMATED.md](docs/TESTS_AUTOMATED.md): all 687 automated checks, by suite, with the test accounts.

## Manual tests

Date: 2026-10-17. Every transaction was signed by hand in Xaman, through xrplwin raw transactions.

| Role | Address |
|---|---|
| A — creator, the hook is on this account | [rNHBiY7RQhmAviL8cj56maSiHMyYPJvzW6](https://test.xahauexplorer.com/account/rNHBiY7RQhmAviL8cj56maSiHMyYPJvzW6) |
| B — recipient 1 | [rn3FvKwmgvwu4HtZt8RpGhSqiFiwQa9WoV](https://test.xahauexplorer.com/account/rn3FvKwmgvwu4HtZt8RpGhSqiFiwQa9WoV) |
| C — recipient 2 | [r9PyoseZW532HCvfwcdCRc1VKSzA5GKpRN](https://test.xahauexplorer.com/account/r9PyoseZW532HCvfwcdCRc1VKSzA5GKpRN) |

**49 tests in 94 steps.** Tests 1–31 are the earlier tests, run again on v14. Tests 32–49 are new in v14.

| | Count |
|---|---|
| Signed transactions | 99 (A 65, B 23, C 11) |
| Succeeded | 58 |
| Rejected by the hook, as expected | 41 |
| Payout Remits sent by the hook | 13 |
| Total on chain | 112 |

All results matched the expectation.

### By group

0. **Setup (steps 1–12).** A mints 12 test tokens, one of them burnable.
1. **AUDIT first (test 1).** PACK before AUDIT is refused (`tgs: audit`). After AUDIT it works.
2. **Locked XAH is safe (tests 15, 18–20).** A box with 2900 XAH. A Payment that eats into it, a DEX offer and a Check are refused. A Payment from free XAH passes. A PACK for more than the account has is refused.
3. **Key link (tests 2–8, 43).** Boxes with a token and XAH, only a token, only XAH. The same token cannot go into two boxes. The same box number cannot be used twice. A signature for another address fails. A second CLAIM fails. The box number alone, with no signature, does not open a key-link box.
4. **Key token, full cycle (test 9).** A packs, sends the key token to B, B opens.
5. **No key, no box (tests 13, 21, 33, 34).** C without the key token is refused. A cannot take back a box with no time limit while the key token exists. After the key token is burned, A can take it back. A second OPEN fails.
6. **Steal a locked token (tests 14, 45).** A cannot send or burn a token that is in a box.
7. **Main story A → B → C (tests 16, 17, 23, 24, 32, 33, 40, 44).** A real item (a voucher token) and 10 XAH in a box. A gives the key token to B for free. B sells it to C for 5 XAH. Only C can open. B, the former holder, cannot. A, the creator, cannot. The locked voucher cannot be put up for sale. A locked token cannot be used as a key.
8. **QR and four boxes at once (tests 22, 25–31).** A bearer QR, a bearer QR with a gift code, a QR for B only, a QR for a key-token box. A repeat fails. C cannot use the QR made for B. C cannot open the key-token box until C gets the key token.
9. **New v14 refusals (tests 35–42, 46–49).** The hook refuses: a token with an open sell offer, a burnable token in the box or as the key, a key that is not a URIToken, the key equal to the content, an unknown MODE, a time limit in the past, an empty box. Another account cannot PACK or OPEN on someone else's hook.
10. **Old signature (test 41).** After the same key is packed again, the old signature does not work. The new one does.
11. **Time limit (tests 10–12).** Before the limit A cannot take the box back. After it, B cannot open, and A takes the contents back.
12. **More than 8 offers (test 50, steps 95–105, 2026-10-22).** A makes 10 DEX offers in a row. All pass. Offers 1–8 fill the hook's 8 slots, and offers 9–10 go to a separate counter of locked XAH (2000 drops). After offer 10 is cancelled, the counter stays at 2000 drops: past 8 commitments the hook only errs toward "still locked". Boxes are not affected. Full steps and tx links: [docs/TESTS_MANUAL.md](docs/TESTS_MANUAL.md#extra-test-50-more-than-8-offers-2026-10-22).

## Automated tests

Date: 2026-10-17. Node.js scripts on fresh testnet faucet accounts. The user's own accounts were not used. Scripts are not in this repo.

**687 checks, 687 passed, 0 failed.** 653 transactions on chain: 567 signed by test accounts, 86 Remits sent by the hook.

| Suite | Checks | Signed tx | Hook Remits | What it covers |
|---|---|---|---|---|
| run_tests | 167 | 112 | 22 | The manual scenarios, plus QR boxes of all four kinds, many boxes at once |
| escape | 29 | 33 | 0 | Escrow, offers and other ways to move locked value |
| escape2 | 14 | 19 | 0 | More escape tries; hook removal (limitation) |
| gaps | 26 | 25 | 2 | Recipient blocks Remit; unrelated tokens move freely |
| gaps2 | 35 | 14 | 4 | CLAIM never in a ledger (old LastLedgerSequence, low fee); EXP left out or zero |
| race | 85 | 64 | 9 | 9 rounds, 4 CLAIMs on one box in one ledger |
| multisign | 13 | 16 | 0 | Multi-signed transactions do not bypass the hook |
| new_tests | 93 | 71 | 15 | New v14 rules: offers, burnable tokens, key token checks, box numbers |
| audit | 99 | 92 | 17 | A1–A14: wrong CLAIMs next to right ones, reused keys, fee burn, RequireDest, AccountDelete |
| auditb | 11 | 8 | 3 | A wrong CLAIM before a right one in the same ledger |
| extra | 115 | 113 | 14 | X1–X8: bad parameters, signature bound to every field, replay on another hook, repack, double CLAIM, CLAIM vs OPEN at the limit, reserve tricks |
| **Total** | **687** | **567** | **86** | |

The extra row includes the rerun of X7 round 4 (2 checks, 5 signed, 1 Remit). In the first run that round failed because the test set the time limit too close. The rerun passed.

### Races

9 rounds. In each round 4 CLAIMs hit one box in one ledger. Each time exactly one rightful opener won, there was exactly one payout, and only the winner got the XAH. Others got `tgs: busy`, `tgs: bad sig` or `tgs: not holder`.

### Earlier failures

Every failure in the first runs was a test error, not a hook error. For each one the chain shows the hook answered correctly. The fixed suites passed in full.

### Not covered

- AccountDelete is disabled on testnet. It was refused by the network, not by the hook.
- A real EmitFailure (a payout Remit that never reaches any ledger) could not be made from a normal account. With `fix20261005` the box is restored in that case. This follows from the node code. It was not seen on chain.
- Mainnet: no tests.
