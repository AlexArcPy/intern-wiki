Grunt support is built into Intern. Install Intern and load the Grunt task into your `Gruntfile` using `grunt.loadTasks('path/to/intern/grunt');` (Intern 1.0.0) or `grunt.loadNpmTasks('intern')` (Intern 1.1+).

An example of the Grunt Intern task is available in the [intern-examples repository](https://github.com/theintern/intern-examples/tree/master/grunt-example).

## Task Options

Options available when running Intern using Grunt are the same as the [options available when running Intern directly from the command-line](Running-Tests), plus the following additional options:

<table>
<tr>
<th scope="col">Name<br>Default</th>
<th scope="col">Description</th>
</tr>

<tr>
<th scope="row"><code>runType</code><br>client</th>
<td>The execution mode in which Intern should be run. This may be either <code>"runner"</code> for the automated test runner, or <code>"client"</code> for the Node.js client.</td>
</tr>

<tr>
<th scope="row"><code>sauceUsername</code><br>(none)</th>
<td>The username for authentication with Sauce Labs.</td>
</tr>

<tr>
<th scope="row"><code>sauceAccessKey</code><br>(none)</th>
<td>The access key for authentication with Sauce Labs.</td>
</tr>
</table>

## Gruntfile example

```js
grunt.initConfig({
    intern: {
        someReleaseTarget: {
            options: {
                runType: 'runner',  // defaults to client,
                config: 'myPackage/tests/intern',
                reporters: [ 'console', 'lcov' ],
                suites: [ 'myPackage/tests/all' ]
            }
        },
        anotherReleaseTarget: { /* ... */ }
    }
});

// Load the Intern task
grunt.loadNpmTasks('intern')

// Register a test task that uses Intern
grunt.registerTask('test', [ 'intern' ]);

// By default we just test
grunt.registerTask('default', [ 'test' ]);
```