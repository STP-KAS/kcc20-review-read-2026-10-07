> **Intentions are good; thought process is questionable.** STP remains delusional. Si vis pacem, para bellum.
>
> **Experimental. Not advice.** The short page is [README.md](README.md). This file is the expansion.

# What to change in the 7 Oct notification

Read on 7 Oct 2026 from the public commits, the pinned artifact, and `contracts/*.ag` at `8c8dc8ee`. `cargo test` was not run.

The open pull's head is now `72767847` (7 Oct 13:44:58Z). See the next section. The code reading below is still the `8c8dc8ee` tree. The README commits do not touch it.

<a id="readme-only"></a>

## Same day: `8c8dc8ee..2058c13c` is README only

The linked compare is [8c8dc8ee..898192dc](https://github.com/argent-lang/kcc20-reference/pull/1/changes/8c8dc8eeff73482ea7b3144e42c92bbf2a25068f..898192dc909dcda94ea65903ef4835b9af9f2054).

| Commit | Time | Author | Message | Files |
| --- | --- | --- | --- | --- |
| `898192dc` | 13:39:21Z | Manyfestation | Refine README around KCC20 convention and app features | `README.md` +153/−176 |
| `cc86ca59` | 13:42:58Z | Manyfestation | Clarify genesis transaction and initial output creation | `README.md` |
| `2058c13c` | 13:43:29Z | Manyfestation | Describe token deployment through the genesis transaction | `README.md` |

`898192dc`'s parent is `8c8dc8ee`. `cc86ca59`'s parent is `898192dc`. `2058c13c`'s parent is `cc86ca59`. Together `898192dc...2058c13c` is `README.md` +13/−10.

At 13:44:58Z those three left the branch. The open head is [`72767847`](https://github.com/argent-lang/kcc20-reference/commit/72767847934bf7afe95efaf97bbad0028850a4f5), Manyfestation, "Refine README around KCC20 convention and reference app". Its parent is `8c8dc8ee`. The diff is `README.md` only (+156/−176). The README bytes match `2058c13c` (9499 bytes). GitHub says the org pull is open and `clean`, with 18 commits. argent-lang `master` is still `76648f99`.

What the rewrite keeps, now in the author's words: anyone can call `split(take, new_owner)` and move a positive allowance of up to half the remaining amount. Minting and splitting require no deposit-owner signature. `TokenSeed.create` creates a zero-token UTXO. The supply comes from `mint`. Output scheme validation covers all 256 byte values. The split and reclaim entrypoints do not yet have a full conformance suite. Examples execute locally and do not submit.

What the rewrite removes from the README: the owner-scheme byte table, the borrow-scheme table, the unkeyed BLAKE3 sentence, the negative-threshold sentence, and the concrete example numbers (11 tokens, threshold 10, mint 25 then 10/10/5). Those sentences are no longer in the README. They are still in the `8c8dc8ee` sources and tests this note already read. This pass did not re-open `kcc20.ag`.

The README now says the specifications define the conventions and points scheme definitions at kaspanet/kccs `main`:

- [kcc-0020 §1 State](https://github.com/kaspanet/kccs/blob/main/kcc-0020.md#1-state)
- [kcc-0020 §2 Transfer Interface](https://github.com/kaspanet/kccs/blob/main/kcc-0020.md#2-transfer-interface)
- [kcc-0020 §5 Borrowed Receive](https://github.com/kaspanet/kccs/blob/main/kcc-0020.md#5-borrowed-receive)
- [kcc-0002 §2.1 Authority Schemes](https://github.com/kaspanet/kccs/blob/main/kcc-0002.md#21-authority-schemes)

Those headings exist on main `411b41bc`. `kcc-0020.md` there still says `Status: Draft` and still contains `P2PKHHash` twice. Following the new README links does not show the unkeyed `blake3(public_key)` this reference compiles. The code pin for that hash stays `kcc20.ag` at `8c8dc8ee`, not the draft file.

`cc86ca59` and `2058c13c` retitle the section "Deployment and genesis". Deploy means one genesis transaction whose output group holds one `PublicMint` (its `remaining` is the advertised supply), at least one `TokenSeed`, and no pre-minted balances. The minter and the seeds have to be in that same group to share a covenant id. A new genesis transaction is a different covenant id and cannot add a seed to this family. That matches the earlier code reading. It does not add a new entrypoint.

Nothing in this range is a reason to re-run the dispatch-tag check. The artifact was not in the diff.

<a id="already-right"></a>

## What the note got right

KCC20 state is amount, owner, owner_scheme, borrow_scheme, borrow_guard, extension_commitment. The default calls are `transfer(next states, witness)` and `transfer_delegator(witness)`. Owner schemes are the five bytes `0x00` through `0x04`. Borrow schemes are `0x00` through `0x03`.

The amount threshold belongs on borrowed receive. A signed transfer of a small amount still has to work. The code checks the threshold only after the path byte `0x01`. A normal transfer (`0x00`) accepts a negative threshold guard and does not compare the amount increase to it.

Continuation closure is a real invariant. The generated SIL now requires the leader's authorized output count to equal the covenant output count.

KCC-0020 in kaspanet/kccs was not moved. Main is still `411b41bc` (1 Oct 13:15Z). `kcc-0020.md` still says `Status: Draft`. The examples still use synthetic keys and do not submit. This push is the reference catching the ABI. It is not a mainnet token standard.

The useful wallet check is the generated artifact around `28dbbe45`. That check holds. See below.

<a id="landing"></a>

## Change the landing

The sentence "merged pull request #1 from argent-lang/kcc20-review" is a fork merge, and `kcc20-review` is a branch name.

- [argent-lang/kcc20-reference#1](https://github.com/argent-lang/kcc20-reference/pull/1) is open. The head at 12:22Z was `8c8dc8eeff73482ea7b3144e42c92bbf2a25068f`. The head is now `72767847`. See the section above. Mergeable state is `clean`. Reviews on that pull are still comments from 10-11 Sep. There is no approving review. The CI workflow removed in `764a1057` is still gone. argent-lang `master` is still the 10 Sep stub `76648f99`.
- The merged pull is [Manyfestation/kcc20-reference#1](https://github.com/Manyfestation/kcc20-reference/pull/1), "Finalize kcc20 initial ref". michaelsutton opened it. Manyfestation merged it at 12:22:29Z. Base was `Manyfestation:finalize-kcc20-reference`. Head was `argent-lang:kcc20-review`.
- `GET /repos/argent-lang/kcc20-review` is 404. The branch `kcc20-review` exists on argent-lang/kcc20-reference. Its tip is `28dbbe45` (7 Oct 09:56Z), the parent of the merge.
- "PR number 1 means this repo is young" does not follow. The org pull #1 has been open since 10 Sep and now lists 18 commits. The fork's pull #1 is the merge that just closed.

<a id="author"></a>

## Change the author

The notification says Manyfestation pushed seven commits. The six content commits are Michael Sutton's, 5 Oct and 7 Oct. Manyfestation authored the merge commit `8c8dc8ee` only. Parent of `7a264039` is `5b2a2312`, the previous pin.

<a id="minter"></a>

## Change the minter paragraph

`ba8f06e6` does not inflate `remaining` inside one split. Retained remaining plus the new remaining equals the old remaining. The second minter is the feature, not an accident.

`PublicMint.split(take, new_owner)` has no signature check. `take` must be `> 0` and `<= remaining / 2`. The retained output keeps the deposit owner and the KAS value. The added output gets `take`, the caller-chosen owner, the same `mint_amount`, the same `extension_commitment`, and caller-funded KAS. The half cap is per call, so a later split can take half of what is left. The lifecycle test builds this call with no signature, rejects `take` 13 against remaining 25, and accepts `take` 7.

`mint` stays permissionless on every deposit. The owner key does not authorize minting. It reclaims the KAS deposit: `reclaim` needs `remaining == 0` and `checkSig`, and `reclaim_into` lets the surviving deposit lead with no signature while the retiring owner signs `reclaim_delegator`.

So the integer supply allowance is conserved across one split, and the right to mint against a slice of it moves without the deposit owner's signature. That is the rule to write down. "A split cannot create a second issuer by accident" fights the code. The code creates the second issuer on purpose, and it does not ask the first owner.

The README says the new entrypoints do not yet have a full conformance suite. Three lifecycle tests is the coverage named there.

<a id="seed"></a>

## Change "token seeding"

`TokenSeed` does not create the initial token supply. `create` requires `amount == 0`, a supported scheme pair, and the seed's `extension_commitment`. It recreates the seed and emits one zero-token KCC20 output. The caller funds that output. The supply comes from `PublicMint.mint`.

The README says a deployment should start with one `PublicMint`, at least one `TokenSeed`, and no initial token balances. Seed `split(new_owner)` is permissionless and has no half cap. Seed `reclaim` consumes a retiring seed and recreates the leader, so every transition leaves a seed. A new genesis would be a different covenant id. Seeds are how a stranger pays to create a receiving UTXO. They are not how the token amount enters the world.

An amount-threshold guard of zero on that new UTXO lets borrowed receive add any positive amount later. The owner bytes are whatever `create` was given. Creation spends the caller's KAS, not the named owner's.

<a id="commits"></a>

## What each commit is, from the files

`7a264039` documents the scheme check. The unsigned upper bound was already at `5b2a2312` (owner `<= 0x04`, borrow `<= 0x03`). The diff in `kcc20.ag` is the comment. It says `signed(0xff)` is -127 and `signed(0x80)` is negative zero, so a signed compare admits `0x80` even with `>= 0`. Both operands stay `unsigned()`. `transfer_validates_output_schemes_across_full_byte_domain` loops `0..=255` for `owner_scheme` and `borrow_scheme` on an output. Valid bytes must build. Every other byte must fail in the VM at input 0. That is the test the note asked for. It was added here. The check was already the unsigned bound.

`ebcba45d` is the threshold commit the note describes. On the borrow path, a negative threshold becomes zero, and the increase must be strictly greater than that. The test `threshold_borrow_clamps_nonpositive_thresholds_to_zero` includes a zero magnitude whose byte 7 is `0x80`, the negative-zero encoding, and requires a strictly positive increase. `threshold_uses_only_the_first_eight_guard_bytes` sets the other 24 bytes to `0x42`. `normal_transfers_accept_negative_threshold_guards` builds a normal transfer of a negative guard. The commit message says both KCC20 script variants shrank by 106 bytes and 50 tests passed. This desk did not re-run that.

`ba8f06e6` is the split, reclaim, and `TokenSeed` commit. See the two sections above. The commit message says 53 tests passed. This desk did not re-run that.

`a0d6a8c3` (7 Oct 08:38Z) makes `KCC20PublicMint` the only app and starts tracking `fixtures/public-mint/` (SIL, artifact, manifest). Temporary output stays in `build/`. That is the ABI-drift pin the note describes. Argent lays state out in declaration order. The names do not save a swapped field.

`9e0fc90e` moves the Argent pin from `94f249a7` (8 Sep, #59, before rules 5 and 6) to `232c6ee6` (#67, 5 Oct). The lock's Silverscript rev moves from `c7d17a15` to v1.0.0 `3ed9733`. The rusty-kaspa pin stays `a41a333b` the whole way. `a41a333b` is the git rev the v1.0.0 compiler stack uses. It is not the rusty-kaspa v2.1.0 tag `01b532e8`. This commit is the first in the reference repo whose generated SIL contains

```
require(OpCovOutputCount(gen__cov_id) == OpAuthOutputCount(this.activeInputIndex));
```

commented "leader authorizes all covenant continuations (rule 5)", on KCC20, PublicMint, and TokenSeed. The `.ag` sources do not contain that line. Argent's own commit for rules 5 and 6 is `e76ee07` (#63, 14 Sep). The reference started emitting it when the pin jumped forward and the fixtures were regenerated. The note is right that a fixture change in this push can be a spec fix or a compiler change. Here the `.ag` transfer function did not gain the check by hand. The compiler pin and the Silverscript lock did.

`28dbbe45` moves Argent from `232c6ee6` to `9a9f4b10` and regenerates fixtures. `git diff 9e0fc90e 28dbbe45 -- contracts/kcc20.ag` is empty. The `.ag` signature was already `transfer(KCC20State[], byte[])` and `transfer_delegator(byte[])`. Argent #68 (`9a9f4b10`, 7 Oct 08:24Z, and that commit is argent master) embeds current-actor template lengths as fixed-width constants and drops the hidden length arguments. The commit message on the reference says the scripts shrink by 33 bytes for KCC20, 14 for PublicMint, and 8 for TokenSeed, and that the README note about a non-standard ABI was removed. Tests now pin the parameter names and the dispatch tags.

At `8c8dc8ee` the pinned artifact says:

| Entry | Params | Tag |
| --- | --- | --- |
| `transfer` | `next_states`, `witness` | `79c71c23` |
| `transfer_delegator` | `witness` | `fd3ef14a` |

Field order after `gen__kcc20_template`: amount, owner, owner_scheme, borrow_scheme, borrow_guard, extension_commitment.

Unkeyed BLAKE3, first four bytes, recomputed this pass:

- `transfer({int,byte[32],byte,byte,byte[32],byte[32]}[],byte[])` → `79c71c23`
- `transfer_delegator(byte[])` → `fd3ef14a`

Those match the artifact and the test `kcc20_dispatch_tags_match_spec_vectors`. The test also compares the compiled tag to the hash. This desk hashed the strings and read the artifact. It did not execute the test. A wallet built against the pre-`28dbbe45` artifact should diff the artifact. The `.ag` text of the transfer entry did not move in that commit.

`8c8dc8ee` is the merge. Parents `5b2a2312` and `28dbbe45`.

<a id="argent"></a>

## Argent master

The board's Argent row was `232c6ee6` (#67, 5 Oct). Master is now `9a9f4b10` (#68, 7 Oct 08:24Z). The tags API returned length 0. The README still says the project is not yet release-ready, it is pinned to SilverScript v1.0.0, and careful early use is for someone who can review the generated `.sil`. #68 was not compiled on this desk. The reference pin and argent master are the same commit. That does not make Argent tagged.

<a id="left"></a>

## Left as they were

Manyfestation/kcc20-live `main` is still `50374a64` (9 Sep). This pass did not re-read that `.ag`. The older pin stands: `borrow_guard` before `borrow_scheme`, and a keyed public-key hash. Do not treat that tree and this reference as one program.

The 4 Oct desk run, 47 passed and 0 failed, was on `5b2a2312`. It does not cover `8c8dc8ee`. The tree now has 54 `#[test]` functions by a source count. Commit messages along the way say 50, then 53. Nobody on this desk ran `cargo test` on the new head.

P2PKH in this reference is still unkeyed `blake3(public_key)`. That matches merged KCC-2. `kcc-0020.md` on kccs main still writes `P2PKHHash(pubkey)`, which KCC-2 does not define. This push did not edit that file.
