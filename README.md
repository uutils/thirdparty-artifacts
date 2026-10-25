# thirdparty-artifacts

Copies of third-party release files used by the uutils CI, so that jobs do not
depend on the availability of the upstream download servers.

The files are attached to the GitHub releases of this repository, one release
per upstream version (e.g. `diffutils-3.12`). They are byte-identical to the
upstream releases: consumers must keep verifying them against a pinned
checksum.

| Release | Source | Used by |
|---|---|---|
| `diffutils-3.12` | https://ftp.gnu.org/gnu/diffutils/ | uutils/diffutils `tests/run-upstream-testsuite.sh` |
