As of Intern 1.6, it is possible to retrieve the current Intern configuration being used to run tests, as well as retrieve custom arguments. This information is available from the main `intern` object:

```js
require([ 'intern' ], function (intern) {
  // arguments object
  intern.args;

  // config object
  intern.config;
});
```

This makes it possible to, for example, define a dynamic proxy URL from the command-line or Grunt task:

```js
// in tests/config.js
define([ 'intern' ], function (intern) {
  return {
    proxyUrl: intern.args.proxyUrl,

    // ... additional configuration ...
  };
});
```

```bash
$ intern-runner config=tests/config proxyUrl=http://www.example.com:1234/
```