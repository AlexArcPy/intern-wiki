## Since 1.0

* *(enhancement, backwards-incompatible)* The [installation method](https://github.com/theintern/intern#quick-start) has changed from using `git clone` to using `npm install`.

  This means that **Intern will now be installed inside `node_modules` instead of directly within the current directory**. As a result, the default base URL for the loader is now down two levels, instead of down one level.

  See the [running tests documentation](Running-Tests) to see how to use Intern from within `node_modules` (hint: basically the same as before). Older installations of Intern will need to be reinstalled, but no changes to tests should be required. ([#10](https://github.com/theintern/intern/issues/10), [#16](https://github.com/theintern/intern/issues/16), [#45](https://github.com/theintern/intern/issues/45))
* *(enhancement, backwards-incompatible)* [Grunt support](Using-Intern-with-Grunt) has been changed to use `grunt.loadNpmTasks`. ([#22](https://github.com/theintern/intern/issues/22))
* *(enhancement)* `client.js` will now exit with a non-zero status code if a test fails. ([#11](https://github.com/theintern/intern/issues/11))
* *(enhancement)* Sauce Labs credentials will now be pulled from the environment when using Grunt if they are not provided explicitly, just as they would be when using `runner.js` directly. ([#15](https://github.com/theintern/intern/issues/15))
* *(enhancement)* A basic `intern/order!` plugin for loading [non-AMD, non-CommonJS code for testing](Writing-Tests#testing-non-amd-code) has been added to facilitate testing legacy browser JavaScript. ([#30](https://github.com/theintern/intern/issues/30))
* *(bug, geezer)* The ES3 Chai API-compatible assertion library has been updated to fix edge cases in old IE that were not represented in the Chai test suite. ([#27](https://github.com/theintern/intern/issues/27), [#28](https://github.com/theintern/intern/issues/28))