# ERMI Batch File Format. (v2.4.0)

ERMI is a lightweight transaction monitoring tool designed to simplify compliance with AML and related regulations. This repository contains information on the ERMI file format and how to provide data to ERMI for analysis.

## Overview

For batch operation ERMI consumes a CSV [RFC 4180](https://tools.ietf.org/html/rfc4180) formatted text file. This file is made up of a number of columns representing the ERMI Data Model. This specification is versioned using the [semvar versioning system](https://semver.org). The version can be found at the top of this document, the latest version can always be found at https://github.com/ermi-ltd/ermi-file-format/

## ERMI Data Model.

The ERMI Data Model splits each transaction into multiple elements. Each row represents one transactions and consists of:

1. Up to four parties (see below) who take a role in the transaction.
2. The transaction details such as date, value and currency
3. Optional configuration and rule targeting settings.

The four parties are:

**Payer** - The payer is the person or legal entity initiating a transaction, sometimes known as the "ultimate" payer.

**Beneficiary** - The beneficiary is the person or legal entity receiving a payment, sometimes known as the "ultimate" beneficiary

**Sender (optional)** - The legal entity which is sending the payment on behalf of the payer. (Individual senders are not supported)

**Receiver (optional)** - The legal entity or person which is receiving the payment on behalf of the beneficiary.

## Examples.

An example of the minimum required for a valid ERMI File Format can be found at [minimal.csv](https://github.com/ermi-ltd/ermi-file-format/blob/master/minimal.csv).

An example of a full ERMI File Format file can be found at [full.csv](https://github.com/ermi-ltd/ermi-file-format/blob/master/full.csv).

## Configuration

During analysis ERMI will reference external sources such as country risk scores and exchange rate data. These external data sources can be customised to provide targeted screening on a per transaction or per payer basis. If a custom list is required, please contact [our support team](mailto:support@ermi.co.uk)

## General Conventions.

The following conventions are true for all ERMI Batch File Format files:

1. The first row **MUST** be a heading row listing columns. Columns **MAY** appear in any order.
2. Date columns **MUST** be in [ISO-8061 format.](https://www.iso.org/iso-8601-date-and-time-format.html) including a time and timezone.
3. Countries **SHOULD** be in [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) format.
4. Currencies **MUST** be referred too using the [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) standard names
5. Column names are case sensitive and must appears exactly as defined below.
## Columns Overview.


### Required Columns:

The following Columns **MUST** be present in all files.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| transactionID | A uniqueID for this transaction | ```0-9[a-Z]``` |
| date | The date and time at which the transaction occurred | [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| currency | The currency used for the transaction represented as a currency code | [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) | 
| value | The quantity of currency sent as a float or integer | ```[0-9]+(\.[0-9][0-9]?)? ```|
| beneficiaryID | A unique ID representing the beneficiary person or legal entity (for example, ERMI Software Ltd) | ```0-9[a-Z]``` |
| beneficiaryCountry | The location where the beneficiary resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| payerID | A unique ID representing the beneficiary person or legal entity (for example, ERMI Software Ltd) | ```0-9[a-Z]``` |
| payerCountry | The location where the payer resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| payerType   | The payer entity type. | 'individual' or 'corporate' |

## Optional Columns

The following columns **MAY** be present for the _payer_.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| payerFirstName | The payers first name. (Blank if corporate) | ```0-9[a-Z]``` |
| payerLastName | The payers last name. (Blank if corporate) | ```0-9[a-Z]``` |
| payerCompanyName | The payers company name (Blank if individual)| ```0-9[a-Z]``` |
| payerCompaniesHouseNumber | The payers company house number if UK (Blank if individual)| ```0-9[a-Z]``` |
| payerPostcode | The payers postcode| ```0-9[a-Z]``` |
| payerCreatedAt | The date and time at which the payer was created| [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| payerAccountID | Human readable unique account ID | ```0-9[a-Z]``` |
| payerAccountNumber | Payer account number within.  | ```0-9[a-Z]``` |
| payerAccountRoutingCode | Payer account routine code, clearing code, or sort code  | ```0-9[a-Z]``` |
| payerAccountCountry | The location of the payers bank account branch | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| payerAccountIBAN | Payer account IBAN  | ```0-9[a-Z]``` |
| payerAccountBicSwift | Payer account Bic Swift  | ```0-9[a-Z]``` |
| payerRisk | The risk profile of the payer | 'very low', 'low', 'medium', 'medium high', 'high' |
| payerCountryRiskList | The country risk list to use during analysis | 'default' or [custom lists](#configuration) | 
| payerCountryRiskList | The country risk list to use during analysis | 'default' or [custom lists](#configuration) | 
| pastYearExpectedTotal | The expected total value of transactions in the period 365 days before the screen period | ```[0-9]+(\.[0-9][0-9]?)? ```| 
| pastYearExpectedTotalCurrency | The currency used for setting the past year expected total | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| pastYearExpectedTotalLastUpdatedDate | The date when the past year expected total was last updated | [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| pastNinetyDayExpectedTotal | The expected total value of transactions in the period 90 days before the screen period | ```[0-9]+(\.[0-9][0-9]?)? ```| 
| pastNinetyDayExpectedTotalCurrency | The currency used for setting the past ninety day expected total | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| pastNinetyDayExpectedTotalLastUpdatedDate | The date when the past ninety day expect total was last updated | [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |

The following columns **MAY** be present for the _beneficiary_.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| beneficiaryFirstName | The beneficiaries first name. (Blank if corporate) | ```0-9[a-Z]``` |
| beneficiaryLastName | The beneficiaries last name. (Blank if corporate) | ```0-9[a-Z]``` |
| beneficiaryCompanyName | The beneficiaries company name (Blank if individual)| ```0-9[a-Z]``` |
| beneficiaryPostcode | The beneficiaries postcode| ```0-9[a-Z]``` |
| beneficiaryCreatedAt | The date and time at which the beneficiary was created| [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| beneficiaryPostcode | The beneficiaries postcode| ```0-9[a-Z]``` |
| beneficiaryAccountID | Human readable unique account ID | ```0-9[a-Z]``` |
| beneficiaryAccountNumber | Beneficiaries account number.  | ```0-9[a-Z]``` |
| beneficiaryAccountRoutingCode | Beneficiaries account routine code, clearing code, or sort code  | ```0-9[a-Z]``` |
| beneficiaryAccountCountry | The location of the beneficiaries bank account branch | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| beneficiaryAccountIBAN | Beneficiaries account IBAN  | ```0-9[a-Z]``` |
| beneficiaryAccountBicSwift | Beneficiaries account Bic Swift  | ```0-9[a-Z]``` |

The following columns **MAY** be present for the _sender_.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| senderID | Human readable unique sender ID | ```0-9[a-Z]``` |
| senderCompanyName | The sender company name | ```0-9[a-Z]``` |
| senderCountry | The location where the sender resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| senderRisk | The risk profile of the sender | 'very low', 'low', 'medium', 'medium high', 'high' |
| senderCreatedAt | The date and time at which the sender was created| [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| senderPostcode | The senders postcode| ```0-9[a-Z]``` |
| senderAccountID | Human readable unique account ID | ```0-9[a-Z]``` |
| senderAccountNumber | Senders account number.  | ```0-9[a-Z]``` |
| senderAccountRoutingCode | Senders account routine code, clearing code, or sort code  | ```0-9[a-Z]``` |
| senderAccountCountry | The location of the senders bank account branch | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| senderAccountIBAN | Senders account IBAN  | ```0-9[a-Z]``` |
| senderAccountBicSwift | Senders account Bic Swift  | ```0-9[a-Z]``` |

The following columns **MAY** be present for the _receiver_.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| receiverId | Human readable unique receiver ID | ```0-9[a-Z]``` |
| receiverType   | The receiver entity type. | 'individual' or 'corporate' |
| receiverFirstName | The receivers first name. (Blank if corporate) | ```0-9[a-Z]``` |
| receiverLastName | The receivers last name. (Blank if corporate) | ```0-9[a-Z]``` |
| receiverCompanyName | The receivers company name (Blank if individual)| ```0-9[a-Z]``` |
| receiverCountry | The location where the receiver resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| receiverRisk | The risk profile of the receiver | 'very low', 'low', 'medium', 'medium high', 'high' |
| receiverCreatedAt | The date and time at which the receiver was created| [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| receiverPostcode | The receivers postcode| ```0-9[a-Z]``` |
| receiverAccountID | Human readable unique account ID | ```0-9[a-Z]``` |
| receiverAccountNumber | Receivers account number.  | ```0-9[a-Z]``` |
| receiverAccountRoutingCode | Receivers account routine code, clearing code, or sort code  | ```0-9[a-Z]``` |
| receiverAccountCountry | The location of the  receivers bank account branch | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| receiverAccountIBAN | Receivers account IBAN  | ```0-9[a-Z]``` |
| receiverAccountBicSwift | Receivers account Bic Swift  | ```0-9[a-Z]``` |

## Support

If you require support, please contact support@ermi.co.uk or speak with your MLRO

