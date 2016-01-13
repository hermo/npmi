# npmi
Locally caching 'npm install'. Useful in CI-like situations.

npmi works by hashing npm-shrinkwrap.json / package.json to get a
reasonably unique id and using that as a cache key.
The cache key is prefixed with node version, platform and arch.

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

NPMI v4.0.0 - a caching 'npm install'
Usage: ./npmi [-hcfv]
-h    Display this help"
-c    Use specified cache dir: Default /tmp/npmi
-e    Cache existing node_modules AKA 'Oops I forgot to npmi' mode
-f    Force install from NPM and update cache
-r    Use specified Redis server for shared cache
-t    Specify Redis TTL in seconds. Default: 86400
-v    Verbose output

```

# Configuration

NPMI can be configured using a file, *.npmirc* located in the working
directory.

The file format is KEY=VALUE separated by newlines.

Some options of interest are described below. See the source for more
exotic options.

Key           | Value
------------- | -------------
VERBOSE       | Verbose output if set to "1". Default: "0"
CACHEDIR      | Where should local file caches be kept. Default: $TMPDIR/npmi
REDIS_SERVER  | Host/IP for Redis server. Default: "" (Don't use Redis)
REDIS_TTL     | How many seconds should modules be cached. Default: 86400 (24 h)
REDIS_PORT    | What port to use when connecting to Redis. Default: 6379

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

