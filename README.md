[![npm version](https://img.shields.io/npm/v/trl.svg)](https://npmjs.com/package/trl) [![license](https://img.shields.io/npm/l/trl.svg)](https://github.com/mklement0/trl/blob/master/LICENSE.md)

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

**Contents**

- [trl &mdash; introduction](#trl-&mdash-introduction)
- [Examples](#examples)
- [Installation](#installation)
  - [From the npm registry](#from-the-npm-registry)
  - [Manual installation](#manual-installation)
- [Usage](#usage)
- [License](#license)
  - [Acknowledgements](#acknowledgements)
  - [npm dependencies](#npm-dependencies)
- [Changelog](#changelog)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# trl &mdash; introduction

`trl` is a Unix CLI that ***tr***ansforms ***l***ists of quoted and/or unquoted strings, by default between single- and multi-line forms.
Both single- and double-quotes are recognized as input field (item) delimiters.

Separators, delimiters, and wrapper strings are configurable, allowing for flexible transformations (reformatting) to and from a wide range of simple formats.

_Caveats_:

 * Unbalanced single- or double-quotes in the input break parsing and result in unpredictable output.
 * While `\'` and `\"` are recognized as literal single- and double-quotes in the input, they are output as literals without (re-)escaping; the only exception is `\'` inside a single-quoted string in the input, which outputs `\'` as well.
  
Input is provided via one or more arguments, or via stdin.

See below for examples and the [usage](#usage) chapter for details.

# Examples

```
  # Single-line list to multi-line list:
$ trl '"one", "two", "three"'
one
two
three

  # List to C-style array:
$ trl -S ', ' -D \" -W '{  }' one two three 'four (4)'
{ "one", "two", "three", "four (4)" }

  # Multi-line to single-line:
$ trl <<EOF
one
two
three
EOF
"one", "two", "three"

  # Multi-line list with multiple items each to Python array:
$ trl -s ' ' -S ', ' -D \' -W '[]' <<EOF
one "two (2)"
three four
EOF
['one', 'two (2)', 'three', 'four']

  # US-format telephone number to CSV:
$ trl -s '[() -]' -S , '(789) 123-456'
789,123,456
```

# Installation

**Supported platforms**

* When installing from the **npm registry**: **Linux** and **OSX**, with [**Perl**](http://www.perl.org/) installed (Perl comes with OSX and many, but not all Linux distros).
* When installing **manually**: any **Unix-like** platform with **Bash** and **Perl**.

## From the npm registry

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

<!-- DO NOT EDIT THE FENCED CODE BLOCK and RETAIN THIS COMMENT: The fenced code block below is updated by `make update-readme/release` with CLI usage information. -->

```nohighlight
$ trl --help

SYNOPSIS
  trl [-s inSep] [-S outSep] [-k|-D outDelim] [-W wrapText] [-R ors] [text ...]

DESCRIPTION
  trl *tr*ansforms *l*ists of unquoted and/or quoted strings, by
  default between single- and multi-line forms.
  Separators, delimiters, and a wrapper string are configurable.
  
  Both single- and double-quotes are recognized as input item delimiters.
  
  Caveats:
   - Unbalanced single- or double-quotes in the input break parsing and
     result in unpredictable output.
   - While \' and \" are recognized as literal single- and double-quotes in
     the input, they are output as literals without (re-)escaping; the only
     exception is \' inside a single-quoted string in the input, which outputs
     \' as well.

  Input is provided via one or more operands or, in their absence, via stdin.
  To disambiguate operands from options, precede operands with '--' as a
  separate argument.
  
  Sensible defaults are used for options that aren't explicitly specified; 
  their values depend on whether the input is single- or multi-line:
  Single-line input:
    List items are assumed to be separated with whitespace, a comma, or 
    a comma followed by whitespace.
    They are transformed into multi-line output with each item on its own
    line, stripped of surrounding quotes.
  Multi-line input:
    List items are assumed to be each on a separate line.
    They are transformed into a single-line list with items double-quoted
    and separated by a comma followed by a space.
  Explicitly specified options override the behavior described.

  Some options described below come in two flavors:
    - lowercase options such as -s describe the *input*
    - corresponding uppercase options such as -S control the *output*

  With the exception of -s, which accepts a Perl regular expression to
  describe the separators between input items, options with arguments expect
  literals rather than escape sequences; if needed, use ANSI C-quoted strings
  in Bash, Ksh, or Zsh to create literal control characters; e.g.,
  $'\n' and $'\t' create a literal newline and tab, respectively.

    -k
      Keeps all items in their input form, whether quoted or unquoted; the
      default is to remove quoting on input processing; double-quoting is
      by default applied on *output* when transforming a multi-line list to a
      single-line list; to unconditionally produce unquoted items, use
      -D '', to enforce a uniform quoting character, use -D <delim>.
    -s inSep
      Specifies the input item separator, i.e., what separates items in the
      input, using a Perl regular expression. Note that *runs* of matches
      (i.e., one or more contiguous instances) are invariably considered a
      *single* separator.
    -S outSep
      Specifies the output item separator, i.e., how to separate the items
      on output, irrespective of what separated them in the input.
    -D outDelim
      Specifies the delimiter to put around the output items; this may be
      any string, but note that on *input* only single- and double-quotes
      are recognized; cannot be combined with -k.
      Use -D '' to make all output items unquoted.
      If a single char. is specified, it is used as both the opening and 
      closing delimiter; otherwise, the first *half* of the specified string
      is used as the opening delimiter, and the second half as the closing one.
    -W wrapText
      Specifies output wrapper text to place around the entire output list,
      including  desired whitespace, if any: if a single char. is specified,
      that char. is used both before and the after the output list; otherwise,
      the first *half* of the text is placed before, and the second one after;
      if the output list is multi-line, a newline is appended / prepended to
      the opening/closing string.
    -R ors
      Specifies the output record separator, which only applies to multi-line
      input whose lines contain multiple items each: by default, the output
      record separator is set to the same value as the input item separator,
      so that a multi-line list results in a uniformly separated single-line
      list. Specify -R $'\n' to instead retain the line breaks from the
      input (while transforming the items on each line), or pass an
      alternative separator to replace them.

PREREQUISITES
  Perl 5 must be installed.

EXAMPLES
  trl '"one", "two", "three"' # -> $'one\ntwo\three'
  trl <<<$'one\ntwo\nthree' # -> '"one", "two", "three"'
  trl -W '(  )' -S ' ' -D '' '"one","two","three"' # -> '( one two three )'
  trl -s '[() -]' -S , '(789) 123-456' # -> '789,123,456'
```

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'LICENSE.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# License

Copyright (c) 2015 Michael Klement <mklement0@gmail.com> (http://same2u.net), released under the [MIT license](https://spdx.org/licenses/MIT#licenseText).

## Acknowledgements

This project gratefully depends on the following open-source components, according to the terms of their respective licenses.

[npm](https://www.npmjs.com/) dependencies below have optional suffixes denoting the type of dependency; the *absence* of a suffix denotes a required *run-time* dependency: `(D)` denotes a *development-time-only* dependency, `(O)` an *optional* dependency, and `(P)` a *peer* dependency.

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the dependencies from 'package.json'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

## npm dependencies

* [doctoc (D)](https://github.com/thlorenz/doctoc)
* [json (D)](https://github.com/trentm/json)
* [replace (D)](https://github.com/harthur/replace)
* [semver (D)](https://github.com/npm/node-semver#readme)
* [tap (D)](https://github.com/isaacs/node-tap)
* [urchin (D)](https://github.com/tlevine/urchin)

<!-- DO NOT EDIT THE NEXT CHAPTER and RETAIN THIS COMMENT: The next chapter is updated by `make update-readme/release` with the contents of 'CHANGELOG.md'. ALSO, LEAVE AT LEAST 1 BLANK LINE AFTER THIS COMMENT. -->

# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

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
