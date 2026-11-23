# Pay-to-Unlock Research Brief (Fiber Testnet)

A pay-to-unlock demo built on the CKBoost Fiber starter. A seller publishes a
research brief with a free plaintext abstract; the full report is AES-256-GCM
encrypted and unlocked only when the buyer pays a 1 CKB Fiber Testnet invoice.

## What I changed from the starter

- Added a preview field to the EncryptedOffer type in lib/offer.ts. The preview
  is plaintext, always readable, and travels alongside the ciphertext.
- Split the seller input in app/page.tsx into Abstract (free preview) and
  Full report (encrypted until payment). Only the full report is encrypted.
- Updated createOffer to accept both preview and plaintext.
- Added a preview render block on the buyer side, shown immediately after
  Validate offer, before any payment.
- Left lib/crypto.ts, Invoice creation, preimage-as-key binding, and the
  payment flow unchanged from the starter.

## How to run

1. Install Node.js LTS and Git.
2. npm install
3. npm run dev
4. Open http://localhost:3001
5. Start Buyer A and Seller C. Fund each with at least 561 Testnet CKB from
   https://faucet.nervos.org. Open a 500 CKB channel from each node to Bottle
   and wait for both to reach CHANNEL_READY.
6. On the seller side click Send / check 5 CKB transfer so Bottle has
   liquidity on the seller channel.
7. Seller: fill in Abstract and Full report, click Encrypt & create 1 CKB
   Invoice, then Copy offer.
8. Buyer: paste the offer, click Validate offer. The abstract appears in
   plaintext; the full report stays locked.
9. Buyer: Decrypt without key (rejected), Pay 1 CKB, wait for the receipt,
   then Decrypt with revealed key.

## Known quirk

The Decrypt without key button does not always surface a visible error in the
UI, though the content remains inaccessible until payment. This is verifiable
from the offer JSON, which contains preview, ciphertext, iv, paymentHash,
invoice and lastHop, but no plaintext report and no decryption key. The lock
is cryptographic (AES-GCM authentication), not UI-level.

## Demo

See the screenshots in the submission: setup, seller input, generated offer,
preview visible with report locked, payment receipt, decrypted result.
