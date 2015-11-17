# Change Log
All notable changes to this project will be documented in this file.
This project adheres to [Semantic Versioning](http://semver.org/).

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

[1.0.0]: https://github.com/hermo/npmi/releases/tag/v1.0.0
[2.0.0]: https://github.com/hermo/npmi//compare/v1.0.0...v2.0.0
