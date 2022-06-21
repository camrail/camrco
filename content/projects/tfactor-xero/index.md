---
title: Xero SyncIO
date: 2019-11-15
description: 'Xero sync for invoice financing.'
---
Invoice financing companies operate by lending against the collateral of their client's invoices.
This means that they need true and accurate descriptions of their client's position, which is historically
provided to them via their clients as they manually email accross each new invoice that they raise.

Xero SyncIO was conceptualised to replace a companies' manual invoice upload process with a direct pull of data from
Xero.  The invoices were then fed into the existing .net factoring loan book tracking software ("FactorS").

# What is it?
Xero SyncIO was designed to enhance and increase the iteration frequency of an existing .net application for a prominent invoice financing company.
When I first started working with the company, invoices were manually inputted once clients emailed scans of the paper versions.  This was a 
very manual and time consuming process.

The project I was asked to work on was to:
- Stand up a separate system which could pull invoices from Xero and stage them for analysis.
- Once approved, the invoices were to be pushed into the existing .net software which manages the loan book.
- As not all clients were Xero clients, introduce OCR technology to digitise paper invoices.

# The Stack.
### Webapp
- Python/Django
- Vanilla JS

### Serving
All on Azure
- Docker
- SQL
- Azure blob storage
- Azure pipelines

### Key python packages
- Google Cloud Vision
- Invoice 2 PDF
- Celery
- pyXero

# Credits
### My roles:
- Development
- Product Management
- Devops

# Key Components
## Invoice Staging
While it is desirable to keep the client's Xero invoices in sync with FactorS, the loan book management software, only approved and genuine changes
should be pushed.  The risk that we are trying to protect against is that a client edits an invoice within Xero fradulently upwards, 
and they then attempt to increase their financing facility off the back of this because it has fed through directly into FactorS.

To prevent this, changes need to be staged within Xero SyncIO for a finance manager to then validate and approve.  The technical flow was built as follows:
- SyncIO susbcribes to and receives a webhook from Xero that an invoice has been updated
- SyncIO queries and saves down the json of the update, ready for analysis
- A finance manager checks the invoices ready to be approved
- The json is mapped to an in-memory Django model instance and compared against the database instance
- Diffs are created to display to the finance manager a 'changed report'
- Finance manager can then assess and approve or reject the change
- If approved, process moves to "Invoice Tfactor Push" below

## Invoice FactorS Push
One of the key success criteria for this project was that it was able to be setup quickly, lays the groundwork for future iterationsl, and also that
it interfaces with legacy systems.  Given there was an existing .net application (FactorS) with multiple databases, the object model for SyncIO 
needed to either match or map to this.  A combination of mapping and matching schemas was used within the Django application which meant that once
an invoice change (or addition) was approved by a finance manager, SyncIO was able to push this into the existing FactorS database and make this
available to business as usual processes.  This also involved the transfer of blobs of the actual invoice PDFs from Xero to Azure Blob Storage.

## Invoice OCR
A direct pull of invoice information from the source-of-truth Xero API is the best case scenario from a data security and fraud perspective.
It is of course still possible for clients to fabricate invoices within their software, however the audit trails are significantly increased
which makes it riskier from their perspective.

Unfortunately, not all clients use Xero and not all accounting software providers have an API.  As such, there wil always be clients who share
invoices the old way, that is by emailing copies of them.  Historically, these invoices are manually reviewed and inputted into FactorS, however
this feature looked to automate the reading and storage of the data via optical character recognition.

I implemented this via a combination of Google Cloud Vision (GCV) which extracts text from the invoices, and then a series of customiseable templates 
which staff could setup to extract the key information of the invoice.  The template solution:
- Had the ability to see the sample raw text available after a GCV process had occured
- A regex tester, so that the result of a regex for a particular value could be seen
- A mapper, to map the variable within the template to the database field for digitising the invoice

## Reporting
Finally, I created a series of reports using D3js to quickly visualise the data within the existing databases.  The company had previously been using
direct SQL queries from Excel to produce this information, however by bringing these capabilities online they were able to reduce the risk of 
a fat finger error directly impacting their data.

  