These are some basic guidelines for getting started, for contributors. Welcome!

### Setting up development environment

1. `git clone git@github.com:theintern/intern.git`
1. `cd intern`
1. `npm install --production`
1. `ln -s .. node_modules/intern` (so `node_modules/intern` and your `intern` directory are the same)

### Switching between master and geezer

Because these two versions of Intern have slightly different dependencies, the easiest way to switch between the two is as follows:

The first time you switch to geezer:

1. `git checkout geezer`
1. `mv node_modules/dojo node_modules/dojo-2`
1. `npm install dojo`

Subsequent switch geezer->master:

1. `git checkout master`
1. `mv node_modules/dojo node_modules/dojo-1`
1. `mv node_modules/dojo-2 node_modules/dojo`

Subsequent switch master -> geezer:

1. `git checkout geezer`
1. `mv node_modules/dojo node_modules/dojo-2`
1. `mv node_modules/dojo-1 node_modules/dojo`

etc.

(Hey, maybe you can write a script to automate this!)

### Run the self-test before you pull request

In order to run Intern’s self-test suite (`tests/selftest.intern.js`), you will be working with *two copies of Intern*: the copy that you’ve changed and need to test, and a *known good* version of Intern to actually test the copy of Intern that has changed. Unfortunately, due to some [stupid choices](https://github.com/theintern/intern/issues/72), the two versions currently need to be the same, so try to avoid changing Intern in any way that will cause it to silently pass even though it is horribly broken. :) The instructions above point Intern to itself, so when it comes time to run the self-tests, you just do it the same way you would run Intern for any other project:

```bash
SAUCE_USERNAME=… SAUCE_ACCESS_KEY=… node node_modules/runner.js config=tests/selftest.intern
```

Travis-CI will warn you later if you’ve done something dumb, but do you really want to broadcast to the world that you did something dumb?!! Run the self-tests yourself first.

### *Never* merge to master when landing a PR

Whenever landing a PR, always use `git pull https://github.com/cool-person/intern.git branch-name --squash --author="Original Author <foo@example.com>"` instead allowing a merge or fast-forward. **NEVER** allow a merge commit to be introduced to the master branch. **ALWAYS** make sure discrete features are committed in a single commit. Or else.

### *Always* merge to geezer when landing a PR

Whenever merging a PR against the master branch, make sure you *also* commit it to the geezer branch, for as long as that thing exists.

1. `git commit`
1. `git checkout geezer`
1. `git merge master --no-commit`
1. Fix conflicts, if any
1. Fix any getter/setters that need to be changed to use `get`/`set` methods
1. `git commit`