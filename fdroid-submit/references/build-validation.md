# Build and release validation

Read this for packaging or CI corrections. Adapt commands to the checked-out fdroiddata revision and installed tool's `--help`; these are examples, not a fixed toolchain.

## Isolate the work and match CI

Use a dedicated submission branch and separate clean upstream build checkout. Follow the project's temporary-directory rules; keep downloads, virtual environments and artifacts outside tracked source. Preserve unrelated edits, including local fdroid configuration.

Inspect the current fdroiddata `.gitlab-ci.yml` and included files for the actual images, fdroidserver selection, schema and formatter. Record resolved versions/commits and container digests. A distro or PyPI release can differ from the tools CI uses, and CI may advance a nominally pinned checkout. Use its resolved tooling for formatting instead of repeatedly alternating formatters. Use a scoped virtual environment or container rather than changing global tools. Run privileged build-server setup only inside the intended disposable environment, not directly on the user's host.

Useful commands, from the fdroiddata checkout:

```sh
fdroid readmeta
fdroid rewritemeta APP_ID
fdroid lint APP_ID
fdroid build -v APP_ID:VERSION_CODE
```

`APP_ID` and `VERSION_CODE` are placeholders. Inspect the formatting diff and run the schema/style checks specified by CI. `rewritemeta` mutates metadata; a local YAML serializer or hand wrapping is not a substitute for the CI formatter. Build flags differ between local and build-server execution: follow the current job rather than assuming `--on-server` is safe on the host. Scope builds to the intended app/version.

## Validate the recipe and release

Derive the build module, flavor, output and tool versions from source. Verify that the tag resolves to the recipe's full commit and that this commit contains all needed build and listing files. Confirm update matching against actual tags, including excluded desktop/prerelease tags. Upstream update commands may mutate metadata; review their diff.

Use a clean source build and retain source/APK scanner results. Follow current inclusion/scanner rules for bundled dependencies. Do not use blanket ignore/delete rules to hide nonfree code. Any narrow exclusion needs a documented purpose and proof it does not enter the release.

Inspect the resulting production APK for application ID, version, debuggability, permissions, signer and included native libraries. Review shrinker configuration and APK size. Enable suitable R8/resource optimization when needed; inspect library consumer rules and runtime-sensitive classes rather than adding blanket keep/dontwarn rules. ABI splitting is conditional on useful size savings and correct update/version-code behavior.

Run available project checks and a smoke test of important workflows on the optimized release when release configuration changes. Debug instrumentation does not by itself establish optimized APK behavior. Separate source review, emulator results and physical-device tests; document platform/tool failures precisely. For native libraries, check applicable ZIP/ELF alignment and device support using current Android guidance.

## Choose and verify signing

Read the current [reproducible-build guide](https://f-droid.org/docs/Reproducible_Builds/) before choosing signing metadata. Developer-signed reproduction is strongly encouraged for new apps, but inability to reproduce is not automatically an inclusion-policy failure. Explain the exception and signing implications rather than silently claiming success.

Inventory existing certificates and distribution channels. Preserve signing identity; do not generate a replacement key for an already published app. Changing the certificate may prevent updates, and Play's app-signing certificate can differ from the upload certificate. Store private keys/passwords outside repositories and logs; metadata uses public fingerprints.

For an upstream-reference workflow, configure `Binaries` or build-specific `binary` and `AllowedAPKSigningKeys` as documented. Verify the reference is the public, correctly versioned production APK and its certificate SHA-256 is the expected signer. Check download status/content, not just the URL.

Independently rebuild the pinned source in a clean environment and use F-Droid's comparison/signature verification against that published reference. A matching certificate, successful Gradle build, or matching extracted files alone is insufficient. Report only the comparison actually performed; distinguish signature verification from a stronger byte-identical reproduction claim supported by equal final hashes. This workflow does not require the developer's private signing key for verification.

When reproduction fails, inspect the differing APK entries before changing anything. Check resolved JDK/SDK/NDK/AGP/R8, dependency inputs, VCS state, generated profiles, timestamps/paths and ZIP metadata. Use diffoscope or suitable APK diff tools for diagnosis; do not apply every historical workaround from the docs or retry indefinitely until a match happens. Retest the changed release and reproduction. Published tags/assets stay immutable; changed release bytes need a new release.

## Evidence to retain

Record source commit, version, build environment, commands, APK hash/size, public signer fingerprint, scan/comparison outcomes and CI URLs. Refresh affected checks after changes; don't attribute an old pipeline to a new recipe. Preserve failure logs and report the smallest concrete blocker instead of calling the whole workflow successful.
