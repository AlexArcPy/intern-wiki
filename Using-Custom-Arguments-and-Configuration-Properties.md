As of Intern 1.6, it is possible to retrieve the current Intern configuration being used to run tests, as well as retrieve custom arguments. This information is available from the main `intern` object:

```js
require([ 'intern' ], function (intern) {
  // arguments object
  intern.args;

  // config object
  intern.config;
});

```