## Versioning

Providing the flexibility to reversion the API or UPDATE version slots as
features or bugfixes arise and using ExifTool's version number is at odds with
eachother, so this library follows [SemVer](http://semver.org/), and the
vendored versions of ExifTool match the version they vendor.

### The `MAJOR` or `API` version is incremented for

* 💔 Non-backward-compatible API changes
* 🌲 New releases of ExifTool that have externally visible changes

### The `MINOR` or `UPDATE` version is incremented for

* 🌱 New releases of ExifTool with no externally visible changes 
* ✨ Backwards-compatible features

### The `PATCH` version is incremented for

* 🐞 Backwards-compatible bug fixes
* 📦 Minor packaging changes

## Changelog

### v3.1.1

* 🐞 Fixed `package.json` references to `types` and `main`

### v3.1.0

* 📦 Completed jsdocs for ExifTool constructor
* 📦 Pulled in batch-cluster v1.1.0 that adds both `on("beforeExit")` and
  `on("exit")` handlers

### v3.0.0

* ✨ Extracted [batch-cluster](https://github.com/mceachen/batch-cluster.js) to
  power child process management. Task timeout, retry, and failure handling has
  excellent test coverage now.
* 💔 Switched from [debug](https://www.npmjs.com/package/debug) to node's
  [debuglog](https://nodejs.org/api/util.html#util_util_debuglog_section) to
  reduce external dependencies
* 🌱 ExifTool upgraded to
  [v10.51](http://www.sno.phy.queensu.ca/~phil/exiftool/history.html#v10.51)
* 📦 Using `.npmignore` instead of package.json's `files` directive to specify
  the contents of the module.

### v2.17.1

* 🐛 Rounded milliseconds were not set by `ExifDateTime.toDate()` when timezone
  was not available. Breaking tests were added.

### v2.16.1

* 📦 Exposed datetime parsing via static methods on `ExifDateTime`, `ExifDate`, and `ExifTime`.

### v2.16.0

* ✨ Some newer smartphones (like the Google Pixel) render timestamps with
  microsecond precision, which is now supported.

### v2.15.0

* ✨ Added example movies to the sample image corpus used to build the tag
  definitions.

### v2.14.0

* ✨ Added `taskTimeoutMillis`, which will cause the promise to be rejected if
  exiftool takes longer than this value to parse the file. Note that this
  timeout only starts "ticking" when the task is enqueued to an idle ExifTool
  process.
* ✨ Pump the `onIdle` method every `onIdleIntervalMillis` (defaults to every 10
  seconds) to ensure all requested tasks are serviced.
* ✨ If `ECONN` or `ECONNRESET` is raised from the child process (which seems to
  happen for roughly 1% of requests), the current task is re-enqueued and the
  current exiftool process is recycled.
* ✨ Rebuilt `Tags` definitions using more (6,412!) sample image files 
  (via `npm run mktags ~/sample-images`), including many RAW image types 
  (like `.ORF`, `.CR2`, and `.NEF`).

### v2.13.0

* ✨ Added `maxReuses` before exiftool processes are recycled
* 🌱 ExifTool upgraded to
  [v10.50](http://www.sno.phy.queensu.ca/~phil/exiftool/history.html#v10.50)

### v2.12.0

* 🐛 Fixed [`gps.toDate is not a function`](https://github.com/mceachen/exiftool-vendored.js/issues/3)

### v2.11.0

* 🌱 ExifTool upgraded to v10.47
* ✨ Added call to `.kill()` on `.end()` in case the stdin command was missed by ExifTool

### v2.10.0

* ✨ Added support for Node 4. TypeScript builds under es5 mode.

### v2.9.0

* 🌱 ExifTool upgraded to v10.46

### v2.8.0

* 🌱 ExifTool upgraded to v10.44
* 📦 Upgraded to TypeScript 2.2
* 🐞 `update/io.ts` error message didn't handle null statuscodes properly 
* 🐞 `update/mktags.ts` had a counting bug exposed by TS 2.2

### v2.7.0

* ✨ More robust error handling for child processes (previously there was no
  `.on("error")` added to the process itself, only on `stderr` of the child
  process).

### v2.6.0

* 🌱 ExifTool upgraded to v10.41
* ✨ `Orientation` is [rendered as a string by
  ExifTool](http://www.sno.phy.queensu.ca/~phil/exiftool/exiftool_pod.html#n---printConv),
  which was surprising (to me, at least). By exposing optional args in
  `ExifTool.read`, the caller can choose how ExifTool renders tag values.

### v2.5.0

* 🐞 `LANG` and `LC_` environment variables were passed through to exiftool (and
  subsequently, perl). These are now explicitly unset when exec'ing ExifTool,
  both to ensure tag names aren't internationalized, and to prevent perl errors
  from bubbling up to the caller due to missing locales.

### v2.4.0

* ✨ `extractBinaryTag` exposed because there are a **lot** of binary tags (and
  they aren't all embedded images)
* 🐞 `JpgFromRaw` was missing in `Tag` (no raw images were in the example corpus!)

### v2.3.0

* ✨ `extractJpgFromRaw` implemented to pull out EXIF-embedded images from RAW formats

### v2.2.0

* 🌱 ExifTool upgraded to v10.40

### v2.1.1

* ✨ `extractThumbnail` and `extractPreview` implemented to pull out EXIF-embedded images
* 📦 Rebuilt package.json.files section 

### v2.0.1

* 💔 Switched from home-grown `logger` to [debug](https://www.npmjs.com/package/debug)

### v1.5.3

* 📦 Switch back to `platform-dependent-modules`.
  [npm warnings](http://stackoverflow.com/questions/15176082/npm-package-json-os-specific-dependency) 
  aren't awesome.  
* 📦 Don't include tests or updater in the published package 

### v1.5.0

* 🌱 ExifTool upgraded to v10.38
* 📦 Use `npm`'s os-specific optionalDependencies rather than `platform-dependent-modules`.

### v1.4.1

* 🐛 Several imports (like `process`) name-collided on the globals imported by Electron

### v1.4.0

* 🌱 ExifTool upgraded to v10.37

### v1.3.0

* 🌱 ExifTool upgraded to v10.36
* ✨ `Tag.Error` exposed for unsupported file types. 

### v1.2.0

* 🐛 It was too easy to miss calling `ExifTool.end()`, which left child ExifTool
  processes running. The constructor to ExifTool now adds a shutdown hook to
  send all child processes a shutdown signal.

### v1.1.0

* ✨ Added `toString()` for all date/time types

### v1.0.0

* ✨ Added typings reference in the package.json
* 🌱 Upgraded vendored exiftool to 10.33 

### v0.4.0

* 📦 Fixed packaging (maintain jsdocs in .d.ts and added typings reference)
* 📦 Using [np](https://www.npmjs.com/package/np) for packaging

### v0.3.0

* ✨ Multithreading support with the `maxProcs` ctor param
* ✨ Added tests for reading images with truncated or missing EXIF headers
* ✨ Added tests for timezone offset extraction and rendering
* ✨ Subsecond resolution from the Google Pixel has 8 significant digits(!!),
  added support for that.

### v0.2.0

* ✨ More rigorous TimeZone extraction from assets, and added the
  `ExifTimeZoneOffset` to handle the `TimeZone` composite tag
* ✨ Added support for millisecond timestamps

### v0.1.1

🌱✨ Initial Release. Packages ExifTool v10.31.
