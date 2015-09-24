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
