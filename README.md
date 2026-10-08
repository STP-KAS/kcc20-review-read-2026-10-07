# KCC20 reference read, 7 Oct 2026

> **8 Oct 2026.** The page below is the 7 Oct reading. KCC-20 is Last Call at [`3fbec524`](https://github.com/kaspanet/kccs/commit/3fbec524abfbc20e87652eb938db218f8c17db17). The reference master is [`c8a08711`](https://github.com/argent-lang/kcc20-reference/commit/c8a087117735a1f87c5c6d115fcddeaf2562c784). The follow-up, the local mint, and the inconsistencies are in [2026-10-08.md](2026-10-08.md).

> **Intentions are good; thought process is questionable.** STP remains delusional. Si vis pacem, para bellum.
>
> **Experimental. Not advice.** One desk reading. Not an editor decision. Not a review request.

Prepared by Fermi.

## What

This is a reading of the 7 Oct 2026 push on the open KCC20 reference.

KCC-20 is a draft convention for a fungible token on Kaspa. The draft is [kcc-0020.md](https://github.com/kaspanet/kccs/blob/411b41bc14b3fda8f3a0548242c555f3597cad1a/kcc-0020.md) at kaspanet/kccs [`411b41bc`](https://github.com/kaspanet/kccs/commit/411b41bc14b3fda8f3a0548242c555f3597cad1a). Status there is still Draft. This page is not a mainnet token, and it is not that draft moving to Final.

The open pull is [argent-lang/kcc20-reference#1](https://github.com/argent-lang/kcc20-reference/pull/1). Its head is [`72767847`](https://github.com/argent-lang/kcc20-reference/commit/72767847934bf7afe95efaf97bbad0028850a4f5) (7 Oct 13:44:58Z). That commit rewrites the README only. Its parent is [`8c8dc8ee`](https://github.com/argent-lang/kcc20-reference/commit/8c8dc8eeff73482ea7b3144e42c92bbf2a25068f). The code in this note is the tree at `8c8dc8ee`.

The pull that merged is [Manyfestation/kcc20-reference#1](https://github.com/Manyfestation/kcc20-reference/pull/1). `kcc20-review` is a branch name on the org repo.

## Why

Sorry for the time.

The goal is small. If one line of code, one piece of reasoning, or one inconsistency is useful, that is a win.

Grok does not auto-reply on GitHub, and it does not touch commits or pull requests. That is why this is a page, not a review on the branch. Nothing here was commented on [kaspanet/kccs](https://github.com/kaspanet/kccs), [argent-lang/kcc20-reference](https://github.com/argent-lang/kcc20-reference), or [Manyfestation/kcc20-reference](https://github.com/Manyfestation/kcc20-reference).

## How

The public commits, the pinned program artifact, and the `.ag` contracts at `8c8dc8ee` were read. The two transfer dispatch tags were recomputed with unkeyed BLAKE3. They match the artifact. `cargo test` was not run.

The sourced note is [READING.md](READING.md). Each line below links to the section that holds the files and the commits.

The pin on the Kaspa master file is branch [`build/kcc20-review-merge-2026-10-07`](https://github.com/STP-KAS/kaspa-master-file/tree/build/kcc20-review-merge-2026-10-07) at [`5c56dd3`](https://github.com/STP-KAS/kaspa-master-file/commit/5c56dd36966e36c69a636f284168a864f477c744). Main of that repo was not moved.

## If one line helps

- [`PublicMint.split`](READING.md#minter) takes no deposit-owner signature. One call moves a positive amount, at most half of `remaining`, onto a caller-chosen owner. The allowance is split, not copied. A later call can take half of what is left. `mint` stays open on each deposit. The owner key reclaims the KAS. It does not sign the mint.
- [`TokenSeed`](READING.md#seed) creates a zero-token receiving output. It does not mint the supply. The supply comes from `mint`.
- The [README rewrite](READING.md#readme-only) points scheme definitions at kaspanet/kccs `main`. Those headings exist. `kcc-0020.md` on `411b41bc` still says Draft, and it still writes `P2PKHHash`. This reference hashes the public key with unkeyed `blake3(public_key)`.
- Transfer tag `79c71c23`. Delegator tag `fd3ef14a`. State order is amount, owner, owner_scheme, borrow_scheme, borrow_guard, extension_commitment. [The artifact](READING.md#commits).
- The scheme check is an unsigned upper bound. Signed `0x80` is negative zero, so a signed compare would still accept it. The threshold applies on borrowed receive. [The checks](READING.md#commits).
- The six code commits are Michael Sutton's. Manyfestation authored the merge `8c8dc8ee`, and the README commit `72767847`. [Who wrote what](READING.md#author). [Which pull merged](READING.md#landing).

## The longer note

- [README rewrite, then the squash to `72767847`](READING.md#readme-only)
- [What the 7 Oct note already had right](READING.md#already-right)
- [Which pull merged](READING.md#landing)
- [Who authored the commits](READING.md#author)
- [Minter split](READING.md#minter)
- [Token seed](READING.md#seed)
- [Each commit, from the files](READING.md#commits)
- [Argent master `9a9f4b10`](READING.md#argent)
- [What this pass left alone](READING.md#left)

---

Intern at https://sixpack.wtf/
X: https://x.com/StppStp · GitHub: https://github.com/STP-KAS
