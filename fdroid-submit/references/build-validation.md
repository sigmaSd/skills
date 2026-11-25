# Build and release validation

Read this for packaging or CI corrections. Adapt commands to the checked-out fdroiddata revision and installed tool's `--help`; these are examples, not a fixed toolchain.

## Isolate the work and match CI

Use a dedicated submission branch and separate clean upstream build checkout. Follow the project's temporary-directory rules; keep downloads, virtual environments and artifacts outside tracked source. Preserve unrelated edits, including local fdroid configuration.

Inspect the current fdroiddata `.gitlab-ci.yml` and included files for the actual images, fdroidserver selection, schema and formatter. Record resolved versions/commits and container digests. A distro or PyPI release can differ from the tools CI uses, so use CI's resolved tooling, in a virtual environment or container. Run privileged build-server setup only inside the intended disposable environment, not directly on the user's host. Prefer CI for heavy builds (SDK downloads, native runtimes); on the user's machine, cap their memory.

Useful commands, from the fdroiddata checkout:

```sh
fdroid readmeta
fdroid rewritemeta APP_ID
fdroid lint APP_ID
fdroid build -v APP_ID:VERSION_CODE
```

`rewritemeta` mutates metadata: inspect its diff, and don't hand-wrap YAML instead. Run the schema/style checks CI runs. Build flags differ between local and build-server execution: follow the current job rather than assuming `--on-server` is safe on the host.

## Validate the recipe and release

Derive the build module, flavor, output and tool versions from source. Verify that the tag resolves to the recipe's full commit and that this commit contains all needed build and listing files. Confirm update matching against actual tags, including excluded desktop/prerelease tags. Upstream update commands may mutate metadata; review their diff.

Use a clean source build and retain source/APK scanner results. Follow current inclusion/scanner rules for bundled dependencies. Do not use blanket ignore/delete rules to hide nonfree code. Any narrow exclusion needs a documented purpose and proof it does not enter the release.

Inspect the resulting production APK for application ID, version, debuggability, permissions, signer and included native libraries. Review shrinker configuration and APK size. Enable suitable R8/resource optimization when needed; inspect library consumer rules and runtime-sensitive classes rather than adding blanket keep/dontwarn rules. ABI splitting is conditional on useful size savings and correct update/version-code behavior.

Run available project checks and a smoke test of important workflows on the optimized release when release configuration changes. Separate source review, emulator results and physical-device tests; document platform/tool failures precisely. For native libraries, check applicable ZIP/ELF alignment and device support using current Android guidance.

## Apps without a standard Gradle build

For apps packed by their own tool, or embedding a runtime (Node, Python, Go, Rust, a game engine), find what ends up in the APK and where each piece comes from before writing a recipe. Separate two kinds of input:

- **Shipped in the APK** (native libraries, dex, bundled runtimes): built from source in the recipe, never downloaded from a release page, however public the CI that produced it. Look inside the packages the build installs too: an npm, PyPI or Maven package can carry a compiled `.so`, `.dex` or `.jar` that git doesn't. Point the tool at the locally built pieces, or patch it to stop using its bundled ones. A build tool can also add its own library to the app (a Kotlin compiler's `kotlin-stdlib.jar`, a language's runtime): that part is shipped, so take it from a repository the policy accepts, such as Maven Central.
- **Build tools only** (a host compiler, a package manager, a host runtime that runs the build): a pinned download checked against a published hash or signature is the usual practice. Confirm no part of it is copied into the APK.

Search fdroiddata metadata for recipes that build the same runtime or use the same packer, including archived ones, and read how they handled toolchains, `timeout`, scanner findings and dependency installs. Reuse an existing srclib for that runtime's source when one fits.

Use a manual build (`build` commands with `output`) when there is no Gradle project. Request an `ndk` version F-Droid can install (check the current SDK transparency log). Put privileged toolchain setup in `sudo`. Estimate the native build's time and memory on a few cores: set `timeout` and the build's job count to fit, and state the expected duration in the MR. Treat byte-reproducibility of a cross-compiled runtime as unproven until builds from differing environments match.

## Choose and verify signing

Read the current [reproducible-build guide](https://f-droid.org/docs/Reproducible_Builds/) before choosing signing metadata. Developer-signed reproduction is strongly encouraged for new apps, but inability to reproduce is not automatically an inclusion-policy failure. Explain the exception and signing implications rather than silently claiming success.

Inventory existing certificates and distribution channels. Preserve signing identity; do not generate a replacement key for an already published app. Changing the certificate may prevent updates, and Play's app-signing certificate can differ from the upload certificate. Store private keys/passwords outside repositories and logs; metadata uses public fingerprints.

For an upstream-reference workflow, configure `Binaries` or build-specific `binary` and `AllowedAPKSigningKeys` as documented. Verify the reference is the public, correctly versioned production APK and its certificate SHA-256 is the expected signer. Check download status/content, not just the URL.

### Compare before publishing

Rebuild the release commit unsigned in a clean environment and run F-Droid's comparison against the signed APK, before that APK is published: `fdroidserver.common.verify_apks(signed, copy_of_unsigned, tmpdir)` returns `None` when they match, else the difference. It is internal to fdroidserver and needs `apksigner`: pin the version, and check the call against it. [Tandem's script](https://github.com/sigmaSd/tandem/blob/main/tools/fdroid-match.py), run by its release workflow, is a working example. The developer's private key is not needed.

A matching certificate, a successful build, equal sizes or matching extracted files are not this check. Nor does it cover a part it didn't rebuild: if CI packs a prebuilt runtime, compare that runtime's own source build separately. Report the comparison actually made, and claim byte-identical files only from equal final hashes.

### Vary the environment

Two builds on one machine, or on two copies of one CI image, share what makes output vary. For native code, change at least:

- the distribution and its tools (F-Droid builds on Debian; check the current image);
- the filesystem and the order directories list in (for example ext4 against overlayfs, tmpfs, or a fresh checkout whose files were created in another order);
- the core count and job count;
- the build path, unless the recipe reproduces the upstream path on purpose.

Compare the native libraries themselves, not only the final APK.

### When it doesn't match

Compare entry by entry before changing anything, and let the result choose the suspect:

| What differs | Look at |
| --- | --- |
| A native library | link order from an unsorted directory walk (`os.walk`, `readdir`, globs); a host tool the configure step detects, such as the linker; embedded paths; timestamps |
| Dex or resources | resolved JDK/SDK/AGP/R8 versions, dependency inputs, baseline profiles, VCS state |
| Nothing inside, only the ZIP (padding, alignment, order) | signing: `apksigner` can realign an APK that was already aligned. Align first, then sign with `--alignment-preserved true` (where its `--help` lists it) |

Use diffoscope or an APK diff tool. Fix the cause where it is, in the runtime's or packer's source, rather than editing the output or applying every workaround in the docs. Show that the fix makes the varied builds match before releasing again, and don't retry until a match happens by chance. Changed release bytes need a new release.

## Evidence to retain

Record source commit, version, build environment, commands, APK hash/size, public signer fingerprint, scan/comparison outcomes and CI URLs. Refresh affected checks after changes; don't attribute an old pipeline to a new recipe.
