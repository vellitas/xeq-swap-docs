# XEQ Testnet Swap — Ledger Verification

## Why This Matters

The ledger file is the **single source of truth** for the entire testnet swap. It contains every verified swap — the legacy XEQ deposited, the new wallet address, and the exact amount to be paid out on the new chain.

The SHA256 hash is a cryptographic fingerprint of this file. If even a single character in the file were changed — a recipient address, an amount, a single digit — the hash would be completely different. This means:

- **You can independently verify** that the ledger has not been tampered with since finalization
- **No trust in the Equilibria team is required** — the math does not lie
- **The hash is permanently recorded** on the public API and cannot be changed after finalization
- **Your payout amount is locked** — exactly what is in this file is what you will receive

We strongly encourage every swap participant to download the ledger file and verify the hash themselves. This is not optional ceremony — it is the mechanism that makes the swap trustless.

---

## Published SHA256
```
4697817ff88f80bef8340e42d03619a8cd675baa51a0e89f36bb5442f0bfa2eb
```

## Ledger File
[xeq-testnet-swap-ledger.tsv](xeq-testnet-swap-ledger.tsv)

---

## Step-by-Step Verification

### Step 1 — Download the ledger file
Click `xeq-testnet-swap-ledger.tsv` above and download it to your computer.

### Step 2 — Compute the SHA256 hash

**Linux / Mac:**
```bash
sha256sum xeq-testnet-swap-ledger.tsv
```

**Windows (PowerShell):**
```powershell
Get-FileHash xeq-testnet-swap-ledger.tsv -Algorithm SHA256
```

### Step 3 — Compare character by character
The output must be **exactly**:
```
4697817ff88f80bef8340e42d03619a8cd675baa51a0e89f36bb5442f0bfa2eb
```

If it matches — the ledger is authentic and unmodified. Your entry is exactly as recorded at finalization.

If it does not match — do not proceed. Contact the Equilibria team immediately in the Telegram channel. A mismatch means the file has been altered since finalization.

### Step 4 — Verify via the public API
The hash is also permanently stored on the swap server and accessible to anyone:

```bash
curl -s https://swap-testnet.xeqlabs.com/api/public/ledger | python3 -m json.tool
```

The `sha256` field in the response must match the published hash above. This gives you a second independent source to compare against — the file on GitHub and the hash on the server must agree.

---

## Find Your Swap in the Ledger

Search for your legacy transaction ID in the ledger file:

**Linux / Mac:**
```bash
grep <your_legacy_txid> xeq-testnet-swap-ledger.tsv
```

**Windows (PowerShell):**
```powershell
Select-String -Path xeq-testnet-swap-ledger.tsv -Pattern "<your_legacy_txid>"
```

Your row will show:
- Your legacy deposit amount
- Your new chain wallet address
- The exact new XEQ amount you will receive
- Your payout transaction ID once payouts are complete

You can also check your swap status directly:
```bash
curl -s https://swap-testnet.xeqlabs.com/api/swap/<your_legacy_txid> | python3 -m json.tool
```

---

## Ledger Format

Tab-separated values with the following columns:

| Column | Description |
|--------|-------------|
| `txid` | Your legacy chain transaction ID |
| `height` | Block height when your deposit was confirmed |
| `timestamp` | Unix timestamp of your deposit |
| `amount_atomic` | Legacy XEQ you deposited (divide by 1,000,000,000 for XEQ) |
| `new_address` | Your new chain wallet address |
| `new_amount_atomic` | New XEQ you will receive (divide by 1,000,000,000 for XEQ) |
| `new_txid` | New chain payout transaction ID (populated after payout) |
| `status` | Status at finalization (1 = Verified, 2 = Paid) |

---

## Swap Summary

| Field | Value |
|-------|-------|
| Total Verified Swaps | 114 |
| Legacy XEQ Deposited | 16,884,936 atomic units (16.884936 XEQ) |
| New XEQ to be Minted | 1,688,493,600,000 atomic units (1,688.4936 new XEQ) |
| Swap Ratio | 1 legacy XEQ = 100 new XEQ |
| Finalized | 2026-03-01 |
| Environment | Testnet |

---

## What Happens Next

1. The Equilibria team mints the exact `new_total` amount on the new chain in a single genesis transaction
2. Payouts are sent individually to each recipient's new chain wallet address as recorded in this ledger
3. Once your payout is sent, the `new_txid` column in the ledger will be updated with your new chain transaction ID
4. You can verify receipt in your new chain wallet

The ledger file itself is the contract. What is written here is what will be paid. Nothing more, nothing less.

---

## Questions or Discrepancies

If your TXID is not in the ledger and you believe your swap was valid, or if any amount appears incorrect, contact the Equilibria team in the Telegram channel **before payouts are sent**. Once payouts are processed, the ledger is final.

---

*Equilibria Token Swap — Testnet Ledger Verification Guide v1.0 | Genesis 2026*
