___Until the OPK reaches 1.0, it is intended for developers only. See the [OPK Roadmap to 1.0](https://github.com/open-eio/Open-Pipe-Kit/issues/1).___

## What is the Open Pipe Kit?
The Open Pipe Kit (OPK) is the missing plumbing between sensors and databases that will empower thousands of data journalists and civic hackers to collect data without needing a programmer's assistance or being locked into one data platform from a proprietary turn-key solution. 

When the OPK is ready, anyone will be able to build a Pipe for $60 of readily accessible parts, access the Pipe User Interface from a smartphone or PC, choose a sensor from the list of supported sensors, plug the sensor into the Pipe, and then choose a location to stream data to. If the sensor or database you're hoping to use isn't on our list of drivers, someone with programming knowledge can contribute a driver for that sensor or database back to the Open Pipe Kit project. It's Open Source!

Some of the first sensor drivers we are building include ...

- Grove Dust sensor driver
- Grove Moisture sensor driver
- Grove Loudness sensor driver
- Grove Temperature and Humidity sensor driver
- Grove Air Quality sensor driver
- Don Blair's Water Depth sensor driver for our friends in New Orleans who need flood alarms

Some of the first database drivers we are building include ...

- CSV data reservoir driver, for local data storage or remote storage over email
- Xively reservoir driver, for remote data storage
- Dat data reservoir driver, for local and/or remote data storage
- Apitronics Hive database (uses CouchDB) reservoir driver, for local and/or remote data storage 
- Cloudant (uses CouchDB) for remote data storage


## Who are the OPK developers?
The Open Pipe Kit developers are software and electrical engineers that have been building environmental monitoring solutions for data journalists and civic hackers for years. They have now figured out a way to empower 95% of these use cases with one piece of software and documentation, they call it Open Pipe Kit. 


## The Open Pipe Kit Manifesto
Our mission is to develop a kit for building Pipes that ...

1. __Empower non-programmers to collect data from a large selection of sensors__.  Other systems require programming to set up data collection.

2. __Give users multiple data storage options. Fight vendor lock-in by giving users the freedom to choose where their data flows__.  Other proprietary turn-key systems lock users' hardware to one proprietary data service.

3. __Spur innovation by giving programmers the freedom to write additional sensor and database drivers__.  Other systems require users to buy and use their own proprietary sensors and databases.

The Internet has often been compared to a system of pipes.  Imagine that these pipes carry water: for someone interested in collecting water from a local river in order to store it for later use, then, to date, nearly all the "Internet of Things" sensor data solutions are like companies that sell customers proprietary pipes and fittings designed to transport the user's water (sensor data) to a remote, hidden reservoir (a cloud-based server); and typically the user is then required to pay a fee in order to access this now-remote resource. 
 
We believe it is vital for people in the fields of sensor journalism, environmental monitoring, and agriculture to have full control over the data they collect, and to be able to use reliable, easily-acquired, open source hardware and software that can be modified and repurposed without permission.

The Open Pipe Kit is a system designed to meet this need, based on a Raspberry Pi and Node.js.  Users of OPK will be able to collect data from sensors and store it either locally (on microSD) or remotely on a server of their own choosing. 

## Architecture

```
   [Thing]
     ^
     |
   [Pipe] <-> [HTTP API] <-> [Apps]
     |
     V
  [Reservoir]

[NetworkSquid] <-> [HTTP API] <-> [Apps]
```
Command line inspired by git. Plugin architecture more convention than framework. Program them in any language.
