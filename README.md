# thirdparty-artifacts

Copies of third-party release files used by the uutils CI, so that jobs do not
depend on the availability of the upstream download servers.

The files are attached to the GitHub releases of this repository, one release
per upstream version (e.g. `diffutils-3.12`). They are byte-identical to the
upstream releases: consumers must keep verifying them against a pinned
checksum.

| Release | Source | License | Used by |
|---|---|---|---|
| `diffutils-3.12` | https://ftp.gnu.org/gnu/diffutils/ | GPL-3.0-or-later | uutils/diffutils `tests/run-upstream-testsuite.sh` |
| `tar-1.35` | https://ftp.gnu.org/gnu/tar/ | GPL-3.0-or-later | uutils/tar `util/build-gnu.sh` |
| `busybox-arch-1.36.1-4` | https://archlinux.org/packages/extra/x86_64/busybox/ | GPL-2.0-only | uutils/coreutils `util/run-gnu-tests-smack-ci.sh` |

Files are downloaded from
`https://github.com/uutils/thirdparty-artifacts/releases/download/<release>/<file>`,
for example:

```sh
curl -fsSL -o diffutils-3.12.tar.xz \
  https://github.com/uutils/thirdparty-artifacts/releases/download/diffutils-3.12/diffutils-3.12.tar.xz
```

## Adding an artifact

1. Download the file from its official source, along with its signature when
   upstream publishes one.
2. Verify it before uploading anything:
   - check the signature against the upstream key (for GNU projects, the
     [GNU keyring](https://ftp.gnu.org/gnu/gnu-keyring.gpg): `gpgv --keyring ./gnu-keyring.gpg FILE.sig FILE`);
   - check the checksum published by upstream or already pinned by the
     consumer, if any.
3. When the artifact is a binary under a copyleft license (GPL, ...), also
   attach the corresponding source: the upstream source tarball and the
   recipe used to build the binary (e.g. the distribution's packaging files).
4. Create one release per upstream version, named `<project>-<version>`
   (add the origin when it is a distribution build, e.g.
   `busybox-arch-1.36.1-4`). The release notes give the original URLs and the
   SHA256 of each file:

   ```sh
   gh release create diffutils-3.12 -R uutils/thirdparty-artifacts \
     --title "GNU diffutils 3.12" --notes "..." \
     diffutils-3.12.tar.xz diffutils-3.12.tar.xz.sig
   ```

   Never replace a file in an existing release: publish a new release
   instead, so that pinned checksums keep working.
5. Add a row to the table above with the release, the source, the license
   and the consumer.
6. In the consumer, download from this repository and verify the pinned
   checksum. Keep the upstream URL as a fallback when it is reliable enough.
