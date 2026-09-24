## Versions

anchor-cli 1.1.2 · node 26.10.0 · @codama/cli 1.6.3

## TODO 3

Required: fundraiser, vault. Optional: contributorAccount, contributorAta,
tokenProgram, systemProgram. `contribute` seeds the fundraiser PDA on
`fundraiser.maker`, a field of the account being derived, so the finder would
need the account to find the account. `initialize` seeds it on the `maker`
account, which the caller has, so there it is optional.

The vault has the same problem: its ATA seeds use `fundraiser.mint_to_raise`,
which is stored in the fundraiser account. In contrast, contributorAccount and
contributorAta use values already supplied directly to the instruction, so the
generated client can derive them locally.

## Bonus

attempted

## One thing that surprised me

Having PDA seeds in the IDL is not sufficient by itself for automatic
derivation; every seed value must also be available to the generated client
without first fetching the account it is trying to locate.
