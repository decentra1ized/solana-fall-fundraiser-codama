## Versions
anchor-cli 1.1.2 · node v24.18.0 · @codama/cli 1.6.3

## TODO 3
Required: fundraiser, vault. Optional: contributorAccount, contributorAta,
tokenProgram, systemProgram. `contribute` seeds the fundraiser PDA on
`fundraiser.maker`, a field of the account being derived, so the finder
would need the account to find the account. `initialize` seeds it on the
`maker` account, which the caller has, so there it is optional. The vault is
required for the same reason: its ATA seeds use `fundraiser.mint_to_raise`.

## Bonus
not attempted

## One thing that surprised me
`anchor test` initially ran 0 tests: the unquoted `tests/**/*.ts` glob in
Anchor.toml is expanded by `sh`, which has no `**`, so it only matched
`tests/helpers/kit-adapter.ts`. Quoting it lets mocha expand the glob itself.
