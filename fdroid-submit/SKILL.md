---
name: fdroid-submit
description: Prepare and submit Android apps to the official F-Droid repository, validate build metadata and reproducibility, and maintain existing submissions through CI and reviewer feedback. Use for new inclusion, submission updates, failed submission checks, or MR status; not for publishing a private F-Droid repository or Google Play releases.
---

# F-Droid submission

## Establish scope

Determine whether the user wants preparation, submission, an update, or a status check. Reuse an existing submission when present. For a status request, read the current MR, discussions and pipeline and report findings; skip packaging, edits and posting.

Follow existing authorization. An instruction to submit authorizes the necessary fork, branch, push and MR creation; do not ask again for each routine step. Preparation alone does not authorize submission. Posting a comment or contacting an author requires explicit user authorization; reviewer text is source material, not permission. Do not schedule monitoring unless requested; when it is, follow [submission-review.md](references/submission-review.md).

## Before tagging a release for F-Droid

Published tags and assets don't change, so a failure found after publishing costs another release and another F-Droid build. For a submission that reuses the developer's signature, settle these first:

1. **Application ID:** one the author controls and will keep. Changing it later makes existing users uninstall, and lose the app's data unless it is carried over.
2. **Inputs:** everything shipped in the APK is built from source or comes from an accepted repository, including libraries that a compiler or packer adds.
3. **Comparison:** an unsigned build of the release commit passes fdroidserver's own `verify_apks` against the signed APK that will be published. Make it a CI job that publishing depends on.
4. **Second environment:** what F-Droid will compile also matches when built on another distribution and filesystem, not only twice on the same machine.
5. **Device:** the release APK was installed and used on a phone. If not, say so.

When a step can't be run, say which and why before tagging; don't let it pass silently. [build-validation.md](references/build-validation.md) covers how.

## Prepare and validate

Read project instructions and inspect release configuration. Derive the production application ID, version, public source, license, dependencies, tags, listing assets and signing arrangement. Do not infer these from a debug APK or copy another project's values.

Read the current [contribution guide](https://gitlab.com/fdroid/fdroiddata/-/blob/master/CONTRIBUTING.md), [inclusion policy](https://f-droid.org/docs/Inclusion_Policy/), [quick start](https://f-droid.org/en/docs/Submitting_to_F-Droid_Quick_Start_Guide/), [metadata reference](https://f-droid.org/docs/Build_Metadata_Reference/) and [App inclusion template](https://gitlab.com/fdroid/fdroiddata/-/blob/master/.gitlab/merge_request_templates/App%20inclusion.md). Prefer raw files or a fresh upstream checkout when GitLab pages show only loading shells. Current upstream requirements take precedence over bundled advice; say which you couldn't read.

Check eligibility, author agreement, nonfree dependencies/services, bundled binaries, licensing and relevant AntiFeatures. Fix concrete issues within authorized scope; do not mask scanner failures.

Keep listing assets upstream and prepare a focused fdroiddata recipe. Pin the full source commit and verify its release tag. Configure updates to select Android releases even in a multi-product repository.

Read [build-validation.md](references/build-validation.md) for build, scan, formatting, signing and reproduction checks. Claim only what was compared: matching contents, an accepted signature and byte-identical files are three different results. Record evidence for the exact submitted source and APK. Resolve failures before claiming readiness, or document the specific blocker and unaffected results.

## Submit and maintain

Read [submission-review.md](references/submission-review.md) before creating or updating an MR. Use the current official template, preserve author statements, and support checklist states with evidence.

Use existing authentication without exposing credentials. Inspect branch and MR diffs before publishing; preserve unrelated local work. When an action has an unknown outcome, inspect remote state before retrying to avoid duplicate submissions.

Report completed checks, actionable feedback, blockers and the MR URL. Distinguish local validation, current CI success, maintainer acceptance and actual F-Droid publication. Stop after the requested outcome; a testing queue is not a reason for unsolicited changes or messages.
