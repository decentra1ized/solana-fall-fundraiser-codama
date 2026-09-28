# Codama client notes

## Versions

- Anchor CLI: 1.1.2
- Solana CLI: 3.1.10
- Node.js: 24.21.0
- Codama CLI: 1.6.3
- JS renderer: 2.5.0; @solana/kit: 8.3.0
- Test validator: `anchor test --validator legacy`

## TODO 3: which accounts can be resolved?

The `contribute` async builder requires `contributor` (signer), `mintToRaise`,
`fundraiser`, `vault`, and the `amount` argument. It derives `contributorAccount`
from the fundraiser and contributor addresses, derives `contributorAta` from
the contributor and mint, and supplies the fixed token and system program IDs.

`contribute` seeds `fundraiser` using `fundraiser.maker`, a field *inside the
account being located*: resolving its address that way would first require
knowing and fetching the same account. Its `vault` ATA is likewise specified
in the IDL using `fundraiser.mintToRaise`, another field inside that account,
so the generator does not infer it from the separately supplied mint address.
`initialize` instead seeds `fundraiser` from the explicit `maker` account;
because that seed is already available to the builder, its `fundraiser` input
is optional. Taking `maker` explicitly in `contribute` could make the PDA
derivable, but would add an account to that instruction.

## Bonus

Attempted: the async-built Kit instruction is converted with the existing
`tests/helpers/kit-adapter.ts`, sent via Anchor's provider, and the vault's
token balance is checked before and after.

## One thing that surprised me

Before changing `Anchor.toml`, `anchor test` printed “0 passing” while
appearing successful: the shell expanded the unquoted `tests/**/*.ts` glob to
only the nested helper. Quoting the glob lets Mocha discover every suite.
