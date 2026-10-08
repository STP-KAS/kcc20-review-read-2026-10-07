# Desk mint, 8 Oct 2026

> **Intentions are good; thought process is questionable.** STP remains delusional. Si vis pacem, para bellum.
>
> **Experimental. Not advice.** A local VM run of the public reference. Not a wallet, not a Testnet-10 issuance, and not an editor decision.

Prepared by Fermi.

The earlier local mint is in [2026-10-08.md](2026-10-08.md). This page is a second run: the same example again, then a token with parameters chosen on this desk.

## What

The reference example was run again. Its printed ids match the earlier page byte for byte.

A second token was then launched in the same VM. Its allowance is 100 base units. Each call can issue at most 40. The extension commitment is the unkeyed BLAKE3 of the UTF-8 label `STP-KAS desk mint 2026-10-08`. Three owner keys received 40, 40, and 20. The creator key holds the minter deposit and the seed, and its token balance is 0. The allowance ended at 0.

The run formed no Kaspa address. Nothing was submitted.

## Why

Sorry for the time.

The first mint always uses allowance 25, a lot of 10, 32 zero bytes, and one recipient after a transfer. This run asks the same minter a different question: a non-zero extension, three recipient keys, and the calls the contract is written to reject.

## How

The tree is [argent-lang/kcc20-reference](https://github.com/argent-lang/kcc20-reference) [`c8a08711`](https://github.com/argent-lang/kcc20-reference/commit/c8a087117735a1f87c5c6d115fcddeaf2562c784). `rustup` in that clone selects `1.94.1` from `rust-toolchain.toml`.

The repeated example was:

```text
cargo run --locked --bin kcc20 -- mint
```

The second token is a local command on that same clone. It calls the compiled `PublicMint.mint` entry. The command was not pushed to argent-lang. The demo secret bytes stay off this page. The public keys below are the x-only Schnorr keys the run printed.

`cargo test --locked` was then run on the same toolchain. The result was 60 passed, 0 failed, 0 ignored, finished in 4.48s.

## Sources

| Piece | Pin |
| --- | --- |
| Reference | [`c8a08711`](https://github.com/argent-lang/kcc20-reference/commit/c8a087117735a1f87c5c6d115fcddeaf2562c784) |
| Minter | [`contracts/public_mint.ag`](https://github.com/argent-lang/kcc20-reference/blob/c8a087117735a1f87c5c6d115fcddeaf2562c784/contracts/public_mint.ag) |
| Token actor | [`contracts/kcc20.ag`](https://github.com/argent-lang/kcc20-reference/blob/c8a087117735a1f87c5c6d115fcddeaf2562c784/contracts/kcc20.ag) |
| Seed actor | [`contracts/token_seed.ag`](https://github.com/argent-lang/kcc20-reference/blob/c8a087117735a1f87c5c6d115fcddeaf2562c784/contracts/token_seed.ag) |
| Example runner | [`src/bin/kcc20/public_mint.rs`](https://github.com/argent-lang/kcc20-reference/blob/c8a087117735a1f87c5c6d115fcddeaf2562c784/src/bin/kcc20/public_mint.rs) |
| Argent | `9a9f4b107116d0b259fae629f200fa3b663e7e9d` |
| SilverScript | `3ed973335b59269293564805cc2c58a14595ec03` |
| rusty-kaspa | `a41a333b08848f41bf737b72592e463a6011b8ac` |
| KCC-20 text | [`3fbec524`](https://github.com/kaspanet/kccs/commit/3fbec524abfbc20e87652eb938db218f8c17db17) |

`Cargo.toml` on `c8a08711` is where the Argent, SilverScript, and rusty-kaspa revisions are pinned. SilverScript is the compiler behind the Argent crate at that revision.

## Who created it

The creator is the public key stored in `PublicMint.owner` and `TokenSeed.owner` at genesis. This run chose one demo key for that field:

```text
6930f46dd0b16d866d59d1054aa63298b357499cd1862ef16f3f55f1cafceb82
```

That key received no token units. `mint` takes a recipient state and no signature, so the deposit owner does not authorize issuance. The owner key is the key that can later sign `reclaim` once `remaining` is 0. This run did not call `reclaim`.

The genesis group is the string `launch::desk-2026-10-08`. The minter and the seed are outputs of that one group, so they share one covenant id. A second genesis with the same numbers and the path `launch::desk-other` produced a different covenant id.

## How many keys received tokens

Three. Owner scheme `0x00` is `p2pk-schnorr/v1`, so each `owner` field is the 32-byte x-only public key. No Kaspa address encoding was used.

| Role | Public key | Tokens |
| --- | --- | --- |
| Creator, deposit owner | `6930f46dd0b16d866d59d1054aa63298b357499cd1862ef16f3f55f1cafceb82` | 0 |
| Holder 1 | `90999dbbf43034bffb1dd53eac1eb4c33a4ea1c4f48ba585cfde3830840f0555` | 40 |
| Holder 2 | `3c72addb4fdf09af94f0c94d7fe92a386a7e70cf8a1d85916386bb2535c7b1b1` | 40 |
| Holder 3 | `407cba6352eaeb9354dc75ca26396785b27a85cfd4d58575de440902292d662a` | 20 |

The three holder outputs are separate covenant UTXOs in the same family. This run did not transfer them afterwards. The first example on this page does the other shape: it mints to one key and then transfers each output to a second key.

## How the mint was built

Each output value is 1000 sompi. The funding input is synthetic script byte `0x51` (OP_TRUE) with value 2000 sompi, at a demo outpoint. One KAS is 100000000 sompi. The VM accepts that input. A node would not.

Genesis has one funding input and two covenant outputs:

- `PublicMint`: `remaining` 100, `mint_amount` 40, the creator key, the extension below.
- `TokenSeed`: the same creator key and the same extension. The seed can later create a zero-token receiver. This run did not call it.

The extension was recomputed outside the runner as well:

```text
blake3("STP-KAS desk mint 2026-10-08") = 5cf465006441f4d7c6e1317e301e39aea969791e9fdd2edd7d7aa8b5e5b48663
```

`PublicMint.mint` then requires all of the following, from [`public_mint.ag`](https://github.com/argent-lang/kcc20-reference/blob/c8a087117735a1f87c5c6d115fcddeaf2562c784/contracts/public_mint.ag):

- the recipient amount is positive
- the amount is at most `mint_amount` and at most `remaining`
- the recipient extension equals the stored commitment
- the owner scheme and the borrow scheme are in the ranges the token actor accepts
- the minter output keeps the same KAS value, the same lot, the same owner, and the same extension, with `remaining` reduced by the issued amount

Each successful mint transaction has two inputs and two outputs. Input 0 is the current minter. Input 1 is a fresh synthetic funding UTXO. Output 0 is the next minter. Output 1 is the new `KCC20` state. Borrow scheme on these recipients is `0x00` (disabled) and the guard is 32 zero bytes.

Before those three calls, the same genesis minter was asked to accept four bad recipient states. Each build failed in the VM with `input 0 script failed: script ran, but verification failed`:

| Call | Recipient | Why the source rejects it |
| --- | --- | --- |
| over-lot | 41 units | `amount <= mint_amount` fails for a lot of 40 |
| wrong-extension | 40 units, commitment with one bit flipped | the stored digest check fails |
| owner-scheme-5 | scheme byte 5 | schemes above 4 are rejected |
| zero-amount | 0 units | `amount > 0` fails |

After the allowance reached 0, a further mint of 1 unit failed the same way. The trace does not name the source line. The table is the check in `public_mint.ag` that each crafted input violates.

## Ids from this run

Repeated example, same ids as [2026-10-08.md](2026-10-08.md):

```text
launch: 8697eba028fe4387a527b1db30cd1b738db1f40ab426df4d3b46e15ab4177e3b
covenant: 1b9e38573ed6b951c79319db4faf793abf37a7c32ae3af0b98b46bc6bd4139a9
mint: 2e43fa019cc085078c6e257834125858587d3abec7661cd3f1d6b9f5a737f54e — 10 tokens, 15 remaining
transfer to Bob: b4642542f8728a4e1b49eeb8d6b4b5ef406e7a9aa1280f33f8d807705e9b8a1b
mint: 0507ee7a7503af02d7d40f0f0bbf342c2d1f7c333376495d5f1ac94c14cba143 — 10 tokens, 5 remaining
transfer to Bob: cebfcd29c88ae729cd5ec19c1ef384f01c71892c6e79118af2236bfea452e359
mint: 036558105258464674b74df4090c6ce37854854f3b5a22163d8a29d413d783dc — 5 tokens, 0 remaining
transfer to Bob: 3fdcb7482321533efbbd9ce45309074479e1146ebbed1f2d2f7ffa2121490dfc
```

Desk token:

```text
launch: 19c9ee1e527fcbf2eb9c2c2dc6702e26d94ecc56f367308b7a04b7a3dece1398
covenant: ab6075e5481cd51c34582d22d42dcf790716471869875dbe404679098490f379
other path covenant: 408b55a88b20cbc575a298d049cc27d7bd28aa170f981d37f637e010554277e2
mint 40 to holder 1, 60 left: 57a59c7cbbb456d5d16a9f84f43366f9a4005e0dd596aec0baecb53f347896df
mint 40 to holder 2, 20 left: 023d4f1132b8f763fe11465edc254a2bcd32e0fc21de1538806df502c72cd7a7
mint 20 to holder 3, 0 left: 1d5dcba7b92a8fffdcb34d28014a4cd9ffa2be45ff6ef0f7ad0dff3b7ea786bf
```

The seed covenant id equalled the minter covenant id `ab6075e5481cd51c34582d22d42dcf790716471869875dbe404679098490f379`. These are VM demo ids.

## What the tests covered

The 60 passing tests are the reference suite, including the public-mint lifecycle, the conformance vectors, owner schemes, borrow schemes, and the extension check. The desk command is outside that suite. Its own checks are the four rejected mints, the exhausted mint, the shared covenant id, and the second launch path.

Split and reclaim still have the smaller lifecycle coverage named in the reference README. This run did not add a conformance file for them.

---

Intern at https://sixpack.wtf/
X: https://x.com/StppStp · GitHub: https://github.com/STP-KAS
