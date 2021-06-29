# Introduction

**PrivacyCredentials** is a generic credential system which is designed to be secure, privacy-preserving, scalable, performant and robust. It is designed specifically for some important use cases where physical, on-person verification of identity of holder is needed and where normal W3C Verifiable Credential flows are not fully suitable as they are normally designed currently.

The system is designed to support credentials in different formats, initially the [W3C Verifiable Credential](https://www.w3.org/TR/vc-data-model/) format and the **HCERT** format used in the [EU Digital COVID Certificate](https://ec.europa.eu/info/live-work-travel-eu/coronavirus-response/safe-covid-19-vaccines-europeans/eu-digital-covid-certificate_en) system for health certificates in the EU.

It enables production implementations today, because it is designed to be compatible with all current regulations in place today in the European Union (and many other regions of the world).

## Requirements

The system supports the life cycle of a cetificate:

1. the collection and registration of data about the citizen and her associated characteristics,  
2. the issuance of a certificate, and  
3. the presentation of the certificate to a verifier for its verification.

The verifier of a certificate should be able to establish that: 

- The certificate has been issued by an entity which is authorised to issue that specific type of certificate; 
- The information presented on the certificate is authentic, valid, and has not been altered; 
- The certificate can be linked to the holder of the certificate;

More detailed requirements are the following.

### Data protection (including data minimisation, purpose limitation, etc.)

The system should protect the data of the involved natural persons, especifically certificate holders. This covers several principles expressed in the GDPR, including purpose limitation and data minimisation ([Articles 5.1.b and 5.1.c of GDPR](https://gdpr.eu/article-5-how-to-process-personal-data/)). In particular, this implies the usage of a minimum set of data in the certificates as required for the supported use cases (data minimisation).

Importantly, this also implies that no personal data is registered in the blockchain. Not even a hash of the certificate is ever registered in the blockchain, because it is not required and it may be in violation of the purpose limitation principle.

The same applies to the act of presentation of the credential to a specific verifier (data minimisation) and the purpose of data presentation should be checked against the use cases (purpose limitation).

### ID binding and verification

The system follows the principles laid out in the eHealth Interoperability documents and generalises them to any type of credential and any type of use case where they can be applied.

ID binding refers to the mechanisms used to associate or bind the real identity of a subject and the certificate. The identity of this subject shall be bound to a certificate when the certificate is issued (ID binding) and has to be verified when the certificate is being presented and verified (ID verification). These two processes (ID binding at the Issuance step and ID verification at the Presentation and Verification step) prevent possible impersonation attempts, and are in line with the data security and privacy by design and default principles of the system.

The processes of ID binding and/or verification rely on nationally/internationally established methods for ID binding and verification, such as national IDs and passports, and regulated customer onboarding processes (in the case of private companies).

Contrary to many SSI Verifiable Credentials implementations, the PrivacyCredentials system does not require any registration on the part of the user like registering her DID in the blockchain or any other repository, as the system relies in pre-existing identification processes (e.g., KYC for private companies).

The personal data elements are incorporated to the certificate and not used for any other thing or purpose. It is assumed that the minimal person identification data specified in this document can be used to perform the ID binding with a national ID, passport or any other suitable nationally issued identity document.


### Data security and privacy by design and by default

Abuse of data by actors (especially, the certificate verifiers and holders) and forgery should be prevented by any reasonable means. The system should by design and default ensure the security and the privacy of data in the compliant implementations, ensuring both security and privacy.
    
The system should promote the restriction of access to data and preventing malicious use of data, and at the same time it shoul enable the establishing of the authenticity of data and its link to the certificate holder.
The design should prevent the collection of identifiers or other similar data which might be crossreferenced with other data and re-used for tracking (‘Unlinkability’).

Unlinkability is one of the main reason why the system does not use any type of hash of personal data registered in the blockchain.

### Inclusiveness (especially medium-neutrality)

The system should be inclusive especially towards the individual citizen (‘no citizen left behind’).
    
The design of the system should attempt to maximize its support for diverse contexts (e.g., high resource vs low resource contexts).

To enable this, the system should support a spectrum of certificate presentation media from plain paper certificate to augmented paper certificates (e.g., paper certificate with printed machinereadable parts such as barcodes, QR codes, Machine Readable Zones) and to purely digital certificates (e.g., in-app certificates).

The PrivacyCredentials system supports paper certificates, augmented paper certificates, QR codes and purely digital certificates.

In addition, contrary to many SSI Verifiable Credentials implementations, the PrivacyCred system is implemented as a PWA (Progressive Web App) that can be used simply in a mobile browser (or tablet/PC) without installing anything in the device or registering for anything.

However, the user can install the PWA in the device if the user so wishes to facilitate future uses. In any case, the system does not require any type of registration of the identity of the user.

### Simplicity and user-friendliness

It is very important that the system is designed with simplicity and user-friendliness of the possible implementation of digital certificate systems in mind.

More formally, the system should not have features or functionalities that would unnecessarily complicate the resulting implementation of a digital certificate system or make them unnecessarily difficult to use. Lack of simplicity could increase the time it takes to implement the compliant digital certificate systems, while lack of user friendliness could hinder the uptake of the resulting implementations. User-friendliness is relevant for quick and easy processing, specifically to certificate holders and to verifiers.

The PrivacyCredentials system follows the rule of **Occam's Razor** eliminating any feature or functionality which is not strictly required for the use case.

This not only provides simplicity and user-friendlyness but also provides a system easier to understand and maintain which is more secure and robust.

### Modularity and scalability

The system architecture should be modular and easily scalable, for instance, to additional usage scenarios, use cases and types of certificates.

The system is massively scalable because generation and verification of credentials are [embarrasingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel) tasks and the blockchain is never a bottleneck.

The system already supports different usage scenarios (e.g. alternative settings in which certificates may be requested or verification may take place).

### Open standards

The system should rely for its implementations on open standards, to the extent that this is possible. This will greatly contribute to the interoperability of the resulting implementations, in addition combined with open governance and open source implementations, it will instil trust in the involved stakeholders.

### Interoperability

Implementations of certificates that comply with the specifications should be interoperable, both at the infrastructure and at the national level.

This means that if Countries A and B implement the specifications, it should be possible for a verifier in Country B to verify a digital certificate that has been issued in Country A.

Cross-border interoperability should be ensured across EU and EEA countries. The system should be also prepared for interoperability with the solutions designed on a global level.
