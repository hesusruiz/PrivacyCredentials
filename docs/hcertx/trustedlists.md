# Trusted List Format (DSC list)

## In HCERT

In HCERT the Trusted List is managed as described in the following diagram.

![Trusted Lists replication](ehealth_PKD.png)

Each Participating country is REQUIRED to provide a list of one or more Certificate Signing Certificate Authorities (CSCAs) and a list of all valid Document Signing Certificates (DSCs), and keep these lists current. The country lists are replicated to the other countries via the centralised EU Public Key Directory/Gateway service.

For the list of CSCA certificates, each certificate:

- MUST contain a valid Country attribute in the subject DN that matches the country of issuance.
- MUST contain DN that is unique within the specified country.
- MUST contain a unique Subject Key Identifier (SKI) according to ([RFC5280](https://tools.ietf.org/html/rfc5280)).

In addition, for the list of DSC certificates, each certificate:

- MUST be signed with the private key corresponding to a CSCA certificate published on the aforementioned list.
- MUST contain an Authority Key Identifier (AKI) matching the Subject Key Identifier (SKI) of the issuing CSCA certificate.
- MUST have a validity period that is in line with or longer than the validity period of all certificates signed using that key.
- SHOULD contain a unique Subject Key Identifier derived from the subject public key.

### Simplified CSCA/DSC

As of this version of the specifications, countries should NOT assume that any Certificate Revocation List (CRL) information is used; or that the Private Key Usage Period is verified by implementers.

Instead, the **primary validity mechanism is the presence of the certificate on the most recent version of that certificate list**.

### ICAO eMRTD PKI and Trust Centers

Member States can use a separate CSCA (as per the WHO advice) but may also submit their existing eMRT CSCA and/or DSC certificates; and may even choose to procure these from (commercial) TSPs - and submit these. However, any DSC certificate must always be signed by the CSCA submitted by that country.


## In HCERTX

In HCERTX the Document Signing Certificates (DSCs) are managed in a Trusted List implemented as a set of Smart Contracts deployed in one or more blockchain networks. This is described in more detail in the section [Trusted Framework](../trustframework/tf-overview.md).
The Trusted List includes not only the DSCs but also additional information about the public identities of the entities authorised to issue certificates. Replication of all the required information across all participants is performed automatically by the blockchain.

Thanks to the secure decentralised management of the Trusted List, HCERTX allows certificates to be issued (and signed) directly by the laboratories, making the whole system much more efficient and still secure and well-managed.  

As in HCERT, the **primary validity mechanism is the presence of the certificate on the most recent version of that certificate list**. The difference is that the list is managed in the blockchain, so the last version of the list is always updated automatically in all nodes of the network.