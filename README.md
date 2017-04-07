# api documentation for  [gulp-tslint (v7.1.0)](https://github.com/panuhorsmalahti/gulp-tslint)  [![npm package](https://img.shields.io/npm/v/npmdoc-gulp-tslint.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-gulp-tslint) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-gulp-tslint.svg)](https://travis-ci.org/npmdoc/node-npmdoc-gulp-tslint)
#### TypeScript linter Gulp plugin

[![NPM](https://nodei.co/npm/gulp-tslint.png?downloads=true)](https://www.npmjs.com/package/gulp-tslint)

[![apidoc](https://npmdoc.github.io/node-npmdoc-gulp-tslint/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-gulp-tslint_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-gulp-tslint/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-gulp-tslint/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-gulp-tslint/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "analyze": true,
    "author": {
        "name": "Panu Horsmalahti",
        "email": "panu.horsmalahti@iki.fi"
    },
    "bugs": {
        "url": "https://github.com/panuhorsmalahti/gulp-tslint/issues"
    },
    "contributors": [
        {
            "name": "Panu Horsmalahti",
            "email": "panu.horsmalahti@iki.fi"
        }
    ],
    "dependencies": {
        "gulp-util": "~3.0.8",
        "map-stream": "~0.1.0",
        "through": "~2.3.8"
    },
    "description": "TypeScript linter Gulp plugin",
    "devDependencies": {
        "gulp": "^3.9.1",
        "gulp-concat": "^2.6.1",
        "gulp-typescript": "^3.1.4",
        "tslint": "^4",
        "typescript": "^2"
    },
    "directories": {},
    "dist": {
        "shasum": "9bd3ff4fbc16d4cbd9abb08ff786db89b563e93d",
        "tarball": "https://registry.npmjs.org/gulp-tslint/-/gulp-tslint-7.1.0.tgz"
    },
    "engines": {
        "node": ">= 4"
    },
    "gitHead": "59db005d295582aa5f373c23abe0613eede7c315",
    "homepage": "https://github.com/panuhorsmalahti/gulp-tslint",
    "keywords": [
        "gulp",
        "typescript",
        "plugin",
        "ts",
        "gulpplugin",
        "gulpfriendly",
        "tslint",
        "linter",
        "lint"
    ],
    "license": "MIT",
    "main": "index.js",
    "maintainers": [
        {
            "name": "nawitus",
            "email": "panu.horsmalahti@iki.fi"
        }
    ],
    "name": "gulp-tslint",
    "optionalDependencies": {},
    "peerDependencies": {
        "tslint": ">=4.0.0-dev"
    },
    "preferGlobal": false,
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git+https://github.com/panuhorsmalahti/gulp-tslint.git"
    },
    "scripts": {},
    "typings": "index",
    "version": "7.1.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module gulp-tslint](#apidoc.module.gulp-tslint)
1.  [function <span class="apidocSignatureSpan">gulp-tslint.</span>default (pluginOptions)](#apidoc.element.gulp-tslint.default)
1.  [function <span class="apidocSignatureSpan">gulp-tslint.</span>report (options)](#apidoc.element.gulp-tslint.report)



# <a name="apidoc.module.gulp-tslint"></a>[module gulp-tslint](#apidoc.module.gulp-tslint)

#### <a name="apidoc.element.gulp-tslint.default"></a>[function <span class="apidocSignatureSpan">gulp-tslint.</span>default (pluginOptions)](#apidoc.element.gulp-tslint.default)
- description and source-code
```javascript
default = function (pluginOptions) {
    var loader;
    var tslint;
    // If user options are undefined, set an empty options object
    if (!pluginOptions) {
        pluginOptions = {};
    }
    return map(function (file, cb) {
        // Skip
        if (file.isNull()) {
            return cb(null, file);
        }
        // Stream is not supported
        if (file.isStream()) {
            return cb(new PluginError("gulp-tslint", "Streaming not supported"));
        }
        // TSLint default options
        var options = {
            fix: pluginOptions.fix || false,
            formatter: pluginOptions.formatter || "prose",
            formattersDirectory: pluginOptions.formattersDirectory || null,
            rulesDirectory: pluginOptions.rulesDirectory || null
        };
        var linter = getTslint(pluginOptions);
        if (pluginOptions.configuration === null ||
            pluginOptions.configuration === undefined ||
            isString(pluginOptions.configuration)) {
            // configuration can be a file path or null, if it's unknown
            pluginOptions.configuration = linter.Configuration.findConfiguration(pluginOptions.configuration || null, file.path).
results;
        }
        tslint = new linter.Linter(options, pluginOptions.program);
        tslint.lint(file.path, file.contents.toString("utf8"), pluginOptions.configuration);
        file.tslint = tslint.getResult();
        // Pass file
        cb(null, file);
    });
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.gulp-tslint.report"></a>[function <span class="apidocSignatureSpan">gulp-tslint.</span>report (options)](#apidoc.element.gulp-tslint.report)
- description and source-code
```javascript
report = function (options) {
    // Notify the user that the old interface is used, this can be removed at some point
    if (isString(options)) {
        throw new Error("Deprecated interface used! See 6.0.0 changelog " +
            "https://github.com/panuhorsmalahti/gulp-tslint/blob/master/CHANGELOG.md");
    }
    // Default options
    if (!options) {
        options = {};
    }
    if (options.emitError === undefined) {
        options.emitError = true;
    }
    if (options.reportLimit === undefined) {
        // 0 or less is unlimited
        options.reportLimit = 0;
    }
    if (options.summarizeFailureOutput === undefined) {
        options.summarizeFailureOutput = false;
    }
    // Collect all files with errors
    var errorFiles = [];
    // Collect all failures
    var allFailures = [];
    // Track how many errors have been reported
    var totalReported = 0;
    // Log formatted output for each file individually
    var reportFailures = function (file) {
        if (file.tslint) {
            var failureCount = file.tslint.failureCount;
            if (failureCount > 0) {
                errorFiles.push(file);
                Array.prototype.push.apply(allFailures, file.tslint.failures);
                if (options.reportLimit <= 0 || (options.reportLimit && options.reportLimit > totalReported)) {
                    if (file.tslint.output !== undefined) {
                        console.log(file.tslint.output);
                    }
                    totalReported += failureCount;
                    if (options.reportLimit > 0 &&
                        options.reportLimit <= totalReported) {
                        log("More than " + options.reportLimit
                            + " failures reported. Turning off reporter.");
                    }
                }
            }
        }
        // Pass file
        this.emit("data", file);
    };
    /**
     * After reporting on all files, throw the error.
     */
    var throwErrors = function () {
        // Throw error
        if (options && errorFiles.length > 0) {
            var failuresToOutput = allFailures;
            var ignoreFailureCount = 0;
            // If error count is limited, calculate number of errors not shown and slice reportLimit
            // number of errors to be included in the error.
            if (options.reportLimit > 0) {
                ignoreFailureCount = allFailures.length - options.reportLimit;
                failuresToOutput = allFailures.slice(0, options.reportLimit);
            }
            // Always use the proseErrorFormat for the error.
            var failureOutput = failuresToOutput.map(function (failure) {
                return proseErrorFormat(failure);
            }).join(", ");
            var errorOutput = "Failed to lint: ";
            if (options.summarizeFailureOutput) {
                errorOutput += failuresToOutput.length + " errors.";
            }
            else {
                errorOutput += failureOutput + ".";
            }
            if (ignoreFailureCount > 0) {
                errorOutput += " (" + ignoreFailureCount
                    + " other errors not shown.)";
            }
            if (options.emitError === true) {
                return this.emit("error", new PluginError("gulp-tslint", errorOutput));
            }
            else if (options.summarizeFailureOutput) {
                log(errorOutput);
            }
        }
        // Notify through that we're done
        this.emit("end");
    };
    return through(reportFailures, throwErrors);
}
```
- example usage
```shell
...
var tslint = require("gulp-tslint");

gulp.task("tslint", () =>
    gulp.src("source.ts")
        .pipe(tslint({
            formatter: "verbose"
        }))
        .pipe(tslint.report())
);
'''

Types should work automatically.

**tslint.json** is attempted to be read from near the input file.
It **must be available** or supplied directly through the options.
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
