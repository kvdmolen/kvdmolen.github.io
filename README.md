# DIDShare: A Lightweight Protocol for Digital Collaboration

## Abstract

European programmes like DSSC and DSIC have made it clear that digital collaboration across organizations is a priority. Current initiatives such as Gaia-X and iSHARE aim to achieve this by creating complex governance models, compliance requirements, and heavy infrastructures. While these initiatives are ambitious, they often come with onboarding barriers and slow adoption, especially for SMEs.

DIDShare takes a different approach. Instead of building another dataspace with strict participation rules, it provides a lightweight layer of trust between organizations. The core principle is simple: if two parties already know and trust each other, they should be able to collaborate digitally without months of onboarding or compliance checks. DIDShare achieves this by using decentralized identifiers (DIDs), verifiable credentials (VCs), and secure communication standards to enable direct, verifiable, and flexible data sharing.

The result is a protocol that aligns with Europe’s trust and sovereignty goals but lowers the threshold for participation. It also creates room for a new market of SaaS tools and services that can operationalize this approach, making it commercially valuable and feasible for broad adoption.

## 1. Why DIDShare is Needed

Today’s digital collaboration initiatives often build walled gardens: closed ecosystems that demand compliance, certification, and strict governance. This slows down innovation and excludes smaller companies who cannot afford the overhead. Yet many organizations only want one thing: to safely and quickly exchange data with partners they already know and trust.

DIDShare is designed precisely for this. It fulfills the generic requirements of a dataspace—trust, sovereignty, interoperability—but without the complexity. There is no mandatory onboarding process, no lock-in, and no central gatekeeper. By using DIDs and VCs, trust is established through cryptographic proofs and credentials that can come from existing trust anchors such as business registries or eIDAS providers. This approach makes it possible to start working together within hours rather than months.

## 2. How DIDShare Works

At its core, DIDShare allows organizations to represent themselves with a DID. In the DID Document, they can expose certain verifiable credentials publicly, such as a company registration or an identity credential from a trust anchor. These credentials make it immediately clear who they are, without needing prior approval from a dataspace operator.

Access to digital resources is granted through “Access Credentials.” These credentials specify which party can access which resource, under which conditions, and with which rights (for example, read, write, or subscribe to updates). If necessary, these credentials can also be delegated to a third party, making it possible to set up flexible supply chains and collaborations.

For communication, DIDShare uses DIDComm, a secure messaging layer. This allows not only negotiation and exchange of access rights but also subscription to updates. For example, if a partner subscribes to a resource, they can automatically receive a notification when that resource changes. Data transfer itself can happen through existing APIs, or optionally through standardized protocols like DSP.

Authorization is handled by presenting verifiable credentials (or a presentation of them) to an authorization service, which checks them with a nonce and then issues a short-lived token for actual API access. This ensures security without requiring new infrastructure on the data provider’s side.

## 3. High-Level Architecture

From a high-level perspective, DIDShare is not a platform but a protocol. It does not demand a new dataspace or marketplace but allows existing systems to interoperate securely. Each organization can operate its own wallet—either on-premise or as a SaaS service—that manages DIDs, credentials, and secure communications.

Such wallets will play a crucial role. They will allow organizations to set which credentials they want to expose publicly, receive and delegate access rights, subscribe to updates, and connect directly to their internal systems. The wallet becomes the bridge between existing IT systems and the world of trusted, verifiable collaboration.

## 4. Commercial Opportunity

Because DIDShare keeps the technical layer lightweight, it opens the door for a vibrant market of online tools and services. SaaS wallets are one obvious category, providing organizations with user-friendly interfaces to manage their digital trust relationships. Other services may emerge for monitoring, analytics, or sector-specific credential schemas.

This commercial feasibility is an important aspect. Where heavy dataspaces struggle with adoption and sustainability, DIDShare lowers the barrier to entry and allows SMEs as well as larger corporates to participate quickly. A broad market of tools will make the ecosystem more resilient, more innovative, and more self-sustaining.

## 5. Conclusion

DIDShare offers a pragmatic alternative to existing dataspace initiatives. By re-using proven standards such as DIDs, VCs, and DIDComm, and by removing unnecessary compliance layers, it enables organizations to collaborate digitally in a way that is both secure and simple. It fulfills the European mission of trustworthy digital collaboration while keeping participation open and low-cost.

Most importantly, it creates immediate value. Organizations can start working together digitally without delay, while a new market for SaaS tools can provide the user-friendly solutions needed for wide adoption. In this way, DIDShare combines technical credibility with commercial viability, making it a feasible and sustainable path forward for digital collaboration in Europe.
