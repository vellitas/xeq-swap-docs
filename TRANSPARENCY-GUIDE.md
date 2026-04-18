# Equilibria (XEQ) Token Swap
## Community Transparency Guide
**Version 3.1 | Genesis 2026**

---

## 1. Overview

During the XEQ token swap, legacy XEQ coins are sent to a dedicated swap deposit address. This address is monitored publicly via a published view key, providing full community transparency over all incoming deposits.

After the swap window closes, the deposit ledger is finalized with a SHA256 hash and published for community verification. New XEQ is minted to match the exact verified total and paid out to participants' new chain wallet addresses.

The spend keys for all wallets involved in the swap are published after the swap is complete. This allows any community member to independently verify the entire swap — from deposit to payout — without trusting the Equilibria team.

---

## 2. Mainnet Swap Deposit Address

| Field | Value |
|-------|-------|
| **Address** | `Tw1AmWavnFtPT4rgfwt7seYHsLW1vPE7wR8s5BBtu7NZCK31H9jWy6r7m4iZ8APaLDfzGvrgqU9fW3BLjKKiHBw82EntBGmLn` |
| **View Key** | `d1c95300b1e473d0a4ac6a81adbc42a5fe432de54b5e71c72fae73e668d88701` |
| **Chain** | Legacy Equilibria (XEQ) — Mainnet |
| **Spend Key** | `67b4883bd1baa7059fcd5282c8b691a73cfd65d466d6fc4ee325682db6d25b0b` — PGP signed release: `mainnet/xeq-deposit-spendkey-release.txt.asc` |

The view key is published here and at swap window open. Any community member can create a view-only wallet and monitor every incoming transaction in real time.

---

## 3. Genesis Wallet

The genesis wallet receives the single mint transaction that creates all new XEQ equivalent to the verified swap total. It was created on an air-gapped machine and the spend key has never touched an internet-connected device.

| Field | Value |
|-------|-------|
| **Address** | `XEQMCkEATxY2RKfr16ecv7Z6QnEtiQ7wdPyBKYc3fNBs6eH2uxCm3Mh1Uauw9iV4uVggHi6i8g7wXgUXHCZXLW3f8Q83szFhHr` |
| **View Key** | `8d6e731705e78ac4efdb0f2b40ff40414317ddd25529c65276959836137a2104` |
| **Chain** | New Equilibria (XEQ) — Mainnet |
| **Spend Key** | Will be published within 72 hours of genesis mint confirmation — PGP signed to `vellitas/xeq-swap-docs` |

The view key is published above and in `mainnet/xeq-mainnet-genesis-viewkey-release.txt.asc`. Any community member can create a view-only wallet and watch the genesis mint arrive in real time.

---

## 4. Transparency Guarantees

- **Full deposit visibility** — the view key is published above. Any community member can monitor every incoming transaction with zero spending capability.

- **Public dashboard** — all deposits are visible on the XEQ Swap Tracker Dashboard at [https://swap-tracker.xeqlabs.com/](https://swap-tracker.xeqlabs.com/) via API endpoints that query the deposit wallet directly.

- **Finalized ledger** — the swap ledger is locked with a SHA256 hash before any payouts are sent. The hash is published to this repository and permanently recorded on the public API. Payouts match the ledger exactly.

- **Spend key releases** — after the swap is complete, the private spend keys for the deposit wallet, genesis wallet, and distribution wallet are published here, PGP signed by the XEQLabs team. Anyone can open any wallet and verify the complete transaction history independently.

- **Chain shutdown** — the legacy Equilibria chain will be permanently shut down shortly after the swap window closes. After this point, no transactions can be made on the legacy chain regardless of who holds any spend key.

- **GitHub lock** — the Equilibria GitHub repository will be locked after shutdown. No further forks or client downloads will be possible.

---

## 5. How to Monitor Deposits

Any community member can independently verify all deposits using the published view key. No trust in the Equilibria team is required.

### 5.1 Create a View-Only Wallet

```bash
wallet-cli --generate-from-view-key /tmp/xeq-swap-monitor \
  --restore-height <swap_start_height>
# Enter the deposit address when prompted
# Enter the view key when prompted
```

Use the swap opening block height as `--restore-height` to capture all deposits from the start of the swap window. The current legacy chain height is visible at [https://swap-tracker.xeqlabs.com/](https://swap-tracker.xeqlabs.com/).

### 5.2 Sync and View Incoming Transactions

```
# Inside wallet-cli after syncing:
refresh
show_transfers in
```

### 5.3 Via the Public API

```bash
# Summary — total deposited, deposit count, sync status
curl -s https://swap.xeqlabs.com/api/public | python3 -m json.tool

# Finalized ledger hash
curl -s https://swap.xeqlabs.com/api/public/ledger | python3 -m json.tool
```

---

## 6. Spend Key Release Schedule

| Wallet | Released | Condition |
|--------|----------|-----------|
| Deposit wallet | ✅ 2026-04-15 | Sweep to RIP wallet confirmed — see `mainnet/xeq-deposit-spendkey-release.txt.asc` |
| Genesis wallet | Pending | After genesis mint confirmed on-chain |
| Distribution wallet | Pending | After last payout confirmed |

All releases are signed with the XEQLabs PGP key — see `pgp/xeqlabs-pgp-public-key.asc` in this repository.

---

## 7. What Happens to the Funds

All legacy XEQ deposited to the swap address is counted toward the total supply verified for the new chain genesis mint.

1. Swap window closes
2. Ledger finalized — SHA256 hash locked and published
3. Community verification period
4. Genesis mint on new chain — exact verified total sent to genesis wallet
5. Payouts sent from distribution wallet to all verified recipients
6. Deposit wallet swept to community engagement wallet
7. Spend keys published — full independent verification possible by anyone
8. Legacy chain shut down
9. Equilibria GitHub locked

---

## 8. Why No Cryptographic Burn Address

True cryptographically unspendable addresses (such as Bitcoin's `OP_RETURN` outputs) do not exist on Monero-fork chains including Equilibria. The transparency model for this swap relies instead on:

1. **Published view keys** — full public visibility of all deposits
2. **Public API** — real-time deposit data available to anyone
3. **Published spend keys** — post-swap independent verification by anyone
4. **Chain shutdown** — legacy network permanently closed after the swap window
5. **GitHub lock** — no ability to fork or continue the legacy chain after shutdown

---

## 9. Testnet Swap Records

The testnet swap was conducted in two rounds prior to the mainnet swap. Full ledger verification documents, TSV ledger files, and PGP signed spend key releases for both rounds are available in the `testnet/` directory of this repository.

| Round | Ledger Verification | SHA256 |
|-------|-------------------|--------|
| Round 1 | [testnet/round-1/LEDGER-VERIFY-ROUND-1.md](testnet/round-1/LEDGER-VERIFY-ROUND-1.md) | `4697817ff88f80bef8340e42d03619a8cd675baa51a0e89f36bb5442f0bfa2eb` |
| Round 2 | [testnet/round-2/LEDGER-VERIFY-ROUND-2.md](testnet/round-2/LEDGER-VERIFY-ROUND-2.md) | `b04ad9a9cfd958d8261eb8e6c996bd11db0080af0302525e7362687844efe3ac` |

---

*Equilibria Token Swap — Community Transparency Guide v3.1 | Genesis 2026*
