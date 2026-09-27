---
title: "Digital Identity Wallets"
permalink: /wallets/
description: "An introduction to digital identity wallets and the European Digital Identity Wallet, the privacy challenges in its cryptographic design, and resources on anonymous credentials and privacy-preserving authentication."
---

Digital identity wallets let people store official credentials, such as their national identity, driving licence, or diplomas, on their own phone, and present them to online and in-person services. Done right, a wallet can give users more control over their data than today's identity solutions, since they can share only what is needed, for example proving that they are over 18 without revealing their name or date of birth. Done wrong, it can become a tool for tracking people across every service they use. Which of these we end up with depends to a large extent on the cryptography inside the wallet.

This page collects resources on the European Digital Identity Wallet, the Norwegian work on a digital wallet, and the research on privacy-preserving credentials that I find most relevant.

*Last updated: September 2026.*

## The European Digital Identity Wallet

The revised eIDAS regulation, [Regulation (EU) 2024/1183](https://eur-lex.europa.eu/eli/reg/2024/1183/oj/eng), requires every EU member state to offer its citizens at least one European Digital Identity (EUDI) Wallet. The wallet must be free for citizens, voluntary to use, and support selective disclosure of attributes, and the regulation requires that relying parties cannot link or track users beyond what is necessary for each transaction.

The technical baseline is the [Architecture and Reference Framework](https://eudi.dev/latest/) (ARF), maintained by the European Commission together with the member states. It specifies the roles in the ecosystem (wallet providers, issuers of person identification data and attestations, relying parties, and trust lists), the credential formats (ISO mdoc and SD-JWT VC), and the protocols for issuing and presenting credentials (OpenID for Verifiable Credential Issuance and OpenID for Verifiable Presentations). The Commission also publishes reference implementations of the wallet, and in December 2025 it organized the [EUDI Wallets Launchpad](https://ec.europa.eu/digital-building-blocks/sites/spaces/EUDIGITALIDENTITYWALLET/pages/924975727/EUDI+Wallets+Launchpad) in Brussels, the first large interoperability testing event for the member states' wallet implementations.

## The privacy challenge

Today, selective disclosure in the ARF is achieved with one-time credentials: the issuer signs a list of salted hashes of the user's attributes with a standard signature scheme such as ECDSA, and the user reveals only the attributes (and salts) needed. Since the signature and the hashes are the same every time a credential is shown, relying parties that collude, or collude with the issuer, can link the presentations of a user across services. To avoid this, the wallet must be issued a batch of single-use credentials and never reuse them, which is costly and still does not protect against a colluding issuer.

For proper privacy, we would like to see *anonymous credentials* implemented in the wallet. Schemes based on BBS signatures let users prove statements about their credentials in zero-knowledge, so that each presentation is unlinkable, even to the issuer. An alternative that keeps today's issuers and formats is to prove knowledge of an ECDSA-signed credential in zero-knowledge, as in Google's [Longfellow ZK](https://eprint.iacr.org/2024/2010). The main obstacles are that the wallet keys must be protected by secure hardware in phones, which today only supports standard algorithms like ECDSA, and that new schemes must be standardized and certified.

Looking further ahead, the wallet should also be post-quantum secure. Neither ECDSA nor BBS is secure against quantum computers, and a long-lived identity infrastructure should be designed with crypto-agility and a migration path in mind. A Longfellow-style zero-knowledge proof for credentials signed with the post-quantum signature standard ML-DSA would be a particularly interesting direction, as would efficient lattice-based anonymous credentials.

In June 2024, a group of cryptographers gave [feedback on the ARF](https://github.com/eu-digital-identity-wallet/eudi-doc-architecture-and-reference-framework/discussions/211), arguing that the current design relies on cryptographic methods that were never intended to meet the privacy requirements of the regulation, and recommending anonymous credentials, crypto-agility, and privacy-preserving revocation. The discussion, including the Commission's response and alternative proposals, is a good introduction to the trade-offs involved.

## Norway

Norway is part of eIDAS 2.0 through the EEA Agreement. The Norwegian Digitalisation Agency (Digdir) coordinates the work on a [Norwegian digital wallet](https://samarbeid.digdir.no/digital-lommebok/digital-lommebok/2897) together with public and private actors, including a national sandbox for testing wallets and services, pilots, and hackathons with municipalities and vendors.

## Research

- [Anja Lehmann](https://hpi.de/lehmann/team/anja-lehmann.html) at the Hasso Plattner Institute leads the work on advanced cryptography in the German EUDI Wallet project, with the goal of enabling anonymous credentials in the wallet. Her group's [EUDI page](https://hpi.de/lehmann/eudi.html) and the paper *SoK: Anonymous Credentials for Digital Identity Wallets* (with C. Bormann, 2025) are excellent starting points.
- The Dagstuhl Seminar [Privacy-Preserving Authentication](https://www.dagstuhl.de/en/seminars/seminar-calendar/seminar-details/26171) (April 2026), organized by Foteini Baldimtsi, Lucjan Hanzlik, Anna Lysyanskaya, and Stefano Tessaro, brought together researchers and practitioners working on anonymous credentials, blind signatures, revocation, and post-quantum security. I participated in the seminar.
- The [International Workshop on Foundations and Applications of Privacy-Enhancing Cryptography](https://privcryptworkshop.github.io) (PrivCrypt), which I co-organized with Lucjan Hanzlik and Daniel Slamanig as an affiliated event of IACR Eurocrypt 2026, is a venue for work on anonymous credentials and other privacy-enhancing cryptography.

## My work

My own research is on advanced digital signatures from lattices, including blind signatures and threshold signatures, with digital identity wallets as an important application: blind signatures are a building block for anonymous credentials, and threshold signatures can protect the keys of issuers and wallets by distributing them across several parties. I currently supervise four master's students working on these topics.

## Cryptology and Social Life

Digital identity wallets are not only a technical problem: whether they protect privacy and are trusted and used depends just as much on how they are regulated, governed, and designed for real people. In the [Cryptology and Social Life](https://www.ntnu.edu/iik/cryptology-and-social-life) project at NTNU, researchers from information security, mathematics, computer science, and sociology study cryptographic systems as sociotechnical systems. In 2026, the project was awarded four PhD positions, one in each participating department, to study secure, privacy-preserving, and democratically aligned digital identity wallets for Norway, with a focus on the EUDI Wallet. The project will address both the technical challenges, such as privacy-preserving and post-quantum credentials, and the social challenges, such as trust, governance, and inclusion.
