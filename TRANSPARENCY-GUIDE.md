# Equilibria (XEQ) Token Swap
## Community Transparency Guide
**Version 3.0 | Genesis 2026**

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
| **Spend Key** | Will be published within 72 hours of sweep confirmation — PGP signed to `vellitas/xeq-swap-docs` |

The view key is published here and at swap window open. Any community member can create a view-only wallet and monitor every incoming transaction in real time.

---

## 3. Transparency Guarantees

- **Full deposit visibility** — the view key is published above. Any community member can monitor every incoming transaction with zero spending capability.

- **Public dashboard** — all deposits are visible on the XEQ Swap Tracker Dashboard at [https://swap-tracker.xeqlabs.com/](https://swap-tracker.xeqlabs.com/) via API endpoints that query the deposit wallet directly.

- **Finalized ledger** — the swap ledger is locked with a SHA256 hash before any payouts are sent. The hash is published to this repository and permanently recorded on the public API. Payouts match the ledger exactly.

- **Spend key releases** — after the swap is complete, the private spend keys for the deposit wallet and distribution wallet are published here, PGP signed by the XEQLabs team. Anyone can open either wallet and verify the complete transaction history independently.

- **Chain shutdown** — the legacy Equilibria chain will be permanently shut down shortly after the swap window closes. After this point, no transactions can be made on the legacy chain regardless of who holds any spend key.

- **GitHub lock** — the Equilibria GitHub repository will be locked after shutdown. No further forks or client downloads will be possible.

---

## 4. How to Monitor Deposits

Any community member can independently verify all deposits using the published view key. No trust in the Equilibria team is required.

### 4.1 Create a View-Only Wallet

```bash
wallet-cli --generate-from-view-key /tmp/xeq-swap-monitor \
  --restore-height <swap_start_height>
# Enter the deposit address when prompted
# Enter the view key when prompted
```

Use the swap opening block height as `--restore-height` to capture all deposits from the start of the swap window. The current legacy chain height is visible at [https://swap-tracker.xeqlabs.com/](https://swap-tracker.xeqlabs.com/).

### 4.2 Sync and View Incoming Transactions

```
# Inside wallet-cli after syncing:
refresh
show_transfers in
```

### 4.3 Via the Public API

```bash
# Summary — total deposited, deposit count, sync status
curl -s https://swap.xeqlabs.com/api/public/burn | python3 -m json.tool

# Full transfer list
curl -s https://swap.xeqlabs.com/api/public/burn/transfers | python3 -m json.tool
```

---

## 5. Spend Key Release Schedule

After the swap is complete the following keys will be published to this repository as PGP signed statements:

| Wallet | Released | Condition |
|--------|----------|-----------|
| Deposit wallet | Within 72 hours | After sweep to community engagement wallet confirmed |
| Distribution wallet | Within 24 hours | After last payout confirmed |

All releases are signed with the XEQLabs PGP key — see `pgp/xeqlabs-pgp-public-key.asc` in this repository.

---

## 6. What Happens to the Funds

All legacy XEQ deposited to the swap address is counted toward the total supply verified for the new chain genesis mint.

1. Swap window closes
2. Ledger finalized — SHA256 hash locked and published
3. Community verification period
4. Genesis mint on new chain — exact verified total
5. Payouts sent to all verified recipients
6. Deposit wallet swept to community engagement wallet
7. Spend keys published — full independent verification possible
8. Legacy chain shut down
9. Equilibria GitHub locked

---

## 7. Why No Cryptographic Burn Address

True cryptographically unspendable addresses (such as Bitcoin's `OP_RETURN` outputs) do not exist on Monero-fork chains including Equilibria. The transparency model for this swap relies instead on:

1. **Published view keys** — full public visibility of all deposits
2. **Public API** — real-time deposit data available to anyone
3. **Published spend keys** — post-swap independent verification by anyone
4. **Chain shutdown** — legacy network permanently closed after the swap window
5. **GitHub lock** — no ability to fork or continue the legacy chain after shutdown

---

## 8. Testnet Swap Records

The testnet swap was conducted in two rounds prior to the mainnet swap. Full ledger verification documents, TSV ledger files, and PGP signed spend key releases for both rounds are available in the `testnet/` directory of this repository.

| Round | Ledger Verification | SHA256 |
|-------|-------------------|--------|
| Round 1 | [testnet/round-1/LEDGER-VERIFY-ROUND-1.md](testnet/round-1/LEDGER-VERIFY-ROUND-1.md) | `4697817ff88f80bef8340e42d03619a8cd675baa51a0e89f36bb5442f0bfa2eb` |
| Round 2 | [testnet/round-2/LEDGER-VERIFY-ROUND-2.md](testnet/round-2/LEDGER-VERIFY-ROUND-2.md) | `b04ad9a9cfd958d8261eb8e6c996bd11db0080af0302525e7362687844efe3ac` |

---

*Equilibria Token Swap — Community Transparency Guide v3.0 | Genesis 2026*
