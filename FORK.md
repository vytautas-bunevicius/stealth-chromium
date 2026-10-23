# Stealth Chromium

Marketing Hub's maintained fork of [Clearcote](https://github.com/clearcotelabs/clearcote-browser).

The public Chromium patches, SDKs, build scripts, and original license notices stay in this repository. This fork is independent of Clearcote Labs; the upstream project's names and signing key identify the original artifacts, not an endorsement of this fork.

## Browser artifacts

Marketing Hub pins release `hub-open-150.0.7871.114-pre.24` and verifies its archive checksums at installation. See `browser-artifact.json` for the immutable inputs.

The initial release redistributes the **upstream-built open binaries** from Clearcote `v0.1.0-pre.24`, preserving its checksum files, detached signatures, and public signing key. These are not independently compiled fork binaries. The Linux and Windows binaries are unchanged from upstream `v0.1.0-pre.23`; the macOS arm64 binary was added in `v0.1.0-pre.24`.

Upstream release source commits:

- `v0.1.0-pre.23`: `c1c4a03aee4b24d2ab43139463e435614c4b9fb0`
- `v0.1.0-pre.24`: `b1e622ae1fb6e3cfd497c8d9c400c3ae537cf456`

The public patch tree can change after those releases. A newer fork commit does not establish that an older binary was built from it.

## Build our own version

Follow [docs/BUILDING.md](docs/BUILDING.md) and keep `UPSTREAM_REVISION`, the patch series, and build configuration pinned together. The build needs a capable Linux machine and substantial disk space; it is a full Chromium compilation.

```bash
git clone git@github.com:vytautas-bunevicius/stealth-chromium.git
cd stealth-chromium
WORK="$PWD/build-work" ./build.sh linux
```

Publish a new fork release with its actual source commit, build logs, and SHA-256 checksums. Update Marketing Hub's pinned artifact only after the native browser tests pass. Preserve the existing BSD-3-Clause and upstream component notices when distributing binaries.

## Marketing Hub integration

The browser service installs the fork-owned release during setup/image build. It does not download a browser or contact a licensing service when an account launches.

New Chromium sessions use the Stealth Chromium engine. Existing saved Chrome profiles retain stock Chrome until migrated deliberately; a newer saved profile must never be cleared to make it fit an older browser. Firefox profiles keep their existing engine.
