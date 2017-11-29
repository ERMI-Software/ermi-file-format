# ERMI Batch File Format. (v2.0.1)

ERMI is a lightweight transaction monitoring tool designed to simplify compliance with AML and related regulations. This repository contains information on the data ERMI requires and documents the format for the input files ERMI accepts when operating in batch analysis mode.

## Overview

For batch operation ERMI consumes a CSV [RFC 4180](https://tools.ietf.org/html/rfc4180) formatted text file. This file is made up of a number of columns representing the ERMI Data Model. This file is versioned using the [semvar versioning system](https://semver.org)

## ERMI Data Model.

For ERMI to effectively monitoring transactions, a few different items of data are needed. The data model consists of 4 componants.

**Payer** - The payer is the person or legal entity initiating a transaction. 

**Benificary** - The benificary is the person or legal entity recieving a payment.

**Transaction Information** - the facts of the payment, such as amount, currency, time and status.

**Account Information** - the specifc accounts used by the benificary and the payee for this transaction 

## Examples.

An example of the minumum required for a valud ERMI File Format can be found at minimal.csv in this repo. 
An example of a full ERMI File Format file can be found at full.csv

## General Conventions.

The following conversion are true for all columns and files.

1. The first row of an ERMI File Format file **MUST** be a heading row listing columns. Columns **MAY** appear in any order.
2. Date columns **MUST** be in [ISO-8061 format.](https://www.iso.org/iso-8601-date-and-time-format.html)
3. Countries **SHOULD** be in [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) format.
4. Currencies are reffered to using the [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) standard names

## Columns Overview.

The following Columns **MUST** be present in all files.

| Name   | Brief Description | Validation |
| :----- | :---------------- | :--------- |
| transactionID | A uniqueID for this transaction | 0-9[a-Z] |
| date | The date and time at which the transaction occured | [ISO-8061](https://www.iso.org/iso-8601-date-and-time-format.html) |
| currency | The currency used for the transaction represented as a current code | [ISO 4217](https://www.iso.org/iso-4217-currency-codes.html) | 
| amount | The quantity of currency exchanged as a float or integer | [0-9]+(\.[0-9][0-9]?)? |
| benificaryID | A unique ID representing the benificary person or legal enetity (for example, ERMI Software Ltd) | string |
| benificaryCountry | The location where benificary resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |
| payerID | A unique ID representing the benificary person or legal enetity (for example, ERMI Software Ltd) | string |
| payerCountry | The location where payer resides | [ISO-3166 ALPHA 2](https://www.iso.org/iso-3166-country-codes.html) |




