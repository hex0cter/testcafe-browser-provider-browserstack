# Why

This repo is used for debugging purpose only.

When running a test in browserstack, there is a chance the test will be put into a queue due to limited parallel test. When this happens, it is not possible to close the browser properly. It seems the `closeBrowser` function is not called.

# How to reproduce

1. Prepare some tests written in testcafe, link the dependency of testcafe-browser-provider-browserstack to this updated repo.
2. Prepare an test account in browserstack
3. Run some tests and make sure it reaches the limit of your browserstack's subscription.
4. Start a new test, for example
```
$ yarn testcafe browserstack:ie tests/functional/pay-later-gb.js
yarn run v1.10.1
$ /Users/daniel.han/git/pay-later-slice-it/packages/pay-later/node_modules/.bin/testcafe browserstack:ie tests/functional/pay-later-gb.js
⠙
Daniel Han >>>>>>>>> init
⠴
Daniel Han >>>>>>>>> isValidBrowserName ie
⠦
Daniel Han >>>>>>>>> openBrowser y_KXdxg http://10.3.222.225:56298/browser/connect/y_KXdxg ie

```

At this monent the new test will be saved into a queue of browserstack (due to a bug in browserstack the size of queue can be infinite, but don't bother about for now)

5. Kill the test by `killall node`

Normally the you kill a running job, `closeBrowser` will be called. But in this case, when the job is in the queue, it will not be called. As a result the test will be on browserstack for 30 minutes until the whole session times out.
