# Tango HDB++ collaboration Teleconf Meeting - 2020/04/15

Held on 2020/04/15 on Zoom.

Participants: Reynald Bourtembourg, Jean-Michel Chaize, Andrew Goetz, Stuart James, Damien Lacoste, Jean-Luc Pons (ESRF),
              Lorenzo Pivetta, Graziano Scalamera, Giacomo Strangolino (ELETTRA), Sergi Rubio (ALBA), Gwenaelle Abeille (SOLEIL), 
              Matteo Di Carlo (INAF), Mangesh Patil (NCRA), Apurva, Alan Bridger (SKAO), 
              Michal Falowski, Ireneusz Zadworny, Thomasz (SOLARIS), Alexander Senchenko, Valdimir Sitnov (BINP SB RAS)
              
# Minutes

## HDB++ TimescaleDB status

- Stuart James' last day of work with the ESRF will be 16th April 2020.
- Damien Lacoste has been hired at the beginning of 2020 by the ESRF and has been working with Stuart over the last months.
- Damien will take over the work done by Stuart on HDB++ with the help from Reynald Bourtembourg.
- Stuart will continue to support the project for the next few months and can be contacted at the e-mail address written on the last slide of its [presentation](overview.pdf).
- Stuart presented the current status of HDB++. The [slides](overview.pdf) are attached to these minutes.
- Stuart and Damien investigated timescaledb table compression feature and observed a 30% compression ratio for scalar attribute data and about 70%-80% for spectrum attribute data.
- Stuart will upload on github some additional documentation related to timescaleDB cluster deployment with Patroni and barman.
- Stuart will benchmark the batch event insertion feature and will push the changes related to this feature to Github.
- Stuart said he would continue to work on the Python Data API in the coming months. Sergi mentioned that he would like to take advantage of this work to use it in pytangoarchiving.
- Andy pointed out that Tango documentation on HDB++ does not mention the timescaleDB backend
- Stuart has started to work on some data migration scripts from HDB++ Cassandra to HDB++ TimescaleDB
- The TTL service might be useful for HDB++ PostgreSQL backend too. But might need to be adapted to fit the DB schema.
- Stuart will publish his work during the investigations on GraphQL. This will be published under services folder of [hdbpp-timescale-project git repository](https://github.com/tango-controls-hdbpp/hdbpp-timescale-project).
- Stuart said that we currently have chunk sizes of 10-20 GB on our cluster, which seems to be a reasonable size for our nodes (which have lots of RAM (128 GB)).
- The reorder-chunks service will no longer be needed with the next TimescaleDB release because this feature will be provided in the community version.
- Damien has implemented the support for DevEnum types in HDB++ timescaleDB, using PostgreSQL triggers. 
This required to change the event subscriptions order in the event subscriber in order to subscribe first to attribute configuration events.

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs


## HDB++ MySQL/MariaDB status

Sergi Rubio mentioned an issue with the fastest MySQL Python driver which has some memory leaks.
This is causing some issues which prevent to do safely some decimation on the client side with this MySQL driver.

## HDB++ Cassandra status

The HDB++ Cassandra backend is no longer used at the ESRF (move to TimescaleDB) and Solaris (move to MySQL).
ESRF has stopped the development of this backend.
Andy suggested to mention that in the git repository README.

**Action - ESRF**: Add a note in libhdbpp-cassandra README (and in Tango HDB++ documentation) that this backend is no longer maintained by the ESRF.
Contributions are always welcome of course.

## HDB++ @ ESRF

- ESRF deployed TimescaleDB on 3 nodes (1 master, 2 replicas), using Patroni.
- The disk usage is currently about 800 GB on each node (the system is up since July 2019 but the ESRF was in a long shutdown until december 2019).
Stuart said that the best would be to use nodes with 8 TB SSDs in the future.
- Aggregations are in operation. This is for all attributes and this is kept for 12 months maximum.
A backup procedure will have to be implemented in order to eventually backup the result of these aggregations before this 12 months expiration.

## HDB++ @ ALBA

- ALBA is using many small instances of MariaDB HDB++ databases.
- ALBA is interested in trying PostgreSQL or TimescaleDB backends to benefit from PostgreSQL native array support.
- ALBA is storing beam orbit spectrum attributes at 20Hz. This can produce 2GB per day for such attributes.
- Some decimation is done on the server side as well as on the client side when needed.
- ALBA tries to keep partitions below 50GB.

## HDB++ @ ELETTRA

- Elettra is using MySQL with InnoDB engine.
- Last 2 years of data are kept in the live HDB++ database (~ 500 GB).
- Data older than 2 years is transferred to another database instance.
- ProxySQL is used to redirect queries to the correct database.

## HDB++ @ SOLARIS

- Solaris moved away from Cassandra backend because of lack of human resources to maintain a Cassandra cluster.
- Solaris is using HDB++ MySQL backend, 1 database per beamline.
- Since plotting data coming from several HDB++ instances is not yet supported by the jhdbviewer, Sergi suggested to use Taurus to be able to plot data coming from several HDB++ database instances.

## HDB++ @ BINP SB RAS

PostgreSQL backend is used at Novosibirsk.

## HDB++ @ SOLEIL

Soleil is still using the legacy Tango HDB and may switch to HDB++ during the upgrade of the synchrotron in the next years.

## HDB++ @ SKA/INAF/NCRA

- SKA is still in a phase of investigations.
- Matteo will continue the work on the ELK backend.

## jhdbViewer

- The support for handling several HDB++ databases from the same jhdbviewer has been discussed.
Jean-Luc Pons said that this feature should not be too difficult to implement.
- It has been suggested to add the possibility to apply a filter on the tree and to add a search bar.
Jean-Luc said that this would be more complex to implement.

## Web viewers

- There seems to be a common interest in HDB++ web viewers. This is an area where collaboration could take place.

## Next meetings

Lorenzo proposed to have a face-to-face HDB++ meeting at the end of 2020.  
We agreed to have regular HDB++ meetings every ~2 months. So the next one will take place in June.

**Action - ESRF**: Send a Framadate Poll to find the best date for the next HDB++ teleconf meeting.

## Summary of remaining actions

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs

**Action - ESRF**: Add a note in libhdbpp-cassandra README (and in Tango HDB++ documentation) that this backend is no longer maintained by the ESRF.
Contributions are always welcome of course.

**Action - ESRF**: Send a Framadate Poll to find the best date for the next HDB++ teleconf meeting.
