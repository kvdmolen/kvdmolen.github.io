---
title: Requirements
nav_order: 2
has_children: false
---

# Requirements for practical digital collaboration

### 1. Open Participation (No Onboarding Burden): Any legitimate party should be able to collaborate digitally without mandatory onboarding into a central governance scheme or technical “walled garden.”
- **DIDShare Fulfillment**:
  - Parties exchange DIDs directly, no central registry required.
  - Trust is established via public VCs in the DID Document (e.g. eHerkenning, KvK, or trust anchor credentials).
  - No gatekeeping by a dataspace operator.
- **Gap in iSHARE/Gaia-X**:
  - Heavy compliance/onboarding processes before participation is allowed.


### 2. Trust Anchoring with Minimal Compliance: Identity and trust should be verifiable through standard credentials, without enforcing one fixed trust framework.
- **DIDShare Fulfillment**:
  - Trust VCs can come from any relevant anchor (e.g. government, sector, or business partner).
  - Verification is cryptographic, not contractual.
- **Gap in iSHARE/Gaia-X**:
  - Trust is tightly bound to compliance frameworks, certifications, and governance models.


### 3. Direct & Flexible Data Sharing: Partners must be able to share data directly, using existing APIs or agreed protocols, without needing to change systems or go through mandatory intermediaries.
- **DIDShare Fulfillment**:
  - Supports both existing APIs and optional DSP-like flows over DIDComm.
  - Authorization via VCs, not proprietary gateways.
- **Gap in iSHARE/Gaia-X**:
  - Requires adopting a predefined protocol stack (DSP, connectors, IDS brokers, etc.).


### 4. Fine-Grained Access Control & Delegation: Access rights should be easily expressible, transferable, and revocable without legal overhead.
- **DIDShare Fulfillment**:
  - Access VCs define rights to resources, usage conditions, and delegation rules.
  - Delegation to 3rd parties is natively supported by issuing new VCs.
- **Gap in iSHARE/Gaia-X**:
  - Delegation is complex or non-existent, often requiring contractual amendments.


### 5. Event-Driven Collaboration: Parties should not only pull data, but also be notified of changes in real-time to collaborate effectively.
- **DIDShare Fulfillment**:
  - DIDComm subscriptions allow event-driven updates (“push model”).
- **Gap in iSHARE/Gaia-X**:
  - Architectures are mainly pull/request-based; no native subscription mechanism.


### 6. Portability & Sovereignty: Identities, credentials, and wallets should not be locked into a single SaaS or infrastructure provider.
- **DIDShare Fulfillment**:
  - Portable DID method allows migration from SaaS wallet to on-prem wallet without vendor lock-in.
- **Gap in iSHARE/Gaia-X**:
  - Participants must remain inside the ecosystem or risk losing access to trust services.


### 7. Low Barrier to Adoption (SME-Friendly): SMEs should be able to participate without high costs, long onboarding, or complex IT changes.
- **DIDShare Fulfillment**:
  - Whitelisting DIDs and exchanging VCs is enough to start collaboration.
  - Works with existing APIs, no new connectors needed.
- **Gap in iSHARE/Gaia-X**:
  - Too complex and costly for smaller companies, designed with large corporates in mind.


### 8. EU Alignment with Minimal Bureaucracy: Must align with EU principles of trust, verifiability, sovereignty, and interoperability, but without creating unnecessary bureaucracy.
- **DIDShare Fulfillment**:
  - Uses open W3C standards (DID, VC, DIDComm).
  - Compatible with EU eIDAS2.0/eHerkenning as credential sources.
  - Lightweight alignment with DSSC/DSIC goals: digital trust, interoperability, sovereignty.
- **Gap in iSHARE/Gaia-X**:
  - Compliance heavy, often criticized for being over-engineered and bureaucratic.
