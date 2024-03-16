---
label: Installing verusid-ts-client
order: 2000
categories:
    - Tutorials
tags:
    - VerusID Login
---
# Installing verusid-ts-client

## Prerequisites

The verusid-ts-client is installed using [`yarn`](https://classic.yarnpkg.com/en/docs/install/) CLI.


| Package Manager | Supported Platforms |
| --- | --- |
| [`yarn`](https://classic.yarnpkg.com/en/docs/install/) | [!badge text="Mac" variant="light"] [!badge text="Win" variant="primary"] [!badge text="Linux" variant="dark"]
!!!danger Only yarn is supported at the moment
!!!
---

## Install

To install the verusid-ts-client in your javascript project run

```bash
yarn add https://github.com/VerusCoin/verusid-ts-client.git
```
---
## Usage

### Initializing the VerusID Interface

```typescript
import VerusIdInterface from 'verusid-ts-client';
import { AxiosRequestConfig } from 'axios';

const config: AxiosRequestConfig = {/* Axios configuration */};
const verusIdClient = new VerusIdInterface('root-system-currency-id', 'http://your-verusd-node.com', config);
```
This will allow the `verusid-ts-client` to connect to the verus blockchain.  You can connect it directly to a node that you hose or to w web hosted API endpoint e.g. `https://api.verus.services`