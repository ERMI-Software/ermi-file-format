# ERMI Batch File Format. (v2.0.1)

ERMI is a lightweight transaction monitoring tool designed to simplify compliance with AML and related regulations. This repository contains information on the data ERMI requires and documents the format for the input files ERMI accepts when operating in batch analysis mode.

## Overview

For batch operation ERMI consumes a CSV [RFC 4180](https://tools.ietf.org/html/rfc4180) formatted text file. This file is made up of a number of columns representing the ERMI Data Model. This file is versioned using the [semvar versioning system](https://semver.org). The version can be found at the top of this document, the latest version can be found at  https://github.com/ermi-ltd/ermi-file-format/releases

## ERMI Data Model.

For ERMI to effectively monitoring transactions, a few different items of data are needed. The data model consists of 4 components.

**Payer** - The payer is the person or legal entity initiating a transaction. 

**Beneficiary** - The receiving is the person or legal entity receiving a payment.

**Transaction Information** - the facts of the payment, such as amount, currency, time and status.

**Account Information** - the specific accounts used by the beneficiary and the payer for this transaction

## Examples.

An example of the minimum required for a value ERMI File Format can be found at minimal.csv in this repo. 
An example of a full ERMI File Format file can be found at full.csv

## General Conventions.

The following conventions are true for all ERMI Batch File Format files:

1. The first row **MUST** be a heading row listing columns. Columns **MAY** appear in any order.
2. Date columns **MUST** be in [ISO-8061 format.](https://www.iso.org/iso-8601-date-and-time-format.html)
3. Countries **SHOULD** be in [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) format.
4. Currencies **MUST** be referred too using the [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) standard names

## Columns Overview.

The following Columns **MUST** be present in all files.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| transactionID | A uniqueID for this transaction | 0-9[a-Z] |
| date | The date and time at which the transaction occurred | [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| currency | The currency used for the transaction represented as a currency code | [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) | 
| value | The quantity of currency sent as a float or integer | [0-9]+(\.[0-9][0-9]?)? |
| benificaryID | A unique ID representing the beneficiary person or legal entity (for example, ERMI Software Ltd) | 0-9[a-Z] |
| benificaryCountry | The location where the beneficiary resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| payerID | A unique ID representing the beneficiary person or legal entity (for example, ERMI Software Ltd) | 0-9[a-Z] |
| payerCountry | The location where the payer resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| payerType   | The payer entity type. | 'individual' or 'corporate' |

The following columns **SHOULD** be present for the payer.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| payerFirstName | The payers first name. (Blank if corporate) | 0-9[a-Z] |
| payerLastName | The payers last name. (Blank if corporate) | 0-9[a-Z] |
| payerCompanyName | The payers company name (Blank if individual)| 0-9[a-Z] |
| payerPostcode | The payers postcode| 0-9[a-Z] |
| payerCreatedAt | The date and time at which the payer was created| [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| payerAccountID | Human readable unique account ID | 0-9[a-Z] |
| payerAccountNumber | Payer account number within.  | 0-9[a-Z] |
| payerAccountRoutingCode | Payer account routine code, clearing code, or sort code  | 0-9[a-Z] |
| payerAccountIBAN | Payer account IBAN  | 0-9[a-Z] |

The following columns **SHOULD** be present for the beneficiary.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| beneficiaryFirstName | The beneficiaries first name. (Blank if corporate) | 0-9[a-Z] |
| beneficiaryLastName | The beneficiaries last name. (Blank if corporate) | 0-9[a-Z] |
| beneficiaryCompanyName | The beneficiaries company name (Blank if individual)| 0-9[a-Z] |
| beneficiaryPostcode | The beneficiaries postcode| 0-9[a-Z] |
| beneficiaryCreatedAt | The date and time at which the beneficiary was created| [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| beneficiaryPostcode | The beneficiaries postcode| 0-9[a-Z] |
| beneficiaryAccountID | Human readable unique account ID | 0-9[a-Z] |
| beneficiaryAccountNumber | Beneficiaries account number.  | 0-9[a-Z] |
| beneficiaryAccountRoutingCode | Beneficiaries account routine code, clearing code, or sort code  | 0-9[a-Z] |
| beneficiaryAccountIBAN | Beneficiaries account IBAN  | 0-9[a-Z] |

## Support

If you require support, please contact support@ermi.co.uk or speak with your MLRO


