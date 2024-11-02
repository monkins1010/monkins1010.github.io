---
label: Liquality
order: 2000
tags:
    - General
---
# Liquailty (metamask style plugin)

Liquailty web extension wallet made by https://www.liquality.io/ is an open source multi blockchain wallet that allowed swaps and you to store, Ethjerum, Bitcoin and other block chain assests.

Work was done by Mike Jr here to get the wallet working on Verus:

https://github.com/michaeltout/wallet/commits/dev/

https://github.com/liquality/wallet

![](/liquailtyvrsc.png)

## Further Work

The wallet at: 

https://github.com/liquality/wallet
https://github.com/liquality/wallet-core

on its last release had a big overhaul from what Mike Jr originally Forked, so to continue this work it is suggested that we:

-  Fork the latest release of Liquality wallet from https://github.com/liquality/wallet
-  look at the changes make by Mike Jr to get it working
    - Study the commit at https://github.com/michaeltout/wallet/commits/dev/
    - Integrate https://github.com/VerusCoin/BitGoJS.git#utxo-lib-verus into the project
    - Add VRSC as a Coin
- Assess the design how it works
- Createa  list of new features

## New feeatures to Liquailty

- Send / Receive with VerusID's
- Login with VerusID
- Revoke and Recover
- Verus DeFi
- Adding PBaaS Chains
- ID Provisioning
- Allow users to recieve objects to store in their ID's