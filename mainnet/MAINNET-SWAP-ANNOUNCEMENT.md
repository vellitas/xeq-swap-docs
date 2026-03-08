# Equilibria (XEQ) Token Swap — Mainnet Opens March 10

**To the Equilibria Community,**

The mainnet XEQ token swap window will open at **block 1,781,094** on **Tuesday, March 10, 2026 at approximately noon Eastern Time**.

The swap window will remain open for **35 days — 25,200 legacy blocks** — and will close at **block 1,806,294**, approximately **Wednesday, April 15, 2026**.

This is the moment the community has been working toward. Every legacy XEQ holder has 35 days to complete their swap to the new chain. There is no extension and no exceptions after the window closes, please do not wait until the last moment.

---

## No KYC. No Exceptions.

Privacy is at the core of everything we build. True to that principle, we will not be requiring any KYC details to participate in the swap, not now, not ever. Your identity is your own business.

With that comes responsibility. **As an individual participating in the swap, it is your responsibility to ensure that your swap is accepted and verified.** We cannot chase you down. We do not know who you are. If your swap does not reach Verified status before the window closes, your legacy XEQ will not be counted.

---

## Before You Do Anything — Read the Guide

**Mainnet Swap Guide:**
https://equilibria-network.gitbook.io/docs/documentation/guides/xeq-legacy-swap-mainnet

Read it before you send a single coin. The guide walks you through every step in detail including how to generate a transaction proof, what to do if something goes wrong, and what the portal status codes mean.

---

## Step-by-Step

### 1. Verify the Deposit Address

**Double check that you are using the new legacy deposit address shown on the swap portal.** Do not use any address posted in Telegram or elsewhere. Verify it matches what is shown on the portal before sending. Transfers sent to the wrong address cannot be counted in the swap, there are no exceptions.

### 2. Send Legacy XEQ

Send from your own wallet only. Do not send from an exchange.

### 3. Wait for Confirmations Before Generating Your Proof

We recommend waiting for **10 confirmations** before generating your proof. Proofs generated too early can be unreliable and may require resubmission.

### 4. Generate Your Transaction Proof

Using the recomended new GUI wallet or the legacy wallet CLI, use `get_tx_proof` with your TXID and the official deposit address. Copy the entire proof output, do not truncate it.

### 5. Submit on the Swap Portal

**Swap Portal:** https://swap.xeqlabs.com

Enter:
- Your legacy XEQ Transaction ID (TXID)
- Your new chain wallet address (starts with **XEQ** on mainnet)
- Your transaction proof

### 6. Watch Your Swap Status

**You are responsible for monitoring your swap.** Check the Swap Tracker for every transaction you submit:

**Swap Tracker:** https://swap-tracker.xeqlabs.com

A successful swap status is **Verified**. Here is what to do for other statuses:

| Status | What It Means | What To Do |
|--------|--------------|------------|
| Verified | ✅ You are done | Nothing — wait for payout |
| Expired | Timeout condition | The backend polls expired transactions periodically and will automatically resubmit. Give it time and check back. |
| Rejected — invalid proof | Proof was not accepted | Generate a new proof and resubmit as a new swap using the same TXID and your new wallet address. A valid proof will update your existing swap record. |

---

## What Happens After the Window Closes

- The swap ledger is finalized with a SHA256 hash and published for community verification
- New XEQ is minted to match the exact verified total — no more, no less
- Payouts are sent to all verified recipients on the new chain
- The full process and all wallet keys are published to the community transparency repository at github.com/vellitas/xeq-swap-docs

---

## Security Reminders

- Only use the official portal at **swap.xeqlabs.com**
- **We will never DM you** to help with your swap, anyone who does is a scammer
- Never share your transaction proof with anyone
- All swap support happens publicly in the main Telegram group

---

See you on the new chain.

**The Equilibria Team**
