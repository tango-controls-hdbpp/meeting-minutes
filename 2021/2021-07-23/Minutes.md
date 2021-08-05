# Tango HDB++ collaboration Teleconf Meeting - 2021/07/23

Held on 2021/07/23 on Zoom.

Participants: 
- Damien Lacoste (ESRF) 
- Graziano Scalamera (ELETTRA)
- Johan Forsberg (Max IV)
- Lorenzo Pivetta (ELETTRA)
- Reynald Bourtembourg (ESRF)
- Sergi Rubio (ALBA)

# Minutes

## Status of [Actions defined in the previous meetings](https://github.com/tango-controls-hdbpp/meeting-minutes/blob/master/2021/2021-05-19/Minutes.md#summary-of-remaining-actions)

**Action - Sergi, Damien, Lorenzo, Max IV, Giacomo**:
Sergi should guide Damien in implementing support for TimescaleDB in pytangoarchiving.
Sergi should review the PR created by Damien on pytangoarchiving (https://github.com/tango-controls/PyTangoArchiving/pull/18).
Max IV is invited to review https://github.com/tango-controls/PyTangoArchiving/pull/18 which is a proposal from Damien for an AbstractReader.  
Sergi will write the MySQL backend and review and approve https://github.com/tango-controls/PyTangoArchiving/pull/18.  
Sergi will share the work with MaxIV for support with Cassandra.  
Sergi will contact Damien and Giacomo to try to get a unified API (python)  
**pytangoarchiving is being migrated to python3, starting with its dependencies (fandango). 
Sergi said the timezones work differently in python2 and python3.
Sergi will work on this topic during the summer.**

**Action Graziano & Matteo**: Converge on a solution for a script to configure easily a bunch of attributes from a text
configuration file and push this solution to tango-controls-hdbpp Github organization.
MaxIV is using some template yml files to configure easily the archiving settings (event thresholds for specific classes, strategies)

**Action - Alba**: Provide MariaDB container(s) for the HDB++ benchmarking suite.
Sergi will get in touch with Graziano on that topic.  
**There is currently an issue in libhdbpp-mysql when compiling using the mysql compat package**

**Action - Damien**:Push the latest version of the Cassandra to timescaledb migration and validation tools.  
**Damien pushed the latest version of the Cassandra to timescaledb migration and validation tools on hdbpp-timescale-project.
Johan tried the migration tool and managed to migrate some data.**

**Action - Sergi**: Create an issue on hdbpp-es (if there is none yet) about the attributes which are sometimes not archived even though
they are configured with the ALWAYS archiving strategy.  
**The issue was not closed. Sergi would like to try to reproduce the problem first, but he's having some compilation issues 
with libhdbpp-mysql when using the Mysql compat package.**

**Action - MaxIV**: Transfer the YAML tools to tango-controls Gitlab HDB++ subgroup to share them with the community and
to help getting some feedback from the community.  
**Not done yet because not production ready**.

**Action - Sergi**: Create an issue in libhdbpp-mysql about the compilation problem on Debian 10.

**Action - Graziano**: Create an issue to improve hdbpp-es interface when adding attributes in the case of an independent
hdbpp-es (without hdbpp-cm managing it).  
**Done: See https://github.com/tango-controls-hdbpp/hdbpp-es/issues/19**

**Action - Reynald**: Create an issue on each HDB++ repository (including pytangoarchiving), asking github participants
to connect to gitlab.com using their github account (only on the repositories where this is necessary), giving
a 1 week or 10 days deadline.  
**Done. Migration can start any time!**

## General discussions

**Action - Sergi**: Publish a script which is monitoring and restarting automatically archiving on attributes in error.

Sergi asked whether the Abstract Reader could have a dependency to fandango.
Lorenzo said that it would be preferable to avoid this dependency if possible.

Lorenzo mentioned that it could be useful to use a Tango Device to extract data from HDB++.  
This Tango class interface could then be used to get data from any HDB++ Backend in the same way.

Damien said that the HDB++ java tools could be recreated as Java tools.

Another idea for the future which had already been mentioned is the concept of Device Archiver which would archive all 
the attributes of a device. 

Damien created https://github.com/tango-controls-hdbpp/hdbpp-es/issues/20 which is a proposal to use the Tango Interface
Change Events for the archivers to detect when a dynamic attribute is no longer supported by a device to avoid useless
re-subscription attempts by the archiver until this dynamic attribute is again supported by the remote device.  
After discussion, it was decided not to spend time on this issue because it would be too much work compared to what we
might gain.

To avoid modifying some device servers which are pushing only change events by code, Sergi said they are using the 
SubscribeChangeAsFallback property from the HDBEventSubscriber devices. The HDBEventSubscriber device will then try to 
subscribe to change events if the subscription to archive events failed.

The HDB++ system still cannot handle smoothly an attribute type change. Reynald said that they usually rename the attribute 
in HDB++ to associate the old archived attribute data to this name. So the old data is still accessible in HDB++, but 
not using the original name. One needs to use the other name defined for the old data, which can be something like 
my/wonderful/device/attribute-before2021-07-23 for instance.

## News on recent developments

Johan made a demo of the HDB++ monitoring web interface they developed to monitor the status of their HDB++ archiving system.  
This tool is using python and pytango underneath.

Johan mentioned they are using a Prometheus exporter for HDB++ to categorize HDB++ errors. 
They can then use grafana to visualize the main error types.

Sergi said he also has a tool to categorize the errors.

**Action - All**: Share the tools which could be useful for the HDB++ community and update read the docs HDB++ section to 
advertise these tools.

## High Priority Issues

No new High Priority issue reported during this meeting.

## AOB

### Migration to Gitlab

**Action - Reynald**: Start migration to gitlab.

### Next HDB++ Teleconf Meeting

The next HDB++ meeting will take place on Wednesday 15th September 2021 at 10:00 CEST. 
It will be a face to face meeting at the ESRF for the ones who would like / can come to the ESRF. 
This will be as well a Zoom meeting for the ones who don't want / cannot come to the ESRF.

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

**Action - Sergi**: Create an issue on hdbpp-es (if there is none yet) about the attributes which are sometimes not archived even though
they are configured with the ALWAYS archiving strategy.

**Action - MaxIV**: Transfer the YAML tools to tango-controls Gitlab HDB++ subgroup to share them with the community and
to help to get some feedback from the community.

**Action - Sergi**: Create an issue in libhdbpp-mysql about the compilation problem on Debian 10.

**Action - Sergi**: Publish a script which is monitoring and restarting automatically archiving on attributes in error.

**Action - All**: Share the tools which could be useful for the HDB++ community and update read the docs HDB++ section to
advertise these tools.

**Action - Reynald**: Start migration to gitlab.