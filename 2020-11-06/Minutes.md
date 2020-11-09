# Tango HDB++ collaboration Teleconf Meeting - 2020/11/06

Held on 2020/11/06 on Zoom.

Participants: Reynald Bourtembourg, Damien Lacoste, Jean-Luc Pons, Pascal Verdier (ESRF), Sergi Rubio (ALBA),  
              Lorenzo Pivetta, Graziano Scalamera, Giacomo Strangolino (ELETTRA),  
              Matteo Di Carlo (INAF), Mirjam Lindberg (Max IV), Mangesh Patil (NCRA), Apurva, ...
              
# Minutes

## Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2020-09-04/Minutes.md#summary-of-remaining-actions)

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs.
**WIP: Damien created https://github.com/tango-controls/tango-doc/pull/356 and Lorenzo already started to review. 
Contributions are welcome to review and improve this documentation.**

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with K8s and load balancing and share 
the answers with the HDB++ community. 
**Mirjam reported that the installation of TimescaleDB on K8s was straight-forward. 
The next step for MaxIV is to deploy the HDB++ components and start archiving data on a beamline.
They will deal with load balancing at a later stage.**

**Action - ALBA**: Sergi should guide Damien in implementing support for TimescaleDB in PyTangoArchiving.
**Damien and Sergi already exchanged on the abstract class for the reader. 
Damien is proposing some changes to this abstract class and created the following PR in PyTangoArchiving:
https://github.com/tango-controls/PyTangoArchiving/pull/18**

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text 
configuration file and push this solution to tango-controls-hdbpp Github organization. 
**Matteo thinks he should be able to provide a new version around end of February.
He also reported that he will work on the ELK backend.**

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite.

**Action - ESRF**: Share scripts used at ESRF to export data with MaxIV
**The current version of the scripts is slow. Damien will improve them and then share them with the community.**

## Status of Project build hdbpp-es PR

Graziano said that there are currently several cmake flags to build., maybe examples needed, need to decide which should be the default library to build against (I think libhdbpp with backend implementations loaded dynamically)
He thinks it would be good to add some examples in the README to show how to use these flags.

A guide is missing to build all the HDB++ components. Matteo mentioned that NRCA team (Apurva) created some docker images 
with the HDB++ components. 
There is 1 docker with a MariaDB HDB++ DB and another one with the main HDB++ components.  
The Dockerfile file of these Dockers could be of a good help to show how to build the HDB++ components.  
SKA has also some k8s charts which could be shared with the community.
Matteo provided a link to these docker files and k8s Helm charts in the zoom chat but Reynald forgot to copy them before closing the meeting.  
So Matteo, please put these links in these minutes.

Following Graziano's proposal, it was decided that the default backend library used to build hdbpp-es should be libhdbpp with backend implementation loaded dynamically.  
After [Damien's PR](https://github.com/tango-controls-hdbpp/hdbpp-es/pull/16) merge, users will have the possibility if they 
want, to link hdbpp-es directly with their backend HDB++ library, the main advantage being identifying some compatibility issues 
between hdbpp-es and the backend library during the compilation instead of at runtime.

Damien proposed to create a skeleton implementation to help developers to implement new HDB++ libraries for new backends.  
This could be done in a new PR in libhdbpp repository with some documentation guide on readthedocs.

**Action - ESRF (Damien):** Create a PR in libhdbpp project to add a skeleton implementation to guid developers who want to develop a new 
HDB++ library for their backend and add some documentation on readthedocs to document the process.

## update minimum c++ standard required to c++14 for hdb++?

It has been decided during this teleconf meeting to impose C++14 as the minimum C++ standard required for HDB++ C++ components.

## Status of hdbpp-benchmark

Graziano reported that he did some benchmarking tests with the batch insert new feature and the tests seem to be now 
limited by the 0MQ HWM setting (High Water Mark, basically the size of the 0MQ buffers). He tested with timescaleDB and MySQL and the results were similar (similar event rate about 10000 ev/s).

He did a simple test with 1 archiver and 20 events generators with only scalar DevLong attributes.

Graziano would like to add tests to check every Tango type (insertion followed by an extraction to check whether the data 
inserted and extracted is the expected one).

## News on recent developments

Sergi reported that in ALBA, they are using pyhdbpp periodic archivers who are doing some polling on the attributes to be archived and insert data 
inside whatever backend. They have developed a python module to insert data into the DB.

**Action - ALBA (Sergi)**: Move 2 repositories (periodic archivers + python module to insert data into HDB++ DB) used for the periodic archivers to tango-controls-hdbpp organization.

Jean-Luc reported that jhdbviewer is now able to display aggregate data for scalar attributes (available only for TimescaleDB at the moment).

## High Priority issues

According to Reynald, the priority is to get stable master branches in the repositories and to review and 
merge the open PRs.

## AOB

### Tango collaboration meeting

Lorenzo volunteered (or has been volunteered by Matteo ;-) ) to present the status of the HDB++ project to the Tango 
community during the coming Tango collaboration virtual meeting which will take place on 17th-18th November 2020.

It was agreed that every institute using HDB++ or contributing to HDB++ could send some slides to Lorenzo.  
It should take about 1 minute max to present the slides provided by an institute.

Results of the benchmarking done by Graziano will probably also be presented.  
Graziano will try to get interesting results before the meeting.

**Action - All Institutes**: Send some slides to Lorenzo Pivetta (1 min presentation max) to present the status of HDB++ 
in the institute (deployment status and recent development on HDB++ project, or resources that can be shared with the 
community). Lorenzo will present the slides during the coming Tango collaboration virtual meeting on 17th or 18th november 2020)

**Action - Lorenzo**: Book a slot for an HDB++ presentation on the coming Tango collaboration virtual meeting (Register on indico: 
https://indico.esrf.fr/indico/event/49/ ). Participants should register too.

### Next HDB++ Teleconf Meeting

The next HDB++ Teleconf meeting will take place on Friday 15th January 2021 at 11:00 CET.

## Summary of remaining actions

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs.

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with load balancing and share 
the answers with the HDB++ community.

**Action - ALBA**: Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving. 
Sergi should review the PR created by Damien on pytangoarchiving (https://github.com/tango-controls/PyTangoArchiving/pull/18).

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text 
configuration file and push this solution to tango-controls-hdbpp Github organization. 

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite.

**Action - ESRF**: Share scripts used at ESRF to export data with MaxIV

**Action - ESRF (Damien):** Create a PR in libhdbpp project to add a skeleton implementation to guid developers who want to develop a new 
HDB++ library for their backend and add some documentation on readthedocs to document the process.

**Action - ALBA (Sergi)**: Move 2 repositories (periodic archivers + python module to insert data into HDB++ DB) used for the periodic archivers to tango-controls-hdbpp organization.

**Action - All Institutes**: Send some slides to Lorenzo Pivetta (1 min presentation max) to present the status of HDB++ 
in the institute (deployment status and recent development on HDB++ project, or resources that can be shared with the 
community). Lorenzo will present the slides during the coming Tango collaboration virtual meeting on 17th or 18th november 2020)

**Action - Lorenzo**: Book a slot for an HDB++ presentation on the coming Tango collaboration virtual meeting (Register on indico: 
https://indico.esrf.fr/indico/event/49/ ). Participants should register too.