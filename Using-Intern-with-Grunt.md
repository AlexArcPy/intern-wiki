Grunt support is built into Intern. Install Intern and load the Grunt task into your <code>Gruntfile</code> using <code>grunt.loadTasks('path/to/intern/grunt');</code>.

An example of the Grunt Intern task is available in the [intern-examples repository](https://github.com/theintern/intern-examples/tree/master/grunt-example).

## Task Options

<table>
<tr>
<th scope="col">Name<br>Default</th>
<th scope="col">Description</th>
</tr>

<tr>
<th scope="row"><code>runType</code><br>client</th>
<td>Example: <code>"runner"</code> or <code>"client"</code></td>
</tr>

<tr>
<th scope="row"><code>config</code><br></th>
<td>Example: <code>"intern-selftest/tests/selftest.intern.js"</code></td>
</tr>

<tr>
<th scope="row"><code>reporters</code><br></th>
<td>Example: <code>['console', 'lcov']</code></td>
</tr>

<tr>
<th scope="row"><code>suites</code><br></th>
<td>Example: <code>['intern-selftest/tests/all']</code></td>
</tr>

<tr>
<th scope="row"><code>proxyOnly</code><br></th>
<td>Example: <code>true</code></td>
</tr>

<tr>
<th scope="row"><code>autoRun</code><br></th>
<td>Example: <code>true</code></td>
</tr>

</table>

## Configuration Examples

### All Task Options

```js
grunt.initConfig({
    intern: {
        someReleaseTarget: {
            runType: 'runner',  // defaults to client,
            options: {
                config: "intern-selftest/tests/selftest.intern.js",
                reporters: ['console', 'lcov'],
                suites: ['intern-selftest/tests/all'],
                proxyOnly: true,
                autoRun: true
            }
        },
        anotherReleaseTarget: { ... }
    }
});
```

### Full example

```js
grunt.initConfig({
    intern: {
        local: {
	    options: {
		teststackDir: './intern',
		config: 'tests/intern.js'
            }
	}
    }
});
// Load the Intern task
grunt.loadTasks('./intern/grunt');

// Register a test task
grunt.registerTask('test', ['intern']);

// By default we just test
grunt.registerTask('default', ['test']);
```