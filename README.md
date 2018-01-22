# npmi
Locally caching 'npm install'. Useful in CI-like situations.

npmi works by hashing package-lock.json / npm-shrinkwrap.json / package.json
to get a reasonably unique id and using that as a cache key.

The cache key is something like
`v8.9.1-darwin-x64-prod-0a8ca5554ece9332de1bb64096d04a57`.

The cache key is determined from node version, platform, cpu
architecture, NODE_ENV and the content of one of the package files
mentioned above.

On the first run npmi will run 'npm install' as usual, storing whatever
ends up in node_modules to $CACHEDIR/$HASH.tgz.

Later npmi will simply extract node_modules from the cache without
hitting the network at all.

It can also use a Redis server to share cached files between hosts.

# NOTE

node_modules may contain compiled/binary files and npmi should only
be run in the same environment as the actual code to prevent nasty
surprises.

# Usage

```
$ npmi -h

NPMI v5.0.0 - a caching 'npm install'
Usage: npmi [-hcefprtv]
-h    Display this help
-c    Use specified cache dir: Default /tmp/npmi
-e    Cache existing node_modules AKA 'Oops I forgot to npmi' mode
-f    Force install from NPM and update cache
-p    Run given command before packages are cached
-r    Use specified Redis server for shared cache
-t    Specify Redis TTL in seconds. Default: 259200
-v    Verbose output

```

# Configuration options

The following configuration options are available.

Key              | Value
---------------- | --------------------------
CACHEDIR         | Local cache directory. Default: `$TMPDIR/npmi`.
VERBOSE          | Verbose output. Default: `0`
FORCE            | Force reinstall. Default: `0`
CACHE_EXISTING   | Cache whatever is currently in node_modules. Default: `0`
REDIS_SERVER     | Host/IP for Redis server. Default: `""` (Don't use Redis)
REDIS_PORT       | What port to use when connecting to Redis. Default: `6379`
REDIS_TTL        | How many seconds should modules be cached. Default: `86400` (24h)
REDIS_PREFIX     | Key prefix for Redis. Default: `NPMI4`

## Configuration with .npmirc

NPMI can be configured using a file, `.npmirc` located in the working
directory.

The file format is `KEY=VALUE` separated by newlines.

## Configuration with environment variables

All configuration can be given as environment variables if necessary.
The options are same for both `.npmirc` but are prefixed with `NPMI_`.
To run NPMI on verbose mode one would use `NPMI_VERBOSE=1 npmi`.

# Speed gains

Even if using a local NPM cache there are often packages which need to
compile something or download an external dependency.
NPMI can skip these steps if nothing has changed and generally is something
like 50-100x faster than running npm install.

Clone some project with a decent amount of dependencies
```
$ git clone https://github.com/request/request
```

Install deps once to fill local caches etc.
```
$ npm install
# Downloading, compiling, extracting, doing stuff...
```

Remove the deps and install again with timing
```
$ rm -rf node_modules && time npm install
```
<strong><pre># npm install  29.38s user 13.26s system 158% cpu 26.960 total</pre></strong>

Install with NPMI
```
$ npmi -v
# - HASH e3102336c4165f1ca58ef03f09e1a9fd determined from package.json (v4.2.1-darwin-x64)
# - Cleaning node_modules...
# - Attempt to install from cache
# - Modules not found in cache
# - Installing modules from NPM
# - Modules cached successfully
```

Install from NPMI cache
```
$ time npmi -v
# - HASH e3102336c4165f1ca58ef03f09e1a9fd determined from package.json (v4.2.1-darwin-x64)
# - Cleaning node_modules...
# - Attempt to install from cache
# - Modules installed successfully
```
<strong><pre># npmi -v  0.57s user 3.65s system 94% cpu 4.486 total</pre></strong>


# Known Issues

Any post-installation script of NPM will NOT get run when installing
from cache. This includes at least the following:
* `install`
* `postinstall`

For instance a `postinstall` block in package.json which does something
outside of node_modules will only be run on a cache MISS. If it merely creates
a file in node_modules etc. those will get cached.





