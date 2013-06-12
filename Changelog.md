# Since 1.0

* The installation method has changed from using `git clone` to using `npm install`. This means that Intern will now be installed inside `node_modules` instead of directly within the current directory. As a result, the default base URL is now down two levels, instead of down one level. Older installations of Intern will need to be reinstalled. (#10, #16, #45)
* `client.js` will now exit with a non-zero status code if a test failed. (#11)
* Sauce Labs credentials will now be pulled from the environment when using Grunt if they are not provided explicitly. (#15)
* [Grunt support](Using-Intern-with-Grunt) has been enhanced to work with `grunt.loadNpmTasks`. (#22)
* A basic `intern/order!` plugin for loading non-AMD, non-CommonJS code in order has been added to facilitate testing legacy browser JavaScript. (#30)
* (geezer) The ES3 Chai API-compatible assertion library has been updated to fix edge cases in old IE that were not represented in the Chai test suite. (#27, #28)