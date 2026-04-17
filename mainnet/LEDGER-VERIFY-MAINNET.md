# XEQ Swap — Mainnet Ledger Verification

| Field | Value |
|---|---|
| Network | Mainnet |
| Swap window close block | 1,806,294 |
| Records | 1,374 |
| SHA256 (canonical unredacted ledger) | 042b8be05b1c7af6ea9a0ffab7273e5623d0b7ad8d5a3fd01bb8aab1c3a77087 |
| Legacy total (atomic) | 2767865413247 |
| New total (atomic) | 276786541324700000 |
| Finalized | April 16, 2026 |

## Public ledger

`ledger-public.tsv` is the community-visible file. The `new_address` and `new_txid`
columns are redacted to protect recipient privacy on the new chain.

The `amount_atomic` (legacy) and `new_amount_atomic` (new chain) columns are fully
visible and can be summed to verify the total supply conversion.

## Canonical ledger

The unredacted ledger is held offline by the Equilibria core team. The SHA256 above
is the hash of the canonical unredacted file. Individual participants may request
verification of their own swap record by txid.

## Verification

```bash
# Sum of all new_amount_atomic values in the public ledger should equal 276786541324700000
awk -F'\t' 'NR>1 {sum += $6} END {print sum}' ledger-public.tsv
```

## Revision note

The initial ledger finalization on April 16, 2026 produced SHA256
`d0df0ff0cbc134be142486cb9e93fef65b8f47ae8cdba52cb7b7a164d5782652` with 1,373 records.
One swap (txid `86db6535ff86393eb657309ed14e12311da32c2f3ee5c0839f4c80653bbb6da8`,
height 1,783,873, 21,551 XEQM) was excluded due to a database bug where its block
height was not recorded (height=0) despite having 23,314 confirmations and a valid
tx proof. The height was corrected from the chain, the ledger was re-finalized to
include all 1,374 verified swaps, and this document updated accordingly.
