# Tango HDB++ collaboration Teleconf Meeting - 2021/05/19

Held on 2021/05/19 on Zoom.

Participants: Reynald Bourtembourg, Damien Lacoste (ESRF), Sergi Rubio (ALBA),  
              Lorenzo Pivetta, Graziano Scalamera (ELETTRA),  
              Johan Forsberg (Max IV), Sonja Vrcic (SKAO)
              
# Minutes

## Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2021/2021-01-15/Minutes.md#summary-of-remaining-actions)

**Action - Sergi, Damien, Lorenzo, Max IV, Giacomo**:
Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.
Sergi should review the PR created by Damien on pytangoarchiving (https://github.com/tango-controls/PyTangoArchiving/pull/18).
Max IV is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 which is a proposal from Damien for an AbstractReader.  
Sergi will write the MySQL backend and review and approve https://github.com/tango-controls/PyTangoArchiving/pull/18.  
Sergi will share the work with MaxIV for support with Cassandra.  
Sergi will contact Damien and Giacomo to try to get a unified API (python)
**Sergi reported that there has been a little progress on this action.  
There is a agreement on most of the review comments. There are still some discussions on some points but they can be 
adressed later on so the Pull Request will be merged soon.** 

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text
configuration file and push this solution to tango-controls-hdbpp Github organization.
MaxIV is using some template yml files to configure easily the archiving settings (event thresholds for specific classes, strategies)

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite. 
Sergi will get in touch with Graziano on that topic.  
**Sergi reported an issue compiling libhdbpp-mysql on Debian 10 as well as an issue with the branches used.  
Graziano suggested to use the batch-insert branch but this branch will be merged very soon anyway.**

**Action - Damien**:Push the latest version of the Cassandra to timescaledb migration tools.
** Damien is currently writing a tool to help to validate that the migration from Cassandra to timescaleDB went well. 
The tool is comparing some random attributes on some random periods and is reporting inconsistencies.
Damien will improve and test the tool a bit more and will make it available with the other Cassandra to TimescaleDB 
migration scripts in hdbpp-timescale-project git repository.**

**Action: Sergi**: Rename  libhdbppinsert repository in libhdbpp-insert **Done**

**Action - Sergi**: Create an issue on hdbpp-es (if there is none yet) about the attributes which are sometimes not archived even though 
they are configured with the ALWAYS archiving strategy. **Sergi will create the issue**

## General discussions

Sergi reported that he is currently doing a migration to python 3 for some pytangoarchiving dependencies.  
The reader part will move to python 3 too.

Damien asked whether we should reconsider having a unified/similar API for all the different languages.  
Lorenzo proposed to finish the current work on the python extraction API and to rediscuss this topic after.  

Johan showed the yaml files they are using at Max IV to configure the archiving on their beamlines.   
They can define some default behaviour per class and easily configure the archiving of a same set of attributes per class.  
They have a yml2archiving python script which reads the yaml files and configure HDB++ according to the configuration defined in the files.  
The archiving strategies should be supported but they have not been much tested/used at MaxIV. The TTL is not yet supported.  

**Action - MaxIV**: Transfer the YAML tools to tango-controls Gitlab HDB++ subgroup to share them with the community and 
to help getting some feedback from the community.

**Action - Sergi**: Create an issue in libhdbpp-mysql about the compilation problem on Debian 10.

## News on recent developments

Graziano reported that enum are now supported on MySQL backend.

Sergi has been working on improving pytangoarchiving and will try to open pytangoarchiving to non-MySQL backends.  
All the configuration process should be done talking to the hdbpp-cm device server, and all this is independent from 
the DB backend.

**Action - Damien**: Merge hdbpp-es batch insert support. **Done at the moment of writing these minutes**

Graziano proposed to improve hdbpp-es interface in order to avoid to have to pass the data format and data type when 
adding an attribute when using an independent hdbpp-es (without any hdbpp-cm).  
He proposed to use the same code as in hdbpp-cm where data format and data type are not required.  
The current hdbpp-es methods to add attributes could be kept but it should be possible to add an attribute without 
having to specify the data format and data type too.

**Action - Graziano**: Create an issue to improve hdbpp-es interface when adding attributes in the case of an independent 
hdbpp-es (without hdbpp-cm managing it).
 
## High Priority Issues

## AOB

### Migration to Gitlab

The migration to Gitlab will be done in one go. 
It has been decided to start the migration process for all the HDB++ repositories.  
Pytangoarchiving will be migrated inside an hdbpp (hdb++?, archiving?) gitlab subgroup.

**Action - Reynald**: Create an issue on each HDB++ repository (including pytangoarchiving), asking github participants 
to connect to gitlab.com using their github account (only on the repositories where this is necessary), giving 
a 1 week or 10 days deadline.

### Packages

Damien suggested to use Gitlab infrastructure to create packages for the HDB++ components, once the migration to gitlab.com will be done. 
Reynald reminded that there used to be some Debian packages for HDB++.
It was agreed that at least Debian packages should be generated.  
Johan said that MaxIV is currently more interested in getting conda packages than rpm packages (to be confirmed).  
He said that MaxIV might work on the development of HDB++ Conda packages.

### ICALEPCS 2021

There will be a Tango workshop organized during ICALEPCS 2021.  
We agreed there should be a presentation/demo dedicated on HDB++ topic during the Tango workshop.

Since there were no big changes since the last ICALEPCS on HDB++, no paper presenting HDB++ status will be submitted 
for ICALEPCS 2021. 

Damien suggested to think about making HDB++ more generic to be able to use it with EPICS or other control 
systems too to attract more users to HDB++.
For EPICS, for instance, this would require to create an HDB++ EPICS archiver and to do some mapping between the Tango 
concepts and EPICS ones.

### Next HDB++ Teleconf Meeting

The next HDB++ meeting will take place on Friday 23rd July 2021 at 11:00 CEST.

## Summary of remaining actions

**Action - Sergi, Damien, Lorenzo, Max IV, Giacomo**:
Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.
Sergi should review the PR created by Damien on pytangoarchiving (https://github.com/tango-controls/PyTangoArchiving/pull/18).
Max IV is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 which is a proposal from Damien for an AbstractReader.  
Sergi will write the MySQL backend and review and approve https://github.com/tango-controls/PyTangoArchiving/pull/18.  
Sergi will share the work with MaxIV for support with Cassandra.  
Sergi will contact Damien and Giacomo to try to get a unified API (python)

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text
configuration file and push this solution to tango-controls-hdbpp Github organization.
MaxIV is using some template yml files to configure easily the archiving settings (event thresholds for specific classes, strategies)

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite. 
Sergi will get in touch with Graziano on that topic.  

**Action - Damien**:Push the latest version of the Cassandra to timescaledb migration and validation tools.

**Action - Sergi**: Create an issue on hdbpp-es (if there is none yet) about the attributes which are sometimes not archived even though 
they are configured with the ALWAYS archiving strategy.

**Action - MaxIV**: Transfer the YAML tools to tango-controls Gitlab HDB++ subgroup to share them with the community and 
to help getting some feedback from the community.

**Action - Sergi**: Create an issue in libhdbpp-mysql about the compilation problem on Debian 10.

**Action - Graziano**: Create an issue to improve hdbpp-es interface when adding attributes in the case of an independent 
hdbpp-es (without hdbpp-cm managing it).

**Action - Reynald**: Create an issue on each HDB++ repository (including pytangoarchiving), asking github participants 
to connect to gitlab.com using their github account (only on the repositories where this is necessary), giving 
a 1 week or 10 days deadline.
