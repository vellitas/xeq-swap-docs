# XEQ Testnet Swap — Round 2 Ledger Verification

## Why This Matters

The ledger file is the **single source of truth** for the Round 2 testnet swap. It contains every verified swap — the legacy XEQ deposited, the new wallet address, and the exact amount paid out on the new chain.

The SHA256 hash is a cryptographic fingerprint of this file. If even a single character were changed — a recipient address, an amount, a single digit — the hash would be completely different. This means:

- **You can independently verify** that the ledger has not been tampered with since finalization
- **No trust in the Equilibria team is required** — the math does not lie
- **The hash is permanently recorded** on the public API and cannot be changed after finalization
- **Your payout amount is locked** — exactly what is in this file is what you received

---

## Swap Window

| Field | Value |
|-------|-------|
| Opening Block Height | 1,776,093 |
| Closing Block Height | 1,778,253 |
| Duration | 2,160 legacy blocks (~3 days) |
| Chain | Legacy XEQ |
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

If it matches — the ledger is authentic and unmodified.

If it does not match — contact the Equilibria team immediately in the Telegram channel.

### Step 4 — Verify via the public API

```bash
curl -s https://swap-testnet.xeqlabs.com/api/public/ledger | python3 -m json.tool
```

The `ledger_sha256` field must match the published hash above.

---

## Find Your Swap in the Ledger

**Linux / Mac:**
```bash
grep <your_legacy_txid> xeq-testnet-swap-ledger.tsv
```

**Windows (PowerShell):**
```powershell
Select-String -Path xeq-testnet-swap-ledger.tsv -Pattern "<your_legacy_txid>"
```

---

## Ledger Format

| Column | Description |
|--------|-------------|
| `txid` | Your legacy chain transaction ID |
| `height` | Block height when your deposit was confirmed |
| `timestamp` | Unix timestamp of your deposit |
| `amount_atomic` | Legacy XEQ deposited (divide by 10,000 for XEQ) |
| `new_address` | Your new chain wallet address |
| `new_amount_atomic` | New XEQ received (divide by 1,000,000,000 for XEQ) |
| `new_txid` | New chain payout transaction ID |
| `status` | 1 = Verified, 2 = Paid |

---

## Swap Summary

| Field | Value |
|-------|-------|
| Total Verified Swaps | 64 |
| Legacy XEQ Deposited | 6,450,720 atomic units (645.072 XEQ) |
| New XEQ Paid Out | 645,072,000,000 atomic units (645.072 new XEQ) |
| Swap Ratio | 1:1 confirmed |
| Finalized | 2026-03-07 |
| Environment | Testnet (Round 2 — Final) |

---

## About the Donated XEQ

This was a donation-based testnet participation swap. Participants were informed before sending:

- Only send an amount you are comfortable donating
- Mainnet XEQ sent will not be returned

All legacy XEQ deposited during Round 2 has been swept to the **Community Engagement Wallet** and will be used for engagement initiatives, ecosystem growth campaigns, and community contests.

**Deposit Wallet Address:**
`TvzqHiqSv5QUrxAc4gtaiwHmUB45PnvzJG4Xtg8jfrsr6qiBUa8zUPuZyBjbq6gjhXB18P4qwxEMgSFeZ8RNF7RN2CQEmkn97`

**Community Engagement Wallet:**
`Tw1NGFkVncSUtoi7GEw5xpAfk3Z3mh2EBTLNJnvego3EgU4VoSK2apyJaJPBgEV9p39iHpEb6xsj3SaU2NZDM67U2x3HS3qZn`

The deposit wallet spend key has been published and PGP signed — see `xeq-testnet-deposit-spendkey-release.txt.asc` in this directory. Any community member can open the deposit wallet and verify the complete transaction history independently.

---

*Equilibria Token Swap — Testnet Round 2 Ledger Verification v1.0 | 2026-03-07*
