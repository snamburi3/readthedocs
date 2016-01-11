****************
Hosted TCGA Data 
****************

The ISB-CGC hosts approximately 1 petabyte of TCGA_ data in Google Cloud
Storage (GCS_) and in BigQuery_.  

.. _TCGA: http://cancergenome.nih.gov/
.. _GCS: https://cloud.google.com/storage/
.. _BigQuery: https://cloud.google.com/bigquery/

The data being hosted by the ISB-CGC was obtained from the two main TCGA data
repositories:

* **TCGA DCC**: this is the TCGA Data Coordinating Center which provides a `Data Portal <https://tcga-data.nci.nih.gov/tcga/>`_ from which users may download open-access or controlled-access data.  This portal provides access to all TCGA data *except* for the low-level sequence data. 
* **CGHub**:  this is NCI's current secure data repository for all TCGA BAM and FASTQ sequence data files.

The ISB-CGC platform is one of NCI's `Cancer Genomics Cloud Pilots <https://cbiit.nci.nih.gov/ncip/nci-cancer-genomics-cloud-pilots>`_ 
and our mission is to host the TCGA data in the cloud so that researchers around the world may work with the data without needing 
to download and store the data at their own local institutions.

The vast majority (over 99%) of this **petabyte** of data consists of low-level sequence data, currently stored as files in
Google Cloud Storage.  Over the course of the TCGA project, this low-level (*"Level 1"*) data has been processed through 
a set of standardized pipelines and the the resulting high-level (*"Level 3"*) data is frequently the data that is used
in most downstream analyses.  The ISB-CGC platform aims to make these different types of data accessible to the widest
possible variety of users within the cancer research community, using the most appropriate Google Cloud Platform 
technologies.

Note that all TCGA metadata is considered open-access.  In other words, information *about* controlled-access data 
files is open-access.  Metadata can be obtained programmatically using the ISB-CGC programmatic API.

An overview of the TCGA data currently hosted on the ISB-CGC platform is provided in the two sections below.
The first section breaks the data down by access class (open *vs* controlled), and the second section breaks
it down by original repository (DCC *and* CGHub).

TCGA Data by Access Class
#########################

Open-Access TCGA Data
=====================

The open-access TCGA data hosted by the ISB-CGC Platform includes:

* Clinical (de-identified) and Biospecimen data: these data were originally provided in XML files (Level-1) by the DCC;
* Somatic mutation data:  these data were originally provided in MAF files (Level-2) by the DCC;
* DNA copy-number segments:  these data were originally provided as segmentation files (Level-3) by the DCC;
* DNA methylation data:  these data were originally provided as TSV files (Level-3) by the DCC;
* Gene (mRNA) expression data:  these data were originally provided as TSV files (Level-3) by the DCC;
* microRNA expression data:  these data were originally provided as TSV files (Level-3) by the DCC;
* Protein expression data:  these data were origially provided as TSV files (Level-3) by the DCC; and
* TCGA Annotations data:  annotations were obtained from the `TCGA Annotations Manager <https://tcga-data.nci.nih.gov/annotations>`_

The underlying data files are available to all ISB-CGC users in an open-access Google Cloud Storage (GCS) bucket (gs://isb-cgc-open), 
but all of these data are also provided in a more accessible form in BigQuery tables.  For more details about these BigQuery
tables and examples illustrating how to work with them from `Python <https://github.com/isb-cgc/examples-Python>`_ or 
`R <https://github.com/isb-cgc/examples-R>`_,  please see our `github repositories <https://github.com/isb-cgc>`_.

Controlled-Access TCGA Data
===========================

The controlled-access TCGA data hosted by the ISB-CGC Platform includes:

* SNP array CEL files:  these Level-1 data files were provided by the DCC and include over 22,000 files for both tumor and matched-normal samples;
* VCF files:  these Level-2 data files were provided by the DCC and include over 15,000 files produced by several different centers (primarily Broad and BCGSC);
* MAF files:  these "protected" mutation files (Level-2) were provided by the DCC (note that these files were not generated uniformly for all tumor types);
* DNA-seq BAM files:  these Level-1 data files were provided by CGHub;
   - over 37,000 of these files are available in Google Cloud Storage (GCS);
   - roughly 90% of these BAM files containe exome data, the remaining 10% contain whole-genome data;
   - BAM index (BAI) files are also available for all BAM files;
* mRNA- and microRNA-seq BAM files:  these Level-1 data files were provided by CGHub;
   - over 13,000 mRNA-seq BAM files are available in GCS;
   - over 16,000 miRNA-seq BAM files are available in GCS;
* mRNA-seq FASTQ files:  these Level-1 data files were provided by CGHub and include over 11,000 tar files.

At this time, all of these controlled-access data files are stored in GCS in the original form, as obtained from the data
repository.  In the future, BAM and VCF data will also be available in other forms in order to allow other modes of data
access (*eg* using the GA4GH API).

In order to access these controlled data, a user of the ISB-CGC must first be authenticated by NIH (via the ISB-CGC web-app).
Upon successful authentication, the users's dbGaP authorization will be verified.  These two steps are required before the user's
Google identity is added to the access control list (ACL) for the controlled data.  At this time, this access must be renewed
every 24 hours.


TCGA Data by Source Repository
##############################

TCGA Data at the DCC
====================

Complete sets of open-access and controlled-access data archives were copied from the DCC on October 4th, 2015
into Google Cloud Storage.

Note that for every archive at the DCC, there may be multiple revisions of an archive.  A list of the current 
`latest archives <http://tcga-data.nci.nih.gov/datareports/resources/latestarchive>`_
can be obtained from the DCC.
The archive 
`naming convention <https://wiki.nci.nih.gov/display/TCGA/TCGA+Data+Archives#TCGADataArchives-NamingConventions>`_
includes the disease code, the platform/pipeline name, the archive type (*eg* data level), the serial index
(which is often the batch number), and the revision number.
If you want to check whether there is a newer version of a specific archive at the DCC than what we currently
have on the ISB-CGC platform, you can check the date column in the latest archive report mentioned above,
or you could compare the archive name to these lists of 
`open-access archives <https://raw.githubusercontent.com/isb-cgc/readthedocs/master/docs/include/DCC_archives.04oct2015.open.tsv>`_
and 
`controlled-access archives <https://raw.githubusercontent.com/isb-cgc/readthedocs/master/docs/include/DCC_archives.04oct2015.cntl.tsv>`_
based on our most recent upload.

Note that all "bio" archives (containing clinical and biospecimen XML files) were recently migrated to a new
XSD which is not backwards compatible with the previous XSD.  This update took place over the course of the 
month of December 2015 and  none of these new archives are included in any of the current ISB-CGC BigQuery tables or files in GCS.

TCGA Data at CGHub
==================

The complete 
`listing <https://raw.githubusercontent.com/isb-cgc/readthedocs/master/docs/include/GCS_listing.v2.tsv>`_
of the TCGA data files from CGHub that are currently available in Google Cloud Storage (GCS)
contains the following three columns of information: 
* unique CGHub id for the file, 
* the partial GCS object path, and
* the size of the file in bytes.

The latest complete CGHub manifest can be 
`downloaded directly from CGHub <https://cghub.ucsc.edu/reports/SUMMARY_STATS/LATEST_MANIFEST.tsv>`_ (67 MB).
