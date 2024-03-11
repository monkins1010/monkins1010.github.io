---
label: QR Code Generation
order: 1950

tags:
    - VerusID Login
---
### Creating a login QR from a Server


```typescript
import { LoginConsentChallenge, primitives } from 'verus-typescript-primitives';
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
        `${process.env.THIS_URL}/verusidlogin`, 
        primitives.LOGIN_CONSENT_WEBHOOK_VDXF_KEY.vdxfid
      ),
    ],
      created_at: Number((Date.now() / 1000).toFixed(0)),
    });
  const signingId = "YourVerusID@";
  const primaryAddrWif = "yourWIFhere"; // process.env.WIF  {the WIF of the signing R address}

  try {
    const request = await verusIdClient.createLoginConsentRequest(signingId, challenge, primaryAddrWif);
    console.log(request);
  } catch (error) {
    console.error(error);
  }
}
```

To turn this login challenge to a Deeplink that can be put into a QR code or a link that can be clicked.

```typescript
// Continuing from before
const request = await verusIdClient.createLoginConsentRequest(signingId, challenge, primaryAddrWif);
const deepLinkURL  = request.toWalletDeeplinkUri()

```