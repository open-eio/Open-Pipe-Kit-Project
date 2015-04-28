[![Stories in Ready](https://badge.waffle.io/open-eio/Open-Pipe-Kit-Project.png?label=ready&title=Ready)](https://waffle.io/open-eio/Open-Pipe-Kit-Project)
## About
The Open Pipe Kit (OPK) is the missing plumbing between sensors and databases that will empower thousands of data journalists and civic hackers to collect data without needing a programmer's assistance or being locked into one data platform from a proprietary turn-key solution. 

___Until the OPK reaches 1.0, it is intended for developers only. See the [OPK Roadmap to 1.0](https://github.com/open-eio/Open-Pipe-Kit/issues/1).___


## For Developers
OPK isn't a framework, it doesn't have plugins. It's the opinion that we should build drivers for sensors as Command Line Interfaces (cli) in any language we want. Follow a couple of well thought out conventions (like pulling data from a sensor should be the `pull` command) so other humans (and computers) can follow you. We're going to build amazing things.

Every sensor or database driver is a Command Line Interface. Sensors have a standard `pull` command, databases have a stand `push` command. They both have a `help` command that shows the parameters for push or pull as well as the options for each of those parameters. The help commands are how the GUI is going to be able to display a form to the user that empowers them to configure pipes without having to go down to the command line.

## OPK compatible Command Line Interfaces (cli)
Name | Type | Status | Link | Description
--- | --- | --- | --- | --- | ---
simpleserial | sensor | prototype |  https://github.com/open-eio/opk-simple-serial-cli | Pipe sensor driver that gets values between `x` and `x` found over serial | 
phant | database | prototype |  https://github.com/open-eio/opk-phant-cli | Pipe database driver that sends data to a Phant service like http://data.sparkfun.com. [Phant project page here](http://phant.io/)
temper1 | sensor | prototype | http://github.com/open-eio/opk-temper1-cli | For the temper1 USB temperature sensor

