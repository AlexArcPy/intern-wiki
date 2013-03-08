Running Test Stack is straightforward. The only real requirement is that the package you are testing should be a sibling of the Test Stack package. There are several different ways to actually use Test Stack, depending upon your needs:

# As a stand-alone browser client

This execution method is useful when you are in the process of writing unit tests that require a browser and you need to quickly check to make sure that they are actually working. It is invoked by navigating directly to `client.html`. The `config` argument should be the module ID of your project’s Test Stack configuration file (typically `project-name/test/teststack`). One or more `suites` and `reporters` arguments may also be used. A typical execution that runs all tests and outputs results to the Web console would look like this:

```text
http://localhost/teststack/client.html?config=my-package/test/teststack
```

A more complex execution might look like this:

```text
http://localhost/teststack/client.html?config=my-package/test/teststack&suites=my-package/test/request
&suites=my-package/test/animation&reporters=console&reporters=html
```

# As a stand-alone Node.js client

This execution method is useful when you are in the process of writing unit tests that do not require a browser and you want to quickly check to make sure that they are actually working. It is invoked by running `node client.js`. The command-line arguments for `client.js` are identical to the URL arguments for running a stand-alone browser client. A typical execution that runs all tests and outputs results to the console would look like this:

```bash
node client.js config=my-package/test/teststack
```

A more complex execution might look like this:

```bash
node client.js config=my-package/test/teststack suites=my-package/test/request \
  suites=my-package/test/animation reporters=console reporters=lcov
```

Note that when running on Windows, all command-line options must be surrounded by quotes.

# As an instrumenting proxy for generating code coverage data

This execution method is useful when you want to generate raw code coverage data for use with Istanbul without needing to set up a browser testing infrastructure. It is invoked by running `node runner.js proxyOnly`. The `config` argument should be the module ID of your project’s Test Stack configuration file (typically `project-name/test/teststack`). The proxy will run indefinitely until you quit using Ctrl+C. An execution of this method would look like this:

```bash
node runner.js config=my-package/test/teststack proxyOnly
```

Note that because this method does not run any tests, the `suites` and `reporters` options are not applicable.

# As a test runner for multi-platform testing

This execution method is useful when you want to run tests across all supported environments without needing a separate continuous integration server. It is invoked by running `node runner.js` without the `proxyOnly` argument. One or more `reporters` arguments may also be added to override the reporters specified in the config.

In order to use this method, you will need one of the following:

* A [Sauce Labs](https://saucelabs.com/) account, if you do not want to self-manage your testing infrastructure
* A [Selenium 2 Server](http://docs.seleniumhq.org/), if you only need to run tests on one platform
* A [Selenium 2 Grid](http://code.google.com/p/selenium/wiki/Grid2), if you need to run tests across multiple platforms

This execution method is required for functional testing, as a server is required in order to drive the client browser.

More information on how to configure Test Stack to work with your testing infrastructure can be found on the [Configuring Test Stack](Configuring-Test-Stack) page.

A typical execution of this method would look like this:

```bash
node runner.js config=my-package/test/teststack
```

A more complex execution would look like this:

```bash
node runner.js config=my-package/test/teststack reporters=runner reporters=lcov
```

# As a test runner for continuous integration

This execution method useful when you want to enforce code quality standards across an entire project and ensure that any broken code committed to your canonical project repository is reported immediately.

In order to use this method, in addition to the above, you will need one of the following:

* A [Travis CI](http://travis-ci.org/) account and a GitHub repository
* A custom post-receive (Git) or post-commit (Subversion) hook that executes `node runner.js` as described above and notifies you if it returns a non-zero exit code.

You may also use other third party continuous integration solutions like [Jenkins CI](http://jenkins-ci.org/) or [TeamCity](https://www.jetbrains.com/teamcity/). We are very interested in providing wider support for these solutions, so please submit a pull request if you have created any plugins that enable easy interoperability.

An execution of this method would use the same command-line arguments from the previous section, except the command is executed by your post-commit script or CI software instead.

More information on integration with Travis CI can be found on the TODO Travis CI wiki page.