---
label: QR Code Generation
order: 1950

tags:
    - VerusID Login
---
### Creating a login QR from a Server


```typescript
import { VerusIdInterface } from 'verusid-ts-client';

// this is the url to your daemon e.g.  http://127.0.0.1:27486  note: make sure that your VRSC.conf has:
// rpcallowip=127.0.0.1
// rpchost=127.0.0.1
const baseURL = `${process.env.REMOTE_PROTOCOL}://${process.env.REMOTE_HOST}:${process.env.REMOTE_PORT}`;
const { getApplicantByExternalId } = sumsubApi;

// these are the rpcuser and rpcpassword in the VRSC.conf
const mainConfig = {
  auth: {
    username: process.env.REMOTE_USER,
    password: process.env.REMOTE_PASS,
  }
};

// "i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV" is the system i.e. the VRSC iaddress
const privateVerusRPC = new VerusIdInterface("i5w5MuNik5NtLcYmNzcvaoixooEebB6MGV", baseURL, mainConfig);

import { LoginConsentChallenge, primitives, I_ADDR_VERSION } from 'verus-typescript-primitives';
const { randomBytes } = require('crypto');

async function createAndSignLoginConsent() {

  const randID = Buffer.from(randomBytes(20))
  const iaddressID = primitives.toBase58Check(randID, I_ADDR_VERSION)

  const challenge = new LoginConsentChallenge({
      challenge_id: iaddressID,
      requested_access: [
        // Permission to view the identtiy to sign the challenge MANDATORY
        new primitives.RequestedPermission("", primitives.IDENTITY_VIEW.vdxfid)
    ],
    redirect_uris: [
      // Either a `LOGIN_CONSENT_WEBHOOK_VDXF_KEY` or a `LOGIN_CONSENT_REDIREC_VDXF_KEY`
      // can be used here to either have the user send data to a webhook,
      // or the user can be redidirected to a website with the login data sent as `URL?params`
      new primitives.RedirectUri(
        `http:/yoururl/verusidlogin`, 
        primitives.LOGIN_CONSENT_WEBHOOK_VDXF_KEY.vdxfid
      ),
    ],
      created_at: Number((Date.now() / 1000).toFixed(0)),
    });
  const signingId = "Your VerusID iaddress"; // the iaddress of your name@S
  const primaryAddrWif = "yourWIFhere"; // process.env.WIF  {the WIF of the signing R address that owns the signingid}

  try {
    const request = await privateVerusRPC.createLoginConsentRequest(signingId, challenge, primaryAddrWif);
    console.log(request);
  } catch (error) {
    console.error(error);
  }
}
```

To turn this login challenge to a Deeplink that can be put into a QR code or a link that can be clicked.

```typescript
// Continuing from before
const request = await privateVerusRPC.createLoginConsentRequest(signingId, challenge, primaryAddrWif);
const deepLinkURL  = request.toWalletDeeplinkUri()

```