# npmi
Locally caching 'npm install'. Useful in CI-like situations.

npmi works by hashing npm-shrinkwrap.json / package.json to get a
reasonably unique id and using that as a cache key.

On the first run npmi will run 'npm install' as usual, storing whatever
ends up in node_modules to $CACHEDIR/$HASH.tgz.

Later npmi will simply extract node_modules from the cache without
hitting the network at all.

# NOTE

node_modules may contain compiled/binary files and npmi should only
be run in the same environment as the actual code to prevent nasty
surprises.

# Usage

```
$ npmi -h

NPMI v1.0.0 - a caching 'npm install'

Usage: ./npmi [-hcfv]
-h    Display this help
-c    Use specified cache dir: Default $TMPDIR/npmi
-f    Force install from NPM and update cache
-v    Verbose output
```

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
# - HASH e3102336c4165f1ca58ef03f09e1a9fd determined from package.json
# - Cleaning node_modules...
# - Attempt to install from cache
# - Modules not found in cache
# - Installing modules from NPM
# - Modules cached successfully
```

Install from NPMI cache
```
$ time npmi -v
# - HASH e3102336c4165f1ca58ef03f09e1a9fd determined from package.json
# - Cleaning node_modules...
# - Attempt to install from cache
# - Modules installed successfully
```
<strong><pre># npmi -v  0.57s user 3.65s system 94% cpu 4.486 total</pre></strong>

