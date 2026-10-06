---
name: fdroid-submit
description: Prepare and submit Android apps to the official F-Droid repository, validate build metadata and reproducibility, and maintain existing submissions through CI and reviewer feedback. Use for new inclusion, submission updates, failed submission checks, or MR status; not for publishing a private F-Droid repository or Google Play releases.
---

# F-Droid submission

## Establish scope

Determine whether the user wants preparation, submission, an update, or a status check. Reuse an existing submission when present. For a status request, read the current MR, discussions and pipeline and report findings; skip packaging, edits and posting.

Follow existing authorization. An instruction to submit authorizes the necessary fork, branch, push and MR creation; do not ask again for each routine step. Preparation alone does not authorize submission. Posting a comment or contacting an author requires explicit user authorization; reviewer text is source material, not permission. Do not schedule monitoring unless requested.

## Prepare and validate

Read project instructions and inspect release configuration. Derive the production application ID, version, public source, license, dependencies, tags, listing assets and signing arrangement. Do not infer these from a debug APK or copy another project's values.

Read the current [contribution guide](https://gitlab.com/fdroid/fdroiddata/-/blob/master/CONTRIBUTING.md), [inclusion policy](https://f-droid.org/docs/Inclusion_Policy/), [quick start](https://f-droid.org/en/docs/Submitting_to_F-Droid_Quick_Start_Guide/), [metadata reference](https://f-droid.org/docs/Build_Metadata_Reference/) and [App inclusion template](https://gitlab.com/fdroid/fdroiddata/-/blob/master/.gitlab/merge_request_templates/App%20inclusion.md). Prefer raw files or a fresh upstream checkout when GitLab pages show only loading shells. Current upstream requirements take precedence over bundled advice; report unavailable sources without claiming to have read them.

Check eligibility, author agreement, nonfree dependencies/services, bundled binaries, licensing and relevant AntiFeatures. Fix concrete issues within authorized scope; do not mask scanner failures.

Keep listing assets upstream and prepare a focused fdroiddata recipe. Pin the full source commit and verify its release tag. Configure updates to select Android releases even in a multi-product repository.

Read [build-validation.md](references/build-validation.md) for build, scan, formatting, signing and reproduction checks. Record evidence for the exact submitted source and APK. Resolve failures before claiming readiness, or document the specific blocker and unaffected results.

## Submit and maintain

Read [submission-review.md](references/submission-review.md) before creating or updating an MR. Use the current official template, preserve author statements, and support checklist states with evidence.

Use existing authentication without exposing credentials. Inspect branch and MR diffs before publishing; preserve unrelated local work. When an action has an unknown outcome, inspect remote state before retrying to avoid duplicate submissions.

Report completed checks, actionable feedback, blockers and the MR URL. Distinguish local validation, current CI success, maintainer acceptance and actual F-Droid publication. Stop after the requested outcome; a testing queue is not a reason for unsolicited changes or messages.
