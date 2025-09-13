---
title: Reference Implementation 
nav_order: 4
has_children: false
---

DIDShare Wallet (SaaS) — Reference Design & Feature Spec (white-paper addendum)

This addendum specifies a SaaS wallet that implements DIDShare’s collaboration paradigm. It focuses on practical features teams need to issue, receive, delegate, verify, and use Verifiable Credentials (VCs) and DIDs in real workflows—without lock-in, with clean APIs, and portable to on-prem.

⸻

1) Product Scope & Principles
	•	Purpose: Be the operational “trust console” for organizations: hold DIDs/keys, manage VCs, automate access decisions, and bridge DIDComm with existing APIs and apps.
	•	Principles:
	•	Sovereign & portable: All identities, keys, and data can be exported; on-prem deployment is a first-class path.
	•	Interoperable by default: W3C DID/VC, DIDComm v2, JSON-LD and JWT VCs, optional reuse of well-designed VC data models (e.g., Gaia-X Self-Description where fit).
	•	Least ceremony: Start collaborating by exchanging DIDs/VCs—no mandatory central onboarding.
	•	Security first: Strong key protection, auditable actions, minimal data retention.

⸻

2) Core Capabilities

2.1 Identity & Keys
	•	Multi-DID support: did:web, did:key, did:jwk, did:ion, did:pkh (and pluggable drivers).
	•	Key management:
	•	Custodial (HSM/KMS-backed), bring-your-own-HSM, or MPC.
	•	WebAuthn/FIDO2 for admin/operator auth; hardware-key support.
	•	Rotation & recovery: Rotate DID keys; publish new DID Documents; export/import seeds (where applicable) with encrypted backups.

2.2 Verifiable Credentials (VC/VP)
	•	Issue / Hold / Verify VCs (JSON-LD Data Integrity or JWT).
	•	Public VCs: Mark selected VCs as public; wallet automatically publishes proofs or credential summaries via DID Document service endpoints or a public profile endpoint.
	•	Revocation & status: Support StatusList2021 (or equivalent) and issuer-local revocation lists.
	•	Templates & re-use: Built-in schemas for common legal/trust artifacts:
	•	Business registration (e.g., KvK, BRIS-aligned)
	•	eHerkenning / eIDAS 2.0 derived assertions (where legally feasible)
	•	Optional: Gaia-X Self-Description fields/SRIs only where they are technically sound for DIDShare purposes (no compliance lock-in).

2.3 Access-VCs & Delegation
	•	Access-VC model: Fine-grained permissions over Resources (URIs), Scopes (read/write/admin), Usage terms, TTL, audience, and delegation constraints.
	•	Re-issue / transfer: Re-issue a new Access-VC embedding the original (or referencing it) to delegate to third parties, respecting delegation rules and expiry.
	•	Policy engine: JSON logic / CEL policy to validate incoming VPs and decide authorization (scopes, resource matching, issuer trust, usage constraints).

2.4 DIDComm v2 Agent
	•	Integrated Agent: Receive, send, and mediate DIDComm messages; support return routes and basic store-and-forward (with optional mediator).
	•	Protocols supported (extensible):
	•	Basic message, trust-ping, pickup.
	•	Access negotiation (DIDShare profiles mirroring useful parts of IDSA DSP—but optional).
	•	Subscription: create/renew/cancel subscription to a resource; receive UpdateNotification when the resource changes.
	•	Message APIs: REST + Webhooks + Outbox/Inbox for messages; idempotent delivery with retries and signatures.

2.5 Token Service (Resource Access)
	•	Nonce-verified VP intake: Accept VP from a wallet, verify nonce/challenge, assert rights, then issue short-lived access token (e.g., OAuth2 bearer/JWT or GNAP) for the target API.
	•	Pluggable token types: OAuth2 (opaque or JWT), GNAP continuation, or simple HMAC tokens for edge systems.
	•	Audit mapping: Token issuance links back to VP/VC IDs for full traceability.

2.6 API & Integrations
	•	Northbound (Your Apps → Wallet):
	•	REST/GraphQL/Events to: register resources, configure policies, request tokens, read message inbox/outbox, manage DIDs/VCs.
	•	Southbound (Wallet → Your Systems):
	•	Webhooks/EventBridge/Kafka for inbound DIDComm messages, Access-VC received, subscription updates, revocation events.
	•	Connector SDKs (Node/Java/Python) to add the “verify VP → issue token” check in front of existing APIs with a few lines of code.

2.7 UI/UX for Operators
	•	Public/Private VC toggle with preview of what gets exposed via DID Document / public profile.
	•	Notifications Center:
	•	Incoming Access-VCs (grant requests, delegated rights)
	•	Subscription status & update feeds
	•	Revocations and expiring credentials
	•	Delegation UI: Select an Access-VC → choose delegate DID → constrain scope/TTL → issue.
	•	Resource Catalog: Register your resources (URLs, types, scopes), attach policies/terms, and manage subscriptions.
	•	Trust Sources: Manage allowlists/denylists of DIDs, issuers, and trust anchors; pin specific schemas; import known anchors.
	•	Audit & Evidence: Timeline of issued/received VCs, negotiations, token grants, and data-access events.

2.8 Portability / On-Prem
	•	One-click export: DIDs, keys (per policy), VC store, policies, message history.
	•	Self-hosted images: Same codebase, with external KMS/HSM support and existing SSO (OIDC/SAML) for console access.

⸻

3) Data Model (High-Level)
	•	DID: { did, method, doc, keys[], services[] }
	•	VC: { id, type[], issuer, subject, claims, format, status, public:boolean, revocationRef }
	•	AccessVC (specialization): { resource, scopes[], audience[], termsURI?, canDelegate:boolean, maxDepth?:number, exp, nbf }
	•	VP: { id, holder, verifiableCredential[], challenge, domain }
	•	Resource: { id, uri, type, scopes[], policyRef }
	•	Policy: { id, engine, expression, version }
	•	Subscription: { id, resourceId, subscriberDID, status, exp }
	•	DIDCommMessage: { id, thid?, from, to, typ, type, body, attachments[] }
	•	TokenGrant: { id, vpRef, subjectDID, resourceId, scopes[], exp, tokenType }

⸻

4) Example Artifacts

4.1 Example DID Document (abbrev., JSON)

{
  "@context": ["https://www.w3.org/ns/did/v1"],
  "id": "did:web:example.com",
  "verificationMethod": [{
    "id": "did:web:example.com#key-1",
    "type": "JsonWebKey2020",
    "controller": "did:web:example.com",
    "publicKeyJwk": { "kty": "EC", "crv": "P-256", "x": "...", "y": "..." }
  }],
  "assertionMethod": ["did:web:example.com#key-1"],
  "authentication": ["did:web:example.com#key-1"],
  "service": [
    {
      "id": "#didcomm",
      "type": "DIDCommMessaging",
      "serviceEndpoint": {
        "uri": "https://wallet.example.com/didcomm",
        "accept": ["didcomm/v2"]
      }
    },
    {
      "id": "#public-credentials",
      "type": "CredentialRegistry",
      "serviceEndpoint": "https://wallet.example.com/public-vc"
    }
  ]
}

4.2 Example Public Identity VC (abbrev., JWT-VC payload)

{
  "iss": "did:web:trust-anchor.eu",
  "sub": "did:web:example.com",
  "nbf": 1735689600,
  "vc": {
    "@context": ["https://www.w3.org/2018/credentials/v1"],
    "type": ["VerifiableCredential", "LegalEntityCredential"],
    "credentialSubject": {
      "legalName": "Example BV",
      "registration": { "authority": "KvK", "number": "12345678" }
    },
    "status": { "type": "StatusList2021Entry", "statusListIndex": "5021" }
  }
}

4.3 Example Access-VC (JSON-LD style)

{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://didshare.org/contexts/access-v1.json"
  ],
  "type": ["VerifiableCredential", "AccessGrant"],
  "issuer": "did:web:example.com",
  "issuanceDate": "2025-09-13T10:00:00Z",
  "expirationDate": "2025-09-20T10:00:00Z",
  "credentialSubject": {
    "id": "did:web:partner.eu",
    "resource": "https://api.example.com/datasets/orders",
    "scopes": ["read", "subscribe"],
    "terms": "https://example.com/terms/v1",
    "canDelegate": true,
    "maxDelegationDepth": 1
  },
  "proof": { "...": "..." }
}

4.4 Example Delegated Access-VC

{
  "type": ["VerifiableCredential", "AccessGrant"],
  "issuer": "did:web:partner.eu",
  "credentialSubject": {
    "id": "did:web:thirdparty.io",
    "resource": "https://api.example.com/datasets/orders",
    "scopes": ["read"],
    "parentGrant": "urn:vc:example:1234",
    "canDelegate": false
  }
}

4.5 DIDComm Subscription Message

{
  "id": "d3b7...",
  "type": "https://didshare.org/protocols/subscription/1.0/subscribe",
  "from": "did:web:partner.eu",
  "to": "did:web:example.com",
  "body": {
    "resource": "https://api.example.com/datasets/orders",
    "grantRef": "urn:vc:example:1234",
    "delivery": { "mechanism": "didcomm", "ack": true }
  }
}

4.6 DIDComm UpdateNotification

{
  "id": "9a2f...",
  "type": "https://didshare.org/protocols/subscription/1.0/update",
  "from": "did:web:example.com",
  "to": "did:web:partner.eu",
  "body": {
    "resource": "https://api.example.com/datasets/orders",
    "change": { "type": "UPSERT", "version": "2025-09-13T10:01:00Z" }
  }
}

4.7 VP → Token Exchange (simplified)

Request:
POST /auth/verify-vp

{
  "challenge": "4b1c... (server-issued)",
  "domain": "api.example.com",
  "presentation": { "...": "..." }
}

Response:

{
  "token_type": "Bearer",
  "expires_in": 600,
  "access_token": "eyJhbGciOi...",
  "scopes": ["read"],
  "resource": "https://api.example.com/datasets/orders"
}


⸻

5) External APIs (Illustrative)

5.1 Wallet REST
	•	POST /did → create/import DID
	•	GET /did/{id} → resolve DID & services
	•	POST /vc/issue → issue VC (supports Access-VC template)
	•	POST /vc/revoke → revoke VC by id
	•	POST /vc/publish → mark VC public/unpublish
	•	POST /vp/verify → verify VP with challenge
	•	POST /auth/verify-vp → nonce-verified VP → token issuance
	•	POST /didcomm/send → send DIDComm
	•	GET /didcomm/inbox → list messages
	•	POST /subscriptions → create subscription (resource + Access-VC ref)

5.2 Webhooks / Events
	•	vc.received, vc.revoked, access.requested, access.granted,
didcomm.message, subscription.updated, token.issued, audit.event

5.3 Admin / Governance
	•	POST /trust/anchors (optional registry entries)
	•	POST /trust/allowlist / POST /trust/denylist
	•	PUT /policy/{id}

⸻

6) Security & Privacy
	•	Isolation: Multi-tenant with per-tenant KMS contexts; data encryption at rest.
	•	Minimal retention: Store hashes/metadata where possible; redact payloads by policy.
	•	Proof-carrying authorization: Decisions are based on VPs/VCs + policies; each decision recorded with decision evidence.
	•	Auditability: Signed audit log; exportable evidence bundles.
	•	Threat model coverage: replay/theft of tokens prevented via short TTL, audience binding, sender-constrained tokens (optional DPoP/MTLS), replay-protected DIDComm (nonces/timestamps).

⸻

7) Additional Features You’ll Likely Want
	•	Schema & Credential Designer: Visual builder for Access-VC templates, with machine-readable policies.
	•	Terms/Conditions Manager: Host canonical terms; reference them from Access-VCs.
	•	Monitoring & SLOs: Latency, verification rate, token issuance success, subscription delivery lag.
	•	Developer Tooling: Local emulator, Postman collection, typed SDKs, CLI.
	•	Migration Tools: Guided on-prem export; validator to check that DID Documents and public VC endpoints resolve correctly post-migration.
	•	Interop Bridges:
	•	OAuth2/OIDC front-door so existing SaaS (e.g., Notion-like apps) can be put behind VP-based authorization with minimal changes.
	•	GNAP support for progressive authorization flows.
	•	ZCAP-LD (optional) for capability-style delegation if teams prefer object-capabilities.

⸻

8) Minimal End-to-End Flows (Mermaid)

8.1 Share with Known Party (No Onboarding)

sequenceDiagram
  autonumber
  participant A as Your Wallet
  participant B as Partner Wallet
  participant R as Resource API

  A->>B: DID & public VCs (out-of-band or DID resolution)
  B->>A: DID & public VCs
  A->>B: Access-VC (read+subscribe on /datasets/orders)
  B->>R: POST /auth/verify-vp (VP incl. Access-VC, challenge)
  R-->>B: 200 { access_token }
  B->>R: GET /datasets/orders (Bearer token)
  R-->>B: 200 ...
  R-->>B: DIDComm UpdateNotification on change

8.2 Delegation

sequenceDiagram
  participant P as Partner
  participant T as Third Party
  participant W as Wallet

  P->>W: Request delegation (select Access-VC, constrain scope TTL)
  W-->>P: Issued Delegated Access-VC
  P->>T: Send Delegated Access-VC via DIDComm
  T->>Resource: VP->Token exchange, then access


⸻

9) Why This Matters for DSSC/DSIC
	•	Stimulates collaboration now: Exchanging DIDs/VCs + policy = working in hours, not months.
	•	Respects EU principles: Verifiable trust, sovereignty, interoperability—without bureaucratic walled gardens.
	•	Re-uses what’s good: Where Gaia-X self-descriptions or sector schemas are technically sound, adopt them as VC templates—skip the compliance overhead.

⸻

10) Quick Feature Checklist (from your list + additions)
	•	Mark VCs public and expose via DID Document service.
	•	DIDComm v2 integrated (send/receive, mediation, protocols for negotiation & subscription).
	•	APIs for inbound/outbound messages, VP verification, token issuance, resource & policy management.
	•	Re-issue / transfer Access-VC with constraints.
	•	UI notifications for incoming VCs, delegated rights, subscription updates.
	•	Nonce-verified VP flow → short-lived tokens for existing APIs.
	•	Delegation UI, Resource Catalog, Policy Engine, Audit/Evidence.
	•	Portability (export, on-prem images), HSM/MPC key options.
	•	Revocation/status & trust allow/deny lists.
	•	SDKs & webhooks for easy integration.

If you want, I can fold this section into your main white-paper and add a concise “Reference Architecture” diagram of the wallet with components (Agent, VC Store, Policy Engine, Token Service, Webhooks, Admin UI).
