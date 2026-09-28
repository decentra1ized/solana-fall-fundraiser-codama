# NOTES

## Versions

- anchor-cli 1.1.2
- solana-cli 4.1.2 (README suggests 3.1.10; the build and `anchor test` both work against this)
- node v24.10.0
- npx codama --version → 1.6.3
- @codama/cli 1.6.3 · @codama/nodes-from-anchor 1.5.6 · @codama/renderers-js 2.5.0
- @solana/kit 8.3.0 · @solana/program-client-core 8.3.0
- @coral-xyz/anchor 0.32.1 · @solana/web3.js 1.99.0

## TODO 3

Required: `fundraiser`, `vault`. Optional: `contributorAccount`, `contributorAta`,
`tokenProgram`, `systemProgram`.

The rule is not "is it a PDA?" — all of them are — it is "can the finder build every
seed out of what the caller already handed over?". `contributorAccount` seeds off
`[b"contributor", fundraiser, contributor]` and `contributorAta` off
`[contributor, TOKEN_PROGRAM, mint_to_raise]`: addresses and constants that are either
direct inputs to the instruction or fixed program ids, so Codama derives them from the
call itself (`fundraiser` is found first, then everything else). `fundraiser` seeds off
`[b"fundraiser", fundraiser.maker]` — the second seed is a *field of the account being
derived*, so a finder would need the account in order to find the account; it can only
use inputs it already has, so you must supply `fundraiser`. `vault` is the same story one
level up: it is the ATA `[fundraiser, TOKEN_PROGRAM, fundraiser.mint_to_raise]`, and
`fundraiser.mint_to_raise` lives inside the fundraiser account, which would mean fetching
an account to derive an address for it. `initialize` seeds the same `fundraiser` account
on `[b"fundraiser", maker]`, and `maker` is an explicit account of that instruction, so
there it is optional — and its `vault` is an ATA of `fundraiser` + `mint_to_raise`, both
explicit there, so optional too.

This is a program-design choice as much as a Codama limitation: `contribute` drops `maker`
from its account list (one fewer account in every transaction) and pays for it by making
its PDA underivable in every client. `refund` still takes `maker` explicitly and seeds
`fundraiser` off `maker.key()`, so in the generated `RefundAsyncInput`, `fundraiser` is
optional again.

## Bonus

Attempted, and green: the `getContributeInstructionAsync` instruction was converted with
`tests/helpers/kit-adapter.ts` and sent through `provider.sendAndConfirm`; the vault grew
by exactly `AMOUNT`.

## One thing that surprised me

`anchor test` reported "0 passing" on this machine: the `test` glob in `Anchor.toml`
(`tests/**/*.ts`) is expanded by `/bin/sh` before mocha ever sees it, and `sh` here is
dash, where `**` behaves like `*` — so the command silently became
`ts-mocha ... tests/helpers/kit-adapter.ts` and only that file ran. Quoting the glob in
`Anchor.toml` hands the pattern to mocha, which does the `**` matching itself, and the
whole suite runs again.
