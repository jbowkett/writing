It's 2020, can I have my ML models now?
==

Recent years have seen many companies consolidate all their data into a data lake/warehouse of some sort.  Once it's all
consolidated, what next?

Data swamp?...data graveyard?

Many companies consolidate data in a field of dreams mindset - "build it and they will come", however a comprehensive data 
strategy is needed if the ultimate goals of an organisation are to be realised: monetisation through Machine Learning and 
AI is an oft-cited goal.  Unfortunately, before one rushes to the sexy world of machine learning, one should not ignore the 
more mundane foundations.  Indeed, Google estimate XXX% of the time taken in data science projects is devoted to so-called 
data-wrangling.  But what foundations are needed?

Data Cataloguing
===

_"what good is data collection if we don't know what we have?"_

The fast track to creating a data graveyard is to diligently add any and all data into it, without keeping track of what 
data has been put in, what it means and what data types it constitutes (strong typing for data is always preferable to just
unstructured collections of strings).  A good data catalogue will feature:
 - a searchable repository - many products on the market will allow for a searchable web site.  This should also be 
   explorable via the data lineage to put data fields into their context - both source(s) and destination(s) 
 - version control / labelling - this often-overlooked feature is possibly the most important, especially when implementing 
   the data governance model, and allowing services to self-describe, based on their version, which levels/versions of a 
   data model they support.  This integration must be supported programmatically from deployment command-line tools, 
   such as Ansible or Salt for integration into CI/CD pipelines.
 - Release cycle integration - while this goes hand-in-hand with versioning it is subtley different.  A good platform 
   will allow a set of catalogue entries to be promoted/labelled in one go so as to constitute a release.  This - again - 
   should be callable from the command-line/API to ensure integration with release tools and the CI/CD process.  
 - Environment-aware - possibly not needed if we are version aware...   
 - Permission model - with the above features, different users (who can and should be identified using a human-centric 
    approach, such as design-thinking), will perform different tasks on the data model at different times, so a 
    role-based permission model is essential to keep catalogue operations secure. 
 
Mention facebook's approach???...is it still valid?

Data Governance
=====

_"What good is data if we can't keep it secure?"_

Of course all data should be governed and permissioned, but there are several granularities to data governance, each more 
difficult - yet powerful - than the last. (also less prevalent as we move down the list...).  Some of the approaches 
below have sometimes been implemented at the application layer, but as data becomes more centralised, it is necessary to 
remove this concern from the application in order to consistently govern data globally.   
 • Table/collection-level permission - this is the easiest and most common way of securing data - the data being published 
   is restricted, often at the application layer.
 • Field/column level permission - the data is restricted by which fields can be returned from a query.  The choice 
   could be to mask out forbidden columns or simply not return them.
 • Row/document-level permission - the data is restricted by which collection of documents/rows are returned from a query. 
 • Cell-level permission - this is a combination of the previous 2 schemes working in concert.  It means individual 
   rows will return different fields based on the permissions (or possibly based on some attribute/characteristic of the 
   data) for the document and either return masked data, null or - in the case of schemaless/nosql databases - the 
   document minus that field. 
 • Source+cell-level permission - this is a new way of securing a data lake, not just based on the individual row or 
   document, but based on the collection of sources - the lineage - of that data.  This allows the data to be secured 
   at a field level based on where the data has come from.  Similarly, this model allows for data derived from a 
   protected field to maintain that protection/permissions if desired.
 • Attribute-based permission - this is also a new way of securing a data lake, where a row, field or document is 
   restricted based on some other property, for instance to grant access for an HR user to see see employee details 
   globally but salary information only for documents from their home region.

Data masking??

When data is the lifeblood of an organisation, the catalogue and its governance is the backbone that guarantees its 
integrity and protects your investment. 

Data chargeback models (for large enterprises)
======
_"what good is data if we can't monetise it?"_
Once the basics of good governance are in place, it is then possible to track and audit access to the data.  This should 
be done at a level as fine-grained as possible, ideally at a field level.  Depending on the size of the organisation, 
this then allows for the relevant department to be charged for their usage.  This may seem mercenary at first, however 
in larger organisations, the capital expenditure of the on-prem hardware and the ongoing labour costs (or purely opex 
if hosting in the cloud), will need to be covered to secure ongoing investment in data availability.

Even without a chargeback model, it is advisable to gather statistics on access and usage (often called "exhaust data") 
to better inform future decisions when planning operational or data model changes.    
 
Data Lineage
======
_"what good is data if we don't know where it came from?"_
Central data stores by their very nature consume data from other sources and often perform ETL-like operations to 
sanitise and clean data and then produce that data and serve to other systems.  As systems become more interconnected 
and interdependent, it will be necessary to track and ensure the provenance of data as it traverses the organisation 
(or even published to external consumers).

Many applications can detail how different source systems are connected.  However this is not enough.  It is necessary 
for individual rows/documents to be able to describe where their values came from, as this may easily change over time 
or vary from document to document within the same collection depending on how or when they were constructed.    

With the correct lineage recording in place, it is then possible to answer the following important questions of your 
data estate:

 • If this field/cell is tainted/incorrect, who does it effect?
 • If this source is re-run, what other data points are effected?
 • How are my data supplying systems inter-connected?

Data lineage is set to become even more important as machine learning models start to derive ever-more data points that 
are used to make decisions, and those decisions will need to be explained and reproduced to regulators and consumers alike.   

Data Delivery
======
_"what good is data if we can't get it to the right place at the right time?"_

No data strategy would be complete without a model or mechanism for delivering data to consumers who need it at the 
right time.  Now that Big Data is "part of the furniture", it is making way for Fast Data.  Streaming data platforms 
such as Kafka and Pulsar mean that data can be delivered in a reliable, scalable and immutable way.  It should be noted 
that while there are designs/patterns/idioms/architectures for a digital nervous system with a streaming platform as its 
backbone, in today's landscape it would require a great deal of work to also incorporate the other facets of data 
strategy outlined herein. 

Data insight
======
_"what good is data without insight?"_

Water, water everywhere/ Nor any drop to drink - Rime of the ancient mariner

It is not enough for an organisation to merely centrally store all the data it generates and collects.  The data needs 
to be analysed and new facts or insight generated from that data.  A comprehensive data science strategy is needed 
(possibly in conjunction with a team of citizen developers), the exact nature of this will align with specific business 
goals and profit/cost lines.  Design thinking workshops can help here by ideating what kinds of insights might be of use    
to different groups of stakeholders, then devising strategies for achieving those goals (or finding out more where 
necessary).

It is imperative that a good data insight vision will be supported by an industrialised data science process, with the 
appropriate tooling and reproducibility enforced and enabled by the platform.  This is explored in our followup blog post 
/coming soon!/.  


Data visualisation
======
_"what good is insight if we can't see/explain what we have?"_

Hand-in-hand with insight, comes data visualisation.  Once the data is collected it is important that it is displayed 
to the user in a way that assists their understanding of what is being displayed.  This is an artform in and of itself, 
however David Mcandles' excellent book "Information is Beautiful" cna be of use for developers, with many of the charts 
being available as D3.js add-on libraries. 

Data testing/Integrity?
=====
_"How can I rely on the data if I don't know how good it is?"_

In order to remain responsive to change, and to continue pouring data into the data lake, it is necessary 
to devise a data testing strategy the aligns with the approach to continuous delivery.  This will ensure the data 
integrity is maintained - i.e. that it is still usable, appropriate, discoverable and secure with correct and queryable 
meta data continually gathered (for lineage and such) with little overhead on application development teams, while 
guaranteeing the consistency that centralising the data function brings.  

When data was smaller and more manageable, database restores from production were commonplace.  However, in the age of 
big data lakes and warehouses this may not always be possible.  In cases where regression testing of new releases 
against production is necessary and data volumes make complete population comparisons prohibitively expensive, inserting 
an immutable message log (such as Apache Kafka or Pulsar) inbetween the data delivery and the ingestion processes can be 
invaluable.  Sometimes named the "wiretap" or "tee" pattern, it enables constant regression testing and comparison with 
production data.  If this is tested within a canary environment with accompanying release note and comparison regression 
test packs, this can provide very early feedback to development teams and empower them to deliver releases more frequently 
into production. 

     




Follow-up posts:
======

How to record data lineage
How to run ML in production