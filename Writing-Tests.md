When writing JavaScript tests, it is common to write them according to a specific testing API that defines Suites, Tests, and other aspects of testing infrastructure. These testing APIs are known as **test interfaces**. Intern currently comes with support for 3 different test interfaces: TDD, BDD, and object. Internally, all interfaces generate the same testing structures, so you can use whichever interface you feel matches your preference and coding style. Examples of each tests using each of these interfaces can be found [below](#example-tests).

## Assertions
A test needs a way to verify some logic about the target being tested, such as whether or not a given variable is truthy. This is known as an **assertion**, and forms the basis for software testing. Intern supports extensible assertions via the Chai Assertion Library. The various assertion interfaces are exposed via the following modules, and should be required and used in your tests:
* `intern/chai!assert`
* `intern/chai!expect`
* `intern/chai!should`

## Functional Testing
In addition to regular unit tests, Intern supports a type of testing that can simulate user interaction with DOM elements, known as **functional testing**. Functional tests are slightly different from normal unit tests because they are executed remotely from the test runner,
whereas unit tests are executed directly on the browser under test. In a functional test, a `remote` object is exposed that has methods for interacting with a remote browser environment. The general flow of a functional test should be as follows:

### 1. Load an html page into the remote context.
Because the actual test code isn't exposed to this remote client at all, this html page should <u>include script tags for all necessary JavaScript</u>. Note that if the functional test needs to explicitly wait for certain widgets or elements on this html page to be rendered (or some other condition) before proceeding, the <code>waitForCondition</code> method can be used. This method waits until a global variable becomes truthy before continuing with execution, and errors out if an optional timeout is exceeded.
<pre>
this.get('remote')
	.get(require.toUrl('./SomeTest.html'))
	.waitForCondition('ready', 5000);
</pre>

### 2. Use the methods available on the <code>remote</code> object to interact with the remote context.
The <code>remote</code> object corresponds to the standard <a href="http://www.w3.org/TR/webdriver/">WebDriver API</a> with a fluid, promises-wrapped <a href="https://github.com/admc/wd">WD.js</a>. See <a href="https://github.com/admc/wd#supported-methods">this link</a> for all methods available for functional testing.
<pre>
this.get('remote')
	.get(require.toUrl('./fixture.html'))
	.elementById('operation')
		.click()
		.type('hello, world')
	.end()
</pre>

### 3. Make assertions just like regular unit testing.</strong>
Just like unit tests, functional tests support extensible assertions via the Chai Assertion Library. The various Chai interfaces are exposed via the <code>intern/chai!assert</code>, <code>intern/chai!expect</code>, and <code>intern/chai!should</code> modules. See the <a href="http://chaijs.com/api/">full Chai API documentation</a> for more information.
<pre>
this.get('remote')
	.waitForElementById('result')
	.text()
	.then(function (resultText) {
		assert.equal(resultText, 'hello world', 'When form is submitted, operation should complete successfully');
	});
</pre>

See the [full Chai API documentation](http://chaijs.com/api/) for more information on each module.

## Testing non-AMD code

*New in Version 1.1*

If you are attempting to test non-AMD code that is split across multiple JavaScript files which must be loaded in a specific order, use the `intern/order` plugin instead of specifying those files as direct dependencies in order to ensure they load correctly:

```js
define([
	'intern!object',
	'intern/chai!assert',
	'intern/order!../jquery.js',
	'intern/order!../plugin.jquery.js'
], function (registerSuite, assert) {
	registerSuite({
		name: 'plugin.jquery.js',

		'basic tests': function () {
			jQuery('<div>').plugin();
			// ...
		}
	});
});
```

(Of course, it is strongly recommended that you upgrade your code to use AMD so that this is not necessary.)

## Example Tests

### BDD
```js
define([
	'intern!bdd',
	'intern/chai!expect',
	'../Request'
], function (bdd, expect, Request) {
	with (bdd) {
		describe('demo', function () {
			var request,
				url = 'https://github.com/theintern/intern';

			// before the suite starts
			before(function () {
				request = new Request();
			});

			// before each test executes
			beforeEach(function () {
				request.reset();
			});

			// after the suite is done
			after(function () {
				request.cleanup();
			});

			// multiple methods can be registered and will be executed in order of registration
			after(function () {
				if (!request.cleaned) {
					throw new Error('Request should have been cleaned up after suite execution.');
				}

				// these methods can be made asynchronous as well by returning a promise
			});

			// asynchronous test for Promises/A-based interfaces
			it('should demonstrate a Promises/A-based asynchronous test', function () {
				// `getUrl` returns a promise
				return request.getUrl(url).then(function (result) {
					expect(result.url).to.equal(url);
					expect(result.data.indexOf('next-generation') > -1).to.be.true;
				});
			});

			// asynchronous test for callback-based interfaces
			it('should demonstrate a callback-based asynchronous test', function () {
				// test will time out after 1 second
				var dfd = this.async(1000);

				// dfd.callback resolves the promise as long as no errors are thrown from within the callback function
				request.getUrlCallback(url, dfd.callback(function () {
					expect(result.url).to.equal(url);
					expect(result.data.indexOf('next-generation') > -1).to.be.true;
				});

				// no need to return the promise; calling `async` makes the test async
			});

			// nested suites work too
			describe('xhr', function () {
				// synchronous test
				it('should run a synchronous test', function () {
					expect(request.xhr).to.exist;
				});
			});
		});
	}
});
```

###TDD
```js
define([
	'intern!tdd',
	'intern/chai!assert',
	'../Request'
], function (tdd, assert, Request) {
	with (tdd) {
		suite('demo', function () {
			var request,
				url = 'https://github.com/theintern/intern';

			// before the suite starts
			before(function () {
				request = new Request();
			});

			// before each test executes
			beforeEach(function () {
				request.reset();
			});

			// after the suite is done
			after(function () {
				request.cleanup();
			});

			// multiple methods can be registered and will be executed in order of registration
			after(function () {
				if (!request.cleaned) {
					throw new Error('Request should have been cleaned up after suite execution.');
				}

				// these methods can be made asynchronous as well by returning a promise
			});

			// asynchronous test for Promises/A-based interfaces
			test('#getUrl (async)', function () {
				// `getUrl` returns a promise
				return request.getUrl(url).then(function (result) {
					assert.equal(result.url, url, 'Result URL should be requested URL');
					assert.isTrue(result.data.indexOf('next-generation') > -1, 'Result data should contain term "next-generation"');
				});
			});

			// asynchronous test for callback-based interfaces
			test('#getUrlCallback (async)', function () {
				// test will time out after 1 second
				var dfd = this.async(1000);

				// dfd.callback resolves the promise as long as no errors are thrown from within the callback function
				request.getUrlCallback(url, dfd.callback(function () {
					assert.equal(result.url, url, 'Result URL should be requested URL');
					assert.isTrue(result.data.indexOf('next-generation') > -1, 'Result data should contain term "next-generation"');
				});

				// no need to return the promise; calling `async` makes the test async
			});

			// nested suites work too
			suite('xhr', function () {
				// synchronous test
				test('sanity check', function () {
					assert.ok(request.xhr, 'XHR interface should exist on `xhr` property');
				});
			});
		});
	}
});
```

###Object
```js
define([
	'intern!object',
	'intern/chai!assert',
	'../Request'
], function (registerSuite, assert, Request) {
	var request,
		url = 'https://github.com/theintern/intern';

	registerSuite({
		name: 'demo',

		// before the suite starts
		setup: function () {
			request = new Request();
		},

		// before each test executes
		beforeEach: function () {
			request.reset();
		},

		// after the suite is done
		teardown: function () {
			request.cleanup();

			if (!request.cleaned) {
				throw new Error('Request should have been cleaned up after suite execution.');
			}
		},

		// asynchronous test for Promises/A-based interfaces
		'#getUrl (async)': function () {
			// `getUrl` returns a promise
			return request.getUrl(url).then(function (result) {
				assert.equal(result.url, url, 'Result URL should be requested URL');
				assert.isTrue(result.data.indexOf('next-generation') > -1, 'Result data should contain term "next-generation"');
			});
		},

		// asynchronous test for callback-based interfaces
		'#getUrlCallback (async)': function () {
			// test will time out after 1 second
			var dfd = this.async(1000);

			// dfd.callback resolves the promise as long as no errors are thrown from within the callback function
			request.getUrlCallback(url, dfd.callback(function () {
				assert.equal(result.url, url, 'Result URL should be requested URL');
				assert.isTrue(result.data.indexOf('next-generation') > -1, 'Result data should contain term "next-generation"');
			});

			// no need to return the promise; calling `async` makes the test async
		},

		// nested suites work too
		'xhr': {
			// synchronous test
			'sanity check': function () {
				assert.ok(request.xhr, 'XHR interface should exist on `xhr` property');
			}
		}
	});
});
```

###Functional
```js
define([
	'intern!object',
	'intern/chai!assert',
	'../Request',
	'require'
], function (registerSuite, assert, Request, require) {
	var request,
		url = 'https://github.com/theintern/intern';

	registerSuite({
		name: 'demo',

		'submit form': function () {
			return this.remote
				.get(require.toUrl('./fixture.html'))
				.elementById('operation')
					.click()
					.type('hello, world')
				.end()
				.elementById('submit')
					.click()
				.end()
				.waitForElementById('result')
				.text()
				.then(function (resultText) {
					assert.ok(resultText.indexOf('"hello, world" completed successfully') > -1, 'When form is submitted, operation should complete successfully');
				});
		}
	});
});
```