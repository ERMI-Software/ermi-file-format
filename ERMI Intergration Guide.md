# ERMI Integration Guide.

The purpose of this document is too provide a high level overview of ERMI and to provide guidance on how to get data into and out of ERMI.

ERMI is intended to be flexible, this document outlines the most popular options and approaches however we're happy to approach things differently if needed. If you require any support, please contact Mike or Jamie.

## Document Outline:

1. High Level Description & Glossary
2. Getting Started
3. Getting Data into ERMI
	1. Push model
	2. Pull model
4. Getting Results out of ERMI
	1. Human Readable Reports
	2. Machine readable data files
5. Error Handling & Validation

## 1. High Level Description & Glossary

ERMI has the following process:

1. A *job* is scheduled (daily or weekly)
2. At the scheduled time, *ERMI* loads data from the *source*.
3. ERMI validates the file, analyses the data, and produces set of *results* consisting of a human readable *report* and some machine readable *data files*. 
4. The *report* and *data files* are placed into an *output* location for the *client* to access.

The key terms are:

**Client** - The client is the organisation which holds the transaction data for ERMI to monitor.

**ERMI** - ERMI is the name of the transaction monitoring system, and the company.

**Job** - a job is a single run of ERMI. Jobs are typically run weekly or daily. 

**Results** - results represent the output of a single job, consisting of a human readable report and one or more machine readable data files.

**Source** - The source is where ERMI gets the data from. This may be an API, or a data storage system such as S3 or Drive.

**Output** - The output is the location where results are too pushed

## 2. Getting started. 

When getting started with ERMI, we may ask you to provide an example file. We will then validate and run it a few times. This gives us a chance to check the file is running smoothly and to correct any minor issues.

We have the ability to do some data conversion on the fly, so if you encounter any issues please let us a know and we will try out best to help you.

## 3. Getting Data into ERMI

To get data into ERMI we require a *source*. Typically a source is a file on a storage system, or an API.  

ERMI supports two types of sources, *Push* sources where data is pushed by the client into a resource managed by ERMI, and *Pull* sources where data is pulled into ERMI from a client's system on demand. 

Which model is best will depend on a number of factors such as frequency of analysis and volume of data. 

### i. Pull Model

The *pull* model is where ERMI accesses a client owned system to pull transaction data on demand for each job. The client system is either an API or access to an object store (such as S3). If the file is loaded from an object store it would be in the ERMI file format.

For a pull source the data is only stored on a client system. The clients data is copied for processing, but deleted once processing is completed.

The pull approach is well suited for clients using currency platforms such as The Currency Cloud. For client using the pull approach we can provide a conversion / integration service to aid in formatting and validating data.

### ii. Push Model.

The push model is where the client pushes data to ERMI ahead of each scheduled job. ERMI can accept data via AWS S3, or via Google Drive. Under the push model, the ERMI owned S3 bucket retains a copy of the clients data until the client deletes it.

## 4. Getting Results out of ERMI. 

Like sources, results can be pulled from ERMI or pushed from ERMI into a client owned system. ERMI can push results into a client managed S3 bucket, or clients can pull results from a ERMI managed S3 bucket or Google Drive account.

Each set of results consists of a report and a collection of machine readable files.

### i. Human Readable Report

Each set of results include a report is a excel xlsx file intended to be read by a human. The data in the file is sorted, grouped and formatted to provide an easy to use file.

The report period depends on how often client jobs are being run. Typically report periods are 1 day or 7 days.

### ii. Machine Readable Data Files.

Alongside the report in the file are one or more machine readable data files. Each files is a CSV file which builds on the ermi-file-format by adding three additional columns:

**Flag Reason** - the reason the transaction or payer or was flagged. 
**Flag Level** - the level of the flag (between yellow, orange and red)
**Flag Code**- an internal code for the flag, used for grouping and internationalisation  

## 5. Error Handling & Validation

ERMI has two complimentary systems. Error handling (for when something has gone wrong with the system) and validation (when something is wrong with the data).

### Error Handling.

We proactively monitoring ERMI during each job. If an error occurs during a job we will retry the job and attempt to work out the issue. If we're unable to resolve the issue we will contact the client to discuss next steps.

### Validation

At the start of each job ERMI runs a validator over each file. The validator checks that all required columns are present and every row in the file contains non null values for each required column. 

Invalid rows are passed back to the client via an invalid.csv file within job results.
