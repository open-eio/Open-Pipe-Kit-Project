
# How we got here

Our goal is to develop tools that lower the barrier to interacting with these drivers. Because there is no universal driver for all sensors and all databases, we have to develop a standard for drivers so we can then build a Graphical User Interfaces that empowers people to use sensors and databases. By standard we don't just mean a specification, we mean something that is widely adopted. I do not believe it is enough to just depend on the good will of programmers by saying "Write a plugin for a sensor this way so people who don't know how to program can use it!" I believe we need a symbiotic medium where the developers write drivers according to a specification because it makes their own work easier. Empowering nonprogrammers should be a side effect.

First, a quick PSA for anyone as foolish as us who thinks they are going to create a standard.

![XKCD comic on Standards](https://imgs.xkcd.com/comics/standards.png)
[[source]](https://xkcd.com/927/)

So how do we avoid creating the fifteenth standard?

## Challenge #1: Don't build a plugin system, build drivers that stand on their own.
There is an interesting talk from Mike McNeil called [Pulling the Plug](https://www.youtube.com/watch?v=72jI0dQx7pw#t=572) where McNeil describes that, "We need a way to liberate these functions from these plugins... it's kind of like this terrible virus".

He lays out some standards on building functions in a predictable way that makes them reusable and less dependent on some outside framework.  He came up with a specification called Node Machine for writing Javascript functions.

> The machine specification is an open standard for Javascript functions. Each machine has a single, clear purpose—whether it be sending an email, translating a text file, or fetching a web page. But whereas traditional functions are supposed to have only a single result, machines are aware of multiple possible outcomes. Machines are self-documenting, quick to implement, and simple to debug.
[[source]](http://node-machine.org/)

So great! We write our drivers as reusable functions. But we can do better.

## Challenge #2: Don't be language dependent, allow any programmer to build drivers in any language.
Writing reusable functions in Javascript is great for me, but maybe not so great for you. Perhaps you're specialty is Python, Ruby, PHP, even Haskell. It turns out that [command-line interfaces (CLI)](http://en.wikipedia.org/wiki/Command-line_interface) using the [Unix shebang](http://en.wikipedia.org/wiki/Shebang_%28Unix%29) to describe what language parser to interpret an executable script as is a great way to wrap functions, the parameters of the CLI are the parameters for the function.

Here is an example of pulling data from a Temper1 temperature sensor and pushing data to a Phant database.
```
  ~ $ ./opk-temper1-cli/pull --port USB_1
  17
  ~ $ ./opk-phant-cli/push --url http://data.sparkfun.com --public_key 438ddke3iJseiI --private_key fei9efenlsi9 --field temperature --value 17
```

Using CLI, a user may never know, or even have to care, what language the command is written in. Since most languages have a way to execute commands like this, we can write controllers in whatever language we wish that do something like run `./opk-temper1-cli/pull` every one minute, take the corresponding output, and use that to run `./opk-phant-cli`. For example, written in Nodejs, that would look like the following.

```javascript
  var sys = require('sys')
  var exec = require('child_process').exec

  setInterval(function() {
    exec('./opk-temper1-cli/pull --port USB_1', function(error, stdout, stderr) {
      exec('./opk-phant-cli/push --url http://data.sparkfun.com --public_key 438ddke3iJseiI --private_key fei9efenlsi9 --field temperature --value ' + stdout)
    })
  }, 60000)
```

Those drivers may be written in python, but javascript doesn't care when it's executing CLI.

## Challenge #3: Do we really need to write controllers?
For the folks who are writing these CLI, do they really need to write controllers? Turns out maybe not. The Unix philosophy was once summarized as follows.

> Write programs that do one thing and do it well. Write programs to work together. Write programs to handle text streams, because that is a universal interface. [[source]](http://en.wikipedia.org/wiki/Unix_philosophy#Doug_McIlroy_on_Unix_programming)

Doug McIlroy, the creator of Unix pipe wrote that. If these sensor and database CLI play nicely with Unix philosophy, they should be able to use pipes to communicate with each other.

For example...
```
~ $ ./opk-temper1-cli/pull --port USB_1 | ./opk-phant-cli/push --url http://data.sparkfun.com --public_key 438ddke3iJseiI --private_key fei9efenlsi9 --field temperature
```

Notice the pipe character between the commands and the lack of a `value` parameter. No `value` parameter is needed because the "standard output" (STDOUT) from `pull` is "piped" as input (STDIN) to the `pull` command. That means instead of scripting a controller to run this every 60 seconds, we can run the old fashioned `watch` command.

```
~ $ watch -n60 "./opk-temper1-cli/pull --port USB_1 | ./opk-phant-cli/push --url http://data.sparkfun.com --public_key 438ddke3iJseiI --private_key fei9efenlsi9 --field temperature"
```

If the sensor drivers play well with pipes, they should also be chainable. Here is an example of two Temper1 temperature sensors plugged into different USB ports all piping to the same database CLI. This works because when data is piped from the first sensor CLI to the second sensor CLI, the second sensor CLI forwards the message along over the next pipe.


```
~ $ watch -n60 "./opk-temper1-cli/pull --port USB_1 | ./opk-temper1-cli/pull --port USB_2 | ./opk-phant-cli/push --url http://data.sparkfun.com --public_key 438ddke3iJseiI --private_key fei9efenlsi9 --field temperature"
```


## Challenge #4: Require command-line interface drivers to explain themselves to GUI
So we've made it this far and it seems the only standard we've really come up with is "follow Unix philosophy". Aren't we going to write a standard?? Yes. We will write a standard and it will be glorious. Well maybe not...
