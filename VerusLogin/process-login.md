---
label: Processing a signed login request
order: 1700

tags:
    - VerusID Login
---
# Processing a signed Login Challenge

Once you receive back a signed challenge from a user then you can process their challenge in the same way i.e. checking the signature they have signed the Response with is valid with the Identities current authorities via the Verus blockchain. Then your system can decide whether they are a valid user, or new user etc..


```typescript
  // if you have receieved a POST command the request will be in the body
  const data = req.body; 
    
  const res = new primitives.LoginConsentResponse(data)
  const success = await VerusId.verifyLoginConsentResponse(_res)

  if (!success) throw new Error("signature does not validate")
```
