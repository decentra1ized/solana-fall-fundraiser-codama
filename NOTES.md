## Todo 3

The `fundraiser` and `vault` are optional during `initialize` stage because codama can derive fundraiser from maker and vault from fundraiser. But for `contribute`, we need it because `fundraiser` used `fundraiser.maker` as seed which is data in `fundraiser` account and codama cannot know that without having fundraiser, similar for `vault` codama need to decode `mint_to_raise`. 

## Tool Versions

`anchor` = 1.1.2
`solana` = 3.1.10
`node` = 24.10.0
`surfpool` = 1.6.0


## Bonus
attempted

## What surprised me?

Todo 3. It was a good thing to get cleared. Knowing what codama can do.