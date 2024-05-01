---
label: Getting Started
order: 2000
categories:
    - Tutorials
tags:
    - General
---
# Getting Started

Attestations on Verus are very easy as all the signing and creation of the objects is handled by the daemon.  The daemon understands how to make a data structure that combines all the values you wish to include in the attestation and put it into a MMR that can be zero knowledge, encyrpted, signed and stored.  Here is an example of a simple proo that could be created for a user.

### Code example JS

To create a signed object we can use the Daemon to take in the values of the attestation then sign them, so the object must contain all the information that links the signed object to the user.  So e.g. it can contain

- The users ID
- The users information being attested to 
- The attestors information document type, data

This information will all be put into a MMR and signed.

So for example this attests a users ID to an attestation ID.  So in words the system will sign the data to be true that the user in the `"myapp.vrsc::attestation.user"` key has the attestataion of `"myapp.vrsc::attestation.id"` ID. 

```bash
yarn add https://github.com/VerusCoin/bitcoind-rpc.git
```

```js
// NOTE: https://github.com/VerusCoin/verusd-rpc-ts-client will replace the bitcoind for signdata TBA.
// NOTE: create a .env file in the root of the directory with the TESTNET
require('dotenv').config();
const RpcClient = require('bitcoind-rpc'); //NOTE: To allow signing of data change the L344: index.js to -> RpcClient.callspec = apisWithWallet;
const VDXF_Data = require("verus-typescript-primitives/dist/vdxf/vdxfDataKeys");

const config = {
    protocol: 'http',
    user: process.env.TESTNET_USERNAME, //daemon connection details from /vrsc(test).conf
    pass: process.env.TESTNET_PASSWORD,
    host: process.env.TESTNET_URL,
    port: process.env.TESTNET_PORT,
  };

const rpc = new RpcClient(config);

const verusdCall = (subcall, req) => {
    return new Promise((resolve, reject) => {
        let reply = { 
        error: null, 
        data: null,
        success: true 
        }
        let payload = req;
        try {
            rpc[subcall](...payload, function (err, ret) {
            if (err) {
                reply.error = err;
                reply.success = false;
                reject(reply); // reject with error
            } else {
                reply.data = ret.result;
                resolve(reply); // resolve with response
            }
            });
        } catch (e){
            reply.error = e;
            reply.success = false;
            reject(reply); // reject with error
        }
    });
};

const attestationData = {
   "address":"signingID@",
   "createmmr":true,
   "mmrdata":[
      {
         "vdxfdata":{
            [VDXF_Data.DataDescriptorKey().vdxfid]:{
               "version":1,
               "flags":0,
               "label":"myapp.vrsc::attestation.id",
               "mimetype":"text/plain",
               "objectdata":"0293480924-032943928"
            }
         }
      },
      {
         "vdxfdata":{
            [VDXF_Data.DataDescriptorKey().vdxfid]:{
               "version":1,
               "flags":0,
               "label":"myapp.vrsc::attestation.user",
               "mimetype":"text/plain",
               "objectdata":"someuser.vrsc@"
            }
         }
      }
   ]
}

const reply = verusdCall('signdata', [attestationData])
    .then((reply) => {
        console.log(reply)
    })
    .catch((e) =>{
        console.error(e)
    });
```

This will return:
```js
const signedDataReply = {
  "mmrdescriptor": {
    "version": 1,
    "objecthashtype": 5,
    "mmrhashtype": 1,
    "mmrroot": {
      "version": 1,
      "flags": 0,
      "objectdata": "34140a8b64d520d00d1c576bc605eb337450ba1a1120b06f1b0214f10cbbbb10"
    },
    "mmrhashes": {
      "version": 1,
      "flags": 0,
      "objectdata": "41d826d3c6cbbc3a96992670d2f604e959fd1a8c01410243482a4132bc18af2333d0764b303b686d2eeb6b3f0742e97783ebe2b377e29366946e3d49ed6449d5f867f41ea874996307eec9a152ba41273b2ddc8583e696"
    },
    "datadescriptors": [
      {
        "version": 1,
        "flags": 2,
        "objectdata": {
          "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv": {
            "version": 1,
            "flags": 96,
            "mimetype": "text/plain",
            "objectdata": {
              "message": "0293480924-032943928"
            },
            "label": "myapp.vrsc::attestation.id"
          }
        },
        "salt": "7bf53c7bb254304993f4543ffc3114c2450001f825764820e41573b049807cfd"
      },
      {
        "version": 1,
        "flags": 2,
        "objectdata": {
          "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv": {
            "version": 1,
            "flags": 96,
            "mimetype": "text/plain",
            "objectdata": {
              "message": "someuser.vrsc@"
            },
            "label": "myapp.vrsc::attestation.user"
          }
        },
        "salt": "1e802aae639a84156178f4e8440d9c774d5aa7fdf042b305e1af607b6f8e27a7"
      }
    ]
  },
  "signaturedata": {
    "version": 1,
    "systemid": "iJhCezBExJHvtyH3fGhNnt2NhU4Ztkf2yq",
    "hashtype": 1,
    "signaturehash": "10bbbb0cf114021b6fb020111aba507433eb05c66b571c0dd020d5648b0a1434",
    "identityid": "iRmBDWNs2WahXDAvS2TEsJyJwwHXhwcs7w",
    "signaturetype": 1,
    "signature": "AgWwYAAAAUEfttjAE8/Q5g5KPaUcxcCukBrJEnfJPfd0pissIU03MjNG/1ZJ4Ww1WqJHODZ2edFzQ6JFzeZ36rRCSpQZHa3tWg=="
  },
}
```

## Storing the data in an ID
Get a vdxfid for the main object to be stored in the ID
```js
./verus -chain=vrsctest getvdxfid myzoo.vrsc::staff.zookeepers
{
  "vdxfid": "iKNUEu3YBmBPgiR3fRdGg7cTC3mmx8Jqap",
  "indexid": "xQCahhUd35Q4JtJ5X7HReW8zDhnnw1gm9G",
  "hash160result": "ad3d88716bcb54aa55fec90e2a72570c03da5cae",
  "qualifiedname": {
    "namespace": "i9TAXTHi3UzYfifTW7BW4YR51T2Z3zEeSp",
    "name": "myzoo.vrsc::staff.zookeepers"
  }
}
```

```js
// As before setup the RPC server then use the reply from the daemon to put that into the contentmap of an ID.
const VDXF_Data = require("verus-typescript-primitives/dist/vdxf/vdxfDataKeys");

const zookeepersVdxfid = "iKNUEu3YBmBPgiR3fRdGg7cTC3mmx8Jqap";

const updateIdentityObj = {
    name: "idnameto update",
    "contentmultimap":{
        [zookeepersVdxfid]: {
            [VDXF_Data.MMRDescriptorKey().vdxfid] : signedDataReply.mmrdescriptor,
            [VDXF_Data.SignatureDataKey().vdxfid] : signedDataReply.signaturedata
        }
    }
}

const reply = verusdCall('updateidentity', [updateIdentityObj])
    .then((reply) => {
        console.log(reply)
    });
```