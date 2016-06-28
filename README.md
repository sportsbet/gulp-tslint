gulp-tslint
===========

[![Build Status](https://travis-ci.org/panuhorsmalahti/gulp-tslint.svg?branch=master)](https://travis-ci.org/panuhorsmalahti/gulp-tslint)
[![Dependency Status](https://david-dm.org/panuhorsmalahti/gulp-tslint.svg)](https://david-dm.org/panuhorsmalahti/gulp-tslint)

TypeScript linter plugin for Gulp.


First install gulp-tslint
```shell
npm install --save-dev gulp-tslint
```

##### Peer dependencies

The `tslint` module is a peer dependency of `gulp-tslint`, which allows you to update tslint independently from gulp-tslint.

Usage:
```typescript
// Importing in ES6
import tslint from "gulp-tslint";

// or requiring in ES5
var tslint = require("gulp-tslint");

gulp.task("tslint", () =>
    gulp.src("source.ts")
        .pipe(tslint({
            formatter: "verbose"
        }))
        .pipe(tslint.report())
);
```

Types should work automatically with TypeScript 1.6 or newer when used in TypeScript.

**tslint.json** is attempted to be read from near the input file.
It **must be available** or supplied directly through the options.

Failures generated by TSLint are added to `file.tslint`.

The format in which failures are outputted may be controlled by specifying a TSLint formatter.
The available formatters include:

* "json" prints stringified JSON to console.log.
* "prose" prints short human-readable failures to console.log.
* "verbose" prints longer human-readable failures to console.log.
* "full" is like verbose, but displays full path to the file
* "msbuild" for Visual Studio
* "vso" outputs failures in a format that can be integrated with Visual Studio Online.

Custom [TSLint formatters](https://palantir.github.io/tslint/develop/custom-formatters/) may also be
used by specifying the `formatter` and `formattersDirectory` properties on the options passed to
`gulp-tslint`.

If upgrading to gulp-tslint v6.0.0 or greater, it should be noted that reporters have been removed
in favour of using TSLint formatters directly. If you were previously specifying a reporter in calls
to `.report()`, these should be removed and instead `formatter` should be specified in calls to
`gulp-tslint`.

If there is at least one failure a PluginError is emitted after execution of the reporters:
```javascript
[gulp] Error in plugin 'gulp-tslint': Failed to lint: input.ts
```

You can prevent emiting the error by setting emitError in report options to false.

```javascript
gulp.task("invalid-noemit", () =>
    gulp.src("input.ts")
        .pipe(tslint({
            formatter: "prose"
        }))
        .pipe(tslint.report({
            emitError: false
        }))
);
```

You can summarize the gulp error message to the number of errors by setting summarizeFailureOutput in report options.

```javascript
gulp.task("invalid-noemit", () =>
    gulp.src("input.ts")
        .pipe(tslint({
            formatter: "prose"
        }))
        .pipe(tslint.report({
            summarizeFailureOutput: true
        }))
);
```

tslint.json can be supplied as a parameter by setting the configuration property.
```javascript
gulp.task("tslint-json", () =>
    gulp.src("input.ts")
        .pipe(tslint({
            configuration: {
              rules: {
                "class-name": true,
                // ...
              }
            }
        }))
        .pipe(tslint.report())
);
```

You can also supply a file path to the configuration option, and the file name
doesn't need to be tslint.json.

```javascript
.pipe(tslint({
    // contains rules in the tslint.json format
    configuration: "source/settings.json"
}))
```

Report limits
-------------

You can optionally specify a report limit in the .report options that will turn off reporting for files after the limit has been reached. If the limit is 0 or less, the limit is ignored, which is the default setting.

```javascript
gulp.task("tslint", () =>
    gulp.src(["input.ts",])
        .pipe(tslint({
            formatter: "prose"
        }))
        .pipe(tslint.report({
            reportLimit: 2
        }))
);
```

Specifying the tslint module
----------------------------

If you want to use a different version of tslint, you can supply it with the `tslint` option.

```bash
npm install tslint@next
```

```javascript
.pipe(tslint({
    tslint: require("tslint")
}));
```

All default tslint options
--------------------------

```javascript
const tslintOptions = {
    configuration: {},
    formatter: "prose",
    formattersDirectory: null,
    rulesDirectory: null,
    tslint: null
};
```

All default report options
--------------------------

```javascript
const reportOptions = {
    emitError: true,
    reportLimit: 0,
    summarizeFailureOutput: false
};
```

Development
===========

Fork this repository, run npm install and send pull requests. The project can be build with ``gulp`` command.
