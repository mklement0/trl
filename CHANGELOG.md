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
