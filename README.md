[![npm version](https://img.shields.io/npm/v/trl.svg)](https://npmjs.com/package/trl) [![license](https://img.shields.io/npm/l/trl.svg)](https://github.com/mklement0/trl/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [trl &mdash; introduction](#trl-&mdash-introduction)
- [Examples](#examples)
- [Installation](#installation)
  - [Installation from the npm registry](#installation-from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# trl &mdash; transform lists of strings

`trl` is a Unix CLI that ***tr***ansforms ***l***ists of quoted and/or unquoted strings, by default between single- and multi-line forms.

Both single- and double-quotes are recognized as input field (item) delimiters, and embedded quotes of the same type must be `\`-escaped.

Separators, delimiters, and wrapper strings are configurable, allowing for flexible transformations (reformatting) to and from a wide range of simple formats.

**Note**:

 * In the input, for embedded quotes of the same type to be properly  
   recognized as literals inside quoted tokens, they must be  
   backslash-escaped.  

 * However, with multi-line input, if a given line is not quoted as a whole,  
   backslash-escaping is implicitly applied to any single- or double-quotes  
   on the line, allowing lines with imbalanced quotes, such as `Ten o'clock`.  
   By contrast, if your input lines each contain multiple, individually quoted  
   tokens, use `-x` to suppress this behavior; otherwise, such lines will be  
   treated as a single token each, with embedded quotes escaped on output.

 * CAVEAT: Malformed input can result in LOSS OF TOKENS on output.

 * Similarly, on output, embedded instances of the output delimiters are  
   `\`-escaped.  

Input is provided via one or more arguments, or via stdin.

See the examples below, concise [usage information](#usage) further below,
or read the [manual](doc/trl.md).

# Examples

```shell
  # Single-line list to multi-line list:
$ trl '"one", "two", "three \" of rain"'
one
two
three " of rain

  # List to C-style array:
$ trl -S ', ' -D \" -W '{  }' one two three 'four (4)'
{ "one", "two", "three", "four (4)" }

  # Multi-line to single-line:
$ trl <<EOF
one
two
three " of rain
EOF
"one", "two", "three \" of rain"

  # Multi-line list with multiple items each to Python array;
  # note the use of -x to ensure that the indvidually quoted
  # tokens are properly recognized. 
$ trl -x -s ' ' -S ', ' -D \' -W '[]' <<EOF
one "two (2)"
three 'four'
EOF
['one', 'two (2)', 'three', 'four']

  # US-format telephone number to CSV:
$ trl -s '[() -]' -S , '(789) 123-456'
789,123,456
```

# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**, with [**Perl**](http://www.perl.org/) installed (Perl comes with OSX, as do most Linux distros).
* When installing **manually**: any **Unix-like** platform with **Bash** and **Perl**.

## Installation from the npm registry

<sup>Note: Even if you don't use Node.js, its package manager, `npm`, works across platforms and is easy to install; try [`curl -L http://git.io/n-install | bash`](https://github.com/mklement0/n-install)</sup>

With [Node.js](http://nodejs.org/) or [io.js](https://iojs.org/) installed, install [the package](https://www.npmjs.com/package/trl) as follows:

    [sudo] npm install trl -g

**Note**:

* Whether you need `sudo` depends on how you installed Node.js / io.js and whether you've [changed permissions later](https://docs.npmjs.com/getting-started/fixing-npm-permissions); if you get an `EACCES` error, try again with `sudo`.
* The `-g` ensures [_global_ installation](https://docs.npmjs.com/getting-started/installing-npm-packages-globally) and is needed to put `trl` in your system's `$PATH`.

## Manual installation

* Download [the CLI](https://raw.githubusercontent.com/mklement0/trl/stable/bin/trl) as `trl`.
* Make it executable with `chmod +x trl`.
* Move it or symlink it to a folder in your `$PATH`, such as `/usr/local/bin` (OSX) or `/usr/bin` (Linux).

# Usage

Find concise usage information below; for complete documentation, read the
[manual online](doc/trl.md), or, once installed, run `man trl`
(`trl --man` if installed manually).

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ trl --help


Transforms lists of unquoted and/or quoted strings.

    trl [<options>] [<text>...]

    -s <inSep>      input list separator
    -S <outSep>     output list separator
    -k              keep input item delimiters
    -D <outDelim>   output item delimiter (cannot be combined with -k)
    -W <wrapText>   text to wrap the result list in
    -R <ors>        output record separator (multi-line + multi-item-per-line
                    input only)
    -x              do not auto-escape quotes on lines not quoted as a whole

By default,

 * a multi-line list is transformed to a single-line list with double-quoted  
   items separated by a comma followed by a space.
 * a single-line list is transformed to a multi-line list with unquoted items.

Standard options: --help, --man, --version, --home
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015-2016 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [marked-man (D)](https://github.com/kapouer/marked-man#readme)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.4.0](https://github.com/mklement0/trl/compare/v0.3.3...v0.4.0)** (2016-06-04):
  * [breaking change, enhancement] embedded instances of output separators are
    now get `\`-escaped on output.
  * [breaking change, enhancement] for multi-line input, any line that isn't 
    quoted as a whole is now by default interpreted as a single token whose
    embedded quotes, if any, are treated as literals; use `-x` to suppress this
    behavior (assumes that your lines contain multiple, indvidually quoted
    tokens whose embedded quotes of the same type, if any, are `\`-escaped).

* **[v0.3.3](https://github.com/mklement0/trl/compare/v0.3.2...v0.3.3)** (2015-09-19):
  * [doc] `trl` now has a man page (if manually installed, use `trl --man`);
          `trl -h` now just prints concise usage information.

* **[v0.3.2](https://github.com/mklement0/trl/compare/v0.3.1...v0.3.2)** (2015-09-15):
  * [dev] Makefile improvements; various other behind-the-scenes tweaks.

* **[v0.3.1](https://github.com/mklement0/trl/compare/v0.3.0...v0.3.1)** (2015-06-24):
  * [doc] Copy-editing of CLI help and read-me.

* **[v0.3.0](https://github.com/mklement0/trl/compare/v0.2.0...v0.3.0)** (2015-06-24):
  * [new feature, behavior change] The output-delimiter string passed to `-D` may now be a symmetrical *multi-character* string such as `()`, in which case the 1st _half_ acts as the opening delimiter, and the 2nd half as the closing delimiter.
  * [new feature, behavior change] The wrapper string passed to `-W` may now be a *single* character (in addition to a symmetrical multi-char. string), in which case that same character is used as both the opening and closing wrapper text.

* **[v0.2.0](https://github.com/mklement0/trl/compare/v0.1.1...v0.2.0)** (2015-06-23):
  * [fix resulting in behavior change] Specifying a multi-line list as the only operand (e.g., `trl <<<$'line 1\nline 2\nline 3'` now behaves the same as passing the same string via stdin; i.e., in both cases, the result is a *single-line* list.

* **[v0.1.1](https://github.com/mklement0/trl/compare/v0.1.0...v0.1.1)** (2015-06-14):
  * [doc] Fixed formatting of examples.

* **v0.1.0** (2015-06-14):
  * Initial release.
