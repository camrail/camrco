---
title: Lending Contract Generator
date: 2020-09-05
description: 'New loan contract and other artifact generation.'
---
In highly regulated industries many processes are still "paper-based" even if they are digitally delivered.
That means that even though the document might go back and forth via email, it still needs to be edited, reviewed, generated
and signed using legacy processes.

I recently worked with a client who accepts funds from lenders, with each new loan taking the client many hours to setup.
Although the process was formulaic, they had to go through 6 different word documents and update all the appropriate areas,
then save different versions of each (think "Lender Copy", "Borrower Copy"), then email.

The goal of this project was to automate this new lender documentation requirement.

# What is it?
The Lending Contract Generator's goal was to replace an existing, erorr prone and highly manual business process with a
consistent online system.  To achieve this, the Contract Generator would need to:

- Source existing clients from the legacy database
- Provide a mechanism to add new clients who were not in the legacy database
- Be able to capture the relevant information against these clients to generate the Contract Documents
- Provide a templating system so that business users could edit the contract documents as their business evolved
- Create an archive of sent documents, visible to clients on their existing .net portal
- Provide drip email scheduling to launch reminders for lack of document progress

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
- Django
- docxtpl
- Celery
- django-drip

# Credits
### My roles:
- Development
- Product Management
- Devops

# Key Components
## Contract and Email Template Management
Given the business was already using Word documents for their contract templates, I leveraged
these within the solution by instructing staff to add {{merge}} fields.  Key person and loan
related database fields were made available within the document and email templates, and staff
could upload new .docx files when templates were required to change.

At runtime, the active .docx template for the given purpose was merged with the variables from the database and
then converted to PDF by the system.

## Key Data Capture
The requirement to issue loan documents usually means that the lender and/or the loan are not currently
in the system, thus part of this project involved a wizard to collect the information required
for the new loan, and then save this information in such a way that it would be available within the
existing legacy system.

I had to therefore map the Django data model to the existing system so that data originating
from either source would be compatible.  The new contract wizard was then able to create a new client,
create a new loan and then send the documents off for generation (see next component).

## Contract and Email Sending
Once key information was available within the system, we could get to work generating the document
packs required for a particular milestone.  Some examples of these milestones are:
- New loan
- Rolling a loan over (lending again once expired)
- Loan expired

What each of these milestones had in common was that an email needed to be sent which included important
information about the milestone, and, one or many PDFs needed to be included in that email which required
the recipient to do somehting.

The app was therefore structured so that staff could link an email template to one or many document templates,
and when a milestone was activated: 
- The email template would be retrieved and merged (forming the fully rendered email message)
- Each document/Contract template linked to the email would be retrieved and merged
- The documents would then be saved to PDF and the PDF and/or the .docx version would be attached to the email
- The email would be sent

## Email Scheduling
Generating the documents and sending them is the upfront time spent with a new lender, but what happens when the 
lender then sits on the docs?

The business I was working with spent a significant amount of time monitoring and following up on applications, so 
I helped this by creating a configurable drip email campaign to auto-follow-up if the status of the application stagnated.

This was implemented via Celery and Django-Drip which was made by the guys over at Zapier.  Although Drip has been archived,
with some tinkering it provides a really great base to have staff configurable email campaigns.  For this implementation,
I made all the key loan variables available to the configurable drip campaign, so that the staff could say trigger the campaign
if status == "Waiting for Signature" and last_update_time on the record is more than 30 days ago.

Monthly loan status email were also configurable via this functionality because once again, all the loan variables were available
to the drip campaign creator.

## Document Archive
Finally, all documents needed to be saved down for regulatory purposes, which includes the original docs as well as they signed docs.
The original documents were fairly easy given that upon generation we could simply save them against the Azure Blob Storage for 
future access within both this new system and the legacy system.

Signed documents however were a bit more complicated, however these were saved down by setting up an email inbox which then ingested attachments
and saved them down to the document vault.  One point of manual intervention however was required by the staff before they forwarded on the signed documents
to the designated email address.  This is the staff needed to remove any non-core attachments (think email signature images) otherwise
these unimportant files would be saved down with the important contracts.
  