# DIDShare Protocol Documentation

Welcome to the **DIDShare** protocol documentation. This protocol enables secure, verifiable sharing of data resources identified by DIDs (Decentralized Identifiers).

## Overview
DIDShare defines a pattern for:
- Assigning DIDs to individual data resources
- Granting access using Verifiable Credentials (VCs)
- Subscribing to updates via DIDComm

## Example Use Case
A live eCMR document in logistics has its own DID. Authorized parties can:
- Pull current state via URL in the DID Document
- Use a VC/VP as an access token
- Receive updates via DIDComm messaging

## DID Document Structure
```json
{
  "@context": "https://www.w3.org/ns/did/v1",
  "id": "did:data:ecmr-5678",
  "controller": "did:org:logisticsco",
  "service": [
    {
      "id": "#data-access",
      "type": "DIDDataService",
      "serviceEndpoint": "https://dataservice.example.com/ecmr/5678"
    },
    {
      "id": "#subscription",
      "type": "DIDCommMessaging",
      "serviceEndpoint": "https://msg.example.com/didcomm",
      "routingKeys": ["did:key:z6Mk..."],
      "accept": ["didcomm/v2"]
    }
  ]
}
```

## Access Credential Example
```json
{
  "@context": ["https://www.w3.org/2018/credentials/v1"],
  "type": ["VerifiableCredential", "DIDDataAccessGrant"],
  "issuer": "did:data:ecmr-5678",
  "credentialSubject": {
    "id": "did:org:carrierco",
    "resource": "did:data:ecmr-5678",
    "permissions": ["read", "subscribe"]
  },
  "issuanceDate": "2025-05-05T12:00:00Z"
}
```

## More
This is a minimal starting point. For more, see:
- [DIDComm v2](https://identity.foundation/didcomm-messaging/spec/)
- [ZCap-LD](https://w3c-ccg.github.io/zcap-ld/)
- [VC Data Model](https://www.w3.org/TR/vc-data-model/)
