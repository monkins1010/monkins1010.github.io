---
label: Identity Storage
order: 2000
categories:
    - Tutorials
tags:
    - General
---
# Storing Data in a ID

Verus ID's have the ability to hold upto ~4kb data in a special area called the `contentmap`.  This is a map where data is stored like this:

```json
{
    "vdxfid":  data,
    "vdxfid":  data
}
```

If for example you made a vdxfid key which meant something to you e.g. myId.vrsc::shop.  This namespace can be turned into a vdxfid or 20byte hash of the namespace turned into an vdxfid.

###  Creating a vdxfid

```bash

./verus getvdxfif "myId.vrsc::shop"

#returns

{
  "vdxfid": "iMvTg2HGhKKGYMqtapvRyfZNahbzmD9R3b",
  "indexid": "xSka8piMYdXwAXivSWaax45ucMd1dbMnRJ",
  "hash160result": "5756341688b6cd130ec192042cdf2957d8f059ca",
  "qualifiedname": {
    "namespace": "i6bVzhjEdMY4DgyLE2XHGKHfpm4FaGPfiL",
    "name": "myId.vrsc::shop"
  }
}
```
So the vdxfid would be `iMvTg2HGhKKGYMqtapvRyfZNahbzmD9R3b`.

The name `myId.vrsc::shop` is made up of  `youridyouown` `.vrsc::` `keyA.KeyB.KeyC` and so on.  So some examples for a shop would be

`myId.vrsc::shop.type`
`myId.vrsc::shop.address`
`myId.vrsc::shop.value`


### Putting it together

When looking at the datain an ID it might only hold one piece of information e.g. a name. Or it may hold 1000's of updates of data, so it is important to make sure the data is indexed so it can be filtered and avoid large lists of data returned.

You can define keys for your `shop` project and them use them and search for them by a master key e.g.

```json

{ "iMvTg2HGhKKGYMqtapvRyfZNahbzmD9R3b": [{  // myId.vrsc::shop
        "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv":{ // this is a dataDescriptor key
               "label":"i3esdByX2PKx5vJiuNrRb61KAKqsBEMxac", // myId.vrsc::shop.type
               "mimetype":"text/plain",
               "objectdata":{
                  "message":"Bookshop"
               }
            }
        },
        {
        "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv":{
               "label":"i7Wgy2oRrJdjxhcRxpmzb9zCKWMbA5UZqw", // myId.vrsc::shop.address
               "mimetype":"text/plain",
               "objectdata":{
                  "message":"15 Sea View, Miami, 132523"
               }
            }
        },
        {
        "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv":{
               "label":"i8bsRok21QeUrnxP97JkdaoYzyBj4HdSix", // myId.vrsc::shop.value
               "mimetype":"text/plain",
               "objectdata":{
                  "message":"$100,000"
               }
            }
        }]
}
```
The `i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv` or `DataDEscriptor` vdxfid is an object that can have may types of data


```json
{"i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv":{
        "label":"i8bsRok21QeUrnxP97JkdaoYzyBj4HdSix", // string limited to 64 utf-8 bytes
        "mimetype":"text/plain",  // string of mime limited to 128 bytes
        "objectdata":{      
            "message":"string"  //limited to about 2000 bytes
        },
        "salt": "string of hex data normally 32 bytes", // encryption public key, data only present if encrypted or data referenced by unencrypted link is encrypted
        "epk": "string",     // encryption public key, data only present if encrypted or data referenced by unencrypted link is encrypted
        "ivk": "string",  // incoming viewing key, optional and contains data only if full viewing key is published at this encryption level
        "ssk": "string"     // specific symmetric key, optional and only to decrypt this linked sub-object
    }
}]
```
In this example we are using unencryted data into the contentmultimap, but in more advance topics see `chain storage data` this allows alarge  files and is encrypted by default, the salt, epk, ivk and ssk cvalues are calulated for you if you need them.

### Updating the ID

```bash
./verus updateidentity '{"name":"yourid", "contentmultimap":{ "iMvTg2HGhKKGYMqtapvRyfZNahbzmD9R3b": [{
        "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv":{
                "version": 1,
               "label":"i3esdByX2PKx5vJiuNrRb61KAKqsBEMxac",
               "mimetype":"text/plain",
               "objectdata":{
                  "message":"Bookshop"
               }
            }
        },
        {
        "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv":{
                "version": 1,
               "label":"i7Wgy2oRrJdjxhcRxpmzb9zCKWMbA5UZqw",
               "mimetype":"text/plain",
               "objectdata":{
                  "message":"15 Sea View, Miami, 132523"
               }
            }
        },
        {
        "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv":{
               "version": 1,
               "label":"i8bsRok21QeUrnxP97JkdaoYzyBj4HdSix",
               "mimetype":"text/plain",
               "objectdata":{
                  "message":"$100,000"
               }
            }
        }]
}}'
```

Retrieving the data uses the function `./verus getidentitycontent  "name@ || iid" (heightstart) (heightend) (txproofs) (txproofheight) (vdxfkey) (keepdeleted)`

```
"name@ || iid"                       (string, required) name followed by \"@\" or i-address of an identity\n"
"heightstart"                        (number, optional) default=0, only return content from this height forward, inclusive\n"
"heightend"                          (number, optional) default=0 which means max height, only return content up to this height,\n"
                                                             inclusive. -1 means also return values from the mempool.\n"
"txproofs"                           (bool, optional) default=false, if true, returns proof of ID\n"
"txproofheight"                      (number, optional) default=\"height\", height from which to generate a proof\n"
"vdxfkey"                            (vdxf key, optional) default=null, more selective search for specific content in ID\n"
"keepdeleted"                        (bool, optional) default=false, if true, return deleted items as well\n"
```
So to get our example we can do

```bash
./verus getidentitycontent "name@"
```

