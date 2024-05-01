---
label: Main Keys
order: 2000
categories:
    - Tutorials
tags:
    - General
    - VDXF 
---
# Using VDXF Keys

VDXF Keys are an unique iaddress that when found by applications tell the software what to do with the data. e.g. if you have a JSON object like this

```json
{
    "iK7a5JNJnbeuYWVHCDRpJosj3irGJ5Qa8c":"I'm a string"
}
```
Then you might guess that `iK7a5JNJnbeuYWVHCDRpJosj3irGJ5Qa8c` is a datatype string to be expected.

This is because the qulaified name of the above key is

```bash
./verus getvdxfid "vrsc::data.type.string"  
{
  "vdxfid": "iK7a5JNJnbeuYWVHCDRpJosj3irGJ5Qa8c",
  "indexid": "xPwgY6oPdusaAgNK3u5yHCQG5NsHEcBpi5",
  "hash160result": "e5c061641228a399169211e666de18448b7b8bab",
  "qualifiedname": {
    "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
    "name": "vrsc::data.type.string"
  }
}
```

## More advanced keys

An object that expects many inputs can also be represented with a vdxfid e.g.

From: [verus-typescript-primitives](https://github.com/monkins1010/verus-typescript-primitives/blob/65ada69cca90f8d04992fdb17a5fd881acb8e715/src/vdxf/vdxfDataKeys.ts#L324)
```js

export function MMRDescriptorKey(): VDXFKeyInterface {
    return {
      "vdxfid": "i9dVDb4LgfMYrZD1JBNP2uaso4bNAkT4Jr",
      "indexid": "xETbgPVRXyaDUj639s2Y1J7QpicP4DvZMt",
      "hash160result": "97273a4c02d6be002f8d69c3979616732ba68243",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.mmrdescriptor"
      }
    }
  }
```

A Datadescriptor is like this:

```ts
class DataDescriptor {
  version: BigNumber; // Version Default 1
  flags: BigNumber;   // Flags indicating what items are present in the object
  objectdata: Buffer; // either hex string, direct data or serialized UTXORef +offset, length, and/or other type of info for different links
  label: string;      // label associated with this data
  mimeType: string;   // optional mime type
  salt: Buffer;       // encryption public key, data only present if encrypted or data referenced by unencrypted link is encrypted
  epk: Buffer;        // encryption public key, data only present if encrypted or data referenced by unencrypted link is encrypted
  ivk: Buffer;        // incoming viewing key, optional and contains data only if full viewing key is published at this encryption level
  ssk: Buffer;        // specific symmetric key, optional and only to decrypt this linked sub-object
}
  ```

So you can populate this item as follows

```json
{
"i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv": {
    "version": 1,
    "objectdata": "Polar Bears",
    "mimetype": "text/plain",
    "label": "myzoo.vrsc::enclosure.name",            
    "salt": "a8edd6a19b359365c797bc27164aeb7f12a2d1cfc4f2786871564926dbb5b96f"
    }
}
```

### Code example JS

```ts

const { DataDescriptor } = require("verus-typescript-primitives/dist/vdxf/classes/DataDescriptor.js");

const dataDescriptorObject = new DataDescriptor();

dataDescriptorObject.fromJson({
    "version": 1,
    "objectdata": "Polar Bears",
    "mimetype": "text/plain",
    "label": "myzoo.vrsc::enclosure.name",            
    "salt": "a8edd6a19b359365c797bc27164aeb7f12a2d1cfc4f2786871564926dbb5b96f"
});
```

## The popular main keys used by the Daemon are:

```json 
{
      "vdxfid": "iBXUHbh4iacbeZnzDRxishvBSrYk2S2k7t",
      "indexid": "xGMakQ89ZtqGGjg257csr6SiUWZksGmjWp",
      "hash160result": "2e97a8bba443773812341e1d761530d3bba04f58",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.byte"
      }
}
{
      "vdxfid": "iDtTv3wf1Vk3M2Y46RjLPKtttx5hydwtY1",
      "indexid": "xJiaNrNjroxhyCR5x7PVMiRRvc6ipg6N9g",
      "hash160result": "ee334ebd432db0b24cc2702eda61c28ff44d3872",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.int16"
      }
    }
{
      "vdxfid": "iHn7urT2yVfS7pQn6WGAmCVWh4HBLV24n3",
      "indexid": "xNcENet7pot6jzHoxBvKjb23iiJCGpekDk",
      "hash160result": "5cfc322d2a216145f7b82714115e7953269de59c",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.uint16"
      }
    }
{
      "vdxfid": "iHpLPprRDv3H5H3ZMaJ9nyHFzkG9xJWZDb",
      "indexid": "xNeSrdHW5EFwhSvbDFxJmMoo2QHAtoEaEM",
      "hash160result": "3e9ba478b23b13232f28d21051d907ce8fdd509d",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.int32"
      }
    }
{
      "vdxfid": "iKSj5zhd6cSsLudaGhtfmisRNgEM7SPFWY",
      "indexid": "xQGqYo8hwvfXy5Wc8PYpk7PxQLFMwfP7Fp",
      "hash160result": "f279818aeb4fe768956b350d1fc7216ca0e82aaf",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.uint32"
      }
    }
{
      "vdxfid": "iKB3TGi9Dg5HZ4nQAgLQAgp3tuXBaRKHpC",
      "indexid": "xQ19v59E4zHxBEfS2MzZ95LavZYCTTeuyg",
      "hash160result": "ab3705f8a7fae59786ef897b014df85fcd9533ac",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.int64"
      }
    }
{
      "vdxfid": "iPamkQf38AeGQ8z4zSsZL7t9kXMeUkYLJL",
      "indexid": "xUQtDD67yUrw2Js6r8XiJWQgnBNfNeeoUq",
      "hash160result": "bb2ae9ed3e9f400def0724937fbf65f23ef690dc",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.uint64"
      }
    }
{
      "vdxfid": "iAAwdbLyKYL39nJ1eQHaHtb75krg4mV1Lq",
      "indexid": "xF146Pn4ArYhmxB3W5wjGH7e7Qsgx9bkpj",
      "hash160result": "d97d2295d4c73f6f6f0697c8086bd822d6977549",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.uint160"
      }
    }
{
      "vdxfid": "i8k7g7z6grtGYrNZmZr5TQ872aHssXuuua",
      "indexid": "xDaE8vRBYB6wB2FbdFWERnee4EJtjbCtMM",
      "hash160result": "939b27bea698d180237c40b2194025acc673cb39",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.uint256"
      }
    }
{
      "vdxfid": "iK7a5JNJnbeuYWVHCDRpJosj3irGJ5Qa8c",
      "indexid": "xPwgY6oPdusaAgNK3u5yHCQG5NsHEcBpi5",
      "hash160result": "e5c061641228a399169211e666de18448b7b8bab",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.string"
      }
    }
 {
      "vdxfid": "iAEShwk1xjdGhaUSz3Maa2XR32o3vRuHq7",
      "indexid": "xF4ZAkB6p3qwKkMUqj1jYR3x4gp4mGz657",
      "hash160result": "503875b0dc301189a98927d3ece56c5f921c1f4a",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.vector"
      }
    }
 {
      "vdxfid": "iKMhRLX1JHQihVZx2t2pAWW2uzmK6AzwW3",
      "indexid": "xQBot8x69bdPKfSytZgy8u2ZwenKzVjR4X",
      "hash160result": "cc3ae6466006629f5105f71325bb2a19107037ae",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.bytevector"
      }
    }
{
      "vdxfid": "iJZt2fcUv1iivbfC3tuPuefabcTppQEoVq",
      "indexid": "xPPzVU3ZmKwPYmYDuaZYt3C7dGUqk939N7",
      "hash160result": "c0847f3025c408059b5a8f6a9e414a8ed8288da5",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.int32vector"
      }
    }
{
      "vdxfid": "i4qtYkFS9iNyu2AkqwoSn1xyCdfH9PUvak",
      "indexid": "x9g11YgX12beXC3nhdTbkQVWEHgJ2jqfz1",
      "hash160result": "c6219ea13884987453692cb14c72d5f6a47c020f",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.int64vector"
      }
    }
{
      "vdxfid": "iMrGhzkZq5fpWWSa1RambRySFPb7CuvKuX",
      "indexid": "xSgPAoBegPtV8gKbs7EvZpVyH3c858ZUvL",
      "hash160result": "25db70c2fcae2571f89201181bec04587e1f8fc9",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.object.currencymap"
      }
    }
 {
      "vdxfid": "iHJComZUXXGniLkDhjYprWYEN8qvQGDoam",
      "indexid": "xN8KGZzZNqVTLWdFZRCypu4mPnrwHFKbCK",
      "hash160result": "32cad57ff1dc5db4b5ba573ce01bc9c89b0d9e97",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.object.ratings"
      }
    }
{
      "vdxfid": "iJ7xdhJTJAvJubNnSJFXyA3jujzqGxjLuZ",
      "indexid": "xNx56VjY9V8yXmFpHyugwYaGwQ1rCt6J9W",
      "hash160result": "7748bfaf53dd2ff63ed5f73a41174c360f30a6a0",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.object.url"
      }
    }
{
      "vdxfid": "i91L6zwZQrkbNVMB1AZ1Z671qybexRmeVK",
      "indexid": "xDqSZoNeGAyFzfECrrDAXUdYsdcfs3Zuku",
      "hash160result": "92f38773849383146037b16a48ea350c1c11ac3c",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.object.transferdestination"
      }
    }
{
      "vdxfid": "iNcKvh7mazaXptzHf85q6EtpFYFE7asKC1",
      "indexid": "xTSSPVYrSJoCT4sKWojz4dRMHCGF3h9tM4",
      "hash160result": "013e760f7451c289672993ea391ae643c21ce4d1",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.object.utxoref"
      }
    }
{
      "vdxfid": "iP3euVSzNcXUrLNHnQnR9G6q8jeYuGSxgw",
      "indexid": "xTsmNHt5Dvk9UWFKe6Sa7edNAPfZmJVgLc",
      "hash160result": "4d33e0aee0f648c7871b2661d1221b57c05aaed6",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.object.crosschaindataref"
      }
    }
 {
      "vdxfid": "iHEEK8ipj58BeKZNWuaaR2tDR5RK2kmf9A",
      "indexid": "xN4Lmw9uaPLrGVSQNbEjPRQkSjSKxsHUQu",
      "hash160result": "8d021acc1b68335bd7d37b28ff773c138ea5dd96",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.encryptiondescriptor"
      }
    }
 {
      "vdxfid": "i92U1nLuLJkC44FZZ4Lq9zk4qW3HrWAWNo",
      "indexid": "xDraUamzBcxrgE8bQjzz8PGbsA4JiFskTD",
      "hash160result": "9e13510e01d0d03a7bc90d7a2ef32824f515e33c",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.salteddata"
      }
    }
{
      "vdxfid": "i4GC1YGEVD21afWudGoFJVdnfjJ5XWnCQv",
      "indexid": "x96JULhKLXEgCqPwUxTQGtAKhPK6Qh1iaW",
      "hash160result": "4d4f12424ded2033a526a4e2a8835fc5b2eba208",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.object.datadescriptor"
      }
    }
 {
      "vdxfid": "i7PcVF9wwPtQ6p6jDtCVpohX65pTZuP2ah",
      "indexid": "xCDix3b2ni74iyym5ZreoCE47jqUTBFRAb",
      "hash160result": "b48b359e9a00042cec64f7f66ac717d388a4f22a",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.signaturedata"
      }
    }
{
      "vdxfid": "i9UgJ2WxGw95PKdoCXjpfnBShtP5gi9fxS",
      "indexid": "xEJnkpx38FMk1VWq4DPyeAhyjYQ6X5Gsti",
      "hash160result": "8c1afd59e904f6d2702699963abccbc6d326d841",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.mmrhashes"
      }
    }
 {
      "vdxfid": "iPQsnA1R8UjHNddZKZ3FxsuKQ5WzKqSC7w",
      "indexid": "xUEzExSVynwwzoWbBEhQwGRrRjY1Bc2MYc",
      "hash160result": "f535a4e9ac0f94eda01695d16489a4a102d6b1da",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.mmrlinks"
      }
    }
 {
      "vdxfid": "i9dVDb4LgfMYrZD1JBNP2uaso4bNAkT4Jr",
      "indexid": "xETbgPVRXyaDUj639s2Y1J7QpicP4DvZMt",
      "hash160result": "97273a4c02d6be002f8d69c3979616732ba68243",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.mmrdescriptor"
      }
    }
 {
      "vdxfid": "iL5MPPHWXQEY3p2Q1UsmGDvXsgPiqd1W1S",
      "indexid": "xQuTrBibNiTCfyuRsAXvEcT4uLQjhxrpyL",
      "hash160result": "ae8d805d9650c0512a6b6ec33e963386542f18b6",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::data.type.typedefinition"
      }
    }
{
      "vdxfid": "i3mbggp3NBR77C5JeFQJTpAxmgMidayLLE",
      "indexid": "x8bi9VF8DVdmjMxLVw4TSChVoLNjUyapgs",
      "hash160result": "6920bb81b420bc95e29a10ed677379b1e39e3a03",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::identity.multimapkey"
      }
    }
 {
      "vdxfid": "i5Zkx5Z7tEfh42xtKfwbJ5LgEWE9rEgpFY",
      "indexid": "xAPsQszCjYtMgCqvBMbkGTsDGAFAmrN33A",
      "hash160result": "d393b986e4f82db7bec82d97b186882d739ded16",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::identity.multimapremove"
      }
    }
{
      "vdxfid": "iEYsp2njSt1M4EVYi9uuAPBU2wpKmThkkr",
      "indexid": "xKNzGqDpJCE1gQNaZqa48mi14bqLaG669g",
      "hash160result": "e95b2ee1abb130a93900ddaef2d8e528010f7c79",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::identity.profile.media"
      }
    }
 {
      "vdxfid": "iNHg1n828PUxktkYeNxC6sdVmuKTipn3L3",
      "indexid": "xT7nUaZ6yhhdP4daW4cM5GA2oZLUaNVaBD",
      "hash160result": "4a8f418203621f10d1a61701be8dbbbb38fa5cce",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::system.zmemo.message"
      }
    }
{
      "vdxfid": "i7mrLLjUfGYuHJwnsxFvd282hsdn4staJG",
      "indexid": "xCbxo9AZWamZuUppjdv5bQeZjXeo1vbaCc",
      "hash160result": "7b47c8cd90c4c3ddc542f37ca77473b7325a272f",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::system.zmemo.signature"
      }
    }
{
      "vdxfid": "iRvxVcGLaCXcDiAfnQ5FfeBCo2AiBibAft",
      "indexid": "xWm4xQhRRWkGqt3he5jQe2hjpgBj5C7Tj3",
      "hash160result": "b537201ca6465976bea7bdb03119644a858052f6",
      "qualifiedname": {
        "namespace": "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV",
        "name": "vrsc::system.currency.startnotarization"
      }

}
```








