# Limitations

These are not bugs found in tests. They are things the hook cannot promise. Know them before you use a box.

## The creator can delete the hook

SetHook does not pass through the hook. So the creator can remove the hook while a box is closed, and then move what was in it. This was done on testnet (escape2).

The recipient has to trust the creator. The only full guard is an account nobody can sign for (master key disabled, no regular key, no signer list). But then the creator cannot pack new boxes or take any box back either.

## Fees can push the balance below the locked XAH

The network charges a fee even for a transaction the hook rejects. So the creator can burn XAH on fees and bring the balance below what is locked in boxes (audit A10).

The hook never pays out part of a box. CLAIM answers `tgs: reserve` and waits. After the account is topped up, CLAIM works.

## The creator can take a box back early, if they hold the key

- A creator who knows the key of a key-link box can sign for a second account of their own and open the box before the limit.
- A creator who holds the key token, or gets it back, can pass it to a second account and open the box.

This is by design. The hook checks the key, not who the person is.

## A box with no time limit

OPEN is refused for a box with no time limit. The creator can take it back only with the key, as above, or, for a key-token box, after the key token is burned.

## The recipient can block the payout

If the recipient turns on RequireDest or blocks incoming Remit, the payout fails. The creator pays the fee of that payout. The box goes back to closed and can be opened again (audit A12, gaps G2).

## Remarks on a locked gift token

v14 lets SetRemarks pass for a locked token. Only the issuer of a token can set its Remarks. So if the box account itself issued the gift token, it can rewrite the gift's Remarks while the token is in a box. The token itself cannot move.

## fix20261005 is required

If a payout Remit with only XAH never lands in any ledger, the box must be restored. That works only with the amendment `fix20261005`. Without it, an XAH-only box would be marked as paid, and the recipient would get nothing. The XAH would stay with the creator.

`fix20261005` is enabled on testnet and on Xahau mainnet (checked 2026-10-20, ledger 26655259). Do not use the hook on a network without it.

## Not audited, testnet only

There was no external audit. All tests were on testnet. Use on mainnet at your own risk.

## No source

The hook source is not published. You can check which hook is on an account only by its HookHash:
`3CEEAB2F96E71E3AAEEA0C7220834B5F3D4643E82B1CB74B89B88A22C9722207`.

## Keys and QR

A bearer key or a bearer QR is a secret. Whoever has it opens the box first. Keeping keys, devices and QR images safe is the user's job.
