## MakeFile Command
This project uses MakeFile commands to simplify executing common tasks from the command line. Using MakeFile commands simplifies common tasks like generating bundles, IPAs, and APKs for different environments. It also handles running build runner, tests, and fixes.

Here are the available MakeFile commands:


```sh
# update the dependencies
$ make get
# run build_runner
$ make build
# watch file change
$ make watch
# generate bundle dev flavor
$ make bundle-dev
# generate bundle stg flavor
$ make bundle-stg
# generate bundle prod flavor
$ make bundle-prod
# generate apk dev
$ make apk-dev
# generate apk stg
$ make apk-stg
# generate apk prod
$ make apk-prod
# generate ipa file in dev
$ make ipa-dev
# generate ipa file in stg
$ make ipa-stg
# generate ipa file in prod
$ make ipa-prod
# run for test coverage
$ make test
# generate test coverage report
$ make coverage-report
# fix code based on dart analysis
$ make fix
# check fix
$ make check-fix
```
