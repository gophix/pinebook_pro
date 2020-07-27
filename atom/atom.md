# atom - text editor / IDE

Using [PlatformIO IDE](https://docs.platformio.org) needs [atom](https://atom.io) or [vscode](https://code.visualstudio.com/) (see: [vscodium](https://vscodium.com/) for non MS-branded version).

## installing atom

needs (maybe something is missing):

```
sudo pacman -Syu gcc make guile fakeroot binutils patch (electron)
sudo pacman -Syu npm nodejs node-gyp semver
```

### PKG-BUILD 

build and install with PKGBUILD script:

```
$ export LC_MESSAGES=POSIX
$ cd path/to/atom
$ makepkg                                               # ~70 min
$ sudo pacman -U atom-1.48.0-1-aarch64.pkg.tar.xz       # install atom
```

uninstall atom:

```
$ sudo pacman -R atom                                   # remove atom
```

### MANUAL BUILD

Before creating the PKGBUILD script the following steps were made to get atom up and running:

bootstrap:

```
$ export LC_MESSAGES=POSIX
$ git clone https://github.com/atom/atom.git --branch=v1.48.0
$ nano atom/script/package.json
----
    "electron-link": "0.4.2",  # "0.4.1",
    ...
    "fs-admin": "^0.13.0",     # "^0.12.0",
    ...
    "minidump": "0.19.0",      # "0.9.0",
----
$nano atom/package.json
----
    "vscode-ripgrep": "1.5.8",    # "^1.2.5",
----
$ export NPM_BIN_PATH=/usr/bin/npm
$ cd atom/
$ script/bootstrap
```

build:

```
$nano atom/node_modules/github/package.json
----
  "dependencies": {
    "@babel/generator": "7.8.7",    # "7.8.0",
    ...
    "@babel/preset-env": "7.8.7",   # "7.8.2",
  "devDependencies": {
    "@babel/compat-data": "~7.8.0", # add
----
$ script/build
```

Many lines in generating snapshot script (script/lib/generate-startup-snapshot.js) were commented out.

### clean out

one-liner:

```
script/clean && npm cache clean --force && rm -rf ~/.npm && rm -rf ~/.node-gyp && rm -rf ~/.cache/node-gyp && rm -rf ~/.atom && rm -rf ~/.cache/electron && rm package-lock.json
```

used commands:

```
script/clean
npm cache clean --force
rm -rf ~/.npm
rm -rf ~/.node-gyp
rm -rf ~/.cache/node-gyp
rm -rf ~/.atom
rm -rf ~/.cache/electron
rm package-lock.json
sudo rm -rf /root/.npm
```

# Appendix

## build log:

```
$ makepkg
==> Making package: atom 1.48.0-1 (Mo 27 Jul 2020 00:00:41 CEST)
==> Checking runtime dependencies...
==> Checking buildtime dependencies...
==> Retrieving sources...
  -> Updating atom git repo...
Fetching origin
  -> Found bootstrap.patch
  -> Found build.patch
==> Validating source files with sha256sums...
    atom ... Skipped
    bootstrap.patch ... Passed
    build.patch ... Passed
==> Extracting sources...
  -> Creating working copy of atom git repo...
Previous HEAD position was 5df9cd3d7 Merge pull request #20855 from akonwi/patch-1
Switched to and reset branch 'makepkg'
==> Starting prepare()...
npm binpath: /usr/bin/npm verison: 6.14.6
node version: v14.5.0
node-gyp version: v7.0.0
git version: git version 2.27.0
installed electron version (ELECTRON_VERSION): v9.1.0
Note: switching to 'tags/v1.48.0'.
<..warnings..>
HEAD is now at 5df9cd3d7 Merge pull request #20855 from akonwi/patch-1
patching file package.json
patching file script/package.json
patching file script/lib/generate-startup-snapshot.js
Node:   v14.5.0
Npm:    v6.14.6
Installing script dependencies
<..warnings..>
Installing apm
<..warnings..>
apm  2.5.0
npm  6.14.5
node 10.20.1 arm64
atom unknown
python 3.8.3
git 2.27.0
Installing modules ✓
Wrote Dependencies Fingerprint: /home/pinebook_pro/atom/src/atom/node_modules/.dependencies-fingerprint 6a9eccf4e085d5385a266693de3234346a1703ac
==> Removing existing $pkgdir/ directory...
==> Starting build()...
delete node_modules then apply patch
patching file node_modules/github/package.json
Node:   v14.5.0
Npm:    v6.14.6
Installing script dependencies
Installing apm
apm  2.5.0
npm  6.14.5
node 10.20.1 arm64
atom unknown
python 3.8.3
git 2.27.0
Installing modules ✓
Wrote Dependencies Fingerprint: /home/pinebook_pro/atom/src/atom/node_modules/.dependencies-fingerprint 6a9eccf4e085d5385a266693de3234346a1703ac
Copying assets to /home/pinebook_pro/atom/src/atom/out/app
Transpiling packages with custom transpiler configurations in /home/pinebook_pro/atom/src/atom/out/app
 transpiling for package github
Installing modules ✓
Transpiling Babel paths in /home/pinebook_pro/atom/src/atom/out/app
Transpiling CoffeeScript paths in /home/pinebook_pro/atom/src/atom/out/app
Transpiling CSON paths in /home/pinebook_pro/atom/src/atom/out/app
Transpiling PEG.js paths in /home/pinebook_pro/atom/src/atom/out/app
Generating module cache for /home/pinebook_pro/atom/src/atom/out/app
Generating pre-built less cache in /home/pinebook_pro/atom/src/atom/out/app/less-compile-cache
Generating metadata for /home/pinebook_pro/atom/src/atom/out/app/package.json
Generating API docs at /home/pinebook_pro/atom/src/atom/docs/output/atom-api.json
Dumping symbols in /home/pinebook_pro/atom/src/atom/out/symbols
Running electron-packager on /home/pinebook_pro/atom/src/atom/out/app with app name "atom"
Downloading tmp-26979-1-SHASUMS256.txt-5.0.13
[============================================>] 100.0% of 4.79 kB (4.79 kB/s)
Packaging app for platform linux arm64 using electron v5.0.13
Changing permissions for node files in /home/pinebook_pro/atom/src/atom/out/atom-1.48.0-arm64
Copying non-ASAR resources to /home/pinebook_pro/atom/src/atom/out/atom-1.48.0-arm64/resources
Writing LICENSE.md to /home/pinebook_pro/atom/src/atom/out/atom-1.48.0-arm64/resources
Application bundle created at /home/pinebook_pro/atom/src/atom/out/atom-1.48.0-arm64
Generating snapshot script at "/home/pinebook_pro/atom/src/atom/out/startup.js" (10)
Minifying startup script
Skipping creating debian package. Specify the --create-debian-package option to create it.
Skipping creating rpm package. Specify the --create-rpm-package option to create it.
Skipping artifacts compression. Specify the --compress-artifacts option to compress Atom binaries (and symbols on macOS)                                                                                                                  
Skipping installation. Specify the --install option to install Atom
==> Entering fakeroot environment...
==> Starting package()...
==> Tidying install...
  -> Removing libtool files...
  -> Purging unwanted files...
  -> Removing static library files...
  -> Stripping unneeded symbols from binaries and libraries...
strip: Unable to recognise the format of the input file `./usr/local/lib/atom/resources/app.asar.unpacked/node_modules/symbols-view/vendor/ctags-linux'
  -> Compressing man and info pages...
==> Checking for packaging issues...
==> WARNING: Package contains reference to $srcdir
usr/local/lib/atom/resources/app.asar
usr/local/lib/atom/resources/app.asar.unpacked/node_modules/spellchecker/package.json
usr/local/lib/atom/resources/app/apm/package.json
usr/local/lib/atom/resources/app/apm/node_modules/brace-expansion/package.json
usr/local/lib/atom/resources/app/apm/node_modules/set-blocking/package.json
usr/local/lib/atom/resources/app/apm/node_modules/es6-weak-map/package.json
usr/local/lib/atom/resources/app/apm/node_modules/aws-sign2/package.json
usr/local/lib/atom/resources/app/apm/node_modules/extend/package.json
usr/local/lib/atom/resources/app/apm/node_modules/abbrev/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mkpath/package.json
usr/local/lib/atom/resources/app/apm/node_modules/github-from-package/package.json
usr/local/lib/atom/resources/app/apm/node_modules/assert-plus/package.json
usr/local/lib/atom/resources/app/apm/node_modules/detect-libc/package.json
usr/local/lib/atom/resources/app/apm/node_modules/deep-extend/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mixto/package.json
usr/local/lib/atom/resources/app/apm/node_modules/os-locale/package.json
usr/local/lib/atom/resources/app/apm/node_modules/xtend/package.json
usr/local/lib/atom/resources/app/apm/node_modules/os-homedir/package.json
usr/local/lib/atom/resources/app/apm/node_modules/jsprim/package.json
usr/local/lib/atom/resources/app/apm/node_modules/http-signature/package.json
usr/local/lib/atom/resources/app/apm/node_modules/ajv/package.json
usr/local/lib/atom/resources/app/apm/node_modules/buffer-alloc-unsafe/package.json
usr/local/lib/atom/resources/app/apm/node_modules/getpass/package.json
usr/local/lib/atom/resources/app/apm/node_modules/jsonfile/package.json
usr/local/lib/atom/resources/app/apm/node_modules/cuint/package.json
usr/local/lib/atom/resources/app/apm/node_modules/expand-template/package.json
usr/local/lib/atom/resources/app/apm/node_modules/verror/package.json
usr/local/lib/atom/resources/app/apm/node_modules/wrench/package.json
usr/local/lib/atom/resources/app/apm/node_modules/ecc-jsbn/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/brace-expansion/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/call-limit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/crypto-random-string/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/set-blocking/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/asap/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-retry-allowed/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/byte-size/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-package-arg/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/tiny-relative-date/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/aws-sign2/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._bindcallback/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/debug/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/debug/node_modules/ms/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/extend/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/abbrev/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/term-size/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-npm/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/yallist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/assert-plus/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/deep-extend/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/byline/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/os-locale/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/os-locale/node_modules/execa/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/os-locale/node_modules/cross-spawn/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/qw/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/xtend/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ignore-walk/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/os-homedir/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/promise-retry/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/promise-retry/node_modules/retry/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs-vacuum/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/jsprim/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/http-signature/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ajv/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash.union/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/pumpify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/pumpify/node_modules/pump/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/slide/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/read-installed/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/color-convert/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/debuglog/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/getpass/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-normalize-package-bin/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/verror/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/split-on-first/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/duplexer3/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-install-checks/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/p-defer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-pick-manifest/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ecc-jsbn/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lockfile/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/stringify-package/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-fullwidth-code-point/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ansi-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lcid/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/dashdash/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/timed-out/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/es6-promisify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/decamelize/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/imurmurhash/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/promise-inflight/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/create-error-class/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._baseindexof/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ansicolors/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._getnative/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sshpk/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/detect-newline/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/core-util-is/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/JSONStream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/spdx-correct/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/invert-kv/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/prr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/parallel-transform/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/parallel-transform/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/parallel-transform/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/which/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cliui/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cliui/node_modules/ansi-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cliui/node_modules/strip-ansi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/smart-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-fetch-npm/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/graceful-fs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/prepend-http/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/agentkeepalive/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/update-notifier/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-obj/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/iconv-lite/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cli-columns/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/util-extend/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mississippi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/map-age-cleaner/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/flush-write-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/flush-write-stream/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/flush-write-stream/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/read-package-tree/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cyclist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/process-nextick-args/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/p-try/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/registry-auth-token/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/opener/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/console-control-strings/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/strip-ansi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mkdirp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mkdirp/node_modules/minimist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/gauge/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/gauge/node_modules/string-width/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/gauge/node_modules/aproba/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sorted-object/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/unique-filename/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/init-package-json/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/delegates/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/bcrypt-pbkdf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/concat-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/concat-stream/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/concat-stream/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/retry/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lru-cache/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ansi-styles/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sorted-union-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sorted-union-stream/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sorted-union-stream/node_modules/from2/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sorted-union-stream/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sorted-union-stream/node_modules/isarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/execa/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/execa/node_modules/get-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-ci/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-ci/node_modules/ci-info/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-registry-fetch/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-registry-fetch/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/wrap-ansi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/wrap-ansi/node_modules/string-width/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ip/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-path-inside/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fast-json-stable-stringify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs-write-stream-atomic/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs-write-stream-atomic/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs-write-stream-atomic/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs-write-stream-atomic/node_modules/iferr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-symbol/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/balanced-match/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/figgy-pudding/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/es-abstract/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/function-bind/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/nopt/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/bluebird/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/concat-map/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/generator/__pycache__/make.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/generator/__pycache__/__init__.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/simple_copy.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/xcode_emulation.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/input.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/common.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/__init__.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/unzip-response/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/pump/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/path-is-absolute/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/wrappy/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/got/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/got/node_modules/get-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/readdir-scoped-modules/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/qrcode-terminal/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/latest-version/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/har-validator/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash.without/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cli-boxes/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/builtins/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mime-db/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs.realpath/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/path-exists/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ci-info/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/object.getownpropertydescriptors/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/defaults/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/string-width/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/string-width/node_modules/is-fullwidth-code-point/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/string-width/node_modules/ansi-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/string-width/node_modules/strip-ansi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/inflight/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cacache/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/es6-promise/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npmlog/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/extsprintf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/define-properties/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/xdg-basedir/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cidr-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-typedarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/global-dirs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cmd-shim/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/which-module/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmpublish/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/jsonparse/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/clone/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/supports-color/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/semver-diff/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/socks/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/forever-agent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/har-schema/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/combined-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/validate-npm-package-name/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/humanize-ms/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/path-is-inside/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/tough-cookie/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/util-deprecate/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-installed-globally/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-callable/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/archy/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/require-directory/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mute-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-redirect/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/rimraf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/end-of-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._root/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/signal-exit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-user-validate/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/dotenv/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/editor/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/stream-each/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/shebang-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/path-key/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lock-verify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/read-package-json/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/find-npm-prefix/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/socks-proxy-agent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/socks-proxy-agent/node_modules/agent-base/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/chownr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/has/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash.uniq/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/through2/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/through2/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/through2/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cli-table3/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/string_decoder/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/from2/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/from2/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/from2/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/colors/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/p-finally/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/http-cache-semantics/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/find-up/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/pacote/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/pacote/node_modules/minipass/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/json-schema-traverse/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/import-lazy/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/once/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/widest-line/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/strip-eof/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-lifecycle/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/chalk/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/umask/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmsearch/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/aws4/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/caseless/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lazy-property/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmteam/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/qs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/json-parse-better-errors/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ansi-align/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/rc/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/rc/node_modules/minimist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/p-is-promise/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/has-flag/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/gentle-fs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/gentle-fs/node_modules/iferr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/gentle-fs/node_modules/aproba/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/unique-slug/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/minimatch/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/proto-list/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/run-queue/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/run-queue/node_modules/aproba/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cross-spawn/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cross-spawn/node_modules/yallist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/cross-spawn/node_modules/lru-cache/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/p-limit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/nice-try/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/semver/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-profile/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmconfig/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmconfig/node_modules/p-try/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmconfig/node_modules/find-up/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmconfig/node_modules/p-limit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmconfig/node_modules/p-locate/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmconfig/node_modules/locate-path/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash.clonedeep/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-date-object/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/punycode/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/agent-base/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/move-concurrently/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/move-concurrently/node_modules/aproba/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/psl/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._createset/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/object-keys/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/es-to-primitive/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/p-locate/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-bundled/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/spdx-exceptions/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/osenv/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/isarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/err-code/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/performance-now/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpm/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/os-tmpdir/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ini/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/code-point-at/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/get-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/detect-indent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/copy-concurrently/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/copy-concurrently/node_modules/iferr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/copy-concurrently/node_modules/aproba/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/dot-prop/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmorg/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/capture-stack-trace/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-run-path/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/dezalgo/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/iferr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/decode-uri-component/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/strip-json-comments/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/typedarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/pseudomap/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lowercase-keys/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/boxen/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/has-symbols/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/shebang-command/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/buffer-from/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/form-data/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/object-assign/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-packlist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/https-proxy-agent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/read-cmd-shim/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libcipm/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/get-caller-file/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/package-json/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/spdx-expression-parse/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpx/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/wide-align/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/wide-align/node_modules/string-width/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/sha/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/oauth-sign/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/uid-number/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/make-dir/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/through/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/co/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/camelcase/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/strict-uri-encode/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/protoduck/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/genfun/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mem/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mem/node_modules/mimic-fn/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/tweetnacl/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/asynckit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmaccess/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/stream-shift/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/has-unicode/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/tar/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/tar/node_modules/minipass/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/y18n/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/query-string/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/require-main-filename/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/configstore/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/bin-links/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/json-stringify-safe/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/minizlib/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/minizlib/node_modules/minipass/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/isstream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash.restparam/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs-minipass/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fs-minipass/node_modules/minipass/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ansistyles/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/text-table/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/are-we-there-yet/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/are-we-there-yet/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/are-we-there-yet/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/worker-farm/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/number-is-nan/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/isexe/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ip-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/stream-iterate/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/stream-iterate/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/stream-iterate/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/escape-string-regexp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/read/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/resolve-from/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/jsbn/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/fast-deep-equal/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/columnify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/spdx-license-ids/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/asn1/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/duplexify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/duplexify/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/duplexify/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/color-name/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/path-parse/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/uuid/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/encoding/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-cache-filename/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ms/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/make-fetch-happen/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/yargs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/yargs/node_modules/y18n/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._cacheindexof/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/normalize-package-data/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/normalize-package-data/node_modules/resolve/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/is-cidr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/unpipe/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/delayed-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-logical-tree/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/mime-types/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/locate-path/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/json-schema/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/errno/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._createcache/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/inherits/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/wcwidth/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/meant/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/lodash._baseuniq/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/aproba/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/tunnel-agent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/ssri/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/promzard/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/url-parse-lax/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/pify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/glob/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/util-promisify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/env-paths/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/request/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/libnpmhook/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/yargs-parser/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/infer-owner/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/registry-url/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/hosted-git-info/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/validate-npm-package-license/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/safer-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/write-file-atomic/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/http-proxy-agent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/unique-string/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/npm-audit-report/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/config-chain/package.json
usr/local/lib/atom/resources/app/apm/node_modules/is-fullwidth-code-point/package.json
usr/local/lib/atom/resources/app/apm/node_modules/ansi-regex/package.json
usr/local/lib/atom/resources/app/apm/node_modules/simple-get/package.json
usr/local/lib/atom/resources/app/apm/node_modules/lcid/package.json
usr/local/lib/atom/resources/app/apm/node_modules/dashdash/package.json
usr/local/lib/atom/resources/app/apm/node_modules/touch/package.json
usr/local/lib/atom/resources/app/apm/node_modules/touch/node_modules/nopt/package.json
usr/local/lib/atom/resources/app/apm/node_modules/decamelize/package.json
usr/local/lib/atom/resources/app/apm/node_modules/klaw/package.json
usr/local/lib/atom/resources/app/apm/node_modules/sshpk/package.json
usr/local/lib/atom/resources/app/apm/node_modules/core-util-is/package.json
usr/local/lib/atom/resources/app/apm/node_modules/invert-kv/package.json
usr/local/lib/atom/resources/app/apm/node_modules/season/package.json
usr/local/lib/atom/resources/app/apm/node_modules/season/node_modules/async/package.json
usr/local/lib/atom/resources/app/apm/node_modules/season/node_modules/fs-plus/package.json
usr/local/lib/atom/resources/app/apm/node_modules/cliui/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar-stream/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar-stream/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar-stream/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar-stream/node_modules/isarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/graceful-fs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/emissary/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fs-constants/package.json
usr/local/lib/atom/resources/app/apm/node_modules/process-nextick-args/package.json
usr/local/lib/atom/resources/app/apm/node_modules/buffers/package.json
usr/local/lib/atom/resources/app/apm/node_modules/git-utils/package.json
usr/local/lib/atom/resources/app/apm/node_modules/git-utils/node_modules/async/package.json
usr/local/lib/atom/resources/app/apm/node_modules/git-utils/node_modules/fs-plus/package.json
usr/local/lib/atom/resources/app/apm/node_modules/console-control-strings/package.json
usr/local/lib/atom/resources/app/apm/node_modules/strip-ansi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mkdirp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/gauge/package.json
usr/local/lib/atom/resources/app/apm/node_modules/delegates/package.json
usr/local/lib/atom/resources/app/apm/node_modules/bcrypt-pbkdf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/wordwrap/package.json
usr/local/lib/atom/resources/app/apm/node_modules/wrap-ansi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fast-json-stable-stringify/package.json
usr/local/lib/atom/resources/app/apm/node_modules/balanced-match/package.json
usr/local/lib/atom/resources/app/apm/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/nopt/package.json
usr/local/lib/atom/resources/app/apm/node_modules/concat-map/package.json
usr/local/lib/atom/resources/app/apm/node_modules/pump/package.json
usr/local/lib/atom/resources/app/apm/node_modules/path-is-absolute/package.json
usr/local/lib/atom/resources/app/apm/node_modules/wrappy/package.json
usr/local/lib/atom/resources/app/apm/node_modules/oniguruma/package.json
usr/local/lib/atom/resources/app/apm/node_modules/decompress-response/package.json
usr/local/lib/atom/resources/app/apm/node_modules/har-validator/package.json
usr/local/lib/atom/resources/app/apm/node_modules/next-tick/package.json
usr/local/lib/atom/resources/app/apm/node_modules/ext/package.json
usr/local/lib/atom/resources/app/apm/node_modules/ext/node_modules/type/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mime-db/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fs.realpath/package.json
usr/local/lib/atom/resources/app/apm/node_modules/ncp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/string-width/package.json
usr/local/lib/atom/resources/app/apm/node_modules/inflight/package.json
usr/local/lib/atom/resources/app/apm/node_modules/npmlog/package.json
usr/local/lib/atom/resources/app/apm/node_modules/extsprintf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/is-typedarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/forever-agent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/har-schema/package.json
usr/local/lib/atom/resources/app/apm/node_modules/block-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/combined-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tough-cookie/package.json
usr/local/lib/atom/resources/app/apm/node_modules/util-deprecate/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mute-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/es6-iterator/package.json
usr/local/lib/atom/resources/app/apm/node_modules/coffee-script/package.json
usr/local/lib/atom/resources/app/apm/node_modules/rimraf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/rimraf/node_modules/glob/package.json
usr/local/lib/atom/resources/app/apm/node_modules/type/package.json
usr/local/lib/atom/resources/app/apm/node_modules/end-of-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mv/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mv/node_modules/mkdirp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mv/node_modules/ncp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mv/node_modules/rimraf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/traverse/package.json
usr/local/lib/atom/resources/app/apm/node_modules/signal-exit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/temp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/temp/node_modules/rimraf/package.json
usr/local/lib/atom/resources/app/apm/node_modules/temp/node_modules/glob/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mimic-response/package.json
usr/local/lib/atom/resources/app/apm/node_modules/chownr/package.json
usr/local/lib/atom/resources/app/apm/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/colors/package.json
usr/local/lib/atom/resources/app/apm/node_modules/json-schema-traverse/package.json
usr/local/lib/atom/resources/app/apm/node_modules/noop-logger/package.json
usr/local/lib/atom/resources/app/apm/node_modules/es6-symbol/package.json
usr/local/lib/atom/resources/app/apm/node_modules/binary/package.json
usr/local/lib/atom/resources/app/apm/node_modules/once/package.json
usr/local/lib/atom/resources/app/apm/node_modules/commander/package.json
usr/local/lib/atom/resources/app/apm/node_modules/aws4/package.json
usr/local/lib/atom/resources/app/apm/node_modules/caseless/package.json
usr/local/lib/atom/resources/app/apm/node_modules/qs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/rc/package.json
usr/local/lib/atom/resources/app/apm/node_modules/minimatch/package.json
usr/local/lib/atom/resources/app/apm/node_modules/open/package.json
usr/local/lib/atom/resources/app/apm/node_modules/which-pm-runs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/cson-parser/package.json
usr/local/lib/atom/resources/app/apm/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/chromium-pickle-js/package.json
usr/local/lib/atom/resources/app/apm/node_modules/semver/package.json
usr/local/lib/atom/resources/app/apm/node_modules/punycode/package.json
usr/local/lib/atom/resources/app/apm/node_modules/psl/package.json
usr/local/lib/atom/resources/app/apm/node_modules/keytar/package.json
usr/local/lib/atom/resources/app/apm/node_modules/isarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/performance-now/package.json
usr/local/lib/atom/resources/app/apm/node_modules/prebuild-install/package.json
usr/local/lib/atom/resources/app/apm/node_modules/xmldom/package.json
usr/local/lib/atom/resources/app/apm/node_modules/os-tmpdir/package.json
usr/local/lib/atom/resources/app/apm/node_modules/ini/package.json
usr/local/lib/atom/resources/app/apm/node_modules/code-point-at/package.json
usr/local/lib/atom/resources/app/apm/node_modules/minimist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/chainsaw/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar-fs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar-fs/node_modules/pump/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fs-extra/package.json
usr/local/lib/atom/resources/app/apm/node_modules/event-kit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/grim/package.json
usr/local/lib/atom/resources/app/apm/node_modules/strip-json-comments/package.json
usr/local/lib/atom/resources/app/apm/node_modules/property-accessors/package.json
usr/local/lib/atom/resources/app/apm/node_modules/form-data/package.json
usr/local/lib/atom/resources/app/apm/node_modules/object-assign/package.json
usr/local/lib/atom/resources/app/apm/node_modules/d/package.json
usr/local/lib/atom/resources/app/apm/node_modules/asar-require/package.json
usr/local/lib/atom/resources/app/apm/node_modules/wide-align/package.json
usr/local/lib/atom/resources/app/apm/node_modules/buffer-alloc/package.json
usr/local/lib/atom/resources/app/apm/node_modules/oauth-sign/package.json
usr/local/lib/atom/resources/app/apm/node_modules/camelcase/package.json
usr/local/lib/atom/resources/app/apm/node_modules/underscore-plus/package.json
usr/local/lib/atom/resources/app/apm/node_modules/window-size/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tweetnacl/package.json
usr/local/lib/atom/resources/app/apm/node_modules/simple-concat/package.json
usr/local/lib/atom/resources/app/apm/node_modules/asynckit/package.json
usr/local/lib/atom/resources/app/apm/node_modules/has-unicode/package.json
usr/local/lib/atom/resources/app/apm/node_modules/@atom/plist/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tar/package.json
usr/local/lib/atom/resources/app/apm/node_modules/y18n/package.json
usr/local/lib/atom/resources/app/apm/node_modules/json-stringify-safe/package.json
usr/local/lib/atom/resources/app/apm/node_modules/isstream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/async/package.json
usr/local/lib/atom/resources/app/apm/node_modules/are-we-there-yet/package.json
usr/local/lib/atom/resources/app/apm/node_modules/are-we-there-yet/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/are-we-there-yet/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/are-we-there-yet/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/are-we-there-yet/node_modules/isarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/number-is-nan/package.json
usr/local/lib/atom/resources/app/apm/node_modules/underscore/package.json
usr/local/lib/atom/resources/app/apm/node_modules/es5-ext/package.json
usr/local/lib/atom/resources/app/apm/node_modules/es5-ext/node_modules/es6-iterator/package.json
usr/local/lib/atom/resources/app/apm/node_modules/es5-ext/node_modules/es6-symbol/package.json
usr/local/lib/atom/resources/app/apm/node_modules/es5-ext/node_modules/d/package.json
usr/local/lib/atom/resources/app/apm/node_modules/read/package.json
usr/local/lib/atom/resources/app/apm/node_modules/jsbn/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fast-deep-equal/package.json
usr/local/lib/atom/resources/app/apm/node_modules/asn1/package.json
usr/local/lib/atom/resources/app/apm/node_modules/xmlbuilder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fs-plus/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fs-plus/node_modules/async/package.json
usr/local/lib/atom/resources/app/apm/node_modules/uuid/package.json
usr/local/lib/atom/resources/app/apm/node_modules/yargs/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tmp/package.json
usr/local/lib/atom/resources/app/apm/node_modules/fstream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/delayed-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/q/package.json
usr/local/lib/atom/resources/app/apm/node_modules/napi-build-utils/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mime-types/package.json
usr/local/lib/atom/resources/app/apm/node_modules/mksnapshot/package.json
usr/local/lib/atom/resources/app/apm/node_modules/node-abi/package.json
usr/local/lib/atom/resources/app/apm/node_modules/asar/package.json
usr/local/lib/atom/resources/app/apm/node_modules/json-schema/package.json
usr/local/lib/atom/resources/app/apm/node_modules/inherits/package.json
usr/local/lib/atom/resources/app/apm/node_modules/bl/package.json
usr/local/lib/atom/resources/app/apm/node_modules/bl/node_modules/string_decoder/package.json
usr/local/lib/atom/resources/app/apm/node_modules/bl/node_modules/string_decoder/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/bl/node_modules/readable-stream/package.json
usr/local/lib/atom/resources/app/apm/node_modules/bl/node_modules/readable-stream/node_modules/safe-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/bl/node_modules/isarray/package.json
usr/local/lib/atom/resources/app/apm/node_modules/to-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/decompress-zip/package.json
usr/local/lib/atom/resources/app/apm/node_modules/decompress-zip/node_modules/q/package.json
usr/local/lib/atom/resources/app/apm/node_modules/aproba/package.json
usr/local/lib/atom/resources/app/apm/node_modules/tunnel-agent/package.json
usr/local/lib/atom/resources/app/apm/node_modules/uri-js/package.json
usr/local/lib/atom/resources/app/apm/node_modules/glob/package.json
usr/local/lib/atom/resources/app/apm/node_modules/request/package.json
usr/local/lib/atom/resources/app/apm/node_modules/buffer-fill/package.json
usr/local/lib/atom/resources/app/apm/node_modules/hosted-git-info/package.json
usr/local/lib/atom/resources/app/apm/node_modules/safer-buffer/package.json
usr/local/lib/atom/resources/app/apm/node_modules/first-mate/package.json
usr/local/lib/atom/resources/app/apm/node_modules/first-mate/node_modules/async/package.json
usr/local/lib/atom/resources/app/apm/node_modules/first-mate/node_modules/fs-plus/package.json
==> Creating package "atom"...
  -> Generating .PKGINFO file...
  -> Generating .BUILDINFO file...
  -> Generating .MTREE file...
  -> Compressing package...
==> Leaving fakeroot environment.
==> Finished making: atom 1.48.0-1 (Mo 27 Jul 2020 02:08:40 CEST)
```
