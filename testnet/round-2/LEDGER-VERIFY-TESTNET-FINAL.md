# XEQ Testnet Swap — Ledger Verification (Final)

## Why This Matters

The ledger file is the **single source of truth** for the entire testnet swap. It contains every verified swap — the legacy XEQ deposited, the new wallet address, and the exact amount to be paid out on the new chain.

The SHA256 hash is a cryptographic fingerprint of this file. If even a single character in the file were changed — a recipient address, an amount, a single digit — the hash would be completely different. This means:

- **You can independently verify** that the ledger has not been tampered with since finalization
- **No trust in the Equilibria team is required** — the math does not lie
- **The hash is permanently recorded** on the public API and cannot be changed after finalization
- **Your payout amount is locked** — exactly what is in this file is what you will receive

We strongly encourage every swap participant to download the ledger file and verify the hash themselves. This is not optional ceremony — it is the mechanism that makes the swap trustless.

---

## Swap Window

| Field | Value |
|-------|-------|
| Opening Block Height | 1,776,093 |
| Closing Block Height | 1,778,253 |
| Duration | 2,160 legacy blocks (~3 days at ~2 minutes per block) |
| Chain | Legacy XEQ (all block heights refer to the legacy chain) |
| Swap Ratio | 1 legacy XEQ = 1 new XEQ |

---

## Published SHA256

```
b04ad9a9cfd958d8261eb8e6c996bd11db0080af0302525e7362687844efe3ac
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
b04ad9a9cfd958d8261eb8e6c996bd11db0080af0302525e7362687844efe3ac
```

If it matches — the ledger is authentic and unmodified. Your entry is exactly as recorded at finalization.

If it does not match — do not proceed. Contact the Equilibria team immediately in the Telegram channel. A mismatch means the file has been altered since finalization.

### Step 4 — Verify via the public API
The hash is also permanently stored on the swap server and accessible to anyone:

```bash
curl -s https://swap-testnet.xeqlabs.com/api/public/ledger | python3 -m json.tool
```

The `ledger_sha256` field in the response must match the published hash above. This gives you a second independent source to compare against — the file and the hash on the server must agree.

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
| `amount_atomic` | Legacy XEQ you deposited (divide by 10,000 for XEQ) |
| `new_address` | Your new chain wallet address |
| `new_amount_atomic` | New XEQ you will receive (divide by 1,000,000,000 for XEQ) |
| `new_txid` | New chain payout transaction ID (populated after payout) |
| `status` | Status at finalization (1 = Verified, 2 = Paid) |

---

## Swap Summary

| Field | Value |
|-------|-------|
| Total Verified Swaps | 64 |
| Legacy XEQ Deposited | 6,450,720 atomic units (645.072 XEQ) |
| New XEQ to be Minted | 645,072,000,000 atomic units (645.072 new XEQ) |
| Swap Ratio | 1 legacy XEQ = 1 new XEQ |
| Finalized | 2026-03-07 |
| Environment | Testnet (Final Round) |

---

## Payout Sequence

The order of operations is fixed and non-negotiable. The ledger is always finalized and published **before** any payouts are sent. This is the guarantee that makes the swap trustless.

1. Swap window closed at block 1,778,253 ✅
2. Ledger finalized — TSV written, SHA256 computed and locked ✅
3. SHA256 published to community ✅
4. Community verification period — participants verify the ledger
5. Payouts enabled — new XEQ sent to all verified recipients
6. Payout transaction IDs recorded in ledger as they complete

The ledger is the **promise**. Payouts are the **fulfillment** of that promise.

---

## Transparency Commitments

The Equilibria team is committed to a level of post-swap transparency that is rare in the cryptocurrency industry. The following actions will be taken after payouts are complete.

### 1. Deposit Wallet View Key — Published at Swap Open

The private view key of the legacy XEQ deposit wallet was published at the opening of the swap window. This allows any participant to independently verify every inbound transaction to the deposit address and confirm all deposits are accounted for in this ledger.

### 2. Deposit Wallet — Sweep to RIP Wallet

After all payouts are confirmed, every legacy XEQ in the deposit wallet will be swept in a single transaction to a permanently retired address — the RIP wallet. The following will be published within 72 hours of the sweep:

| Item | Description |
|------|-------------|
| RIP Wallet Address | The destination address of the sweep |
| Inbound Transaction ID | Verifiable on any block explorer |
| RIP Wallet Private View Key | Allows anyone to independently verify the balance and transaction history |

### 3. Deposit Wallet Spend Key — Released Within 72 Hours of Sweep

Within 72 hours of the sweep to the RIP wallet being confirmed, the **spend key of the legacy XEQ deposit wallet** will be published. With the spend key, anyone can open the deposit wallet and verify the complete transaction history independently.

### 4. Distribution Wallet Spend Key — Released Within 24 Hours of Final Payout

Within 24 hours of the last payout being confirmed on the new chain, the **spend key of the distribution wallet** will be published. With the spend key, anyone can verify every individual payout matches this ledger and confirm the wallet balance is zero.

### 5. PGP Signed Statements

All spend key releases will be accompanied by a PGP signed statement published to [vellitas/xeq-swap-docs](https://github.com/vellitas/xeq-swap-docs).

---

## Why This Matters — End-to-End Verifiability

When all of the above steps are complete, any community member can independently verify the entire swap:

1. **Deposit wallet** — open it with the spend key, see every deposit
2. **Ledger** — verify the SHA256, confirm every deposit is recorded correctly
3. **RIP wallet** — verify via view key that the deposit wallet was fully swept and retired
4. **Distribution wallet** — open it with the spend key, verify every payout matches this ledger
5. **Block explorer** — verify every transaction independently on chain

No step requires trusting the Equilibria team. This level of transparency — publishing spend keys for both the deposit and distribution wallets after a token swap — is exceptionally rare in the cryptocurrency industry.

---

## Questions or Discrepancies

If your TXID is not in the ledger and you believe your swap was valid, or if any amount appears incorrect, contact the Equilibria team in the Telegram channel **before payouts are sent**. Once payouts are processed, the ledger is final.

---

*Equilibria Token Swap — Testnet Ledger Verification (Final Round) v1.0 | 2026-03-07*
