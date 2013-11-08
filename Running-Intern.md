The following arguments can be used when running Intern:

<table>
<tr>
<th scope="col">Argument</th>
<th>Optional?</th>
<th>Description</th>
<th>Environment</th>
</tr>
<tr>
<th scope="row"><code>autoRun</code></th>
<td>Yes</td>
<td>When set to <code>false</code>, tests will not start running automatically after they are all loaded. Otherwise, if <code>true</code> or left undefined, tests will start running automatically.</td>
<td>browser, client cli</td>
</tr>
<tr>
<th scope="row"><code>config</code></th>
<td>No</td>
<td>The module ID of the configuration file that should be used when running Intern. The module ID will be resolved relative to the current working directory on the command-line, and relative to the <code>baseUrl</code> option in the browser.</td>
<td>browser, cli</td>
</tr>
<tr>
<th scope="row"><code>proxyOnly</code></th>
<td>Yes</td>
<td>When using <code>runner.js</code>, the <code>proxyOnly</code> argument may be provided. This causes the runner to start the instrumenting proxy but perform no other work, so you can load instrumented tests by manually navigating to <code>{{proxyUrl}}/__intern/client.html</code>.</td>
<td>runner cli</td>
</tr>
<tr>
<th scope="row"><code>reporters</code></th>
<td>Yes</td>
<td>One or more reporters that should be used in lieu of the reporters listed in the configuration file. To specify multiple reporters, simply provide the <code>reporters</code> argument multiple times.</td>
<td>browser, cli</td>
</tr>
<tr>
<th scope="row"><code>suites</code></th>
<td>Yes</td>
<td>One or more module IDs that should be tested in lieu of the full list of suites in the configuration file. To specify multiple suites, simply provide the <code>suites</code> argument multiple times.</td>
<td>browser, cli</td>
</tr>
</table>

More detailed argument usage examples can be found below.

There are several different ways to actually use Intern, depending upon your needs:

## As a stand-alone browser client

This execution method is useful when you are in the process of writing unit tests that require a browser and you need to quickly check to make sure that they are actually working. It is invoked by navigating directly to `client.html`. The `config` argument should be the module ID of your project’s Intern configuration file (typically `tests/intern`). One or more `suites` and `reporters` arguments may also be used. A typical execution that runs all tests and outputs results to the Web console would look like this:

```text
http://localhost/my-project/node_modules/intern/client.html?config=tests/intern
```

A more complex execution might look like this:

```text
http://localhost/my-project/node_modules/intern/client.html?config=tests/intern&suites=my-package/tests/request
&suites=my-package/tests/animation&reporters=console&reporters=html
```

## As a stand-alone Node.js client

This execution method is useful when you are in the process of writing unit tests that do not require a browser and you want to quickly check to make sure that they are actually working. It is invoked by running `intern-client`. The command-line arguments for `client.js` are identical to the URL arguments for running a stand-alone browser client. A typical execution that runs all tests and outputs results to the console would look like this:

```bash
intern-client config=tests/intern
```

A more complex execution might look like this:

```bash
intern-client config=tests/intern suites=my-package/tests/request \
  suites=my-package/tests/animation reporters=console reporters=lcov
```

Note that when running on Windows, all command-line options must be surrounded by quotes. Also note that the commands above rely on npm being installed and configured properly; if you do not have your environment PATH set properly, you may run `my-package/node_modules/intern/bin/intern-client.js` directly instead.

## As an instrumenting proxy for generating code coverage data

This execution method is useful when you want to generate raw code coverage data for use with Istanbul without needing to set up a browser testing infrastructure. It is invoked by running `node runner.js proxyOnly`. The `config` argument should be the module ID of your project’s Intern configuration file (typically `tests/intern`). The proxy will run indefinitely until you quit using Ctrl+C. An execution of this method would look like this:

```bash
intern-runner config=tests/intern proxyOnly
```

Note that because this method does not run any tests, the `suites` and `reporters` options are not applicable. Also note that the commands above rely on npm being installed and configured properly; if you do not have your environment PATH set properly, you may run `my-package/node_modules/intern/bin/intern-runner.js` directly instead.

## As a test runner for multi-platform testing

This execution method is useful when you want to run tests across all supported environments without needing a separate continuous integration server. It is invoked by running `node runner.js` without the `proxyOnly` argument. One or more `reporters` arguments may also be added to override the reporters specified in the config.

In order to use this method, you will need one of the following:

* A [Sauce Labs](https://saucelabs.com/) account, if you do not want to self-manage your testing infrastructure
* A [Selenium 2 Server](http://docs.seleniumhq.org/), if you only need to run tests on one platform
* A [Selenium 2 Grid](http://code.google.com/p/selenium/wiki/Grid2), if you need to run tests across multiple platforms

This execution method is required for functional testing, as a server is required in order to drive the client browser.

More information on how to configure Intern to work with your testing infrastructure can be found on the [Configuring Intern](Configuring-Intern) page.

A typical execution of this method would look like this:

```bash
intern-runner config=tests/intern
```

A more complex execution would look like this:

```bash
intern-runner config=tests/intern reporters=runner reporters=lcov
```

## As a test runner for continuous integration

This execution method useful when you want to enforce code quality standards across an entire project and ensure that any broken code committed to your canonical project repository is reported immediately.

In order to use this method, in addition to the above, you will need one of the following:

* A [Travis CI](http://travis-ci.org/) account and a GitHub repository
* A custom post-receive (Git) or post-commit (Subversion) hook that executes `node runner.js` as described above and notifies you if it returns a non-zero exit code.

You may also use other third party continuous integration solutions like [Jenkins CI](http://jenkins-ci.org/) or [TeamCity](https://www.jetbrains.com/teamcity/). We are very interested in providing wider support for these solutions, so please submit a pull request if you have created any plugins that enable easy interoperability.

An execution of this method would use the same command-line arguments from the previous section, except the command is executed by your post-commit script or CI software instead.

More information on integration with Travis CI can be found on the [Travis CI Integration](Travis-CI-Integration) page.