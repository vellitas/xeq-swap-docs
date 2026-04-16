# XEQ Swap — Mainnet Ledger Verification

| Field | Value |
|---|---|
| Network | Mainnet |
| Swap window close block | 1,806,294 |
| Records | 1,373 |
| SHA256 (canonical unredacted ledger) | d0df0ff0cbc134be142486cb9e93fef65b8f47ae8cdba52cb7b7a164d5782652 |
| Legacy total (atomic) | 2767649903247 |
| New total (atomic) | 276764990324700000 |
| Finalized | April 2026 |

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
# Sum of all new_amount_atomic values in the public ledger should equal 276764990324700000
awk -F'\t' 'NR>1 {sum += $6} END {print sum}' ledger-public.tsv
```
