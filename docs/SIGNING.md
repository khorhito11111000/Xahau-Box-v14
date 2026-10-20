# Key-link signature

A key-link box is opened with a signature. The signature is made with the box key, off chain, for one opener address. It is not a transaction. Nothing is sent when you sign.

## What is signed

Build a 124-byte message, in this order:

| Bytes | Field |
|---|---|
| 20 | opener's account ID (the address that will send CLAIM) |
| 32 | VC, the box number |
| 20 | hook account ID |
| 4 | NetworkID, big-endian (21338 testnet, 21337 mainnet) |
| 4 | EXP of the box, or `00000000` |
| 32 | TID of the box, or 32 zero bytes |
| 8 | AMT of the box in drops, big-endian |
| 4 | NONCE of the box, big-endian |

Take the SHA-512Half of the message (the first 32 bytes of SHA-512). Sign those 32 bytes with the box private key. Put the signature in SIG and the 33-byte box public key in PK.

Account IDs are the 20 bytes behind an r-address.

## NONCE

The hook gives every successful PACK on the account a new number, one higher than the last. It is stored with the box. So a signature for an old box does not work after the same key is packed again (tested: manual test 41, automated X5).

The creator knows the number by counting PACKs on the hook account, or reads it from the box record in the hook state.

## What the signature is bound to

Change any field and the hook answers `tgs: bad sig`. This was tested for: another NetworkID, another hook account, another EXP, TID, AMT, NONCE ±1, another opener, another key (automated X3). The same key on a second hook does not work either (X4).

## Bearer and named

- **Bearer.** The holder of the box key signs for their own address and sends CLAIM.
- **Named.** The creator signs for the recipient's address and gives only PK and SIG. A copy of that CLAIM from another address fails.

## Keys

Tested with secp256k1 keys (DER signature). The hook takes a 33-byte public key and a signature of 64 to 72 bytes.

Keep the box private key off chain and off public pages. Whoever has it can open a bearer box. It can also open a named box for any address.
