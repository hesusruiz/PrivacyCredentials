# Abstract

**PrivacyCredentials** is a generic credential system which is designed to be secure, privacy-preserving, scalable, high-performance and robust. It is designed specifically for some important use cases where physical, on-person verification of identity of holder is needed and where normal W3C Verifiable Credential flows are not fully suitable as they are normally designed currently.

The system is designed to support credentials in different formats, initially the [W3C Verifiable Credential](https://www.w3.org/TR/vc-data-model/) format and the **HCERT** format used in the [EU Digital COVID Certificate](https://ec.europa.eu/info/live-work-travel-eu/coronavirus-response/safe-covid-19-vaccines-europeans/eu-digital-covid-certificate_en) system for health certificates in the EU.

It enables production implementations today, because it is designed to be compatible with all current regulations in place today in the European Union (and many other regions of the world).

This document describes the overall architecture of the system, including the credential formats, interaction among the actor involved and the Trust Framework which it implements.

Special focus is put on privacy of personal data in real-life scenarios like health-related credentials, and in interoperability with other current and future systems.

