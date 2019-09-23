# Tango HDB++ collaboration Meeting - 2019/09

Held from 2019/09/18 to 2019/09/20 at the ESRF, Grenoble, France.

Participants: Reynald Bourtembourg, Stuart James, Jean-Luc Pons, Pascal Verdier, Andrew Goetz, Jean-Michel Chaize (ESRF),
              Sergi Rubio (ALBA), Gwenaelle Abeille (SOLEIL), Lorenzo Pivetta, Graziano Scalamera (ELETTRA),
              Remotely: Matteo Di Carlo (INAF), Mangesh Patil (NCRA), Marco Bartolini, Nick Rees (SKAO), Raj Uprade (GMRT)
              
# Minutes
## Agenda and presentations
Agenda and presentations can be found on the dedicated indico web page: https://indico.esrf.fr/indico/event/37/

## Discussions


## Outcome

A new hdbpp slack channel has been created on tango-controls slack to discuss HDB++ specific topics.

A new repository has been created to keep track of the minutes of the HDB++ collabration meeting: https://github.com/tango-controls-hdbpp/meeting-minutes

Since HDB++ archivers and configuration manager can run on any computer having network access to a Tango Control System, it has been decided that C++11 can be used as a requirement to build libhdbpp.

## Roadmap
### Priorities

1- ICALEPCS Paper, poster and HDB++ presentation for ICALEPCS Tango workshop (https://indico.esrf.fr/indico/event/34) preparations

2- Benchmarking

3- Review and merge refactoring proposal from Stuart (CMake and meta-projects)

4- Extraction library - Implement missing backend supports in Python, investigations in using GraphQL

5- Update hdbpp-es and libhdbpp API to be able to batch write the whole received events queue when the queue is not empty.
The vector currently used for the queue will first have to be replaced with a more efficient collection (use a lockless structure for the queue?).

## Actions

- **ESRF**: See if there is a way to remove some indexes on timescale schema (or some primary keys)
- **SOLEIL**: Share REST API developed at Soleil and used to query HDB data
- **ALBA**: PyTangoArchiving: Add support for PostgreSQL and Timescale backends (not before November)
- **ESRF**: Prepare some docker files to help to setup benchmarking tests
- **INAF**: Share repository hosting Elastic Search backend. It could be cloned to tango-controls-hdbpp github organization or transferred there.
- **ESRF**: Add goals defined in this meeting in hdbppMetrics git repository
- **ELETTRA**: Redo the PostgreSQL benchmark tests but loading the data ordered by time, instead of by attribute
