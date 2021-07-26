
# EU Digital COVID Credentials

The system supports the **EU Digital COVID Credentials** both for storing and verifying them with the wallet and verifier applications.
The system supports generating credentials using exactly the same format as the `EU Digital COVID Credential`, but in our case the public keys are stored in the blockchain, together with the identities of all entities authorised to issue credentials.

The format and interoperability guidelines are defined in the related documentation from the EU. A brief summary (which should not be considered authoritative) follows.

## Common structure of EUDCC

```json
{
    "ver": <version information>,
    "nam": {
        <person name information>
    },
    "dob": <date of birth>,
    "v" or "t" or "r": [
        {<vaccination dose or test or recovery information, one entry>}
    ]
}
```

### Version

Version information MUST be provided. Versioning is following Semantic Versioning (semver: 
https://semver.org). In production, it MUST be one of the officially released (current or one of the older 
officially released) versions. See Section JSON Schema location for more details.

| Field id | Field name | Instructions |
|----------|------------|--------------|
| **ver** | Schema version | MUST match the identifier of the schema version used for producing the EUDCC. Example: "ver": "1.3.0 |

### Person name and date of birth

Person name is the official full name of the person, matching the name stated on travel documents. The 
identifier of the structure is nam. Exactly 1 (one) person name MUST be provided.

| Field id | Field name | Instructions |
|----------|------------|--------------|
| **nam.fn** | Surname(s) | Surname(s), such as family name(s), of the holder.<br>Exactly 1 (one) non-empty field MUST be provided, with all surnames included in it. In case of multiple surnames, these MUST be separated by a space. Combination names including hyphens or similar characters must however <br>stay the same.<br>Examples:<br>"fn": "Musterfrau-Gößinger"<br>"fn": "Musterfrau-Gößinger Müller" |
| **nam.fnt** | Standardised surname(s) | Surname(s) of the holder transliterated using the same convention as the one used in the holder’s machine readable travel documents (such as the rules defined in ICAO Doc 9303 Part 3).<br>Exactly 1 (one) non-empty field MUST be provided, only including characters A-Z and "<". Maximum length: 80 characters (as per ICAO 9303 specification).<br>Examples:<br>"fnt": "MUSTERFRAU<GOESSINGER"<br>"fnt": "MUSTERFRAU<GOESSINGER<MUELLER"
| **nam.gn** | Forename(s) | Forename(s), such as given name(s), of the holder.<br>If the holder has no forenames, the field MUST be skipped.<br>In all other cases, exactly 1 (one) non-empty field MUST be provided, with all forenames included in it. In case of multiple forenames, these MUST be separated by a space.<br>Example:<br>"gn": "Isolde Erika"
| **nam.gnt** | Standardised forename(s) | Forename(s) of the holder transliterated using the same convention as the one used in the holder’s machine readable travel documents (such as the rules defined in ICAO Doc 9303 Part 3).<br>If the holder has no forenames, the field MUST be skipped.<br>In all other cases, exactly 1 (one) non-empty field MUST be provided, only including characters A-Z and "<".<br>Maximum length: 80 characters.<br>Example:<br>"gnt": "ISOLDE<ERIKA"
| **dob** | Date of birth | Date of birth of the DCC holder.<br>Complete or partial date without time restricted to the range from 1900-01-01 to 2099-12-31. <br>Exactly 1 (one) non-empty field MUST be provided if the complete or partial date of birth is known. If the date of birth is not known even partially, the field MUST be set to an empty string "". This should match the information as provided on travel documents.<br>One of the following ISO 8601 formats MUST be used if information on date of birth is available. Other options are not supported. <br>YYYY-MM-DD<br>YYYY-MM<br>YYYY<br>(The verifier app may show missing parts of the date of birth using the XX convention as the one used in machinereadable travel documents, e.g. 1990-XX-XX.)<br>Examples:<br>"dob": "1979-04-14"<br>"dob": "1901-08"<br>"dob": "1939"<br>"dob": ""


### Groups for certificate type specific information

The JSON Schema supports three groups of entries encompassing certificate type specific information. 
Each EUDCC MUST contain exactly 1 (one) group. Empty groups are not allowed.

| Group identifier | Group name | Entries |
|------------------|------------|---------|
| **v** | Vaccination group | If present, MUST contain exactly 1 (one) entry describing exactly 1 (one) vaccination dose. |
| **t** | Test group | If present, MUST contain exactly 1 (one) entry describing exactly 1 (one) test result. |
| **r** | Recovery group | If present, MUST contain exactly 1 (one) entry describing 1 (one) recovery statement. |

## Certificate type specific information

### Vaccination certificate

Vaccination group, if present, MUST contain exactly 1 (one) entry describing exactly one vaccination 
event. All elements of the vaccination group are mandatory, empty values are not supported.

| Field id | Field name | Instructions |
|----------|------------|--------------|
| **v.tg** | Disease or agent targeted: COVID-19 (SARS-CoV or one of its variants) | A coded value from the value set `disease-agent-targeted.json`.<br>This value set has a single entry 840539006, which is the code for COVID-19 from SNOMED CT (GPS).<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"tg": "840539006"
| **v.vp** | COVID-19 vaccine or prophylaxis | Type of the vaccine or prophylaxis used.<br>A coded value from the value set `vaccine-prophylaxis.json`.<br>The value set will be distributed from the EUDCC Gateway starting with the gateway version 1.1.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"vp": "1119349007" (a SARS-CoV-2 mRNA vaccine)
| **v.mp** | COVID-19 vaccine product | Medicinal product used for this specific dose of vaccination. A coded value from the value set `vaccine-medicinal-product.json`.<br>The value set will be distributed from the EUDCC Gateway starting with the gateway version 1.1.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"mp": "EU/1/20/1528" (Comirnaty)
| **v.ma** | COVID-19 vaccine marketing authorisation holder or manufacturer | Marketing authorisation holder or manufacturer, if no marketing authorization holder is present.<br>A coded value from the value set `vaccine-mah-manf.json`.<br>The value set will be distributed from the EUDCC Gateway starting with the gateway version 1.1.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"ma": "ORG-100030215" (Biontech Manufacturing GmbH)
| **v.dn** | Number in a series of doses | Sequence number (positive integer) of the dose given during this vaccination event. 1 for the first dose, 2 for the second dose etc.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Examples:<br>"dn": "1" (first dose in a series)<br>"dn": "2" (second dose in a series)<br>"dn": "3" (third dose in a series, such as in case of a booster)
| **v.sd** | The overall number of doses in the series | Total number of doses (positive integer) in a complete vaccination series according to the used vaccination protocol. The protocol is not in all cases directly defined by the vaccine product, as in some countries only one dose of normally two-dose vaccines is delivered to people recovered from COVID19. In these cases, the value of the field should be set to 1.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Examples:<br>"sd": "1" (for all 1-dose vaccination schedules)<br>"sd": "2" (for 2-dose vaccination schedules)<br>"sd": "3" (in case of a booster)
| **v.dt** | Date of vaccination | The date when the described dose was received, in the format YYYY-MM-DD (full date without time). Other formats are not supported.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"dt": "2021-03-28"
| **v.co** | Member State or third country in which the vaccine was administered | Country expressed as a 2-letter ISO3166 code (RECOMMENDED) or a reference to an international organisation responsible for the vaccination event (such as UNHCR or WHO). A coded value from the value set `country-2-codes.json`.<br>The value set will be distributed from the EUDCC Gateway starting with the gateway version 1.1.<br>Exactly 1 (one) field MUST be provided.<br>Example:<br>"co": "CZ"<br>"co": "UNHCR"
| **v.is** | Certificate issuer | Name of the organisation that issued the certificate. Identifiers are allowed as part of the name, but not recommended to be used individually without the name as a text. Max 80 UTF-8 characters.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"is": "Ministry of Health of the Czech Republic"<br>"is": "Vaccination Centre South District 3"
| **v.ci** | Unique certificate identifier | HCERT: Unique certificate identifier (UVCI) as specified in the vaccinationproof_interoperability-guidelines_en.pdf (europa.eu)<br>The inclusion of the checksum is optional. The prefix "URN:UVCI:" may be added.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Examples:<br>"ci": "URN:UVCI:01:NL:187/37512422923"<br>"ci": "URN:UVCI:01:AT:10807843F94AEE0EE5093FBC254BD813#B"<br><br>HCERTX: UUID version 4 (see below)

**HCERTX and the Unique certificate identifier (UVCI)**: In the EU HCERT the UVCI identifier is structured, reducing the anonymity features of the identifier and introducing some problems in certificate revocation, especially in relation with personal data regulations. This is the reason why a short HCERT Signature Validity Time is required, and requires the holder of a health certificate to renew it at periodic intervals.

In HCERTX the UVCI is a **U**niversally **U**nique **ID**entifier version 4 (UUID) as defined in [RFC 4122](https://datatracker.ietf.org/doc/html/rfc4122#section-4.4). This UUID is completely urelated to the rest of the information in the credential and reduces the possibility of many types of attacks. This UUID can be used to implement efficient revocation lists without it ever becoming personal information.

### Test certificate

Test group, if present, MUST contain exactly 1 (one) entry describing exactly one test result.

| Field id | Field name | Instructions |
|----------|------------|--------------|
| **t.tg** | Disease or agent targeted: COVID-19 (SARS-CoV or one of its variants) | A coded value from the value set `disease-agent-targeted.json`.<br>This value set has a single entry 840539006, which is the code for COVID-19 from SNOMED CT (GPS).<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"tg": "840539006"
| **t.tt** | The type of test | The type of the test used, based on the material targeted by the test. A coded value from the value set `test-type.json` (based on LOINC). Values outside of the value set are not allowed.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"tt": "LP6464-4" (Nucleic acid amplification with probe detection)<br>"tt": "LP217198-3" (Rapid immunoassay)
| **t.nm** | Test name (nucleic acid amplification tests only) | The name of the nucleic acid amplification test (NAAT) used. The name should include the name of the test manufacturer and the commercial name of the test, separated by a comma.<br>For NAAT: the field is optional.<br>For RAT: the field SHOULD NOT be used, as the name of the test is supplied indirectly through the test device identifier (t/ma).<br>When supplied, the field MUST NOT be empty.<br>Example:<br>"nm": "ELITechGroup, SARS-CoV-2 ELITe MGB® Kit"
| **t.ma** | Test device identifier (rapid antigen tests only) | Rapid antigen test (RAT) device identifier from the JRC database. Value set (HSC common list):<br><ul><li>[All Rapid antigen tests in HSC common list](https://covid-19-diagnostics.jrc.ec.europa.eu/devices?manufacturer&text_name&marking&rapid_diag&format&target_type&field-1=HSC%20common%20list%20%28RAT%29&value-1=1&search_method=AND#form_content) (human readable).</li><li>[https://covid-19-diagnostics.jrc.ec.europa.eu/devices/hsc-common-recognition-rat](https://covid-19-diagnostics.jrc.ec.europa.eu/devices/hsc-common-recognition-rat) (machine-readable, values of the field id_device included on the list form the value set).</li></ul>In EU/EEA countries, issuers MUST only issue certificates for tests belonging to the currently valid value set. The value set MUST be updated every 24 hours.<br>Values outside of the value set MAY be used in certificates issued by third countries, however the identifiers MUST still be from the JRC database. The use of other identifiers such as those provided directly by test manufacturers is not permitted.<br>Verifiers MUST detect values not belonging to the up to date value set and render certificates bearing these as invalid. If an identifier is removed from the value set, certificates including it MAY be accepted for a maximum of 72 hours after the removal.<br>The value set will be distributed from the EUDCC Gateway starting with the gateway version 1.1.<br>For RAT: exactly 1 (one) non-empty field MUST be provided.<br>For NAAT: the field MUST NOT be used, even if the NAA test identifier is available in the JRC database.<br>Example:<br>"ma": "344" (SD BIOSENSOR Inc, STANDARD F COVID-19 Ag FIA)
| **t.sc** | Date and time of the test sample collection | The date and time when the test sample was collected. The time MUST include information on the time zone. The value MUST NOT denote the time when the test result was produced.<br>Exactly 1 (one) non-empty field MUST be provided.<br>One of the following ISO 8601 formats MUST be used. Other options are not supported.<br>YYYY-MM-DDThh:mm:ssZ<br>YYYY-MM-DDThh:mm:ss[+-]hh<br>YYYY-MM-DDThh:mm:ss[+-]hhmm<br>YYYY-MM-DDThh:mm:ss[+-]hh:mm<br>Examples:<br>"sc": "2021-08-20T10:03:12Z" (UTC time)<br>"sc": "2021-08-20T12:03:12+02" (CEST time)<br>"sc": "2021-08-20T12:03:12+0200" (CEST time)<br>"sc": "2021-08-20T12:03:12+02:00" (CEST time)
| **t.tr** | Result of the test | The result of the test. A coded value from the value set `test-result.json` (based on SNOMED CT, GPS).<br>Exactly 1 (one) non-empty field MUST be provided.<br>Example:<br>"tr": "260415000" (Not detected)
| **t.tc** | Testing centre or facility | Name of the actor that conducted the test. Identifiers are allowed as part of the name, but not recommended to be used individually without the name as a text. Max 80 UTF-8 characters. Any extra characters should be truncated. The name is not designed for automated verification.<br>For NAAT tests: exactly 1 (one) non-empty field MUST be provided.<br>For RAT tests: the field is optional. If provided, MUST NOT be empty.<br>Example:<br>"tc": "Test centre west region 245"
| **t.co** | Member State or third country in which the test was carried out | Country expressed as a 2-letter ISO3166 code (RECOMMENDED) or a reference to an international organisation responsible for carrying out the test (such as UNHCR or WHO). This MUST be a coded value from the value set `country-2-codes.json`.<br>The value set will be distributed from the EUDCC Gateway starting with the gateway version 1.1.<br>Exactly 1 (one) field MUST be provided.<br>Examples:<br>"co": "CZ"<br>"co": "UNHCR"
| **t.is** | Certificate issuer | Name of the organisation that issued the certificate. Identifiers are allowed as part of the name, but not recommended to be used individually without the name as a text. Max 80 UTF-8 characters.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Examples:<br>"is": "Ministry of Health of the Czech Republic"<br>"is": "North-West region health authority"
| **t.ci** | Unique certificate identifier | HCERT: Unique certificate identifier (UVCI) as specified in the vaccinationproof_interoperability-guidelines_en.pdf (europa.eu)<br>The inclusion of the checksum is optional. The prefix "URN:UVCI:" may be added.<br>Exactly 1 (one) non-empty field MUST be provided.<br>Examples:<br>"ci": "URN:UVCI:01:NL:187/37512422923"<br>"ci": "URN:UVCI:01:AT:10807843F94AEE0EE5093FBC254BD813#B"<br><br>HCERTX: UUID version 4 (see below)

**HCERTX and the Unique certificate identifier (UVCI)**: In the EU HCERT the UVCI identifier is structured, reducing the anonymity features of the identifier and introducing some problems in certificate revocation, especially in relation with personal data regulations. This is the reason why a short HCERT Signature Validity Time is required, and requires the holder of a health certificate to renew it at periodic intervals.

In HCERTX the UVCI is a **U**niversally **U**nique **ID**entifier version 4 (UUID) as defined in [RFC 4122](https://datatracker.ietf.org/doc/html/rfc4122#section-4.4). This UUID is completely urelated to the rest of the information in the credential and reduces the possibility of many types of attacks. This UUID can be used to implement efficient revocation lists without it ever becoming personal information.



### Recovery certificate

Recovery group, if present, MUST contain exactly 1 (one) entry describing exactly one recovery 
statement. All elements of the recovery group are mandatory, empty values are not supported.

Its specification is not detailed here, but you can get it from the official eHealth site.