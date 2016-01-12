# Change Log
All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](http://semver.org/).

## [4.1.0] - 2016-01-12
## Added
- Added support for global configuration file in ~/.npmirc

## [4.0.0] - 2016-01-12
## Added
- Configuration file support. If an .npmirc file is found in the working
  directory it will be parsed for configuration options. See the [README]
  for details.

## Changed
- Redis cache now uses SETEX instead of HSET. This means that old entries will
  automatically be cleared from redis after a specified time. Default is 24
  hours.

## [3.0.3] - 2015-12-18
## Fixed
- It works again. The previous release was quite broken after enabling
  "-o errexit" even though everything looked ok.
- Check HASH declaration safely

## 3.0.2 - 2015-12-18 [YANKED]
### Fixed
- Use "-o nounset" compatible TMPDIR declaration detection

## [3.0.1] - 2015-12-17
### Changed
- Improved error handling
- Internal refactoring
- Fix lint errors discovered by [Shellcheck](https://github.com/koalaman/shellcheck)

## [3.0.0] - 2015-12-16
### Added
- Add support for storing cached files in redis

## [2.1.1] - 2015-12-02
### Fixed
- When running NPMI non-verbosely it incorrectly returned 1 instead of 0
  after successfully storing modules.

## [2.1.0] - 2015-12-01
### Added
- Add support for MD5 hashing using BSD/Mac OS X md5 and g-prefixed md5sum

## [2.0.0] - 2015-11-17
### Breaking changes

#### New Cache file naming convention
```
Previous:  1a9ada9790762573c59237e39c46f8df.tgz
Current:   v4.2.1-darwin-x64-1a9ada9790762573c59237e39c46f8df.tgz
```

Cache file name now includes node version, platform and arch. This is
useful when working with multiple node versions, operating systems etc.


### Added
- (-e) Cache existing node_modules

## [1.0.0] - 2015-09-24
### Added
- First versioned release
- (-f) Force update even if cache exists

[README]: "README.md"
[1.0.0]: https://github.com/hermo/npmi/releases/tag/v1.0.0
[2.0.0]: https://github.com/hermo/npmi/compare/v1.0.0...v2.0.0
[2.1.0]: https://github.com/hermo/npmi/compare/v2.0.0...v2.1.0
[2.1.1]: https://github.com/hermo/npmi/compare/v2.1.0...v2.1.1
[3.0.0]: https://github.com/hermo/npmi/compare/v2.1.1...v3.0.0
[3.0.1]: https://github.com/hermo/npmi/compare/v3.0.0...v3.0.1
[3.0.3]: https://github.com/hermo/npmi/compare/v3.0.1...v3.0.3
[4.0.0]: https://github.com/hermo/npmi/compare/v3.0.3...v4.0.0
[4.1.0]: https://github.com/hermo/npmi/compare/v4.0.0...v4.1.0
