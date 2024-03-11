---
label: Handling the QR code
order: 1800

tags:
    - VerusID Login
---
### How to sign the QR code to login

The mobile app or other app that recieves the Deeplink checks the valididty of the challenge, it also carefully displays to the user anti-phishing information to allow the user to verify then trust that the Challeng is coming from a legitemate service.


```typescript
async function verifyLoginConsent() {

  const request = /* previously created request */;
  const isValid = await verusIdClient.verifyLoginConsentRequest(request);
  console.log(isValid ? "Valid request" : "Invalid request");
  if (isValid) {
    const res = await VerusId.createLoginConsentResponse(
    "<your identities I address>",
    new primitives.LoginConsentDecision({
      decision_id: request.challenge.challenge_id,
      request: request,
      created_at: Number((Date.now() / 1000).toFixed(0)),
    }),
    "<your i addresses primaryaddress WIF>"
    );

    const redirects = res.decision.request.challenge.redirect_uris
      ? res.decision.request.challenge.redirect_uris
      : [];
  
    let redirectsObj = {};

    redirects.forEach(a => {redirectsObj[a.vdxfkey] = a;});

    if (redirectsObj[primitives.LOGIN_CONSENT_REDIRECT_VDXF_KEY.vdxfid]) {
      redirectinfo = redirectsObj[primitives.LOGIN_CONSENT_REDIRECT_VDXF_KEY.vdxfid];
      // Post response to webhook URL
      await axios.post(
        redirectinfo.uri,
        res)
    } else {
      redirectinfo = redirectsObj[primitives.LOGIN_CONSENT_WEBHOOK_VDXF_KEY.vdxfid];
      const url = new URL(redirectinfo.uri)
      url.searchParams.set(
        primitives.LOGIN_CONSENT_RESPONSE_VDXF_KEY.vdxfid,
        base64url(res.toBuffer())
      );

      const urlstring = url.toString()
      // Open URL in webviewer
      openUrl(urlstring.toString())
    }
  }
} 
```

To turn this login challenge to a Deeplink that can be put into a QR code or a link that can be clicked.

```typescript
// Continuing from before
const request = await verusIdClient.createLoginConsentRequest(signingId, challenge, primaryAddrWif);
const deepLinkURL  = request.toWalletDeeplinkUri()

```