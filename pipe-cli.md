
# Example
```
$ npm install -g thingpipe
$ pipe
Hello. I am ThingPipe. I create pipes that direct the flow of data from sensors to databases. I can also create pumps to control the flow.
$ # Let's get started with adding sensor and database drivers.
$ pipe drivers add grovepi_temp_pro http://github.com/rjsteinert/thingpipe-sensor-grovepi_temp_pro.git
$ pipe drivers add phant http://github.com/rjsteinert/thingpipe-database-phant.git
$ pipe drivers
name                         type           status          version    origin
grovepi_temp_pro             sensor         not installed   0.2.1      http://github.com/rjsteinert/thingpipe-thing-grovepi_temp_pro.git
phant                        database       not installed   0.1.0      http://github.com/rjsteinert/thingpipe-reservoir-phant.git
$ # Install the drivers now.
$ pipe install dht11_temperature
Successfully installed grovepi_temp_pro
$ pipe install phant
Successfully installed phant
$ # Configure them. These will be global settings, a kind of "global" pipe on the device.
$ pipe config phant global driver.phant.url "http://data.sparkfun.com"
$ pipe config phant global driver.phant.stream_id "g6DJEnagnohd3pVnY1j5"
$ # Now let's try out these drivers.
$ pipe pull grovepi_temp_pro
23
$ pipe push phant "42"
Pushed 42 to phant at http://data.sparkfun.com/stream/g6DJEnagnohd3pVnY1j5
$ # With a command line trick, we combine the two.
$ pipe push phant `pipe pull grovepi_temp_pro`
Pushed 23 to phant at http://data.sparkfun.com/stream/g6DJEnagnohd3pVnY1j5
$
$
$
$ # Now lets create a pipe that connects the two so we can then manage the flow of data with a pump.
$ pipe create pipe1
$ # Connect to pipe1 the grovepi_temp_pro as the source and the phant as the destination.
$ pipe connect pipe1 grovepi_temp_pro phant
$ # Now lets override the global values on these drivers.
$ pipe config pipe1 driver.phant.url "http://data.sparkfun.com"
$ pipe config pipe1 driver.phant.stream_id "g6DJEnagnohd3pVnY1j5"
$ # Now we can use the pump command to do a simultaneous pull and then push.
$ pipe pump pipe1
Pushed 23 to phant at http://data.sparkfun.com/stream/g6DJEnagnohd3pVnY1j5
$
$
$
$ # Let's create that pump now that will manage the flow of data in pipe1.
$
```
