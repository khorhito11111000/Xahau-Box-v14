# Transactions

All box commands are `Invoke` transactions with hook parameters. Every name and value is hex, upper case, no `0x`.

The examples use testnet, `NetworkID 21338`. Xahau mainnet is `21337`. Replace every `<...>` with your own value. These are placeholders, not real keys. Never paste a seed or a private key into a transaction.

Paste the JSON into a wallet or a tool that takes raw JSON (for example xrplwin → raw transaction), and sign it there. Fee and Sequence are filled by the tool.

The hook answers with a text in `HookReturnString`, like `tgs: packed`. A rejected command is `tecHOOK_REJECTED` with a `tgs: ...` reason.

## Parameters

| Name | Hex name | Size | Meaning |
|---|---|---|---|
| CMD | `434D44` | text | `PACK` `5041434B`, `CLAIM` `434C41494D`, `OPEN` `4F50454E`, `AUDIT` `4155444954`, `SYNC` `53594E43` |
| VC | `5643` | 32 bytes | Box number. Key link: hash of the box public key. Key token: any 32 bytes the creator picks |
| TID | `544944` | 32 bytes | URIToken in the box, or 32 zero bytes for none |
| AMT | `414D54` | 8 bytes | XAH in drops, or 8 zero bytes for none |
| EXP | `455850` | 4 bytes | Ledger number of the time limit. Leave it out, or `00000000`, for no limit |
| MODE | `4D4F4445` | 1 byte | `01` = key token. Leave it out for key link |
| KTID | `4B544944` | 32 bytes | The key URIToken, only with MODE `01` |
| PK | `504B` | 33 bytes | Box public key, only in a key-link CLAIM |
| SIG | `534947` | 64–72 bytes | Signature for the opener's address, only in a key-link CLAIM. See [SIGNING.md](SIGNING.md) |

Who sends what:
- AUDIT, PACK and OPEN are sent by the creator, from the hook account, with no Destination.
- CLAIM is sent by the opener, with `Destination` = the hook account.
- SYNC can be sent by anyone, with `Destination` = the hook account.

## AUDIT

Needed before PACK. The hook counts the objects on the account so it knows what is locked. If the hook answers `tgs: audit more`, send the same AUDIT again until it says `tgs: audited`. If PACK answers `tgs: audit`, the account has new objects: send AUDIT again.

```json
{
  "TransactionType": "Invoke",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "4155444954"
      }
    }
  ]
}
```

## PACK, key link

The creator makes a new key pair for the box. VC is the SHA-512Half of the 33-byte box public key. The private key is the box key. Keep it off chain.

The same PACK is used for a bearer box and for a named box. The difference is only who gets what afterwards:
- **Bearer.** The creator gives out the box key. Whoever has it opens.
- **Named address.** The creator keeps the box key, signs for the recipient's address, and gives out only the public key and that signature. Only that address can open.

XAH and a token:

```json
{
  "TransactionType": "Invoke",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "5041434B"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<VC_FROM_BOX_PUBLIC_KEY>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "544944",
        "HookParameterValue": "<GIFT_URITOKEN_ID>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "414D54",
        "HookParameterValue": "<AMOUNT_DROPS_8_BYTES>"
      }
    }
  ]
}
```

Only a token (AMT is zero):

```json
{
  "TransactionType": "Invoke",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "5041434B"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<VC_FROM_BOX_PUBLIC_KEY>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "544944",
        "HookParameterValue": "<GIFT_URITOKEN_ID>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "414D54",
        "HookParameterValue": "0000000000000000"
      }
    }
  ]
}
```

Only XAH, with a time limit (TID is zero):

```json
{
  "TransactionType": "Invoke",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "5041434B"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<VC_FROM_BOX_PUBLIC_KEY>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "544944",
        "HookParameterValue": "0000000000000000000000000000000000000000000000000000000000000000"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "414D54",
        "HookParameterValue": "<AMOUNT_DROPS_8_BYTES>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "455850",
        "HookParameterValue": "<EXP_LEDGER_4_BYTES>"
      }
    }
  ]
}
```

## PACK, key token

`MODE 01` and `KTID` bind the box to a key URIToken. VC is any 32-byte box number: not all zero, and not used by another box.

The key token must exist, must not be burnable, must not be in this box or another box, and must not be the same token as TID.

```json
{
  "TransactionType": "Invoke",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "5041434B"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<BOX_ID_32_BYTES>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "4D4F4445",
        "HookParameterValue": "01"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "4B544944",
        "HookParameterValue": "<KEY_URITOKEN_ID>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "544944",
        "HookParameterValue": "<GIFT_URITOKEN_ID>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "414D54",
        "HookParameterValue": "<AMOUNT_DROPS_8_BYTES>"
      }
    }
  ]
}
```

Only XAH, with a time limit:

```json
{
  "TransactionType": "Invoke",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "5041434B"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<BOX_ID_32_BYTES>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "4D4F4445",
        "HookParameterValue": "01"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "4B544944",
        "HookParameterValue": "<KEY_URITOKEN_ID>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "544944",
        "HookParameterValue": "0000000000000000000000000000000000000000000000000000000000000000"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "414D54",
        "HookParameterValue": "<AMOUNT_DROPS_8_BYTES>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "455850",
        "HookParameterValue": "<EXP_LEDGER_4_BYTES>"
      }
    }
  ]
}
```

Answers you may see at PACK: `tgs: packed`, `tgs: audit`, `tgs: not enough`, `tgs: exists`, `tgs: locked`, `tgs: empty`, `tgs: exp past`, `tgs: mode`, `tgs: ktid`, `tgs: ktid=tid`, `tgs: ktid locked`, `tgs: token offer`, `tgs: burnable`, `tgs: owner`.

## CLAIM, key link

Sent by the opener. PK is the box public key. SIG is the box key's signature for the opener's own address. A signature made for one address does not work from another (`tgs: bad sig`).

```json
{
  "TransactionType": "Invoke",
  "Account": "<OPENER>",
  "Destination": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "434C41494D"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "504B",
        "HookParameterValue": "<BOX_PUBLIC_KEY_33_BYTES>"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "534947",
        "HookParameterValue": "<SIGNATURE_FOR_OPENER>"
      }
    }
  ]
}
```

## CLAIM, key token

Sent by the account that holds the key token right now. Anyone else gets `tgs: not holder`.

```json
{
  "TransactionType": "Invoke",
  "Account": "<KEY_TOKEN_HOLDER>",
  "Destination": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "434C41494D"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<BOX_ID_32_BYTES>"
      }
    }
  ]
}
```

After `tgs: claiming` the hook sends a Remit with the contents to the opener. Wait for it (about 10–20 seconds) before the next command on the same box, or you get `tgs: busy`.

Other answers: `tgs: unknown` (no such box, or already opened), `tgs: expired`, `tgs: key box` (a key-link box opened as a key-token box), `tgs: token box` (the other way), `tgs: reserve` (the account has less XAH than locked; wait for a top-up).

## OPEN (take back)

Sent by the creator.
- A box with a time limit: only after the limit ledger. Before it: `tgs: not expired`.
- A box with no time limit: refused (`tgs: not expired`). The one case it works: a key-token box whose key token was burned.

```json
{
  "TransactionType": "Invoke",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "4F50454E"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<VC_OR_BOX_ID>"
      }
    }
  ]
}
```

## SYNC

A rare helper. If a payout Remit did not land in a ledger, SYNC lets the hook finish or restore the box. Without VC it reports the hook's state.

```json
{
  "TransactionType": "Invoke",
  "Account": "<ANY_ACCOUNT>",
  "Destination": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "HookParameters": [
    {
      "HookParameter": {
        "HookParameterName": "434D44",
        "HookParameterValue": "53594E43"
      }
    },
    {
      "HookParameter": {
        "HookParameterName": "5643",
        "HookParameterValue": "<VC_OR_BOX_ID>"
      }
    }
  ]
}
```

## Key token: give, sell, pass on

These are normal URIToken transactions. The hook does not take part. The right to open moves with the token.

Mint a key token (do not set the burnable flag):

```json
{
  "TransactionType": "URITokenMint",
  "Account": "<HOOK_ACCOUNT>",
  "NetworkID": 21338,
  "URI": "<URI_HEX>"
}
```

Offer it to one buyer (price 0 for a gift):

```json
{
  "TransactionType": "URITokenCreateSellOffer",
  "Account": "<SELLER>",
  "NetworkID": 21338,
  "URITokenID": "<KEY_URITOKEN_ID>",
  "Amount": "<PRICE_IN_DROPS>",
  "Destination": "<BUYER>"
}
```

Buy it:

```json
{
  "TransactionType": "URITokenBuy",
  "Account": "<BUYER>",
  "NetworkID": 21338,
  "URITokenID": "<KEY_URITOKEN_ID>",
  "Amount": "<PRICE_IN_DROPS>"
}
```

Or just hand it on:

```json
{
  "TransactionType": "Remit",
  "Account": "<KEY_TOKEN_HOLDER>",
  "NetworkID": 21338,
  "Destination": "<NEW_HOLDER>",
  "URITokenIDs": [
    "<KEY_URITOKEN_ID>"
  ]
}
```
