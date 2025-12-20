For this exercise, the name to register is STONE.  It is case insensitive. STONE = stone = StONe ...

The goal of this exercise is to use the pbaas chain `vrsctest` to create an ID from the CLI.

The `STONE@` identity will have only a primary address at creation.

There will be no private address, no additional revocation or recovery addresses.


First step is `registernamecommitment`
```
verus -chain=vrsctest registernamecommitment "STONE" "RJP1qzY2x8VXwtaSqMNLrhCpGZu2pLwYSY" "komodefi@"
```
This responds with a name reservation json which makes up part of the `registeridentity` command. Copy+Paste this output for using later.

```
{
  "txid": "f9e0ca5658380ec9acb2434f058374690b1d252b9e93dcc458205965aa198ab7",
  "namereservation": {
    "version": 1,
    "name": "STONE",
    "parent": "iJhCezBExJHvtyH3fGhNnt2NhU4Ztkf2yq",
    "salt": "5939fdbd9de44c9656e1319dccaa3df22e27e90e87fd99c3bb0bb163e4ab5b92",
    "referral": "iDh9YykH3bDAW6o11PVzp4MBP6pyfiLDQC",
    "nameid": "iHZB4DPHsT2Z1XaiqkTW7BJraD4oTEpzbL"
  }
}
```

This transaction must be put into a block before continuing.

Then we use `registeridentity`

```
verus -chain=vrsctest registeridentity '{"txid": "f9e0ca5658380ec9acb2434f058374690b1d252b9e93dcc458205965aa198ab7","namereservation": {"version": 1,"name": "STONE", "parent": "iJhCezBExJHvtyH3fGhNnt2NhU4Ztkf2yq","salt": "5939fdbd9de44c9656e1319dccaa3df22e27e90e87fd99c3bb0bb163e4ab5b92","referral": "iDh9YykH3bDAW6o11PVzp4MBP6pyfiLDQC","nameid": "iHZB4DPHsT2Z1XaiqkTW7BJraD4oTEpzbL"}, "identity": { "name": "STONE", "primaryaddresses": ["RJP1qzY2x8VXwtaSqMNLrhCpGZu2pLwYSY"], "privateaddress": "", "minimumsignatures": 1}}'
```
for the resulting txid `35e0570a12e9454397a51b528f20f2df3cbe02cd223c513a5a5d294de5dad4f1`

After waiting for the `35e0570a12...` transaction to be confirmed, we will have the `STONE@` identity in our wallet. We can check it out by using `getidentity` to view its attributes.
```
verus -chain=vrsctest getidentity "STONE@"
{
  "friendlyname": "STONE.VRSCTEST@",
  "fullyqualifiedname": "STONE.VRSCTEST@",
  "identity": {
    "version": 3,
    "flags": 1,
    "primaryaddresses": [
      "RJP1qzY2x8VXwtaSqMNLrhCpGZu2pLwYSY"
    ],
    "minimumsignatures": 1,
    "name": "STONE",
    "identityaddress": "iHZB4DPHsT2Z1XaiqkTW7BJraD4oTEpzbL",
    "parent": "iJhCezBExJHvtyH3fGhNnt2NhU4Ztkf2yq",
    "systemid": "iJhCezBExJHvtyH3fGhNnt2NhU4Ztkf2yq",
    "contentmap": {
    },
    "contentmultimap": {
    },
    "revocationauthority": "iHZB4DPHsT2Z1XaiqkTW7BJraD4oTEpzbL",
    "recoveryauthority": "iHZB4DPHsT2Z1XaiqkTW7BJraD4oTEpzbL",
    "timelock": 0
  },
  "status": "active",
  "canspendfor": true,
  "cansignfor": true,
  "blockheight": 454082,
  "txid": "d9ef76d2080b3f368666dc207dfded67be4abb0c58e13a378e8e74cd0953886f",
  "vout": 0
}
```
