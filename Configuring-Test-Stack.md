Test Stack configuration occurs through a single AMD module that is specified using the `config` command-line argument. By convention, this module is typically placed at `my-project/test/teststack`, though Test Stack is designed to allow multiple configurations for instances where you want to quickly switch between different test configurations. For an example of a working Test Stack configuration, you may look at the [Test Stack self-test configuration](../blob/master/test/teststack.js).

The following configuration options are available:

<table>
<tr>
<th scope="col">Name<br>Default</th><th scope="col">Description</th>
</tr>

<tr>
<th scope="row"><code>capabilities</code><br>(empty object)</th>
<td>The default desired capabilities for all environments being tested. These capabilities can be overridden per-environment in the <code>environments</code> array. See <a href="https://code.google.com/p/selenium/wiki/DesiredCapabilities">Selenium DesiredCapabilities</a> for standard Selenium capabilities and <a href="https://saucelabs.com/docs/additional-config#desired-capabilities">Sauce Labs Additional Configuration</a> for Sauce Labs capabilities. The <code>build</code> capability will be filled in with the current commit ID from the <code>TRAVIS_COMMIT</code> environment variable automatically, if it exists.</td>
</tr>

<tr>
<th scope="row"><code>environments</code><br>(empty array)</th>
<td>Environments (browsers) to run integration testing against. The same options used in <code>capabilities</code> are used for each environment specified in the array. If arrays are provided for <code>browserName</code>, <code>version</code>, <code>platform</code>, or <code>platformVersion</code>, all possible options will be permutated. For example:

<pre><code>environments: [ {
  browserName: 'chrome',
  version: [ '23', '24' ],
  platform: [ 'Linux', 'Mac OS 10.8' ]
} ]</code></pre>

This will generate 4 environments: Chrome 23 on Linux, Chrome 23 on Mac OS 10.8, Chrome 24 on Linux, and Chrome 24 on Mac OS 10.8. Other capabilities options specified for an environment are not permutated, but are simply used as-is.<br>
<br>
Note that version numbers must be strings if using Sauce Labs.</td>
</tr>

<tr>
<th scope="row"><code>excludeInstrumentation</code><br>(none)</th>
<td>A regular expression that matches the path-part of URLs (starting from the end of <code>proxyUrl</code>, excluding any trailing slash) that should not be instrumented during testing. Use this to exclude dependencies from being reported in your code coverage results. (Test Stack code—that is, anything that loads from <code>{{proxyUrl}}/__teststack/</code>—is always excluded from code coverage results.)<br>
<br>As an example, <code>excludeInstrumentation: /^(?:dojo|jquery|sencha)\//</code> would prevent any code in the <code>dojo</code>, <code>jquery</code>, and <code>sencha</code> packages from being included in code coverage analysis.</td>
</tr>

<tr>
<th scope="row"><code>functionalSuites</code><br>(empty array)</th>
<td>An array of module IDs corresponding to individual functional test suites that you want to execute when running Test Stack. Functional tests are different from unit tests because they are executed on the server, not the client, so are only available when using <code>runner.js</code>.</td>
</tr>

<tr>
<th scope="row"><code>loader</code><br><pre><code>{ baseUrl: teststackDir + '/..',
  packages: [
    { name: 'teststack',
      location: teststackDir },
    { name: 'dojo-ts',
      location: teststackDir + '/dojo' }
  ] }</code></pre></th>
<td>Configuration options for the AMD module loader. Any <a href="https://github.com/amdjs/amdjs-api/wiki/Common-Config">AMD configuration options</a> supported by the Dojo loader can be used here. Modifying <code>baseUrl</code> may break Test Stack. If you are testing an AMD application and need to use stub modules for testing, the <code>map</code> configuration option is the correct way to do this.</td>
</tr>

<tr>
<th scope="row"><code>maxConcurrency</code><br>3</th>
<td>The maximum number of environments that should be tested simultaneously. Set this to <code>Infinity</code> to test in all environments at once. You may want to reduce this if you have a limited number of test machines available, or are using a shared Sauce Labs account.</td>
</tr>

<tr>
<th scope="row"><code>proxyPort</code><br>9000</th>
<td>The port on which the instrumenting proxy will listen for requests.</td>
</tr>

<tr>
<th scope="row"><code>proxyUrl</code><br>http://localhost:9000/</th>
<td>The URL to the proxy. You may decide to change this if you need to run the instrumenting proxy through another reverse proxy, or if your instrumenting proxy needs to be exposed on a public interface that your Selenium servers can access directly.</td>
</tr>

<tr>
<th scope="row"><code>reporters</code><br>'runner' or 'console'</th>
<td>An array of reporter names (for reporters in <code>teststack/lib/reporters</code>) or complete module IDs (for custom reporters) corresponding to reporters that should be used to report test results.</td>
</tr>

<tr>
<th scope="row"><code>suites</code><br>(empty array)</th>
<td>An array of module IDs corresponding to individual unit test suites that you want to execute when running Test Stack. This option can be overridden in most cases by specifying one or more <code>suites</code> options on the command-line.</td>
</tr>

<tr>
<th scope="row"><code>useSauceConnect</code><br>true</th>
<td>Whether or not to start Sauce Connect before running tests. This is necessary if you are using Travis CI. For quicker Test Stack runtimes, you may start Sauce Connect manually instead and set this to <code>false</code>.</td>
</tr>

<tr>
<th scope="row"><code>webdriver</code><br><pre><code>{ host: 'localhost',
  port: 4444 }</code></pre></th>
<td>Connection information for the remote WebDriver (a.k.a. Selenium 2) service. Available options are <code>host</code>, <code>port</code>, <code>username</code>, and <code>accessKey</code>.<br>
<br>
For Sauce Labs without Sauce Connect, the host should be <code>ondemand.saucelabs.com</code>.<br>
For Sauce Connect, the host should be <code>localhost</code>. (Note that by default, Sauce Connect starts on port <strong>4445</strong> if it is not started by Test Stack with <code>useSauceConnect</code>.)<br>
For custom Selenium 2 or Selenium 2 Grid servers, the <code>host</code> should be the address of your server.<br>
<br>
<code>username</code> and <code>accessKey</code> are used by Sauce Labs only. If you do not want to expose your username and access key, put them in the <code>SAUCE_USERNAME</code> and <code>SAUCE_ACCESS_KEY</code> environment variables instead.</td>
</tr>
</table>