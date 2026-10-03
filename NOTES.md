## Versions
anchor-cli 1.1.2
node v20.20.2
codama 1.6.3

## TODO 3
Required: fundraiser, vault. Also passed mintToRaise because the instruction needs the mint and it is not a PDA Codama can derive here. Optional: contributorAccount, contributorAta, tokenProgram, systemProgram.

contribute seeds the fundraiser PDA on fundraiser.maker, a field of the account being derived, so the finder would need the account to find the account. initialize seeds the same PDA on the maker account, which the caller already has, so there maker is optional.

vault is the fundraiser ATA. Deriving it needs the mint, and the mint is not a seed Codama can read without the fundraiser account it is still trying to find.

## Bonus
not attempted

## One thing that surprised me
The generated client uses base58 strings and bigint, not PublicKey and BN, so the same account has to be wrapped with address() before it can be compared to Anchor.
