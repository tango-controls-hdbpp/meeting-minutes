# Tango HDB++ collaboration Meeting - 2019/09

Held from 2019/09/18 to 2019/09/20 at the ESRF, Grenoble, France.

Participants: Reynald Bourtembourg, Stuart James, Jean-Luc Pons, Pascal Verdier, Andrew Goetz, Jean-Michel Chaize (ESRF),
              Sergi Rubio (ALBA), Gwenaelle Abeille (SOLEIL), Lorenzo Pivetta, Graziano Scalamera (ELETTRA),
              Remotely: Matteo Di Carlo (INAF), Mangesh Patil (NCRA), Marco Bartolini, Nick Rees (SKAO), Raj Uprade (GMRT)
              
# Minutes
## Agenda and presentations
Agenda and presentations can be found on the dedicated indico web page: https://indico.esrf.fr/indico/event/37/

## Discussions

The first 1/2 day was mainly dedicated to presentations describing how the archiving system is managed in the different 
institutes (DB backend used, partitions management, backups, statistics, decimation, long term archiving).

Stuart mentioned that indexes are currently taking about 60% of disk usage with the current timescale schema. 
It was suggested to investigate whether some indexes or keys (primary and foreign keys) could be removed to improve performances and disk usage.

A REST API inspired by the Tango REST API was developed/specified at Soleil for HDB and they use Grafana, React JS and Django to display the HDB data from a web browser (only scalar attributes are currently supported).
It was suggested to share this REST API with the HDB++ community since it could be used as well to define queries to get HDB++ data.

Sergi mentioned that PyTangoArchiving will support Postgres and timescale backends, but not before November.

Discussions took place about the possibility to query data coming from different databases.
At Elettra, ProxySQL is used for this purpose. 
At Alba, there is a layer in PyTangoArchiving which is configurable via Tango properties and which is able to load the adapted python module to extract data, according to some criterias (device name, time range, ...).
It should be easy to develop/provide a new Python module to query data coming from a new DB backend or from a known backend with a different schema.

Graziano presented the results of some benchmark tests comparing Timescale, Postgres and MySQL HDB++ backends when doing a standard query to retrieve HDB++ data coming from a scalar DevDouble attribute.
The results highlighted the importance of the clustering timescale operation and showed very few differences between Timescale and Postgres performances for this kind of query, which is the most common query in an HDB++ system.
The data were ingested into Timescale and Postgres, using a dump where the data was grouped by attribute. This is not the case in real life.
It was suggested to redo the test after ingesting data ordered by time, as in a real system, not grouped by attributes to see whether this could make a difference in PostgreSQL results.
Graziano said that InnoDB is better than MyIsam to retrieve data because there are some issues with table locks when using MyIsam.
InnoDB is slower than MyIsam for inserting data.

Mangesh presented the requirements for SKA Engineering Data Archive which will use HDB++. 
The presented numbers were very impressive and can be seen on their slides. They were advised to avoid storing everything at high frequency and to rather tune carefully their archive event thresholds to ensure they store only relevant data.
They were recommended to have a look at ScyllaDB, a drop-in Apache Cassandra alternative written in C++.

Matteo described briefly the work he did with the Elastic Search backend he developed, where the data was stored in json format.
He suggested the idea of providing the possibility to send events containing the values of all the attributes of a Tango device (in Json?).
He also mentioned that he would like to be able to store data which are not necessarily attributes. 
It could be properties, or any external data describing the context of the current system.

Stuart mentioned that there is a limitation with json format/spec because it does not support NaN , inf and -inf.

Lorenzo suggested the idea of adding something into Tango to be able to archive/to be notified when a command is executed.

It was suggested to share some tests where all Tango types officially supported by HDB++ would be tested.

Another suggestion was to add an attribute in the HdbEventSubscriber class to give the current latency of the system/archiver.

A discussion took place on the extraction process. It would be good to have a common extraction library API in different languages.
Sergi said that this common extraction library should give the possibility to send custom queries.

Some discussions took place to discuss the possibilities which could improve the user experience when extracting data.
Using decimation could improve performances on queries requesting a big amount of data. Decimation can be done on the client side and/or on the server side.
Using decimation on the server side will require new queries when zooming on the data but will allow to handle bigger amount of data.
Using decimation on the client side is efficient until a certain amount of data, depending on the network bandwidth,available RAM, CPU...
 
Stuart suggested to have a look at timescale aggregated views which could help to compute some statistics and decimation data on the server side. 

Stuart presented a new architecture proposal with the introduction of a layer to control the queries done by the users and to be able to do some throttling.
A bulk download service could then be used to help the user to be notified when the data would be ready to be downloaded.

Stuart expressed the wish to have a look at GraphQL first, which could be used for general purpose extraction.

A discussion took place on how the HDB++ archiving process could be improved to take advantage of batch inserts performances.
Graziano suggested to modify the HdbEventSubscriber and libhdbpp libraries in order to be able to pass all the elements currently contained in hdbpp-es received events queue to the libhdbpp implementation when the queue contains more than one element.
The libhdbpp implementation could then use batches to insert all the elements of this queue.
Before implementing these changes, it is important to modify the type used to store the event queue (vector currently) to a more efficient collection type. 
Lorenzo suggested to have a look at a lock-less structure for this queue. So this change will require the following steps:

1- Review and merge the changes suggested by Stuart to use CMake and have meta-projects for the different back-ends

2- Change the hdbpp-es received event queue type from vector to a more efficient type

3- Add method(s) in libhdbpp libraries to pass queue/list of events data to libhdbpp backend

4- Modify hdbpp-es to use these new method(s) when the queue contains more than one element

On Friday, Stuart presented the refactoring work he has done in a branch of the different repositories.
He suggested to use CMake and to create one meta-project for each supported back-end.
This meta-project would allow the user to retrieve, from one single Github repository, all the code needed to build a configuration manager and event subscriber for a given backend, by cloning other repositories or by using git submodules.
He also suggested some changes to introduce an hdbpp namespace, to use smart pointers, to change the API to pass some parameters by reference instead of by value.
The changes are impacting the include files too (location, name). 
He also identified some common code which is used by every libhdbpp backend implementation and which could be moved to libhdbpp.
All these changes will break the compatibility with the current implementation so, as a consequence, the major release number will have to be increased.
It should be possible to install this new version of the library alongside with the older version on the same computer.

TCS GMRT presented the current status of their HDB++ deployment. 
They got the following advices:
- Update to cppTango >= 9.3.3 to solve the heartbeat issues they are encountering
- Create DB partitions in their long term archive DB
- Ensure DB files are smaller than the available RAM

### Icalepcs

Reynald shared a link on icalepcs-2019 channel from tango-controls slack to be able to collaborate on the HDB++ paper for ICALEPCS.

The Tango workshop at ICALEPCS 2019 will provide an introduction to HDB++.
Sergi and Reynald will share a 1 hour slot to present the HDB++ concepts and tools.
The agenda of the workshop is available at https://indico.esrf.fr/indico/event/34

### Benchmarking

Discussions took place to agree on the goals of the HDB++ Benchmarking tools.
The following goals were mentioned:
* Know the attributes limits per subscriber (max number of attributes, max event throughput) 
* Know the attributes limits per attribute type
* Know the disk space used by the DB Backend
* Evaluate the user experience (Need to get some metrics on the client side (extraction)).
Metrics like amount of data and time to retrieve this amount of data could be interesting 

What do we want to measure?
- insert performances (max number of events/second for a given Tango type)
- Disk usage
- extraction performances
- difference between SSD and HDD
- Max number of events per subscriber before the event reception queue growth too much (so we need to see the evolution of the AttributePendingNumber attribute)
- Latency
 
Graziano pointed out that there is currently an hard-coded limit of 10000 attributes per HDB++ subscriber device.

Stuart presented what he did on the hdbppMetrics repository and the patches provided in this repo which could be used to patch the event subscriber code.
It was suggested to move these patches to the event subscriber code and to give the possibility to enable these performances measurements at run time or by changing the HdbEventSubscriber configuration.

Following the results of the benchmarks done by Graziano, a discussion took place and the following timescale blog post was mentioned by Stuart and could help to understand the results and to better understand when timescale can provide better performances than PosgreSQL: https://blog.timescale.com/blog/timescaledb-vs-6a696248104e/amp/ .
Sergi suggested to repeat the tests with spectrum attributes.

We agreed on the following goals and metrics as a first step:

| Goal                         | Question                                                                                                  | Metrics                                                |
| -----------------------------|-----------------------------------------------------------------------------------------------------------|--------------------------------------------------------|
| Measure insertion throughput | What is the max number of events inserted? Comparison SSD vs HDD? Evolution of pending queue in hdbpp-es? | Nb of rows inserted per second per subscriber, latency |
| Measure extraction throughput| Behaviour when extracting devDouble scalar data, devDouble arrays? Comparison SSD vs HDD?                 | time in seconds to retrieve the data |
| Measure the disk usage for each supported Tango Data Type | What is the disk usage for a given data set?                                | disk usage (before and after insertion) 

Reynald will write these goals in the HdbppMetrics repository.

An AWS account can be provided, financed by the Tango community to execute these benchmark tests. Please contact Andy Goetz if you need AWS resources.

Stuart and Lorenzo will share Tango classes they used for their tests to generate events for every supported Tango type.

Stuart will prepare some docker files which could be used for the tests

A suggestion was made to use `dsconfig` to configure easily the Tango DB used during the tests from a json file. 

Stuart suggested to improve the system in order to be able to run an HDB++ system with only 1 HdbEventSubscriber and without HdbConfigurationManager.
This would simplify the life of the testers. This would imply to move some code from the HdbConfigurationManager to the HdbEventSubscriber.
All the participants to this discussion agreed this would be useful.

Stuart suggested the possibility for the HdbEventSubscriber to automatically create an entry in `att_conf` table the first time it receives an event for an attribute which is not yet in att_conf.

### Tango kernel

Lorenzo suggested that a week of training to share the knowledge related to the Tango C++ library and PyTango should be organized next year.
According to Matteo, it would be useful to produce some class diagrams. This could be combined with a code camp to solve issues and exercises.

A first draft of RFCs should be done first. Then, after the training, the RFCs could be refined.

## Outcome

A new hdbpp slack channel has been created on tango-controls slack to discuss HDB++ specific topics.

A new repository has been created to keep track of the minutes of the HDB++ collaboration meeting: https://github.com/tango-controls-hdbpp/meeting-minutes

Since HDB++ archivers and configuration manager can run on any computer having network access to a Tango Control System, it has been decided that C++11 can be used as a requirement to build libhdbpp.

## Roadmap
### Priorities

1- ICALEPCS Paper, poster and HDB++ presentation for ICALEPCS Tango workshop (https://indico.esrf.fr/indico/event/34) preparations

2- Benchmarking

3- Review and merge refactoring proposal from Stuart (CMake and meta-projects)

4- Extraction library - Implement missing backend supports in Python, investigations in using GraphQL

5- Update hdbpp-es and libhdbpp API to be able to batch write the whole received events queue when the queue is not empty.
The vector currently used for the queue will first have to be replaced with a more efficient collection (use a lock-less structure for the queue?).

## Actions

- **ESRF**: See if there is a way to remove some indexes on timescale schema (or some primary keys)
- **SOLEIL**: Share REST API developed at Soleil and used to query HDB data
- **ALBA**: PyTangoArchiving: Add support for PostgreSQL and Timescale backends (not before November)
- **ESRF**: Prepare some docker files to help to setup benchmarking tests
- **INAF**: Share repository hosting Elastic Search backend. It could be cloned to tango-controls-hdbpp github organization or transferred there.
- **INAF**: Benchmark Elastic Search backend.
- **ESRF**: Add goals defined in this meeting in hdbppMetrics git repository
- **ELETTRA**: Redo the PostgreSQL benchmark tests but loading the data ordered by time, instead of by attribute
- **All Institutes**: Share Tango classes used to generate events for every Tango type
