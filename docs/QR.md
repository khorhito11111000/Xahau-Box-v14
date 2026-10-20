# QR codes

QR is not a hook feature. The hook does not read QR codes. It only checks the signed transaction.

A QR is one way to carry the key link or the box number. A wallet, an xApp or a site can make it and read it. This is not built here. Below are the options and the format used in the tests.

## Options

1. **QR with the box key (bearer).** Whoever scans it opens the box. It is a secret. Share it only in private.
2. **QR for a named box.** It holds the box public key and a signature for one address. Only that address can open. Safe to show.
3. **QR on the key token.** The box number sits on the image of the key token. Anyone can scan it. Only the current holder of the token can open. The QR moves with the token.
4. **QR on a gift token inside the box.** The box holds a token with a QR, for example a link to a gift card, a subscription or a shop item. The recipient uses it after the box is opened. That part is outside the hook. The shop or site has to accept it.
5. **Combined.** The box opens with one QR. Inside lies a token with another QR.

**Rule.** A QR with the key inside is a secret. A QR with only the box number, for a named box or a key-token box, is not. There the protection is the signature for the right address, or holding the token.

## Format used in the tests

Not a standard. Only an example. The text is `tgs1:` and then base64url of a JSON object.

Bearer (secret):

```json
{
  "v": 14,
  "h": "<HOOK_ACCOUNT>",
  "b": "<BOX_PRIVATE_KEY>"
}
```

Bearer with a gift code (secret). The gift token's URI holds an encrypted code. `k` is the key to it:

```json
{
  "v": 14,
  "h": "<HOOK_ACCOUNT>",
  "b": "<BOX_PRIVATE_KEY>",
  "k": "<GIFT_CODE_KEY>",
  "t": "<GIFT_URITOKEN_ID>"
}
```

Named address (safe to show):

```json
{
  "v": 14,
  "h": "<HOOK_ACCOUNT>",
  "a": "<RECIPIENT>",
  "p": "<BOX_PUBLIC_KEY>",
  "s": "<SIGNATURE_FOR_RECIPIENT>"
}
```

Key token (safe to show):

```json
{
  "v": 14,
  "h": "<HOOK_ACCOUNT>",
  "c": "<BOX_ID>"
}
```

For a bearer QR, the opening app reads EXP, TID, AMT and NONCE of the box from the hook state, then signs for the scanning account. See [SIGNING.md](SIGNING.md).

## Remarks on a key token

The Remarks amendment is enabled on testnet and mainnet. Only the token issuer can set Remarks. So an issuer could write the box number, the amount or the time limit on a key token, for wallets to show. This was tested only as a separate check, not as part of the hook.
