# Description of the Argo GDAC File Merge Process

This document describes the process of creating “merge files” – a merged core-argo/bio-argo file.  The core-file and bio-file are submitted to the GDACs by the DACs.  The merge-file is created at the GDACs.

Version 1.2, March 2017

Found at <https://archimer.ifremer.fr/doc/00410/52154/>.

DOI: <https://doi.org/10.13155/52154>

Argo data management

Description of the Argo GDAC File Checks

Authors: Mark Ignaszewski / FNMOC

***

## Introduction

This document describes the process of creating "merge files" – a merged core-argo/bio-argo file. The core-file and bio-file are submitted to the GDACs by the DACs. The merge-file is created at the GDACs.

The term "merge-pair" will be used to refer to the corresponding core-argo/bio-argo pair of files.

In the following discussion, the dimensions of a variable are often referred to. For the purpose of these discussions, the “STRING” dimensions are ignored since they merely provide the space to hold a single string. For instance, the declaration of “STATION_PARAMETERS(N_PROF, N_PARAM, STRING16)” is considered to be dependent only on (N_PROF, N_PARAM).

In the examples below, the variable indices are referred to. These indices start at 1, not 0 (zero) as in many programming languages.

### Requirements Validation

There are requirements placed on the data contained within the merge-pair files. The FileChecker does not enforce these requirements. The merge-file processing will enforce these requirements.

See §2.14 (Merge-pair Data Validation) for the details.

***

## Profile merge-file

### Merge-file Naming

The merge-file naming template is: ==Md<float-num\>_<cyc\>.nc==

where:

-  d is the overall data-mode of the file and is the same as the first character of the core-file, either “R” or “D”
-  <float-num\> and <cyc\> are the float-number and cycle number from the merge-pair

### Dimensions

**DATE_TIME** and **STRINGx**: These dimensions are essentially constants. They are set to their typical values as documented in the User’s Manual

**N_PROF**: Set to the same value as N_PROF in both the core-file and bio-file, which are required to be the same. (See §2.14 for the validation details.)

**N_PARAM**: Described in the next section.

**N_LEVELS**: This setting is required to be the same in the core-file and bio-file. N_LEVELS in the merge-file will be set to this value. (See §2.14 for the validation details.)

**N_CALIB**: The N_CALIB setting in the merge-file will be the maximum of the settings in the core-file and bio-file.

**N_HISTORY**: History variables are not included in the merge-file. This dimension is included in the merge file as the UNLIMITED dimension for symmetry with the core- and bio-files but its value will always be 0.

### Physical Parameters and N_PARAM

The physical parameters are divided into three categories in the "Argo physical parameters list" on the
ADMT website (reference table 3 in the User’s Manual): the core parameters – category "c"; the bio
parameters – category "b"; the intermediate parameters – category "i".

The merge-file will contain the physical parameters identified as core ("c") and bio ("b") parameters.

The N_PARAM dimension is the maximum number of parameters that will be in single profile of the
merge-file. To determine this, both of the merge-pair files must be examined, profile-by-profile to
determine which <PARAM\>s will be transferred for each profile.

NOTE that the process of selecting which parameters to merge from each file, and profile therein,
establishes the template for the data merging process discussed in §2.9, §2.10, and §2.11, below.

SPECIAL CASE: If there is a case where the only parameter in the core-file is PRES and the only
parameters in the bio-file are PRES and i-parameters, the merge-file will include the profile with 
PRES being the only parameter, to maintain the one-to-one profile correspondence between the core-, 
bio-, and merge-files.

Example 1: Selecting merge <PARAM\>s and N_PARAM. The core-file contains only core-parameters, 
by definition. The bio-file parameter-types are indicated following the parameter name.

| Core-file<br />N_PROF=2; N_PARAM=3                                          | Bio-file<br />N_PROF=2 ; N_PARAM=5                                                                                                               |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------|
| Profile 1<br />Number of parameters=3                                       | Profile 1<br />Number of parameters=2                                                                                                            |
| STATION_PARAMETERS<br />1,1) PRES (c)<br />1,2) TEMP (c)<br />1,3) PSAL (c) | STATION_PARAMETERS<br />1,1) PRES (c)<br />1,2) DOXY (b)                                                                                         |
| Profile 2<br />Number of parameters=1                                       | Profile 2<br />Number of parameters=5                                                                                                            |
| STATION_PARAMETERS<br />2,1) PRES (c)                                       | STATION_PARAMETERS<br />2,1) PRES (c)<br />2,2) BBP (b)<br />2,3) UV_INTENSITY_REF_NITRATE (i)<br />2,4) MOLAR_NITRATE (i)<br />2,5) NITRATE (b) |


### Variable Definitions

### Variable Attributes

### **General Information on the File** Variables

### **General Information for each Profile** Variables

### PROFILE_<PARAM>_QC

!!! note "Note"

    This variable is not handled as described here. It is part of the physical parameter merge process described in §2.10 (STATION_PARAMETERS and PARAMETER_DATA_MODE)

### STATION_PARAMETERS and PARAMETER_DATA_MODE

### <PARAM\> Variables

### Calibration Variables

#### PARAMETER

#### SCIENTIFIC_*_*

### History Variables

### Global Attributes

### Merge-pair Data Validation

***

## Trajectory merge file

The User’s Manual refers to trajectory merge files. This process will be defined at a later time and is not currently implemented.
