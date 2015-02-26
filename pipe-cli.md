
# Example
```
$ npm install -g thingpipe
$ pipe
Hello. I am ThingPipe. I create pipes that direct the flow of data from sensors to databases. I can also create pumps to control the flow.
$ # Let's get started with adding sensor and database drivers.
$ pipe driver clone grovepi_temp_pro http://github.com/rjsteinert/thingpipe-sensor-grovepi_temp_pro.git
grovepi_temp_pro (sensor) now available
$ pipe driver clone phant http://github.com/rjsteinert/thingpipe-database-phant.git
phant (database) now available
$ pipe driver
name                         type           status          version    origin
grovepi_temp_pro             sensor         not installed   0.2.1      http://github.com/rjsteinert/thingpipe-thing-grovepi_temp_pro.git
phant                        database       not installed   0.1.0      http://github.com/rjsteinert/thingpipe-reservoir-phant.git
$ # Install the drivers now.
$ pipe install dht11_temperature
Successfully installed grovepi_temp_pro
$ pipe install phant
Successfully installed phant
$ # Configure the drivers. These will be global settings for a kind of "global" pipe on the device.
$ pipe config global driver.phant.url "https://data.sparkfun.com"
$ pipe config global driver.phant.stream_id "g6DJEnagnohd3pVnY1j5"
$ # Now let's try out these drivers.
$ pipe pull grovepi_temp_pro
23
$ pipe push phant "42"
Pushed 42 to phant at https://data.sparkfun.com/streams/g6DJEnagnohd3pVnY1j5
$ pipe connect global grovepi_temp_pro
$ pipe pump
Pushed 23   to phant at https://data.sparkfun.com/streams/g6DJEnagnohd3pVnY1j5
$
$
$
$ # Now lets create a pipe that connects the two so we can then manage the flow of data with a pump.
$ pipe create pipe pipe1
$ # Connect to pipe1 the grovepi_temp_pro as the source and the phant as the destination.
$ pipe connect pipe1 grovepi_temp_pro phant
$ # Now lets override the global values on these drivers.
$ pipe config pipe1 driver.phant.url "https://data.sparkfun.com"
$ pipe config pipe1 driver.phant.stream_id "wpKwzOZaVotqOz0XJq6p"
$ # Now we can use the pump command to do a pull and then push given the pipe's settins.
$ pipe pump pipe1
Pushed 23 to phant at https://data.sparkfun.com/streams/wpKwzOZaVotqOz0XJq6p
$ # Review the status of the pipe(s).
$ pipe -ls
... @todo
$
$
$
$ # Let's create a pump now that will manage the flow of data in pipe1.
$ pipe create pump pump1
$ pipe config pump1 interval 5000
$ pipe config pump1 capacity 4
$ pipe connect pipe1 pump1
$ pipe start pump1
$ # Now every 5 seconds the pump will pull and that data will stay in the pipe until a capacity of 4 is reached, at which point the pump will flush the pipe with pipe push.
Pushed 23,23,24,23 to phant at https://data.sparkfun.com/streams/wpKwzOZaVotqOz0XJq6p
$
$
```
i have two sensors datas
have also to reserves
ups two
and for each sensor a different pump interval

goal: serve the most complicated use case while also serving the most simple use case in an easy to use command

creating minimal viable complex example
