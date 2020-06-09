# atom - text editor / IDE

Using [PlatformIO IDE](https://docs.platformio.org) needs [atom](https://atom.io) or [vscode](https://code.visualstudio.com/) (see: [vscodium](https://vscodium.com/) for non MS-branded version).

## installing atom

needs (maybe something is missing):

```
sudo pacman -Syu gcc make guile
sudo pacman -Syu npm nodejs node-gyp semver
```

### PKG-BUILD 

build and install with PKGBUILD script:

```
$ export LC_MESSAGES=POSIX
$ cd path/to/atom
$ makepkg                                               # ~70 min
$ sudo pacman -U atom-1.46.0-1-aarch64.pkg.tar.xz       # install atom
$ sudo pacman -R atom                                   # remove atom
```

### MANUAL BUILD

Before creating the PKGBUILD script the following steps were made to get atom up and running:

bootstrap:

```
$ export LC_MESSAGES=POSIX
$ git clone https://github.com/atom/atom.git --branch=v1.46.0
$ nano atom/script/package.json
----
    "electron-link": "0.4.2",  # "0.4.0",
    ...
    "fs-admin": "^0.13.0",     # "^0.5.0",
    ...
    "minidump": "0.19.0",      # "0.9.0",
----
$nano atom/apm/package.json
----
    "atom-package-manager": "2.5.0"    # "2.4.3"
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
==> Making package: atom 1.46.0-1 (Di 09 Jun 2020 13:41:31 CEST)
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
Cloning into 'atom'...
done.
==> Starting prepare()...
npm binpath: /usr/bin/npm verison: 6.14.5
node version: v14.3.0
node-gyp version: v6.1.0
git version: git version 2.26.2
installed electron version (ELECTRON_VERSION): v8.2.5
Note: switching to 'tags/v1.46.0'.
<..warnings..>
HEAD is now at c1fea480e Bump github@0.34.1-3
patching file apm/package.json
patching file package.json
patching file script/package.json
patching file script/lib/generate-startup-snapshot.js
Node:   v14.3.0
Npm:    v6.14.5
Installing script dependencies
<..warnings..>
Installing apm
<..warnings..>
apm  2.5.0
npm  6.14.5
node 10.20.1 arm64
atom unknown
python 3.8.3
git 2.26.2
Installing modules ✓
Wrote Dependencies Fingerprint: /home/pinebook_pro/atom/src/atom/node_modules/.dependencies-fingerprint 80f5e0a7b12fff146d2a5da12338a52c0fd73841
==> Starting build()...
delete node_modules then apply patch
patching file node_modules/github/package.json
Node:   v14.3.0
Npm:    v6.14.5
Installing script dependencies
Installing apm
apm  2.5.0
npm  6.14.5
node 10.20.1 arm64
atom unknown
python 3.8.3
git 2.26.2
Installing modules ✓
Wrote Dependencies Fingerprint: /home/pinebook_pro/atom/src/atom/node_modules/.dependencies-fingerprint 80f5e0a7b12fff146d2a5da12338a52c0fd73841
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
Downloading tmp-57874-1-SHASUMS256.txt-4.2.7
[============================================>] 100.0% of 4.74 kB (4.74 kB/s)
Packaging app for platform linux arm64 using electron v4.2.7
Changing permissions for node files in /home/pinebook_pro/atom/src/atom/out/atom-1.46.0-arm64
Copying non-ASAR resources to /home/pinebook_pro/atom/src/atom/out/atom-1.46.0-arm64/resources
Writing LICENSE.md to /home/pinebook_pro/atom/src/atom/out/atom-1.46.0-arm64/resources
Application bundle created at /home/pinebook_pro/atom/src/atom/out/atom-1.46.0-arm64
Generating snapshot script at "/home/pinebook_pro/atom/src/atom/out/startup.js" (4234)
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
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/generator/__pycache__/make.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/generator/__pycache__/__init__.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/simple_copy.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/xcode_emulation.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/input.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/common.cpython-38.pyc
usr/local/lib/atom/resources/app/apm/node_modules/npm/node_modules/node-gyp/gyp/pylib/gyp/__pycache__/__init__.cpython-38.pyc
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
==> Finished making: atom 1.46.0-1 (Di 09 Jun 2020 14:51:59 CEST)
```
