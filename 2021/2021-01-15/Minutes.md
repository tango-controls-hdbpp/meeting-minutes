# Tango HDB++ collaboration Teleconf Meeting - 2021/01/15

Held on 2021/01/15 on Zoom.

Participants: Reynald Bourtembourg, Damien Lacoste (ESRF), Sergi Rubio (ALBA),  
              Lorenzo Pivetta, Graziano Scalamera, Giacomo Strangolino (ELETTRA),  
              Johan Forsberg, Mirjam Lindberg (Max IV), Mangesh Patil (NCRA), Apurva, ...
              
# Minutes

## Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2020-11-06/Minutes.md#summary-of-remaining-actions)

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs.
**Damien created the following MR: https://gitlab.com/bourtemb/tango-doc/-/merge_requests/356**

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with load balancing and share
the answers with the HDB++ community.

**Action - ALBA**: Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.
Sergi should review the PR created by Damien on pytangoarchiving (https://github.com/tango-controls/PyTangoArchiving/pull/18).
Max IV is also interested in this development, they are invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 
which is a proposal from Damien for an AbstractReader.  
Sergi will write the support for MySQL backend and review and approve https://github.com/tango-controls/PyTangoArchiving/pull/18.  
Sergi will share the work with MaxIV for support with Cassandra.  
Lorenzo said that the Python Extraction library should follow the concepts of the C++ extraction Library.  
Some of these concepts are described at some point in this section of the documentation 
https://tango-controls.readthedocs.io/en/latest/administration/services/hdbpp/hdb++-design-guidelines.html?highlight=data%20extraction#deployment-best-practices.
It seems like the title of the section "Data Extraction" got lost when the documentation was imported into readthedocs.  
Lorenzo is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 as well to ensure the concepts 
used by the C++ extraction library are kept.  
Sergi said there was an attempt at some point to use the C++ extraction library from python using SWIG.  
Sergi will contact Damien and Giacomo to try to get a unified API (python)**

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text
configuration file and push this solution to tango-controls-hdbpp Github organization.

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite.

**Action - ESRF**: Share scripts used at ESRF to export data with MaxIV.
**The scripts are now available on [hdbpp-timescale-project](https://github.com/tango-controls-hdbpp/hdbpp-timescale-project/tree/master/resources/cassandra-import) repository.**
Graziano suggested during the meeting to have a look at [pt-fifo-split](https://www.percona.com/doc/percona-toolkit/LATEST/pt-fifo-split.html) 
to help dealing with huge csv files. It was suggested to have a look at [timescaledb-parallel-copy](https://github.com/timescale/timescaledb-parallel-copy) to improve import performances in timescaledb.

**Action - ESRF (Damien):** Create a PR in libhdbpp project to add a skeleton implementation to guide developers who want to develop a new
HDB++ library for their backend and add some documentation on readthedocs to document the process.
**Damien created the PR: https://github.com/tango-controls-hdbpp/libhdbpp/pull/6**

**Action - ALBA (Sergi)**: Move 2 repositories (periodic archivers + python module to insert data into HDB++ DB) used for the periodic archivers to tango-controls-hdbpp organization.

## News on recent developments

Giacomo reported that Cumbia now integrates HDB++ and merges HDB++ data with live data in graphs.  
This feature is available as a plugin in the latest version of Cumbia.

### Benchmarking

Graziano did more benchmarking tests, using the batch insert feature.
He multiplied the number of archivers and event generators in several dockers.

Testing with scalar attributes, he could write ~100 000 events/s using timescaledb and 24 archivers.
With MySQL, he could write ~80 000 events/s using 16 archivers.
When using more than 16 archivers in his setup, the performances started to degrade with MySQL.

He used 32 cores with everything running on the same host (archivers and events generators).

In Alba, they will try(/suggest?) to test with events generators running in different host.

Johan Forsberg said he would be interested in seeing some benchmarking results with array types.

Graziano said, about querying performances, that without clustering timescaledb was slower than MySQL-InnoDB and with clustering, timescaledb 
was slightly faster than MySQL.

Damien reminded that automatic clustering and compression is now available for free in the latest timescaledb version.
The length of a chunk should fit in memory for it to work in an optimal way.

## Status of Project build hdbpp-es PR

This is work in progress and requires some approvals to move on.

## PR approval rules
It has been decided during the meeting to adopt the following approval rules:
- For very simple Pull Requests, approval can be skipped. The developer can take the responsibility to merge directly
- For the backend repository at least 1 approval should be required, ideally from another institute.
- For the common parts (libhdbpp, hdbpp-es, hdbpp-cm, ...), 2 approvals should be required with some flexibility.
_ The PR developer should contact the best person(s) for each specific review.
  
## High Priority issues

According to Reynald, the priority is to get stable master branches in the repositories and to review and 
merge the open PRs.

Johan Forsberg reported that they are stuck with an old version of HDB++ and they would need to update their DB schemas 
and some data in their HDB++ tables to be able to use the latest HDB++ versions.

**Action - Reynald**: Provide information to MaxIV on how to upgrade the schemas and the data to use the latest HDB++ versions.

## Migration to Gitlab

In December 2020, the Tango Steering committee has decided to migrate the Tango git repositories from Github to Gitlab.

HDB++ repositories will have to be moved to gitlab.com/tango-controls but these repositories are not in top-priority 
repositories to be migrated first. Is has been decided to wait for the next HDB++ teleconf meeting before to do anything 
on the HDB++ repositories migration.
The repositories will probably be moved to a Gitlab hdbpp SubGroup under gitlab.com/tango-controls/.
There should be 1 person per institute which should get owner permissions on that subgroup.

## AOB

### Tools

Reynald said that a very basic command line tool (written in java) to retrieve HDB++ data for a single scalar numeric ReadOnly attribute has been
developed at the ESRF and can be used to create some csv files (by redirecting the output to a file) for this kind of attributes.
The code is available on this repository: https://gitlab.esrf.fr/accelerators/System/HDBGetAttributeData

### Next HDB++ Teleconf Meeting

The next HDB++ Teleconf meeting will take place on Friday 12th March 2021 at 11:00 CET.

## Summary of remaining actions

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with load balancing and share
the answers with the HDB++ community.

**Action - Sergi, Damien, Lorenzo, Max IV, Giacomo**:
Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.
Sergi should review the PR created by Damien on pytangoarchiving (https://github.com/tango-controls/PyTangoArchiving/pull/18).
Max IV is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 which is a proposal from Damien for an AbstractReader.  
Sergi will write the MySQL backend and review and approve https://github.com/tango-controls/PyTangoArchiving/pull/18.  
Sergi will share the work with MaxIV for support with Cassandra.  
Lorenzo is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 as well to ensure the concepts
used by the C++ extraction library are kept.  
Sergi will contact Damien and Giacomo to try to get a unified API (python)

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text
configuration file and push this solution to tango-controls-hdbpp Github organization.

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite.

**Action - ALBA (Sergi)**: Move 2 repositories (periodic archivers + python module to insert data into HDB++ DB) used for the periodic archivers to tango-controls-hdbpp organization.

**Action - Reynald**: Provide information to MaxIV on how to upgrade the schemas and the data to use the latest HDB++ versions.
