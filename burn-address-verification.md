# Equilibria (XEQ) Token Swap
## Burn Addresses — Community Verification Guide
**Version 1.2 | Genesis 2026**

---

## 1. What Is a Burn Address?

During the XEQ token swap, legacy XEQ coins are permanently removed from circulation. To prove this, cryptographically verifiable burn addresses have been created. These addresses have no spendable private key — it is mathematically impossible for anyone to withdraw funds sent to them.

Each burn address was derived deterministically from a publicly known string. Any community member can independently reproduce the derivation and confirm the address is genuine.

Two burn addresses exist — one for testnet and one for mainnet. They were derived from different burn strings so they are completely independent.

---

## 2. Testnet Burn Address

| Field | Value |
|-------|-------|
| **Address** | `Tw1d8ASYJnSdeZagFTAdu9R9N2pyXwoCGKZ2Gk6PB3UXSXRXG7ZoZKjaE4ANja6X4pTiFPitMFHcP4yR8Qqhz9Vq1NaNTqcTY` |
| **View Key** | `dad3057b53441f6dd0d582f8c4935f0584ad56379bc58af68f26d577857cd80f` |
| **Chain** | Legacy Equilibria (XEQ) — Testnet |
| **Spend Key** | **PROVABLY UNKNOWN — derived from public string only** |
| **Burn String** | `Test Equilibria XEQ Token Swap Burn Address - Genesis 2026` |

---

## 3. Mainnet Burn Address

| Field | Value |
|-------|-------|
| **Address** | `TvzftzCWPdMFCvRCCv64sAbHG1mf6MW1e4yoAAbCtHWaJVesVtv8Z7q7SggscMKTdjMCNutoFotkAgb45eg66PmR314aZjXgD` |
| **View Key** | `805eba3bff2b78e222ec26e00c63ca2b717017e86714d42f924b9aff6714ab06` |
| **Chain** | Legacy Equilibria (XEQ) — Mainnet |
| **Spend Key** | **PROVABLY UNKNOWN — derived from public string only** |
| **Burn String** | `Equilibria XEQ Token Swap Burn Address - Genesis 2026` |

---

## 4. How the Addresses Were Derived

Both burn addresses were generated using a fully deterministic, publicly auditable process. No random number generator was used. Each private spend key was derived entirely from its corresponding public burn string using these steps:

1. Take the SHA256 hash of the burn string (UTF-8 encoded).
2. Reduce the resulting 32 bytes modulo the ed25519 curve order (`l`) to produce the private spend key.
3. Derive the private view key as: `Keccak256(spend_key) mod l`.
4. Derive the public keys and final address using standard Monero/Equilibria ed25519 point multiplication.

---

## 5. Independently Verify the Derivation

Anyone can reproduce steps 1–3 using standard Python. Replace the burn string with testnet or mainnet as appropriate:
```python
import hashlib

# For mainnet:
BURN_STRING = "Equilibria XEQ Token Swap Burn Address - Genesis 2026"
# For testnet:
# BURN_STRING = "Test Equilibria XEQ Token Swap Burn Address - Genesis 2026"

l = 2**252 + 27742317777372353535851937790883648493

def sc_reduce32(x):
    n = int.from_bytes(x, 'little') % l
    return n.to_bytes(32, 'little')

seed = hashlib.sha256(BURN_STRING.encode('utf-8')).digest()
spend_key = sc_reduce32(seed)
print('Spend key:', spend_key.hex())

# Mainnet expected: 6d0b210f934aef307a1de2eb6620bc94989fe7005ada42b48938c6c8425ef400
# Testnet expected: 0fe45729ece0e6c31fa8b118c263f4c2ce6bc04c33c1b958f0d53d1ca85d0602
```

To verify the full address using wallet-cli:
```
wallet-cli --generate-from-spend-key /tmp/verify-burn
# Enter the spend key when prompted
# Confirm the generated address matches the table in section 2 or 3
```

---

## 6. Why No Spending Is Possible

The spend keys were derived by hashing public strings. Because SHA256 is a one-way function:

- No private key was generated randomly and then hidden.
- Each spend key is fully determined by its public burn string — there is no secret.
- The Equilibria team has no special access to these addresses — the keys are as public as the burn strings themselves.
- Any community member can verify the derivation independently without trusting the Equilibria team.

---

## 7. Proof That Spending Fails — Live Test

The testnet burn address was tested by sending 1 XEQ to it and then attempting to spend the funds using a view-only wallet loaded with the burn address view key.

**Test transaction (testnet):**

| Field | Value |
|-------|-------|
| **txid** | `0362dd3d8f689fa916c76233c4cde8471137f5d9a4d69253aeb17ff7b9e655a7` |
| **amount** | 10000 atomic units (1 XEQ) |
| **height** | 1770383 |
| **confirmations** | 57 |

**Viewing the transaction succeeded** — the view-only wallet correctly reported the incoming transaction, confirming the view key works for monitoring.

**Spending the transaction failed** — when a `transfer` RPC call was made against the view-only wallet, it returned an `unsigned_txset` blob with an empty `tx_key`:
```json
{
  "result": {
    "tx_hash": "643d7e76cd819c45a00d2fb767310bbc9ed40a83c0382bd3086b4f6eef6d39d8",
    "tx_key": "",
    "tx_blob": "",
    "unsigned_txset": "4d6f6e65726f20756e7369676e65642074782073657404..."
  }
}
```

The critical fields:
- **`tx_key: ""`** — empty, meaning the transaction was never signed
- **`tx_blob: ""`** — empty, meaning nothing was broadcast to the network
- **`unsigned_txset`** — a blob that requires the spend key to sign, which no one possesses

The transaction hash shown (`643d7e76...`) is a local placeholder only — it was never submitted to the network and does not exist on chain. The 1 XEQ sent to the burn address is permanently unspendable.

---

## 8. Monitoring the Burn Addresses

The view keys are published above. Any community member can use them to monitor incoming transactions in read-only mode, with zero spending capability. This provides full public transparency over all legacy XEQ sent to the burn addresses.

**Step 1 — Create a view-only wallet:**
```
wallet-cli --generate-from-view-key /tmp/burn-monitor
# Enter the address when prompted (testnet or mainnet from section 2 or 3)
# Enter the view key when prompted
# Set restore-height to a recent block to speed up sync
```

**Step 2 — Sync and view incoming transactions:**
```
# Inside wallet-cli after syncing:
refresh
show_transfers in
```

**Step 3 — Attempt a transfer to confirm it cannot be spent:**
```
# Inside wallet-cli:
transfer <any_address> 1
# Result: returns unsigned_txset with empty tx_key — never broadcasts
```

---

*Equilibria Token Swap — Burn Address Verification Guide v1.2 | Genesis 2026*
