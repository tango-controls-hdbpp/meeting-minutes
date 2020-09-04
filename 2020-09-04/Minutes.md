# Tango HDB++ collaboration Teleconf Meeting - 2020/09/04

Held on 2020/09/04 on Zoom.

Participants: Reynald Bourtembourg, Damien Lacoste (ESRF), Sergi Rubio (ALBA),  
              Lorenzo Pivetta, Graziano Scalamera, Giacomo Strangolino (ELETTRA),  
              Matteo Di Carlo (INAF), Mirjam Lindberg, Abdullah Amjad (Max IV)
              
# Minutes

## Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2020-06-26/Minutes.md#summary-of-remaining-actions)

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs. **Still needs to be done**

**Action - SKAO**: Add a link to Grafana with HDB++ Demo in these minutes. **Here is the link: https://gitlab.com/ska-telescope/TANGO-grafana**

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with K8s and load balancing and share 
the answers with the HDB++ community. **Not done yet**

**Action - Elettra**: Clarify with s2innovation which docker containers should/could be used for the Tango archivers.
**Graziano reported that the SKA ones should be used because they are more up to date. Still an open question on the future, 
see Matteo's comments on this topic below.** 

**Action - ALBA**: Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.
**Damien and Sergi agreed on what needs to be done. They will work together on implementing the agreed solution.**

## News on recent developments

### SKA docker files

Matteo reported that SKA plans on moving away from using docker-compose for their docker files. This will no longer be 
maintained by SKA in the future. He advises to move to K8s.

Matteo mentioned that the SKA docker images are not fully up to date. The dsconfig one is in the process of being upgraded to python3.  

### How to configure a reproducible environment with a predefined HDB++ configuration (list of attributes to be archived + archiving parameters)?

In the SKA projects, Matteo mentioned that there are about 15 teams which are working in different environments and they 
would like to be able to set up easily an environment where defined sets of attributes are configured to be archived 
in HDB++ with some specific archiving parameters.

Graziano mentioned that he developed a script which is able to get a list of attributes to be added in HDB++ with their 
archiving strategies from a simple text file. The script sends some commands to the HDB++ configuration manager.

Matteo said that he developed a similar script which is taking a json file as an input.

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text 
configuration file and push this solution to tango-controls-hdbpp Github organization. 

### HDB++ and Grafana

Matteo presented the work and investigations he has done on using Grafana to display dashboards presenting HDB++ related data.

The git repository can be found on https://gitlab.com/ska-telescope/TANGO-grafana

Matteo worked on the Data Source part of Grafana to be able to display HDB++ related data. He said that it is 
possible to display information coming from the Tango Database, Prometheus or Elastic Search in Grafana.   
Matteo showed a dashboard with some buttons to send commands to Tango devices. Commands are sent using TangoGQL.

He said that one limitation with Grafana is the refresh period which normally cannot be lower than 5 seconds.

### HDB++ Benchmarking

Graziano presented some [slides](hdb++_benchmark_docker_2020_2.pdf) to show the work he has done on creating benchmarks 
for HDB++ using docker containers.

The first tests he did on benchmarking the insertion of data for a Tango::DevDouble scalar attribute are showing better 
results for MySQL-innoDB compared to TimescaleDB, in terms of throughput and disk space usage.  
He mentioned that these results should be taken with care because the disk space usage could be taking into account the logs as 
well for instance.  
He said the tests are pretty reproducible.  
This work could be used to compare different HDB++ backends, to compare different backend configuration, to compare
 different HDB++ library and device servers versions performances. 

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite.

### HDB++ MySQL (libhdbpp-mysql)

Graziano worked on libhdbpp-mysql which is now using CMake.  
He is preparing a new repository for MySQL, which will be similar to hdbpp-timescale-project but for MySQL.

### Decimation and old data storage strategy

Alba is transferring old HDB++ data to another HDB++ database, decimating the data during the transfer process.
Alba is producing about 2TB HDB++ data per year.

At Elettra, old data is transferred to another HDB++ database without any modification because they currently don't 
have too much data. 
They are producing about 100 GB HDB++ data per year.

### Exporting Cassandra data 

At the ESRF, Damien has been working on exporting all the HDB++ data which was stored in ESRF HDB++ Cassandra cluster to csv files. 
This simple operation was not so easy and led to several crashes of some of the nodes of the Cassandra cluster.
To overcome these issues, Damien changed the Cassandra nodes configuration to give them more memory (in /etc/cassandra/cassandra.env.sh file).  
He also used dsbulk tool from Datastax which has some options to control the throughput and throttling. 
Exporting data at a slower pace helped to avoid Cassandra nodes crashes.

**Action - ESRF**: Share scripts used at ESRF to export data with MaxIV

### libhdbpp

Damien has been working on fixing the problems introduced recently.  

### libhdbpp-timescale

The batch insert feature is ready on the library side for the timescaleDB backend.
Some changes will be required on the hdbpp-es side to start testing it with the HDB++ archivers.
According to Damien and from the results of the preliminary tests done by Stuart, this change should greatly improve the insert performances.

### News from Max IV

Max IV is planning to add some new Cassandra nodes to their HDB++ cluster. Mirjam said this should be the last time 
they add nodes to the cluster before the migration to another DB backend.

## High Priority issues

This topic which was on the agenda has been shamefully skipped/forgotten!
If there are any high priority issue you would like to emphasize, please edit these minutes and notify the concerned 
developers. 

## AOB
### Next HDB++ Teleconf Meeting

It was agreed to keep the ~2 months period between 2 HDB++ Teleconf Meetings.  
The next HDB++ Teleconf meeting will take place on Friday 6th November 2020 at 14:00 CET.

## Summary of remaining actions

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs.

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with K8s and load balancing and share 
the answers with the HDB++ community.

**Action - ALBA**: Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text 
configuration file and push this solution to tango-controls-hdbpp Github organization. 

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite.

**Action - ESRF**: Share scripts used at ESRF to export data with MaxIV
