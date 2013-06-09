Intern is designed to easily integrate with the [Travis CI](http://travis-ci.org/) continuous integration service. In order to enable Travis CI for your project, you must first create a `.travis.yml` in your repository root that will load and execute Intern. For a typical project using Sauce Labs, it should look like this:

```yaml
language: node_js
node_js:
  - 0.8
env:
  global:
    # Sauce Labs are OK with this and it is currently necessary to expose this information for testing pull requests;
    # please get your own free key if you want to test yourself
    - SAUCE_USERNAME: username
    - SAUCE_ACCESS_KEY: access-key
# This extra install section is only necessary if your project has already installed AMD dependencies like Dojo using
# npm, since AMD path resolution does not follow Node.js path resolution rules but npm does not know this
install:
  - npm install
  - cd node_modules/intern
  - npm install --production
  - cd ../..
script: node node_modules/intern/runner.js config=tests/intern
```

If you are not OK exposing your Sauce Labs username and access key, you may use [secure environment variables](http://about.travis-ci.org/docs/user/build-configuration/#Secure-environment-variables) to encrypt them (`travis encrypt "SAUCE_USERNAME=username SAUCE_ACCESS_KEY=access-key"`). However, this will mean that pull requests are no longer tested.

Once you have a Travis configuration, you just need to actually start the thing:

1. Go to https://travis-ci.org/
2. Click “Sign in with GitHub” at the top-right
3. Allow Travis CI to access your GitHub account
4. Go to https://travis-ci.org/profile
5. Click “Sync now”, if necessary, to list all your GitHub projects
6. Click the “Profile” tab
7. Copy the “Token” value to your clipboard
8. Click the “Repositories” tab
9. Click the on/off switch next to the repository you want to test
10. Click the wrench icon next to the repository you want to test
11. Find Travis in the list of services and click it
12. Paste the token from your clipboard to the Token field
13. Click “Save changes”
14. Click “Test Hook”

If the test hook was successful (it may take several minutes for the test hook to trigger a build), you will be able to watch Intern happily execute all your tests directly from the Travis CI Web site. Any time you make a new commit, or a new pull request is issued, Travis will automatically re-run your test suite. Simple!