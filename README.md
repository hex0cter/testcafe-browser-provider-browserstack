# Background

This repo is used for the purpose of debugging testcafe [issue 3764](https://github.com/DevExpress/testcafe/issues/3764) only.

When running a test in browserstack, there is a chance the test will be put into a queue due to limited parallel tests. When this happens, it is not possible to close the browser properly. It seems the `closeBrowser` function will never be called.

# How to reproduce

1. Prepare some tests written in testcafe, link the dependency of testcafe-browser-provider-browserstack to this updated repo.
2. Prepare a test account in browserstack
3. Run some tests and make sure it browserstack reaches its subscription limitation.
4. Start a new test, for example
```
$ yarn testcafe browserstack:ie my-test.js
yarn run v1.10.1
$ /Users/daniel.han/git/.../node_modules/.bin/testcafe browserstack:ie tests/functional/my-test.js
⠙
Daniel Han >>>>>>>>> init
⠴
Daniel Han >>>>>>>>> isValidBrowserName ie
⠦
Daniel Han >>>>>>>>> openBrowser y_KXdxg http://10.3.222.225:56298/browser/connect/y_KXdxg ie

```

At this monent the new test will be saved into a queue of browserstack (due to a bug in browserstack the size of queue can be infinite, but don't bother about that for now)

5. Kill the test by `killall node`

Normally when the you kill a running job, `closeBrowser` will be called. But in this case, when the job is in the queue, it will not be called. As a result the test will be hanging on browserstack for 30 minutes until the whole session times out.
