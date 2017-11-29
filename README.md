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

## Headers.

The first row of an ERMI File Format file MUST be a heading row listing columns. Column MAY appear in any order.

## Columns.

The following Columns are required in ALL ERMI File Format files.

| Name   | Brief Description | Validation |
| ------ | ----------------- | ---------- |
| transactionID | A uniqueID for this transaction | 0-9[a-Z] |



