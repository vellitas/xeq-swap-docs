# Equilibria (XEQ) Token Swap — Final Testnet Update

**To the Equilibria Community,**

The final testnet swap is now live. This is your opportunity to walk through the entire swap process end-to-end before mainnet opens. We strongly encourage every community member to participate in the testnet swap — not because it is required, but because doing it once on testnet gives you the confidence and familiarity to move your entire mainnet wallet without hesitation when the time comes.

The testnet swap uses real legacy XEQ and real new-chain wallets. The only difference from mainnet is that it is a test round. The mechanics, the portal, the proof generation, and the verification steps are identical. If you run into a problem on testnet, that is exactly the right time to solve it — not on mainnet with your full balance.

---

## Before You Do Anything — Read the Guide

This step is not optional. The swap guide walks you through every step in detail, including how to generate a transaction proof, what to do if something goes wrong, and what the portal status codes mean.

**Testnet Swap Guide:**
https://equilibria-network.gitbook.io/docs/documentation/guides/xeq-legacy-swap-testnet

Read it before you send a single coin.

---

## Step-by-Step

### 1. Send Legacy XEQ to the Testnet Deposit Address

The official deposit address is shown on the swap portal. Copy it from there — do not use any address posted in Telegram or elsewhere. Verify it matches what is shown on the portal before sending.

### 2. Wait for Confirmations Before Generating Your Proof

The minimum requirement to submit is 2 confirmations. However, based on testing, **we recommend waiting for 10 confirmations** before generating your proof. Proofs generated too early can be unreliable and may require you to generate a new proof and resubmit. Waiting 10 confirmations takes a little longer but saves you that extra step and reduces the chance of a failed submission.

### 3. Generate Your Transaction Proof

In your legacy wallet CLI, use `get_tx_proof` with your TXID and the official deposit address. Copy the entire proof output — do not truncate it.

### 4. Submit on the Swap Portal

Once your transfer has at least 2 confirmations (10 recommended) and you have your proof, visit the testnet portal:

**Testnet Swap Portal:** https://swap-testnet.xeqlabs.com

Enter:
- Your legacy XEQ Transaction ID (TXID)
- Your new network wallet address (starts with **XEQT** on testnet)
- Your transaction proof

The portal will show you the status of your swap in real time.

### 5. Verify Your Swap Went Through

Submitting your swap is not the finish line — **you are responsible for confirming your swap reaches Verified status.** The best way to do this is to check the XEQ Swap Tracker:

**Swap Tracker:** https://swap-tracker.xeqlabs.com

Search for your Transaction ID. You will be able to see your swap status and monitor it through to completion.

**If your status shows Expired:** This is generally a timeout condition. The swap backend polls expired transactions periodically and will automatically resubmit them for verification. Give it some time and check back.

**If your status shows Rejected with an invalid proof:** You will need to generate a new proof and resubmit using the same TXID and your new wallet address. A valid proof will update your existing swap record and move your status from Rejected to Verified.

---

## Privacy, Responsibility, and Community

Equilibria is a privacy network. That principle does not stop at the blockchain — it extends to this swap. We do not know who you are. We do not have your contact information. We cannot reach out to you if your swap is stuck.

That means **you are responsible for monitoring your own swap** and making sure it reaches Verified status before the window closes. The tools to do that are the portal and the tracker linked above. Use them.

If you run into a problem, **ask for help in the main Telegram group.** The community is active and helpful. We will work through it with you.

A few important security points:

- **We will never DM you to resolve a swap issue.** If someone sends you a private message offering to help with your swap, it is a scam. Do not engage.
- **All problem solving happens in the open.** This is intentional. Public troubleshooting maintains security, keeps scammers out, and helps other community members who may be facing the same issue.
- **Never share your transaction proof with anyone.** Treat it like your seed phrase. We will never ask for it.

---

## Swap Window

| | |
|--|--|
| **Portal** | https://swap-testnet.xeqlabs.com |
| **Tracker** | https://swap-tracker.xeqlabs.com |
| **Opens** | Block 1,776,093 |
| **Closes** | Block 1,778,253 |
| **Duration** | 2,160 blocks (~3 days) |
| **Ratio** | 1 legacy XEQ = 1 new XEQ |

Do not wait until the last moment. Submit well before the closing block height. Transactions confirmed close to the window closing may not have enough time to reach the required confirmation threshold.

---

We are looking forward to a clean testnet run. See you on the new chain.

**— The Equilibria Team**
