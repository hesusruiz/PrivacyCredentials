# Introduction

In the blockchain space, many people and many projects put the technology before the citizen, trying to retrofit privacy and data protection into a system that was not designed with those requirements from the beginning.

Surprisingly, this also applies to many implementations and initiatives in the [Self-Sovereign Identity (SSI)](https://en.wikipedia.org/wiki/Self-sovereign_identity) space, where many initiatives insist on registering and recording the identities of citizens in a globally shared infrastructure, even though it is not really required or desirable for many important use cases.

Even though they claim that they use cryptographic techniques (from hashes to Zero-Knowledge Proofs) to achieve privacy, many claim so without a proper PIA (Privacy Impact Assessment) of the actual implementation in a concrete system. And most importantly, they assume that registering the identities of citizens is required without providing a proper justification, violating the principles of purpose limitation and data minimisation ([Articles 5.1.b and 5.1.c of GDPR](https://gdpr.eu/article-5-how-to-process-personal-data/)).

To make things worst, due to the COVID19 pandemia there has been a proliferation of initiatives and projects advocating the use of Verifiable Credentials and blockchain technologies, but lacking the principle of "citizen first, technology last". Those initiatives tend to be the most publicised, creating a generalised distrust on blockchain for use cases where strong privacy and citizen protection is critical.

However, many important use cases can leverage the power of Verifiable Credentials and the blockchain without compromising personal data.

## Relationship with the EU Digital COVID Credential

The [recent Guidelines](https://ec.europa.eu/health/ehealth/covid-19_en) from the [eHealth Network](https://ec.europa.eu/health/ehealth/policy/network_en), especially the document on interoperability of the Trust Framework (Interoperability of health certificates Trust framework) have made clear the requirements and characteristics that such solutions must comply with, striking the right balance between citizen and public health.

Even though the documents assume centralised technology for implementation, they provide leeway in some areas where proper use of blockchain technology and Verifiable Credentials could add value, complementing and enhancing the solution delineated in the Guidelines while maintaining the principles of strong privacy and data protection, cross-border interoperability, inclusiveness and simplicity and user-friendliness.

The Guidelines can be generalised to be used for the correct implementation of many different use cases in many industries (not limited to COVID certificates), ensuring that the same principles mentioned above are implemented.

## About PrivacyCred

PrivacyCred is a generic digital credential system which is designed to be secure, privacy-preserving, scalable, performant and robust. It is designed specifically for some important use cases where physical, on-person verification of identity of holder is needed and where normal W3C Verifiable Credential flows are not fully suitable as they are normally designed currently.