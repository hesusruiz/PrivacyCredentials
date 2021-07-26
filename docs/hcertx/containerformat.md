# Electronic Health Certificate Container Format

The Electronic Health Certificate Container Format (HCERT) is designed to provide a uniform and standardised vehicle for health certificates from different Issuers. The aim is to harmonise how these health certificates are represented, encoded and signed with the goal of facilitating interoperability.

When appropriate, we describe here the standard HCERT as used in the EU Digital COVID Certificate, and the extensions for the HCERTX certificate.

## CBOR Web Token (CWT)

The certificate is structured and encoded as a CBOR with a COSE digital signature. This is commonly known as a "CBOR Web Token" (CWT), and is defined in [RFC 8392](https://tools.ietf.org/html/rfc8392). The actual health certificate data is transported in a `hcert` claim (as specified in [CBOR Web Token (CWT) Claims](https://www.iana.org/assignments/cwt/cwt.xhtml)).

There may be in the future many types of Health Certificates transported in this `hcert` claim, but at this moment the EU Digital COVID Certificates (the three subtypes of Vaccination, test and Recovery), are assigned a sub-claim key **1** inside this 'hcert` claim.

The integrity and authenticity of origin of payload data MUST be verifiable by the Verifier. To provide this mechanism, the issuer MUST sign the CWT using an asymmetric electronic signature scheme as defined in the COSE specification ([RFC 8152](https://tools.ietf.org/html/rfc8152)).


### CWT Structure Overview

- Protected Header

    - Signature Algorithm (`alg`, label **1**)
    - Key Identifier (`kid`, label **4**)

- Payload

    - Issuer (`iss`, claim key **1**, optional, ISO 3166-1 alpha-2 of issuer)
    - Issued At (`iat`, claim key **6**)
    - Expiration Time (`exp`, claim key **4**)
    - Health Certificate (`hcert`, claim key **-260**)

        - EU Digital Covid Certificate v1 (`eu_dcc_v1` aka `eu_dgc_v1`, claim key **1**)

- Signature

As mentioned before, the EU Digital Covid Certificate v1 travels **inside** the generic `hcert` claim, which is a registered IANA claim ([CBOR Web Token (CWT) Claims](https://www.iana.org/assignments/cwt/cwt.xhtml)). But the `hcert` claim can be used to transport other Health Certificates in the future.

### Signature Algorithm

The Signature Algorithm (`alg`) parameter indicates what algorithm is used for creating the signature. It must meet or exceed current SOG-IT guidelines.

One primary and one secondary algorithm is defined. The secondary algorithm should only be used if the primary algorithm is not acceptable within the rules and regulations imposed on the implementer.

However, it is essential and of utmost importance for the security of the system that all implementations incorporate the secondary algorithm. For this reason, both the primary and the secondary algorithm MUST be implemented.

For this version of the specification, the SOG-IT set levels for the primary and secondary algorithms are:

- Primary Algorithm: The primary algorithm is Elliptic Curve Digital Signature Algorithm (ECDSA) as defined in (ISO/IEC 14888–3:2006) section 2.3, using the P–256 parameters as defined in appendix D (D.1.2.3) of (FIPS PUB 186–4) in combination with the SHA–256 hash algorithm as defined in (ISO/IEC 10118–3:2004) function 4.

This corresponds to the COSE algorithm parameter `ES256`.

- Secondary Algorithm: The secondary algorithm is RSASSA-PSS as defined in ([RFC 8230](https://tools.ietf.org/html/rfc8230)) with a modulus of 2048 bits in combination with the SHA–256 hash algorithm as defined in (ISO/IEC 10118–3:2004) function 4.

This corresponds to the COSE algorithm parameter: `PS256`.

### Key Identifier

The Key Identifier (`kid`) claim is used by Verifiers for selecting the correct public key from a list of keys pertaining to the Issuer (`iss`) Claim. Several keys may be used in parallel by an Issuer for administrative reasons and when performing key rollovers. The Key Identifier is not a security-critical field. For this reason, it MAY also be placed in an unprotected header if required. Verifiers MUST accept both options.  If both options are present, the Key Identifier in the protected header MUST be used.

Due to the shortening of the identifier (for space-preserving reasons) there is a slim but non-zero chance that the overall list of DSCs accepted by a validator may contain DSCs with duplicate `kid`s. For this reason, a verifier MUST check all DSCs with that `kid`, until a successful verification is achieved. If no DSC succeeds, a validation error is raised.

### Issuer

In HCERT, the Issuer (`iss`) claim is a string value that MAY optionally hold the ISO 3166-1 alpha-2 Country Code of the entity issuing the health certificate. This claim can be used by a Verifier to identify which set of DSCs to use for validation. The Claim Key 1 is used to identify this claim.

HCERTX supports the HCERT format but adds the possibility to use in `iss` the DID corresponding to the Issuer entity, with the W3C Verifiable Credentials format. On issuance the system uses the ELSI DID Method, as described later. On verification, the system can accept several DID Methods for resolution of the DID into the DID Document, most notably EBSI and AlastriaID. Resolution could be extended to any DID Method that provides a **Universal Resolver interface**. 

### Expiration Time

The Expiration Time (`exp`) claim SHALL hold a timestamp in the integer NumericDate format (as specified in [RFC 8392](https://tools.ietf.org/html/rfc8392) section 2) indicating for how long this particular signature over the Payload SHALL be considered valid, after which a Verifier MUST reject the Payload as expired. The purpose of the expiry parameter is to force a limit of the validity period of the health certificate. The Claim Key 4 is used to identify this claim.

The Expiration Time MUST not exceed the validity period of the DSC.

### Issued At

The Issued At (`iat`) claim SHALL hold a timestamp in the integer NumericDate format (as specified in [RFC 8392](https://tools.ietf.org/html/rfc8392) section 2) indicating the time when the health certificate was created. 

The Issued At field MUST not predate the validity period of the DSC.

Verifiers MAY apply additional policies with the purpose of restricting the validity of the health certificate based on the time of issue. The Claim Key 6 is used to identify this claim.

### Health Certificate Claim

The Health Certificate (`hcert`) claim is a JSON ([RFC 7159](https://tools.ietf.org/html/rfc7159)) object containing the health status information, represented in CBOR as a *map*. It is registered as claim key **-260** in the IANA claim registry ([CBOR Web Token (CWT) Claims](https://www.iana.org/assignments/cwt/cwt.xhtml)). Several different types of health certificate MAY exist under the same claim, of which the European Digital COVID Certificate is one.

Note here that the JSON is purely for schema purposes. The wire format is CBOR as defined in ([RFC 7049](https://tools.ietf.org/html/rfc7049)). Application developers may not actually ever decode or encode to and from the JSON format, but use the in-memory structure.

Strings in the JSON object SHOULD be [NFC normalised according to the Unicode standard](http://www.unicode.org/reports/tr15/#Norm_Forms). Decoding applications SHOULD however be permissive and robust in these aspects, and acceptance of any reasonable type conversion is strongly encouraged. If non-normalised data is found during decoding, or in subsequent comparison functions, implementations SHOULD behave as if the input is normalised to NFC.

## Transport Encodings

### Raw

For arbitrary data interfaces the HCERT container and its payloads may be transferred as-is, utilising any underlying, 8 bit safe, reliable data transport. These interfaces MAY include NFC, Bluetooth or transfer over an application layer protocol, for example transfer of an HCERT from the Issuer to a holder’s mobile device.

If the transfer of the HCERT from the Issuer to the holder is based on a presentation-only interface (e.g., SMS, e-mail), the Raw transport encoding is obviously not applicable.

### Barcode

![Overview: QR code generation and verification](container_overview.png)

#### Payload (CWT) Compression

To lower size and to improve speed and reliability in the reading process of the HCERT, the CWT SHALL be compressed using ZLIB ([RFC 1950](https://tools.ietf.org/html/rfc1950)) and the Deflate compression mechanism in the format defined in ([RFC 1951](https://tools.ietf.org/html/rfc1951)). 

#### QR 2D Barcode

In order to better handle legacy equipment designed to operate on ASCII payloads, the compressed CWT is encoded as ASCII using [Base45](https://datatracker.ietf.org/doc/draft-faltstrom-base45) before being encoded into a 2D barcode.

The QR format as defined in ([ISO/IEC 18004:2015](https://www.iso.org/standard/62021.html#:~:text=ISO%2FIEC%2018004%3A2015%20defines%20the%20requirements%20for%20the%20symbology,algorithm%2C%20production%20quality%20requirements%2C%20and%20user-selectable%20application%20parameters.)) SHALL be used for 2D barcode generation. An error correction rate of ‘Q’ (around 25%) is RECOMMENDED.  The Alphanumeric (Mode 2/QR Code symbols 0010) MUST be used in conjunction with Base45. 

In order for Verifiers to be able to detect the type of data encoded and to select the proper decoding and processing scheme, the base45 encoded data (as per this specification) SHALL be prefixed by the Context Identifier string `HC1:`. Future versions of this specification that impact backwards-compatibility SHALL define a new Context Identifier, whereas the character following `HC` SHALL be taken from the character set [1-9A-Z]. The order of increments is defined to be in that order, i.e., first [1-9] and then [A-Z].

The optical code is RECOMMENDED to be rendered on the presentation media with a diagonal size between 35 mm and 60 mm to accommodate readers with fixed optics where the presentation media is required to be placed on the surface of the reader.

If the optical code is printed on paper using low-resolution (< 300 dpi) printers, care must be taken to represent each symbol (dot) of the QR code exactly square. Non-proportional scaling will result in some rows or columns in the QR having rectangular symbols, which will hamper readability in many cases.

