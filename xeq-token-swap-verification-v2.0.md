# Equilibria (XEQ) Token Swap
## Swap Deposit Addresses — Community Transparency Guide
**Version 2.0 | Genesis 2026**

---

## 1. Overview

During the XEQ token swap, legacy XEQ coins are sent to dedicated swap deposit addresses. These funds are monitored publicly via published view keys, providing full community transparency over all incoming deposits. The spend keys are held privately by the Equilibria team and will never be used.

The legacy Equilibria chain will be permanently shut down shortly after the swap window closes. After shutdown, the Equilibria GitHub will be locked — no further forks or client downloads will be possible. At that point, legacy XEQ has no network to transact on and the deposit address funds become permanently inaccessible in practice.

---

## 2. Deposit Addresses

### 2.1 Mainnet Swap Deposit Address

| Field | Value |
|-------|-------|
| **Address** | `Tw1AmWavnFtPT4rgfwt7seYHsLW1vPE7wR8s5BBtu7NZCK31H9jWy6r7m4iZ8APaLDfzGvrgqU9fW3BLjKKiHBw82EntBGmLn` |
| **View Key** | `d1c95300b1e473d0a4ac6a81adbc42a5fe432de54b5e71c72fae73e668d88701` |
| **Chain** | Legacy Equilibria (XEQ) — Mainnet |
| **Spend Key** | **PRIVATE — held by Equilibria team, never published, never used** |

### 2.2 Testnet Swap Deposit Address

| Field | Value |
|-------|-------|
| **Address** | `Tw1TCBUSPjMCePC3unP98V8FuwRTY6cJq7kwbKWCYZNgf9vG4mQ1KbRSCeaUDPSTycBS1TcWi31nZHA7DvGKka7m2oXmFnb7f` |
| **View Key** | `567e1326f061ae75a61a1d2ec69523f95465a6fb7b7cd4b3f871c72d8fb8a605` |
| **Chain** | Legacy Equilibria (XEQ) — Mainnet |
| **Spend Key** | **PRIVATE — held by Equilibria team, never published, never used** |

---

## 3. Transparency Guarantees

The following guarantees are provided to the community:

- **Full deposit visibility** — the view keys are published above. Any community member can create a view-only wallet and monitor every incoming transaction in real time with zero spending capability.

- **Public dashboard** — all deposits are visible on the XEQ Swap Tracker Dashboard at [https://swap-tracker.xeqlabs.com/](https://swap-tracker.xeqlabs.com/) via API endpoints that query the deposit wallet directly.

- **Chain shutdown** — the legacy Equilibria chain will be permanently shut down shortly after the swap window closes. After this point, no transactions can be made on the legacy chain regardless of who holds any spend key.

- **GitHub lock** — the Equilibria GitHub repository will be locked after shutdown. No further forks or client downloads will be possible, preventing anyone from creating a competing legacy chain.

---

## 4. How to Monitor Deposits (Community Verification)

Any community member can independently verify all deposits using the published view key. No trust in the Equilibria team is required to monitor incoming transactions.

### 4.1 Create a View-Only Wallet

To speed up sync, use `--restore-height` when creating your wallet. Check the current legacy chain height at [https://swap-tracker.xeqlabs.com/](https://swap-tracker.xeqlabs.com/) before running this command. Use the swap start height to ensure you capture all deposits from the beginning of the swap window.

```bash
wallet-cli --generate-from-view-key /tmp/xeq-swap-monitor \
  --restore-height <swap_start_height>
# Enter the deposit address when prompted (section 2.1 or 2.2)
# Enter the view key when prompted
```

### 4.2 Sync and View Incoming Transactions

```
# Inside wallet-cli after syncing:
refresh
show_transfers in
```

### 4.3 Via the Public Dashboard API

All deposit data is available via public API endpoints. No API key required.

```bash
# Summary — total deposited, deposit count, sync status
curl -s https://swap.xeqlabs.com/api/public/burn | python3 -m json.tool

# Full transfer list
curl -s https://swap.xeqlabs.com/api/public/burn/transfers | python3 -m json.tool
```

**Summary response fields:**

| Field | Description |
|-------|-------------|
| `total_burned_xeq` | Total XEQ deposited (in XEQ, not atomic units) |
| `deposit_count` | Number of individual deposit transactions |
| `wallet_sync_height` | Current height of the deposit wallet |
| `chain_height` | Current legacy chain height |
| `synced` | Whether wallet is fully synced |
| `as_of` | Timestamp of the response |

**Transfer list response fields:**

| Field | Description |
|-------|-------------|
| `txid` | Transaction ID on the legacy chain |
| `amount_xeq` | Amount deposited in XEQ |
| `height` | Block height of the deposit |
| `timestamp` | Unix timestamp of the deposit |
| `confirmations` | Number of confirmations |

---

## 5. What Happens to the Funds

All legacy XEQ deposited to the swap address is counted toward the total supply verified for the new chain genesis mint. The verified total is published as a SHA256-signed ledger before the genesis mint occurs. After the swap window closes:

- The swap ledger is finalized and published with a SHA256 hash for community verification.
- A single genesis mint on the new Equilibria chain issues new XEQ equivalent to the verified swap total.
- Payouts are sent to recipients' new chain wallet addresses.
- The legacy chain is shut down shortly after the swap window closes.
- The Equilibria GitHub is locked — no forks or downloads after this point.

Once the legacy chain is shut down, the deposit address funds are permanently inaccessible. No transaction can be broadcast on a network that no longer exists.

---

## 6. Why No Cryptographic Burn Address

True cryptographically unspendable addresses (such as Bitcoin's `OP_RETURN` outputs) do not exist on Monero-fork chains including Equilibria. The transparency model for this swap relies instead on:

1. **Published view keys** — full public visibility of all deposits
2. **Public API** — real-time deposit data available to anyone
3. **Chain shutdown** — legacy network permanently closed shortly after the swap window
4. **GitHub lock** — no ability to fork or continue the legacy chain after shutdown

These measures together ensure that deposited legacy XEQ cannot be meaningfully accessed after the swap is complete.

---

*Equilibria Token Swap — Swap Deposit Address Transparency Guide v2.0 | Genesis 2026*
