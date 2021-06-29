# Relationship with EU Digital COVID Certificate

We could say that PrivacyCredentials is a blockchain-based implementation of the EU Digital COVID Certificates (EUDCC) specification.

The meaning of this sentence is multiple.

On one hand, it means that the system is interoperable with the EU Digital COVID Certificates:

- The PrivacyCredentials wallet can scan and store any EUDCC QR code, and can coexist with credentials in other formats (e.g., W3C VC).
- The wallet can display the human-readable information of a stored EUDCC, including its QR code in EUDCC format so it can be verified by any compatible verifier.
- The PrivacyCredentials verifier can scan and verify any EUDCC certificate.

But usage of EUDCC goes beyond the above:

- The system can issue and verify credentials using the same technical format as the "official" EUDCC credentials (vaccination, test, and recovery), the only difference being that the certificates would be signed by entities which not necessarily are in the trusted list maintained by the EU Member States.
- Those additional certificates can be not only health-related (like in the Lanzarote CovidSafe project), but they can belong to any other use case in any field where the same strong-privacy guarantees are required.
- The major difference is that the identities of the entities involved in the use case are managed in the blockchain, making the system easier to implement, more scalable and resilient.

## Relationship with EU Digital COVID Certificates

To better describe the system, the next section compares it with the "standard" EUDCC system. We use the information from the eHealth Network in the document [Interoperability of health certificates Trust framework](https://ec.europa.eu/health/sites/health/files/ehealth/docs/trust-framework_interoperability_certificates_en.pdf), describing its characteristics and the differences.

For the explanation of how PrivacyCred can complement and improve the proposed eHealth Network system, first we need to draw the proposed Trust Framework in a different but completely equivalent way, in the following figure.

![EU PKD system](ehealth_PKD.png)

In the standard eHealth Network system, each country uploads to a central service (operated by the EU Commission) the keys/certificates specific to that country, and downloads from that service the keys/certificates from all the other countries that use the system. In this way, the **EU Public Key Directory/Gateway** (EU PKD) helps the different countries to maintain in each country a database with all the keys/certificates for all authorised issuers.

When one verifier entity in a country needs to verify a certificate presented by a traveler, it can do so by checking against the local copy (meaning in the verification country) of all keys/certificates maintained via the replication mechanism described above.

There are several ways in which a blockchain-based system like PrivacyCred could add value without modifying the essential processes or safety of the proposed system.

### Option 1

Regarding the list of authorised issuers, the eHealth Network system requires that for each country its list should be published on its PHA’s website (national backend server). In addition, the list may also be published through an open API.

In Option 1, in addition to publishing the list in the website it could be published in a blockchain. In that way, the list is hyper-replicated in a secure and tamper-resistant way in all the nodes of the blockchain network.

This facilitates secure verification by any entity (hotels, restaurants, etc) without overloading the website of the PHA. In other words, it implements a **massively scalable and highly available read-only database for checking the keys/certificates of authorised issuers**. The number of writes to the blockchain is very low (only writing when the list of authorised issuers changes), and the reads are performed locally in each of the nodes operated by each entity participating in the blockchain.

In a sense, it would be a mechanism complementary to the open API mechanism but cheaper, more available and more scalable.

### Option 2

Similar to Option 1, but publishing the full EU list in the blockchain. This could be done by a given country using the database that it has using the EU PKD, or it could even be performed by the EU entity providing the PKD service (most probably the Commission, as it happens with the European Federation Gateway Service).

Please note that in Option 1, there could be several countries that coordinate with each other and publish their lists in the same way in the blockchain, creating a single read-only list for any entity that wants to verify certificates.

### Other options

In the future, there could be more "ambitious" options. For example, when EBSI (European Blockchain Services Infrastructure) is in production, it could be used as a complement or even replacing completely the EU PKD centralised system. Each country would keep their sovereignty regarding managing their authorised issuers list, but the replication of that data across the EU could be simplified enormously using the EBSI blockchain network.

In the same way, there could be different "national" or even pan-european blockchain networks that could be used by countries to "disseminate" the master lists in a safe, cheap and available way.

The eHealth Network document mentions the [ICAO PKD](https://www.icao.int/Security/FAL/PKD/Pages/default.aspx), and in its own words (emphasis added):

!!! quote "ICAO PKD"
    The publication of a Master List enables other receiving States to obtain a set of CSCA certificates from a single source (the Master List issuer) rather than establish a direct bilateral exchange with each of the Issuing Authorities or organizations represented on that list. However, **the more instances of a CSCA certificate that a receiving State acquires** — whether through multiple Master Lists, bilateral exchange, or both — **the more confident** the receiving State can be that the CSCA certificate they are using for validation is authentic. In this respect, Master Lists contribute to building and improving trust based on CSCA certificates.

The blockchain-based PKD could coexist with the centralised PKD (at least while the centralised one is worthwhile), complementing it and providing in a secure way more places where the lists are available for verifiers.
For example, the current ICAO PKD service is hosted in identical systems within two geographically separate sites (location A being located in Berlin, Germany and location B being located in Abu Dhabi, United Arab Emirates). An operator location is additionally provided within the ICAO headquarters (being located in Montreal, Canada). The two hosting sites are designed so that each of them can take over the work of the other site should one of them fail.

### Summary

A blockchain-based system could provide several benefits, including:

* Greater resiliency by replicating in a simple and secure way the Master Lists and associated data.

* Better scalability, as most of the operations in the PKD system are reads (for verifications). Using a blockchain the data is hyper-replicated in a tamper-resistant way in all the nodes of the network, and the verifications can be done to servers which are very close to the geographical location of the verifier.

* An alternative method to the current download method for users of the PKD data. It is enough to operate a node in the blockchain network and the data is updated automatically when the central PKD repository is updated (assuming the update process includes updating the data in the blockchain). Nobody can tamper with the data and the history of the previous versions of the Master Lists are available if needed.

