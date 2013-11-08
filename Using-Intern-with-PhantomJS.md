While PhantomJS is not a suitable replacement for testing in real browsers, it is often quicker and easier to test with PhantomJS when writing your tests and then let your CI service handle running tests across all platforms.

Using a local PhantomJS installation with Intern is very simple:

1. In your Intern configuration, set `environments: [ { browserName: 'phantom' } ]` and `useSauceConnect: false`
2. Run `phantomjs --webdriver=4444`
3. Run `node runner.js config=path/to/config`

*Note: Due to [PhantomJS issue #10522](https://github.com/ariya/phantomjs/issues/10522), you must either use the `geezer` edition of Intern, apply [PhantomJS PR #11225](https://github.com/ariya/phantomjs/pull/11225), use an alternative AMD loader that supports Safari 5.0 and older, or wait for PhantomJS 2.*