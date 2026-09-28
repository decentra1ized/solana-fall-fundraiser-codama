## Versions
anchor-cli 1.1.2 · node v24.10.0 · @codama/cli 1.6.3 · @codama/renderers-js 2.5.0 · @solana/kit 8.3.0

## TODO 3
Required: fundraiser, vault. Optional: contributorAccount, contributorAta, tokenProgram, systemProgram. `contribute` seeds the fundraiser PDA on `fundraiser.maker`, a field of the account being derived, so the finder would need the account to find the account. The vault is seeded on `fundraiser` plus `fundraiser.mint_to_raise`, so it also needs a field that only exists inside the fundraiser account data. `initialize` seeds the same PDA on the `maker` signer, which the caller already has in hand, so there it is optional.

## Bonus
attempted

## One thing that surprised me
The sync builder only fills in constants (tokenProgram, systemProgram) while the async builder fills in PDAs too — optionality is decided purely by whether every seed is either a constant or another input the caller passed, not by whether the account is a PDA at all.
