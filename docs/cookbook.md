# ARGO GDAC Cookbook

Based on [Version 1.1](https://archimer.ifremer.fr/doc/00351/46202/53031.pdf), November 29th 2017

Argo data management

GDAC cook-book

DOI: <http://doi.org/10.13155/46202>

Authors: Thierry Carval (Ifremer), Mark Ignaszewski (FNMOC), Michael Frost (FNMOC)

***

## :peace: Introduction

This document describes the implementation of the GDAC services.

It ensures that both GDACs provides the same services.

***

## Detailed Specification of GDAC Services

### Greylist Synchronization

Each DAC maintains a greylist that is submitted to the GDAC for updates. The DACs greylist are collected by the GDAC and merged into a global Argo greylist.

Greylist file collection from DAC to GDAC:

1.  Query xxx_greylist.csv file in each DAC submit directory; xxx must be identical to the DAC (eg : aoml, coriolis); otherwise the file is rejected.

2.  Check the format of ==xxx_greylist.csv==. The whole file is rejected if the format check fails.

    -  Floatid : valid Argo float id; the corresponding meta-data file must exist
    -  Parameter : PSAL, TEMP, PRES or DOXY
    -  Start date : YYYYMMDD valid, mandatory
    -  End date : YYYYMMDD valid, fill value : ',,'
    -  Flag : valid argo flag
    -  Comment : free
    -  DAC : valid DAC, mandatory

3.  Remove all the floats of the DAC from the GDAC grey list and add the content of the submitted xxx_greylist.csv file

!!! note "Note"

    After each submission, a copy of the Argo greylist is stored in the directory

-  etc/greylist/ar_greylist.txt_YYYYMMDD

The global Argo greylist is sorted by ==DAC==, ==PLATFORM_CODE== and ==START_DATE== in alphabetical order.


#### Daily comparison of the US and Coriolis greylist

Once a day, the US and Coriolis greylist are compared. If differences are detected, an error message is
sent to both GDACs.



### File Removal
A DAC may ask the GDAC to remove individual profile, trajectory, technical or meta-data files. A "removal file" is submitted to GDAC which will perform the removals.

The "removal file" contains one line per file to remove.

"Removal file" collection from DAC to GDAC:

-  Query xxx_removal.txt file in each DAC submit directory; xxx must be identical to the DAC (eg : aoml, coriolis); otherwise the file is rejected
-  Check the format of xxx_removal.txt . The whole file is rejected is the format check fails.
    -  File name : valid Argo file name; the corresponding meta-data file must exist for this DAC
-  Move all the named files from GDAC into an etc/removed directory
-  The removed files are kept for 3 months in the etc/removed directory and erased after that delay.

#### Metadata file removal

A float's metadata file will not be removed if other file(s) exists.

When a float's metadata is removed, the empty float's directory is also removed.

***

### Scheduled Services Time Table

This chapter documents the time of activation of the two GDACs services.

-  Argo DACs files collect: every 30 minutes
-  Update of the index file: every hour
-  GDAC synchronization: once a day
-  Profile merging on ***dac*** directory: every hour
-  Profile merging on ***geo*** directory: every hour
-  Profile merging on ***latest_data*** directory: every hour
-  Index files generation: every hour

***

### ARGO File Format Checker

The file format checker is applied on each NetCDF file submitted by DAC to GDACs

The format checker documentation is available on:


!!! Info "Description"

    Argo GDAC File Checks: Data Format and Consistency Check: [http://dx.doi.org/10.13155/46120](http://dx.doi.org/10.13155/46120)


#### File Rejection
The files that do not comply with the format check are rejected and directly moved to the DAC's "reject" directory. The files from reject directory are deleted after one month.

For every data submission from a DAC, an error message for the rejected files is sent with the list of files and errors.

The Argo format checker can be downloaded from:

-  [http://usgodae.org/pub/outgoing/argo/etc/FileChecker](http://usgodae.org/pub/outgoing/argo/etc/FileChecker)

***

### ARGO Profile Files Merger

#### Core-Argo profile files merger

The GDAC merges individual profile files into 3 types of merged files:

-  float file: one file containing all the core-Argo profiles of an individual float.

-  geo file: one file containing all the core-Argo profiles for a day and an ocean, organized by year and month.

-  latest_data file: one file per day of arrival containing all the core-Argo profiles collected by the GDAC during the last 30 days. Real-time profiles and delayed mode profiles are stored in separated file.

The merger merges only core Argo parameters.

A profile file without TEMP parameter is ignored by the merger.

##### Profile sort order

In the merged file of the “dac” directory, profiles are sorted by cycle number (CYCLE_NUMBER) and date (JULD).

In the merged file of the “geo” directory, profiles are sorted by date (JULD).  

#### BGC-Argo profile files merge

The BGC-Argo profile files merger is documented on:

-  Ignaszewski Mark (2017). Description of the Argo GDAC Core-BGC File Merge Process. [http://doi.org/10.13155/52154](http://doi.org/10.13155/52154)

***

### Argo reference tables management

Argo reference tables are managed online on Argo data management site. The reference tables describe standardized parameters.

##### Parameters attributes

Each parameter has the following attributes:

-  Status
    -  Active
    -  Publication underway (approved but not yet active in the format checker)
    -  Creation underway
    -  Obsolete
    -  Refused (?)
    -  Creation date
    -  Update date
    -  Publication date
    -  Who performed the last update
    -  Comment

##### How to display parameters on the ADMT web site
-  By default, all parameters with an active status are displayed
-  All parameters with any status can be displayed


##### Who can edit the parameters?

The parameters edition is limited to authorized people.

Technical parameter names and units tables: Birgit Klein and Ann Thresher

-  Configuration parameter names
    -  *John Gilson* and *Esmee Van Wijk* (core parameters)
    -  Catherine Schmechtig and Jean-Philippe Rannou (bio parameters)
-  Argo reference table: Thierry Carval and Mathieu Belbeoch
-  Standard format table: Megan Scanderberg and Mathieu Belbeoch