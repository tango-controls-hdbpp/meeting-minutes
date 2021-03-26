# Tango HDB++ collaboration Teleconf Meeting - 2021/03/12

Held on 2021/03/12 on Zoom.

Participants: Reynald Bourtembourg, Damien Lacoste (ESRF), Sergi Rubio (ALBA),  
              Lorenzo Pivetta, Graziano Scalamera, Giacomo Strangolino (ELETTRA),  
              Johan Forsberg (Max IV), Jayant Kumbhar, Mangesh Patil (NCRA), Apurva, Dima, ...
              
# Minutes

## Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2021/2021-01-15/Minutes.md#summary-of-remaining-actions)

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with load balancing and share
the answers with the HDB++ community. **We can close this action. MaxIV will share the results when they will get them**

**Action - Sergi, Damien, Lorenzo, Max IV, Giacomo**:
Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.
Sergi should review the PR created by Damien on pytangoarchiving (https://github.com/tango-controls/PyTangoArchiving/pull/18).
Max IV is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 which is a proposal from Damien for an AbstractReader.  
Sergi will write the MySQL backend and review and approve https://github.com/tango-controls/PyTangoArchiving/pull/18.  
Sergi will share the work with MaxIV for support with Cassandra.  
Lorenzo is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 as well to ensure the concepts
used by the C++ extraction library are kept.  **Lorenzo did a review**.
Sergi will contact Damien and Giacomo to try to get a unified API (python)
**At Alba, there are already using a feature from pyTangoArchiving allowing to query several DBs in parallel. 
They archive the data from the last 6 months at full resolution and the data older than 6 months is decimated on other DBs.
This feature is configurable via a free property**

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text
configuration file and push this solution to tango-controls-hdbpp Github organization.
**MaxIV is using some template yml files to configure easily the archiving settings (event thresholds for specific classes, strategies)**

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite. 
**Sergi is currently doing some tests. Sergi will get in touch with Graziano on that topic**

**Action - ALBA (Sergi)**: Move 2 repositories (periodic archivers + python module to insert data into HDB++ DB) used for the periodic archivers to tango-controls-hdbpp organization.
**Done: See https://github.com/tango-controls-hdbpp/PyHdbppPeriodicArchiver and https://github.com/tango-controls-hdbpp/libhdbppinsert**

**Action - Reynald**: Provide information to MaxIV on how to upgrade the schemas and the data to use the latest HDB++ versions.
**Max IV does not plan to upgrade the Cassandra DB schema before moving to timescale**

## General discussions

Dima was invited for the first time in the HDB++ meeting. He developed a short module for python 3.7 to interact easily with HDB++.  
His work can be found at https://github.com/dvjdjvu/hdbpp. 
Here is a link to the English documentation: https://habr.com/ru/post/544536/  
He added support for PostgreSQL after the meeting.

Sergi mentioned the subscribeChangeasFallback property of the HdbEventSubscriber DS to be able to use change events configuration when the alarm event 
configuration is not provided.
Sergi advised to use a Sardana dedicated Event Subscriber.

At MaxIV, they will add disk to their Cassandra cluster.

Graziano reminded that the latest version of hdbpp-es provides a new API which is currently not compatible with Cassandra.
If support for Cassandra is desired, some development is required on the libhdbpp-cassandra backend.
ESRF will not do this job but could provide some help and pieces of advise if there is a need to improve libhdbpp-cassandra.

Mangesh asked what are the current recommendations in terms of DB backend.  
Lorenzo replied that there are currently at least 4 valid options: 
- MySQL
- MariaDB
- timescaleDB
- PostgreSQL (Not ported to latest HDB++ version yet)

According to Sergi's first tests, timescaleDB is a disk eater, but the compression feature can greatly help to reduce the 
disk usage footprint (80% compression rate on some data). TimescaleDB supports aggregates which is a nice feature.

Cassandra makes sense, but at very big scale only and it is important to have a dedicated specialized team for the 
maintenance of the cluster.

Apurva asked some questions about the enum and devulong64 data types.
These types are currently not supported by the viewer. The DevEnum shouldd be supported in the future.
For the devulong64, it is not yet clear since there is no direct equivalent type in java.

**Action - Reynald**: Create hdbpp-tickets repository which would be the equivalent of TangoTickets repository for HDB++.  
This repository should be used to create issues involving several HDB++ components or when the issue creator does not 
know where to create the HDB++ related issue. **Done just after the meeting: https://github.com/tango-controls-hdbpp/hdbpp-tickets**

Reynald encouraged to create issues in the HDB++ repositories directly or using the hdbpp-tickets repository instead of 
using the Tango Forum, because not everybody gets a notification for the messages in the forum and because they could 
be forgotten after a while.

Apurva mention an issue with the string attributes.
Graziano mentioned that this bug should be fixed in the batch_insert version which will be soon merged.

Mangesh said that they are also interested in the ELK backend with Grafana because they are already using these 
technology for their logs.  
Johan tried grafana which can be used directly with the PostgreSQL plugin (for PostgreSQL and timescaleDB backends, but 
the equivalent should exist for MySQL/mariaDB). It is straight-forward to setup but the browsing of the attributes is not 
very convenient.

## News on recent developments

Johan started some tests for the migration of the Cassandra data to timescaleDB. He added some parallelization to speed up the migration process.

Damien mentioned that a hardware problem (before the firs backup was saved) forced the ESRF to redo the migration/import of the data from Cassandra to timescaleDB.  
The migration script got significantly improved compared to the first import which took several weeks. It now took about 5 days instead.  
Damien said the migration tools could be further improved. Produced csv files could be split.
Johan mentioned a flag in the copy command which can be used to limit the number of lines in the generated dump (max lines or something similar).  

**Action - Damien**:Push the latest version of the Cassandra to timescaledb migration tools.

Some work has been done on the Event Subscriber side to get a new build system with CMake, providing an option to link directly with the HDB++ lib backend.

2 other merge requests will be soon merged:
- One related to the possibility to configure the es attribute lists from a file.
- One related to the batch insertion feature

Feedback from Sergi for the MySQL backend is expected.

Graziano suggested to rename libhdbppinsert into libhdbpp-insert to follow the convention of the other repositories.

**Action: Sergi**: Rename libhdbppinsert repository in libhdbpp-insert
 
## Issues

Sergi mentioned an issue with the ALWAYS Strategy and some event subscribers which do not always archive attributes 
configured to be archived with the ALWAYS strategy.
The problem typically happens when the Context memorized attribute has never been set.

**Action - Sergi**: Create an issue on hdbpp-es (if there is none yet) about the attributes which are sometimes not archived even though 
they are configured with the ALWAYS archiving strategy. 

It seems like there could be a problem when e-s are down and switching context on the c-m.

There was a discussion about the Never context which should be used when a hardware is in temporary maintenance.
The Never context should be preferred in this use case compared to the AttributeStop command. 

Sergi will try to reproduce all these issues around the end of March.

Johan noticed a problem when trying to build HDB++ SW. He had to add some options related to librt to be able to compile. 

## Migration to Gitlab

The migration to Gitlab will be done in one go. It has been decided to wait for the end of the github.com/tango-controls 
migration before to do it.

## AOB

The next HDB++ meeting will take place on Friday 14th May 2021 at 11:00 CEST.

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

**Action - Damien**:Push the latest version of the Cassandra to timescaledb migration tools.

**Action: Sergi**: Rename  libhdbppinsert repository in libhdbpp-insert

**Action - Sergi**: Create an issue on hdbpp-es (if there is none yet) about the attributes which are sometimes not archived even though 
they are configured with the ALWAYS archiving strategy. 
