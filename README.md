# ember-cli-code-coverage [![Build Status](https://travis-ci.org/kategengler/ember-cli-code-coverage.svg?branch=master)](https://travis-ci.org/kategengler/ember-cli-code-coverage)

Code coverage using [Istanbul](https://github.com/gotwarlost/istanbul) for Ember apps.

## Requirements
* If using Mocha, Testem `>= 1.6.0` for which you need ember-cli `> 2.4.3`
* If using Mirage you need `ember-cli-mirage >= 0.1.13`
* If using Pretender you need `pretender >= 0.11.0`


## Installation

* `ember install ember-cli-code-coverage`

## Usage

Coverage will only be generated when an environment variable is true (by default `COVERAGE`) and running your test command like normal.

For example:

`COVERAGE=true ember test`

When running with `parallel` set to true, the final reports can be merged by using `ember coverage-merge`. The final merged output will be stored in the `coverageFolder`.

## Configuration

Configuration is optional. It should be put in a file at `config/coverage.js`.

#### Options

- `coverageEnvVar`: Defaults to `COVERAGE`. This is the environment variable that when set will cause coverage metrics to be generated.

- `reporters`: Defaults to `['lcov', 'html']`. The `json-summary` reporter will be added to anything set here, it is required. This can be any [reporters supported by Istanbul](https://github.com/gotwarlost/istanbul/tree/master/lib/report).

- `excludes`: Defaults to `['*/mirage/**/*']`. An array of globs to exclude from instrumentation. Useful to exclude files from coverage statistics.

- `coverageFolder`: Defaults to `coverage`. A folder relative to the root of your project to store coverage results.

- `useBabelInstrumenter`: Defaults to `false`. Whether or not to use Babel instrumenter instead of default instrumenter. The Babel instrumenter is useful when you are using features of ESNext as it uses your Babel configuration defined in `ember-cli-build.js`.

- `parallel`: Defaults to `false`. Should be set to true if parallel testing is being used, for example when using [ember-exam](https://github.com/trentmwillis/ember-exam) with the `--parallel` flag. This will generate the coverage reports in directories suffixed with `_<random_string>` to avoid overwriting other threads reports. These reports can be joined by using the `ember coverage-merge` command.

## Using when intercepting all ajax requests in tests

Aka when using ember-cli-mirage or Pretender. You may require a version of Pretender that includes [this fix](https://github.com/pretenderjs/pretender/pull/130).

To work, this addon has to post coverage results back to a middleware at `/write-coverage`.

```
// in mirage/config.js

  this.passthrough('/write-coverage');
  this.namespace = 'api';  // It's important that the passthrough for coverage is before the namespace, otherwise it will be prefixed.
```

## Inspiration

This addon was inspired by [`ember-cli-blanket`](https://github.com/sglanzer/ember-cli-blanket).
The primary differences are that this addon uses Istanbul rather than Blanket for coverage and it instruments your application code as part of the build, when enabled.
