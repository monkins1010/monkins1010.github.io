---
label: Storing files on Chain
order: 2000
categories:
    - Tutorials
tags:
    - General
---
# Storing a File on chain

!!!danger Please make sure you start your daemon with `-enablefileencryption` in the `VRSCTEST.conf`
!!!

To store data on chain it is by default stored encrypted so to encrypt it on chain you need a Z address.  To store a PDF you can do the following.

```bash
{
./verus -chain=vrsctest sendcurrency "*" '[{
      "address":"zs1vme2mq0sfc80jjv923485hmgknw93fn64g0she53a7kdd42myu6pafp3ufajueyep4djg6qkjzf", 
      "amount":0, 
      "data":{
         "filename":"/home/chris/myDocument.pdf"
         }
      }]'
}
```

This will return a transaction opid.  Using this you can get the transaction hash e.g.

`a6afefc0d0b00733651b6be7dc7ac25f4df206f8272f5e635d485e78088a3b3b`

### To retrieve the data

To get the data off the chain you will need two things

- A way of decrpting it e.g. the viewing key
- Where the data is in an object.

For the above example if we want to recieve the document you can look at the transaction:

```bash
./verus -chain=vrsctest z_listreceivedbyaddress zs1vme2mq0sfc80jjv923485hmgknw93fn64g0she53a7kdd42myu6pafp3ufajueyep4djg6qkjzf
```

This will return the transaction like this:

```bash
  "txid": "a6afefc0d0b00733651b6be7dc7ac25f4df206f8272f5e635d485e78088a3b3b",
    "amount": 0.00000000,
    "memo": [
      {
        "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv": { // this is the datadescriptor iaddress
          "version": 1,
          "flags": 0,
          "objectdata": {
            "iP3euVSzNcXUrLNHnQnR9G6q8jeYuGSxgw": {
              "type": 0,
              "version": 1,
              "flags": 1,
              "output": {
                "txid": "0000000000000000000000000000000000000000000000000000000000000000",
                "voutnum": 0
              },
              "objectnum": 0,
              "subobject": 0
            }
          }
        }
      },
```

Next you need the viewing key for the Z_address that you encrypted the data to.

```bash
./verus -chain=vrsctest z_exportviewingkey zs1vme2mq0sfc80jjv923485hmgknw93fn64g0she53a7kdd42myu6pafp3ufajueyep4djg6qkjzf
```


Then from the datadescriptor above (the JSON after the i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv), you should copy that data and put that data in the datadescriptor section below.

Also you should put in the viewing key for the data as `"evk":"hex"`.
The `"txid":"hex"` and `"retrieve":true`


```bash
./verus -chain=vrsctest decryptdata '{"datadescriptor":{
          "version": 1,
          "flags": 0,
          "objectdata": {
            "iP3euVSzNcXUrLNHnQnR9G6q8jeYuGSxgw": {
              "type": 0,
              "version": 1,
              "flags": 1,
              "output": {
                "txid": "0000000000000000000000000000000000000000000000000000000000000000",
                "voutnum": 0
              },
              "objectnum": 0,
              "subobject": 0
            }
          }
        },
      "txid":"a6afefc0d0b00733651b6be7dc7ac25f4df206f8272f5e635d485e78088a3b3b",
      "retrieve":true,
      "evk":"zxviews1q00pzmf8qqqqpqqdc4ttkrn3c52kek24tggfhspqrf0vyv70x49vqan72253y3dgn02ezzax0yusldz4j3xzpeay7h02ujjtpdksy83gvd3gg4jc7t67d5mmrc76astjlejapk6jwjma0qd7jawqk3tk26u55n2aqw9pn83ytya70ne70mzfy9h97ksu9d5cze9pqk27mwhffv5mn65ck6jj739q47urpa32rhlug6ty6h7zfgxf3nht46urfunvamnjlg3vtudsmqs37etmt"
      }'
```

This will give out the pdf document as hex:

```json
[
  {
    "version": 1,
    "flags": 2,
    "objectdata": "64736c6b6661736b6c6a66616473666a6b616864736668616473686b6a686a0a...", // PDF document as hex
    "salt": "786c27fc7f0014a6c35b94f6bb289b4624dac254ee0078c870a4eba0cd6d7433"
  }
]
```

