Intern allows client.html/client.js to be used with alternative AMD loaders. This is useful in the case that you are using a specific feature that only exists in a particular loader (most commonly, RequireJS’s `shim` functionality, which is normally implemented via [loader plugin](https://github.com/tbranyen/use.js) in other loaders).

To use an alternative loader, simply add a `useLoader` configuration option to your Intern configuration file. Two keys are currently supported, `host-node` and `host-browser`, which correspond to the loader that will be used in Node.js and in the browser, respectively.

`host-node` should be a standard Node.js module ID, like `dojo/dojo`, or `requirejs`. The module will be resolved using the standard Node.js module resolution system.
`host-browser` should be a path to a script file, relative to `client.html`, like `node_modules/dojo/dojo.js`, or `../../bower_components/requirejs/require.js`. The module will be loaded using script injection.

You may omit either key, in which case the default (Dojo) loader will be used instead.

Additional information about `useLoader` is available from the [[Configuring Intern]] documentation.