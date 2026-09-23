# NOTES.md — Codama Challenge

## Versions

- anchor-cli: 1.1.2
- node: v25.2.1
- codama CLI: @codama/cli 1.6.3
- @codama/renderers-js: 2.5.0
- @solana/kit: 8.3.0

## TODO 3 Answer — Why fundraiser and vault had to be passed

`contributorAccount` and `contributorAta` are PDAs whose seeds are made entirely from other accounts
already present in the same instruction (`fundraiser`, `contributor`). Codama can see these seeds in
the IDL and derive the addresses at call time without any external data.

`fundraiser` is seeded by `fundraiser.maker` — a field stored inside the on-chain Fundraiser account,
not by another account address in the instruction. Codama cannot read on-chain account data at
instruction-build time, so it cannot derive `fundraiser` and must ask for it explicitly.

`vault` is seeded by `fundraiser.mint_to_raise` — again a field inside the on-chain Fundraiser account.
Same reason: Codama can only resolve seeds that come from other instruction accounts or constants, not
from fields inside accounts that require a network fetch. So `vault` must also be passed explicitly.

In short: Codama resolves what the IDL can prove from the instruction's own accounts. Anything that
needs on-chain data to compute its address stays required.
