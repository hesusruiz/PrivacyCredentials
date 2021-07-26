# Features

The system supports the life cycle of a certificate:

1. the collection and registration of data about the citizen and her associated characteristics,  
2. the issuance of a certificate, and  
3. the presentation of the certificate to a verifier for its verification.

The verifier of a certificate is able to establish that: 

- The certificate has been issued by an entity which is authorised to issue that specific type of certificate; 
- The information presented on the certificate is authentic, valid, and has not been altered; 
- The certificate can be linked to the holder of the certificate;

The features of the PrivacyCredentials system are described below.

## Interoperable with the EU Digital COVID Certificates from EU Member States

- The PrivacyCredentials wallet can scan and store any EUDCC QR code in digital or paper format. Those credentials can coexist in the wallet with credentials in other formats (e.g., W3C VC).
- The wallet can display the human-readable information of a stored EUDCC, including its QR code in EUDCC format so it can be verified by any compatible verifier.
- The PrivacyCredentials verifier can scan and verify any EUDCC certificate, in paper of digital format.

## Issuance and verification of many types of credentials in EU DCC format

The PrivacyCredentials system allows entities to issue, manage and verify credentials in the same format as the standard EU DCC.

This means that the system reuses and leverages the knowledge and expertise involved in the development and setup of the EU Digital COVID Certificate, where privacy is of utmost importance and millions of certificates have been already issued with independently by the different Member States, while being fully interoperable.

Of course, this does not mean that any certificates of any type issued by PrivacyCredentials are automatically verifiable by the standard EU DCC verifiers, because this only happens for health certificates of type **hcert** and subtypes **v**, **t** and **r** (vaccination, test and recovery) and which have been signed by the entities in the Member States authorised list.

In other words, the PrivacyCredentials system can verify both the standard EU DCC credentials and the credentials issued with PrivacyCredentials in the EU DCC format, because PrivacyCredentials includes both Trusted Lists.

## Supports W3C Verifiable Credentials

In addition to the The EU Digital COVID Certificate format, the wallet supports standard W3C Verifiable Credentials, which can coexist in the same wallet. The wallet is easily extensible to support other types of credentials found in real-life, in order to start being useful before all credentials are migrated to standard formats.

## Strong privacy by design and by default

The system is designed from scratch to protect the data of the involved natural persons, specifically certificate holders. This covers several principles expressed in the GDPR, including **purpose limitation and data minimisation** ([Articles 5.1.b and 5.1.c of GDPR](https://gdpr.eu/article-5-how-to-process-personal-data/)). In particular, this implies the usage of a minimum set of data in the certificates as required for the supported use cases (data minimisation).

Importantly, this also implies that **no personal data is registered in the blockchain**. Not even a hash of the certificate is ever registered in the blockchain, because it is not required by the typical use cases supported by the system and so it may be in violation of the purpose limitation principle.

Abuse of data by actors (especially, the certificate verifiers and holders) and forgery should be prevented by any reasonable means. The system should by design and default ensure the security and the privacy of data in the compliant implementations, ensuring both security and privacy.
    
The system promotes the restriction of access to data and prevents malicious use of data, and at the same time it enables the establishing of the authenticity of data and its link to the certificate holder.
The design prevents the collection of identifiers or other similar data which might be cross-referenced with other data and re-used for tracking (‘Unlinkability’).

Unlinkability is one of the main reason why the system does not use any type of hash of personal data registered in the blockchain.

## Inclusiveness (especially medium-neutrality)

The system should be inclusive especially towards the individual citizen (‘no citizen left behind’).

The design of the system should attempt to maximize its support for diverse contexts (e.g., high resource vs low resource contexts).

To enable this, the system supports a spectrum of certificate presentation media from plain paper certificate to augmented paper certificates (e.g., paper certificate with printed machine-readable parts such as barcodes, QR codes, Machine Readable Zones) and to purely digital certificates (e.g., in-app certificates).

In addition, contrary to many SSI Verifiable Credentials implementations, the PrivacyCredentials system is implemented as a PWA (Progressive Web App) that can be used simply in a mobile browser (or tablet/PC) without installing anything in the device or registering for anything.

However, the user can install the PWA in the device if the user so wishes to facilitate future uses. In any case, the system does not require any type of registration of the identity of the user.

## Simplicity and user-friendliness

The PrivacyCredentials system follows the rule of [Occam's Razor](https://en.wikipedia.org/wiki/Occam%27s_razor) eliminating any feature or functionality which is not strictly required for the target use cases. Notwithstanding this simplicity, the resulting system can be used in a surprisingly big number of real-life use cases apart from health-related ones.

This not only provides simplicity and user-friendliness but also provides a system easier to understand, maintain and reason about, which is more secure and robust.

## Prepared for today and the future European Digital Identity

In PrivacyCredentials the processes of ID binding and/or verification are isolated from the certificates and rely on **nationally/internationally established methods for ID binding and verification, such as national IDs and passports, and regulated customer onboarding processes** (in the case of private companies).

In this way, PrivacyCredentials can be used today in real world use cases with full legal validity of the credentials managed in the system. At the same time, it is well prepared to use any future legal identification system like the upcoming [European Digital Identity](https://ec.europa.eu/info/strategy/priorities-2019-2024/europe-fit-digital-age/european-digital-identity_en).

ID binding refers to the mechanisms used to associate or bind the real identity of a subject and the certificate. The identity of this subject shall be bound to a certificate when the certificate is issued (ID binding) and has to be verified when the certificate is being presented and verified (ID verification). These two processes (ID binding at the Issuance step and ID verification at the Presentation and Verification step) prevent possible impersonation attempts, and are in line with the data security and privacy by design and default principles of the system.

The system follows the principles laid out in the eHealth Interoperability documents and generalises them to **any type of credential** and any type of use case where they can be applied.

Contrary to many SSI Verifiable Credential implementations, the PrivacyCredentials system **does not require any registration on the part of the user** like registering her DID in the blockchain or any other repository, as the system relies in pre-existing identification processes (e.g., KYC for private companies).

This clear separation of concerns allows PrivacyCredentials to work with almost any existing or future identification systems, for example with off-line ID binding systems (EU Digital COVID Certificates) or with online ID binding like what will be possible with the future European Digital Identity system.

The certificates should be issued according to existing national and international regulations, where the personal data elements are incorporated to the certificate and not used for any other thing or purpose. It is assumed that the minimal person identification data in the certificate can be used to perform the ID binding with a national ID, passport or any other suitable nationally issued identity document and in full compliance with the current regulations.

## Allows offline and online verification

The system is fully aligned with the future European Digital Identity, which will be available to EU citizens, residents, and businesses who want to identify themselves or provide confirmation of certain personal information. It can be used for both online and offline public and private services across the EU.

## The wallet does not need to be downloaded from app markets

The wallet is implemented as a PWA (Progressive Web Application) with local storage and offline capabilities. Of course, native apps can coexist in parallel, but the incentives are low as the PWA approach is enough for typical scenarios in these use cases.

The wallet is Open Source standard HTML/JavaScript code, so it can be easily analysed by security experts and industry specialists for any security or privacy problem.

## The provider of the wallet does not know about usage of the wallet

In order to start using the wallet, the user does not have to register in any place and never has to provide any personal data to any entity. The same wallet software can be hosted in many different places, typically in trusted servers operated by trusted entities in the ecosystem. The user can choose the provider that she wants. Even power users could host the wallet in their own servers, or citizen associations or communities could host their own version of the wallet, making sure it complies with all required properties of strong privacy and independence from commercial interests.

Once the wallet is loaded in the user's mobile, it never communicates back to the server that hosted the wallet, except for downloading newer versions of the software. Even this upgrade operation should be performed by initiative of the user.

## Verification does not require intermediate servers

The system supports true P2P (peer-to-peer) transmission of certificate information from the wallet to the verifier, or from paper to the verifier. This reduces potential privacy concerns, increases its independence from centralised entities, and "democratises" the verification process. Essentially, it shares many of the good properties of paper-based processes but with the efficiency and tamper-resistance of machine-readable structured documents with digital signatures.

This is performed via two independent and complementary mechanisms.

When CWT/COSE encoding can be used the resulting encoded certificate can typically be represented in a self-contained QR which can be read directly by the verifier devices (or wallets). This is in contrast with typical credentials where the QR just has a URL of a server where the real data of the credential is stored.

When the encoded certificate is too big to fit in a single QR, it is sent via a mechanism described elsewhere in this document. Essentially, the encoded certificate is divided into smaller packets and sent in  sequence directly to the verifier, where the packets are assembled together into the original encoded certificate. Each packet is transmitted as a QR in a looping video where each frame is a QR representing one of the pieces. The mechanism can be used in practice to send certificates with several KB in size, where the trade-off is the amount of time required to present and scan all pieces of the encoded certificate.

The system also supports "traditional" QRs where the QR has a URL pointing to the certificate, for situations where the above mechanisms can not be used. However this mechanism should be avoided if possible, given the problems mentioned above. Essentially, if the credential is too big for the P2P transmission mechanism, then it probably contains too much information and is in violation of the **minimisation** principle. In this situation it would be preferable to break the credentials into smaller ones, or use some of the selective disclosure techniques available.

The next version of PrivacyCredentials will include a simple but powerful selective disclosure system that does not require advanced techniques (like Zero-knowledge proof) and that is designed for most use cases where citizen data is big and is managed "on-custody", like it happens in health applications, government, banking, insurance, etc. It is also very easy to integrate in current systems, and easy to understand and reason about in terms of privacy and regulatory compliance.

## Full separation of natural person identities from juridical person identities

In the real world the identities of juridical persons are fundamentally different to the identities of natural persons, especially with respect to privacy and consumer protection regulations.

As mentioned before, for natural persons PrivacyCredentials relies on legally accepted and widely deployed ID binding mechanisms, facilitating implementation and ensuring full regulatory compliance.

For juridical persons PrivacyCredentials uses **ELSI**, a DID Method which is based in widely deployed standards and that is extremely easy to integrate with existing mechanisms used globally for identification of juridical entities, again facilitating implementation and ensuring full regulatory compliance.

Clean separation of the mechanisms used for both types of identities  allows for easier implementation of each set of requirements, without polluting the code with logic to handle both cases, which is prone to error and promotes failures with harsh legal consequences.

ELSI is described in detail in [ELSI - PrivacyCredentials](didmethods/elsi.md).

## Modularity and scalability

The system architecture is modular and easily scalable, to additional usage scenarios, use cases and types of certificates.

The system is massively scalable because generation and verification of credentials are [embarrassingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel) tasks and the blockchain is never a bottleneck.

The system already supports different usage scenarios (e.g. alternative settings in which certificates may be requested or verification may take place).

## Open standards

The system relies for its implementations on open standards, to the extent that this is possible. This contributes to the interoperability of the resulting implementation, and combined with open governance and open source implementation, it will instil trust in the involved stakeholders.

## Interoperability

Implementations of certificates that comply with the specifications should be interoperable, both at the infrastructure and at the national level.

This means that if Countries A and B implement the specifications, it should be possible for a verifier in Country B to verify a digital certificate that has been issued in Country A.

Cross-border interoperability should be ensured across EU and EEA countries. The system should be also prepared for interoperability with the solutions designed on a global level.
