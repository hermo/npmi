# Change Log
All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](http://semver.org/).

## [6.2.1] - 2019-11-12
## Fixed
- Add `--dev` flag for npm when NODE_ENV=development to work around an apparent
  bug in npm

## [6.2.0] - 2018-09-18
## Added
- The USE_NPM_CI option can be used to force `npm install` when `npm ci` is the
  default

## [6.1.0]
## Fixed
- NPMI will abort when given invalid command line options

## Changed
- NPMI will now output NPM error logs when installation fails

## [6.0.0]
## Added
- npm modules are installed using 'npm ci' instead of 'npm install' when
  supported (npm ci was added in npm 5.7.1). The new behaviour might cause npmi
  to run differently when package-lock.json is not in sync, hence the major
  version bump. Note that 'npm ci' will only be used with a package-lock.json or
  npm-shrinkwrap.json file.

## [5.0.1]
## Fixed
- Fixed bug introduced in v4.4.2 where installation would fail if modules were
  found in cache but a node_modules directory doesn't already exist

## [5.0.0]
## Fixed
- NPMI will now exit properly if a suitable 'head' command cannot be found

## Changed
- Added package-lock.json as the primary file to calcute cache hash from
- Cache checksums now take NODE_ENV into account.
  If NODE_ENV is "production" the key will contain "-prod" and "-dev" otherwise.

## Added
- Added support for precache command that is run before caching packages

## [4.4.2] - 2017-12-05
## Fixed
- Cached modules are extracted inside node_modules/ without attempting
  to change the directory itself. This allows NPMI to work in an
  environment where node_modules/ is a mount.

## [4.4.1] - 2017-12-04
## Changed
- The node_modules directory is not removed when cleaning up, only it's
  contents. This is useful with container environments where node_modules might
  be a mount.

## [4.4.0] - 2017-10-30
## Added
- Configuration can be passed with environment variables. Thanks to [@ambis](https://github.com/ambis) for the PR.
## [4.3.0] - 2016-09-13
## Added
- Added rudimentary man pages for npmi(1) and npmirc(5).

## [4.2.3] - 2016-08-30
## Added
- Added support for a global npmirc in /etc/npmirc

## [4.2.2] - 2016-08-30
## Fixed
- #1 Using built-in "md5 -q" command for hashing actually works. This
  was broken in [3.0.1]. Thanks to @ilarimakela for the report.

## Improved
- Use "hash" to determine if a command is in path instead of "which"

## [4.2.1] - 2016-01-13
## Fixed
- redis-cli GET xxx always return an extra newline which makes GNU
  gzip/tar think the cached module data is corrupted. This newline
  will now be removed when fetching data from the cache.

## [4.2.0] - 2016-01-13
## Added
- Added REDIS_PORT configuration option

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
[4.2.0]: https://github.com/hermo/npmi/compare/v4.1.0...v4.2.0
[4.2.1]: https://github.com/hermo/npmi/compare/v4.2.0...v4.2.1
[4.2.2]: https://github.com/hermo/npmi/compare/v4.2.1...v4.2.2
[4.2.3]: https://github.com/hermo/npmi/compare/v4.2.2...v4.2.3
[4.3.0]: https://github.com/hermo/npmi/compare/v4.2.3...v4.3.0
[4.4.0]: https://github.com/hermo/npmi/compare/v4.3.0...v4.4.0
[4.4.1]: https://github.com/hermo/npmi/compare/v4.4.0...v4.4.1
[4.4.2]: https://github.com/hermo/npmi/compare/v4.4.1...v4.4.2
[5.0.0]: https://github.com/hermo/npmi/compare/v4.4.2...v5.0.0
[5.0.1]: https://github.com/hermo/npmi/compare/v5.0.0...v5.0.1
[6.0.0]: https://github.com/hermo/npmi/compare/v5.0.1...v6.0.0
[6.1.0]: https://github.com/hermo/npmi/compare/v6.0.0...v6.1.0
[6.2.0]: https://github.com/hermo/npmi/compare/v6.1.0...v6.2.0
[6.2.1]: https://github.com/hermo/npmi/compare/v6.2.0...v6.2.1
