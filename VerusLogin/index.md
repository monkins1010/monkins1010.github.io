# VerusID Login Client

The Verus Login Consent Client provides a passwordless login to services using the VerusID avaliable on the Verus Blockchain.  For more information on the VerusID checkout [the VerusID website.](https://docs.verus.io/verusid/). The library is a TypeScript client designed to utilize VerusID functionality in a lite client, providing functionalities for creating and verifying digital signatures, handling login consents, managing VerusPay invoices, and more, using VerusID's blockchain technology.

## Features

- **VerusID Interface**: Main class providing methods to interact with VerusID.
- **Blockchain Interaction**: Functions to get current blockchain height and chain ID.
- **Message and Hash Signing**: Methods to sign messages and hashes with VerusID signatures.
- **Login Consent Handling**: Tools to create, sign, and verify VerusID login consent requests and responses.
- **VerusPay Invoice Management**: Functions to create, sign, and verify VerusPay v3 invoices.