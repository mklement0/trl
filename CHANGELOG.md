# Changelog

Versioning complies with [semantic versioning (semver)](http://semver.org/).

<!-- NOTE: An entry template for a new version is automatically added each time `make version` is called. Fill in changes afterwards. -->

* **[v0.2.0](https://github.com/mklement0/trl/compare/v0.1.1...v0.2.0)** (2015-06-23):
  * [fix resulting in behavior change] Specifying a multi-line list as the only operand (e.g., `trl <<<$'line 1\nline 2\nline 3'` now behaves the same as passing the same string via stdin; i.e., in both cases, the result is a *single-line* list.

* **[v0.1.1](https://github.com/mklement0/trl/compare/v0.1.0...v0.1.1)** (2015-06-14):
  * [doc] Fixed formatting of examples.

* **v0.1.0** (2015-06-14):
  * Initial release.
