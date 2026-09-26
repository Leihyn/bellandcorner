# Bell & Corner

A fight bet that settles itself. No bookmaker, no escrow, no custody.

A working [Discreet Log Contract](https://adiabat.github.io/dlc.pdf) you can run in the browser.
Publish an oracle announcement, compute the anticipation point for each outcome, lock a bet into
adaptor signatures, then settle. One published number pays the winner; the loser's signature
stays locked.

**Live:** https://bellandcorner.vercel.app · **Full flow:** https://bellandcorner.vercel.app/?demo

## Why the oracle cannot cheat

It signs an outcome, not a payout. The same signature settles every contract on that fight and
settles none of them differently. It cannot pay the loser, because the losing adaptor signature
only completes with a number it did not publish. It cannot steal, because it never held anything.

## What is implemented

BIP-340 oracle attestation with x-only keys and even-y lifting, anticipation point derivation,
Schnorr adaptor signature creation and completion, and explicit verification that the wrong
outcome fails to complete.

## What is not

Funding transactions, CET construction, and broadcast. This proves the settlement mechanism; it
does not move coins.

## Verification

Tested before release:

```
1. anticipation point computed before attestation
2. oracle attests, s·G equals the predicted point:      match: true
3. adaptor signature completes with the oracle scalar:  valid: true
4. the WRONG outcome completes it:                      valid: false
```

Property 4 is the one the scheme rests on.

## Running it

No build step. Serve the directory, or open `index.html` over `http://` (ES modules will not
load from `file://`).

## Dependencies

`@noble/secp256k1` v2, vendored as `secp256k1.js`.

## Security

Demonstration keys. Do not paste a key that holds funds.

MIT
