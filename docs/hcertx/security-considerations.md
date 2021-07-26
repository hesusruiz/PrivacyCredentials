# Security and Trust Management considerations

## Security Considerations

When designing a scheme using this specification, several important security aspects must be considered. These cannot preemptively be accounted for in this specification but must be identified, analysed and monitored by the Participants.

As input to the continuous analysis and monitoring of risks, the following topics SHOULD be taken into account:

### HCERT Signature Validity Time

#### In HCERT

It is anticipated that health certificates can not be reliably revoked once issued, especially not if this specification would be used on a global scale. Publishing of revocation information containing identifiers may also create privacy concerns, as this information is per definition Personally Identifiable Information (PII). For these reasons, this specification requires the Issuer of HCERTs to limit the validity period of the signature by specifying a signature expiry time. This requires the holder of a health certificate to renew it at periodic intervals. 

The acceptable validity period may be determined by practical constraints. For example, a traveller may not have the possibility to renew the health certificate during a trip overseas. However, it may also be the case that an Issuer is considering the possibility of a security compromise of some sort, which requires the Issuer to withdraw an DSC (invalidating all health certificates issued using that key which are still within their validity period). The consequences of such an event may be limited by regularly rolling Issuer keys and requiring renewal of all health certificates, on some reasonable interval.

#### In HCERTX

As described in the section on data model, the UVCI identifier is completely anonymous and can be used for secure and efficient implementation of revocation lists of certificates. That means that the decision of the certificate validity period can be taken based on the actual intended usage of the certificate and not because the security issue that happens in the HCERT implementation. 

### Key Management

This specification relies heavily on strong cryptographic mechanisms to secure data integrity and data origin authentication. Maintaining the confidentiality of the private keys is therefore of utmost importance.

The confidentiality of cryptographic keys can be compromised in a number of different ways, for instance:

- The key generation process may be flawed, resulting in weak keys.
- The keys may be exposed by human error.
- The keys may be stolen by external or internal perpetrators.
- The keys may be calculated using cryptanalysis.

To mitigate against the risks that the signing algorithm is found to be weak, allowing the private keys to be compromised through cryptanalysis, this specification recommends all Participants to implement a secondary fallback signature algorithm based on different parameters or a different mathematical problem than the primary.

The other risks mentioned here are related to the Issuers' operating environments. One effective control to mitigate significant parts of these risks is to generate, store and use the private keys in Hardware Security Modules (HSMs). Use of HSMs for signing health certificates is highly encouraged.

However, regardless of whether an Issuer decides to use HSMs or not, a key roll-over schedule SHOULD be established where the frequency of the key roll-overs is proportionate to the exposure of keys to external networks, other systems and personnel. A well-chosen roll-over schedule also limits the risks associated with erroneously issued health certificates, enabling an Issuer to revoke such health certificates in batches, by withdrawing a key, if required.


### Input Data Validation

This specification may be used in a way that implies receiving data from untrusted sources into systems that may be of mission-critical nature. To minimise the risks associated with this attack vector, all input fields MUST be properly validated by data types, lengths and contents. The Issuer Signature SHALL also be verified before any processing of the contents of the HCERT takes place. However, the validation of the Issuer Signature implies parsing the Protected Issuer Header first, in which a potential attacker may attempt to inject carefully crafted information designed to compromise the security of the system.



## Trust Management

The signature of the HCERT requires a public key to verify. Countries, or institutions within countries, need to make these public keys available. Ultimately, every Verifier needs to have a list of all public keys it is willing to trust (as the public key is not part of the HCERT).

### List of public keys

#### In HCERT

In HCERT, a simplified variation on the ICAO "_Master list_" will be used, tailored to this health certificate application, whereby each country is ultimately responsible for compiling their own master list and making that available to the other Participants. The aid of a coordinating Secretariat for operational and practical purposes will be available.

The _"Secretariat"_ is a functional role; not a person or a piece of software. It is expected that the Digital Covid Certificate Gateway (DCCG) will automate most of these tasks.

The system consists of (only) two layers; for each Member State one or more country level certificates that each signs one or more document signing certificates that are used in day to day operations.

The Member State certificates are called Certificate Signer Certificate Authorities (CSCAs) and are (typically) self-signed certificates. Countries may have more than one (e.g., in case of regional devolution). These CSCA certificates regularly sign the Document Signing Certificates (DSCs) used for signing HCERTs. Member States will each maintain a public register of the DSC certificates that is kept current, communicated to the Secretariat and also published at a stable URL for bilateral exchange. Member States MUST remove any revoked or stale certificates from this list.

The Secretariat will regularly aggregate and publish the Member States DSCs, after having verified these against the list of CSCA certificates (which have been conveyed and verified by other means). 

The resulting list of DSC certificates then provides the aggregated set of acceptable public keys (and the corresponding `kid`s) that Verifiers can use to validate the signatures over the HCERTs. Verifiers MUST fetch and update this list regularly.

Member States may also bilaterally exchange CSCA certificates with a number of other Member States, verify these bilaterally and thus compile their own lists of CSCA and DSC certificates which is specific to that Member State. Verifiers may choose to rely on such a national list.

Such Member State-specific lists are expected to be adapted in the format for their own national setting. As such, the file format of this trusted list may vary, e.g., it can be a signed JWKS ([JWK set format per RFC 7517 section 5](https://tools.ietf.org/html/rfc7517#section-5)) or any other format specific to the technology used in that Member State.

For the sake of simplicity: Member States may both submit their existing CSCA certificates from their ICAO eMRTD systems or, as recommended by the WHO, create one specifically for this health domain. 

#### In HCERTX

One way of looking at HCERTX is that it implements a "special Member State-specific list" as described above. That is, HCERTX is complementary to HCERT and coexists with it. In this respect, the consolidated list of DSCs from all Member States is published in the blockchain(s) used by HCERTX. Verifier applications can use this consolidated list published in every node of the blockchain network to verify standard HCERT certificates issued by the Member States.

In addition, thanks to the blockchain, HCERTX allows the usage of the same mechanism of Trusted List in the blockchain to facilitate the verification of certificates issued by any other entity which is authorised and added to the Trusted List in a secure way.

### The Key Identifier (`kid`)

The Key Identifier (`kid`) is calculated when constructing the list of trusted public keys from DSC certificates and consists of a truncated (first 8 bytes) SHA-256 fingerprint of the DSC encoded in DER (raw) format.

Note that Verifiers do not need to calculate the `kid` based on the DSC certificate and can directly match the Key Identifier in issued health certificates with the `kid` on the trusted list.

### Differences to the ICAO eMRTD PKI Trust Model

While patterned on best practices of the ICAO eMRTD PKI trust model, there are a number of simplifications made in the interest of speed (and recognising the fact that the EU Regulation for EHN is sharply limited in time and scope):

*  A Member State may submit multiple CSCA certificates.
* The DSC (key usage) validity period may be set to any length not exceeding the CSCA _and MAY be absent_.
* The DSC certificate MAY contain policy identifiers (Extended Key Usage) that are EHN specific.
* Member States may choose to never do any verification of published revocations; but instead purely rely on the DSC lists they get daily from the Secretariat or compile themselves.

