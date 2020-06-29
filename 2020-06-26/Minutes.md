# Tango HDB++ collaboration Teleconf Meeting - 2020/04/15

Held on 2020/06/26 on Zoom.

Participants: Reynald Bourtembourg, Jean-Michel Chaize, Damien Lacoste, Jean-Luc Pons (ESRF),
              Lorenzo Pivetta, Graziano Scalamera, Giacomo Strangolino (ELETTRA), Sergi Rubio (ALBA), 
              Gwenaelle Abeille (SOLEIL), Mangesh Patil (NCRA), Apurva, Alan Bridger, Juande Santander-Vela (SKAO), 
              George Fatkin (BINP SB RAS), Mirjam Lindberg, Artur Barczyk (Max IV)
              
# Minutes

## Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2020-04-15/Minutes.md#summary-of-remaining-actions)

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs. **Still needs to be done**

**Action - ESRF**: Add a note in libhdbpp-cassandra README (and in Tango HDB++ documentation) that this backend is no longer maintained by the ESRF.
Contributions are always welcome of course. **Done**

**Action - ESRF**: Send a Framadate Poll to find the best date for the next HDB++ teleconf meeting. **Done**


## Questions from MaxIV to the HDB++ experts:
### Is there any consensus on the selection of the DB? (Will other facilities continue using other DB? MySQL, Cassandra, Postgres ) 

No, but as Lorenzo pointed out, it is also one of the strengths of HDB++, the ability to switch easily from one DB backend to another.

Sergi said that Alba is planning on using TimescaleDB.

George Fatkin (BINP) said that they are also considering using the TimescaleDB backend.

ESRF moved from Cassandra to TimescaleDB during the long shutdown, they are also planning to use and to maintain the timescaleDB HDB++ backend.

The MySQL/MariaDB backend will probably still be used in the future because it can be convenient to use for small HDB++ databases.

### Is it possible to use the TimescaleDB Setup for Long Term storage?

In theory it should be possible.

### What are the pitfalls and obvious things to look out for when switching infrastructure?

The main advice is to run the new DB backend in parallel with the previous one. 
So to have HBEventSubscribers archiving in the previous DB backend and new HDBEventSubscribers storing data into the new DB 
backend in parallel.
This way you should not lose any data, even if the new DB backend is not correctly configured.
When the configuration and deployment of the new DB backend is stable, you can stop the old one and start the migration 
of the old data.

To handle this transition phase, on the extraction phase, Sergi is suggesting to introduce another abstract layer on the 
extraction side which is able to extract data from different backends with some intelligence so it knows where to find 
the data associated with a given timestamp. (Data before one date should be extracted from the old DB backend, 
else extract from new DB backend).

### We are considering using our central data store (using ESS CSI)   - pros/cons?

ESS = Elastic Storage System = IBM's GPFS product (HW+SW)

CSI = Container Storage Interface (for using the GPFS storage in k8s)

The advice given during the meeting is to contact the timescaledb experts via their 
[timescaledb slack channel](https://slack.timescale.com/) to get their opinion on the topic.

### Are there any test/use cases on a Kubernetes cluster?  (TimescaleDB, load-balancing)

Not yet in the HDB++ community but [this timescale blog](https://blog.timescale.com/blog/new-helm-charts-for-deploying-timescaledb-on-kubernetes/) is suggesting that it should be possible.

MaxIV will investigate and let the HDB++ community know the results of the investigations.

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with K8s and load balancing and share 
the answers with the HDB++ community.

### Will there be any Analytical layer? (i.e Spark )

For the moment, ESRF is taking advantage of Timescale features to make Timescale compute the aggregates directly (using views).

It should be possible to use Spark if deemed necessary in the future.

## Benchmarking brainstorming (Topic proposed by Graziano)

Graziano presented a slide showing the different components which could be docker containerized.

- Goals:
  - automate tests
  - make test environment easily sharable between institutes
- Questions:  
  - having everything in dockers could be a good idea?  
  Answer from the HDB++ community: Yes.
  - using ska docker tango-archiver (https://github.com/ska-telescope/ska-docker/tree/master/docker/tango/tango-archiver) could be a good starting point?
  - contributions?
  ESRF can maintain the TimescaleDB docker image created by Stuart  
  Other contributions?

Graziano said he read several papers discussing the using of Docker for Benchmarking.
He said that the results should be taken with great care.
Having Docker images for many HDB++ components is in any case an advantage for the HDB++ community.

About the SKA docker question, this should be clarified with s2innovation.

**Action - Elettra**: Clarify with s2innovation which docker containers should/could be used for the Tango archivers.

## News on recent developments
Damien merged the following PRs recently:  

- Fix potential crash in HdbConfigurationManager::add_domain ([#6](https://github.com/tango-controls-hdbpp/hdbpp-cm/issues/6))
 [hdbpp-cm#9](https://github.com/tango-controls-hdbpp/hdbpp-cm/pull/9)
- Integrated build ([hdbpp-cm#11](https://github.com/tango-controls-hdbpp/hdbpp-cm/pull/11))
- Exp refactor ([libhdbpp#3](https://github.com/tango-controls-hdbpp/libhdbpp/pull/3))

INAF/SKA have prepared a demo using Grafana with HDB++ and also using APM to trace requests among the system.
Juande said he will add a link to the demo in these minutes.

**Action - SKAO**: Add a link to Grafana with HDB++ Demo in these minutes

## AOB

### Python API for TimescaleDB

Having a python API to be able to extract HDB++ data from HDB++/Timescale is now one of the priorities for the ESRF.  
Damien said he is volunteer in implementing this part in pytangoarchiving.
Sergi will guide him in this process. According to Sergi, there is only 1 file to modify and about 8 methods to implement.

**Action - ALBA**: Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.

### Next HDB++ Teleconf Meeting

The next HDB++ Teleconf meeting will take place on Friday 4th September 2020 at 14:00 CEST.

## Summary of remaining actions

**Action - ESRF**: Add HDB++ timescaleDB documentation to Tango HDB++ documentation on readthedocs

**Action - SKAO**: Add a link to Grafana with HDB++ Demo in these minutes

**Action - MaxIV**: Contact TimescaleDB experts to know more about TimescaleDB with K8s and load balancing and share 
the answers with the HDB++ community.

**Action - Elettra**: Clarify with s2innovation which docker containers should/could be used for the Tango archivers.

**Action - ALBA**: Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.