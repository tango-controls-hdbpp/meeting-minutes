# HDB++ Teleconference Meeting - 2020/06/26

To be held on 2020/06/26 at 15:00 CEST on zoom.

# Agenda
 
 1. Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2020-04-15/Minutes.md#summary-of-remaining-actions)
 2. Questions from MaxIV to the HDB++ experts:
     - 1/ Is there any consensus on the selection of the DB? (Will other facilities continue using other DB? MySQL, Cassandra, Postgres ) 
     - 2/ Is it possible to use the TimescaleDB Setup for Long Term storage?
     - 3/ What are the pitfalls and obvious things to look out for when switching infrastructure?
     - 4/ We are considering using our central data store (using ESS CSI)   - pros/cons?
     - 5/ Are there any test/use cases on a Kubernetes cluster?  (TimescaleDB, load-balancing)
     - 6/ Will there be any Analytical layer? (i.e Spark )
 3. Benchmarking brainstorming (Topic proposed by Graziano)
    - Goals:
        - automate tests
        - make test environment easily sharable between institutes
    - Questions:  
        - having everything in dockers could be a good idea?
        - using ska docker tango-archiver (https://github.com/ska-telescope/ska-docker/tree/master/docker/tango/tango-archiver) could be a good starting point?
 4. News on recent developments
 5. AOB
