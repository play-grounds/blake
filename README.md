# blake

An **experimental** client-side taproot wallet for the **Bitcoin BLAKE2b chain, mainnet**. A playground copy of [blaketest](https://github.com/bitcoin-blake/blaketest) with the network switched.

Live: https://play-grounds.github.io/blake/

## Read this first

- **Playground software.** Unaudited, single-file, written in a day. Use only amounts you can afford to lose.
- **Keys are stored unencrypted** in your browser's local storage. Anyone with access to this browser profile has your coins.
- **It spends only on the BLAKE2b chain.** Every spend is signed with `SIGHASH_ALL | SIGHASH_UNIFIED` (0x21), which nodes without the hardfork cannot verify, so a spend here can never be replayed onto the SHA256d chain. Coins at the same address on the SHA256d chain are never touched.
- **The address scheme is not BIP86.** The x-only public key is used directly as the taproot output key, with no tweak, so the `bc1p` address and the Nostr npub are the same key. Importing a key from Sparrow or Shrike gives a different address than those wallets show for it.

## What it does

- Generates or imports a key (hex, Nostr nsec, or WIF).
- Shows balance and history from an Esplora-style API. Default is `https://mempool.guide/api`, the BLAKE2b chain's public explorer.
- Sends to `bc1q`, `bc1p` and P2WSH addresses, RBF on, unified sighash always.
- Can relay a legacy-signed payment made to your address on the SHA256d chain onto the BLAKE2b chain, if its inputs exist there. It signs nothing for this.

## Pointing it at your own node

```
https://play-grounds.github.io/blake/?api=http://localhost:3006/api
https://play-grounds.github.io/blake/?api=reset
```

The base must serve `address/:a`, `address/:a/txs`, `address/:a/utxo`, `v1/fees/recommended`, and `POST tx`.

## Files

Same layout as blaketest: `index.html` (Preact plus htm, no build step), `bitcoin.js`, `unified.js` with the unified sighash for all four script types, the Knots spec and 166 vectors, and `test.mjs`.

```
npm install
npm test
```

## Origin

Derived from [testcoin.org](https://github.com/liquid-pub/testcoin.org) via [blaketest](https://github.com/bitcoin-blake/blaketest). Part of [awesome-bitcoin-blake](https://github.com/bitcoin-blake/awesome-bitcoin-blake).
