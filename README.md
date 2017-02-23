# fake-mocha

[![Greenkeeper badge](https://badges.greenkeeper.io/TechnologyAdvice/fake-mocha.svg)](https://greenkeeper.io/)
Run arbitrary commands instead of mocha to integrate with popular IDEs

## Install

    npm install --save-dev fake-mocha

## What? Why?
If you're developing for a production platform, chances are you're either
currently running your tests in a production-like virtual machine, container,
or remote dev server, or you should be.

Popular IDEs such as those in the JetBrains family -- IntelliJ IDEA, WebStorm,
PHPStorm, PyCharm, etc., have excellent mocha integration for Node.js projects,
provided by the optional NodeJS plugin. It provides a fantastic interface for
running and exploring the results of tests, jumping right to errors or test
definitions when they're clicked on.

The problem is, that mocha integration is local-only. There's no way to
configure a remote run in a Docker container or a Vagrant VM, and so you're
sunk if your tests require a local database, a specific version of Node.js, or
some other production-like dependency.

Using fake-mocha, you can configure the mocha integration to look into
`node_modules/fake-mocha` instead of `node_modules/mocha`, and in the
"Extra Mocha options" section, tell it to execute your own test script. For
example, if you'd prefer mocha to run `make test`, just set those extra
options to `--command "make test"`. Fake-mocha will ignore all the other
arguments and just run the command you gave it. It will also set the working
directory to your project root, pass through stdio, and exit with the same exit
status code as the command it executed.

## Getting the mocha integration working
Running a different command is only half the battle, and that's what fake-mocha
solves for you. It's up to you to invoke _actual_ mocha in your dev
environment, and pass it the IntelliJ mocha reporter provided by the IDE
plugin. See the `examples` folder for a Makefile that will do exactly that
to run your tests in the official 'node' Docker container.

## License
fake-mocha is licensed under the MIT license. Please see `LICENSE.txt` for full
details.

## Credits
fake-mocha was created in a flurry of frustration at
[TechnologyAdvice](http://technologyadvice.com).
