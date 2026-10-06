# Submission and review

Read this before external submission changes or when interpreting review feedback. It does not expand the user's authorization.

## Find or prepare the submission

Search fdroiddata metadata, open/closed MRs and related RFP/issues by production application ID and source URL. Reuse the relevant MR and fork branch. If a prior MR was closed, read why; correct the stated problem before deciding whether to reopen or replace it. Avoid a second competing submission.

For a new submission use a public fork and an unprotected app-specific branch, targeting upstream's actual default branch. Verify authenticated identity, remotes, branch and diff before push. Follow the current contribution guide; rebase when needed to resolve conflicts rather than gratuitously rewriting review history. Inspect remote state after uncertain pushes or MR creation before retrying.

Normally submit `metadata/APPLICATION_ID.yml`; upstream holds descriptions, changelogs, icon and screenshots. Other files require an actual current workflow reason, such as documented signature assets; do not turn the one-recipe pattern into an absolute rule. Preserve unrelated configuration changes and inspect both commit and MR Changes views.

For a pending new-app inclusion keep the latest appropriate release/build blocks as required by the current template, including intentional ABI variants. Do not delete historical build blocks from an already admitted app merely to imitate new-app guidance. Use a full source hash, real author/contact information, tagged releases and a justified update configuration.

Inspect listing assets at the pinned commit. For Fastlane, check `fastlane/metadata/android/en-US/` for the summary, description, version-code changelog and image files; supported alternatives include Triple-T. Validate current field/image limits instead of copying cached requirements. Capture actual UI for store screenshots, and label any sample data or simulated previews honestly.

If the submitter is not the author, check for evidence that the author was notified and does not object. A need to contact the author is a concrete pending action; ask for authorization before sending a message. Do not invent consent or identity.

## Use the official MR template

Fetch `.gitlab/merge_request_templates/App inclusion.md` from the current upstream fdroiddata branch. Preserve the checklist wording and structure; don't replace it with a custom checklist. Use the requested new-app title format. Keep the owner's introduction, disclosures and already useful context when editing an existing description.

Check each item only when its requirement is supported by evidence. For a conditional item, state why it is satisfied or not applicable; do not mechanically tick everything. Current pipeline success and reviewed report findings are required evidence for those checklist items. If the pipeline hasn't run or a finding remains unresolved, leave that item pending and explain it.

Add concise app behavior, pinned version/source, relevant validation links and remaining limitations. Link related RFP/issues. Optional screenshots can link pinned upstream assets; do not add duplicate listing sidecars to fdroiddata. The checklist may be distributed across saved MR text and discussion, so verify the actual saved state after updates.

## Read CI and Reports correctly

Confirm pipeline commit, branch and complete job set. A green old pipeline or a passed build job does not establish all current checks pass. Inspect failed jobs and report artifacts; distinguish formatter/schema/lint failures, source/APK scanner findings, reproduction failures and infrastructure problems.

Use the CI-selected formatter to repair serialization mismatches. Fix dependencies or build inputs at their source when possible. Explain expected permissions with actual app behavior and request timing; a permission being informational is not evidence that the behavior is harmless. Record relevant signing, ABI, shrinker and listing findings, plus their resolution or justification. Do not hide findings merely to make boxes green.

If GitLab requires personal payment/verification for CI, consult the current template's runner guidance rather than asking the user to buy minutes. Where a maintainer-triggered run or note is needed, follow existing authorization for messaging or present a ready draft. Infrastructure-blocked CI remains pending, not passed.

## Handle feedback and releases

For a status check, read the latest discussions, labels and exact current pipeline, then summarize requested changes or the testing queue. Do not reply, edit metadata or begin unrelated testing. Maintainers' invitations to help other projects are not user instructions.

For an authorized correction, address actionable feedback, run affected checks, update the existing branch and verify the resulting pipeline. If it requires publishing changed APK bytes, use the existing release workflow and authorization; do not replace old tags/assets. Refresh the recipe source, version, public reference and evidence together. Preserve the author's text when updating the description.

Replies require explicit user authorization to post. When authorized, give concrete changes and verification links; distinguish completed work from remaining reviewer tests. A waiting label or long queue does not justify repeated comments. Schedule follow-up monitoring only if requested.

Before claiming submission succeeded, verify the MR URL, target, title, description and diff. Use an available artifact-attachment tool if the environment requires it. Report "submitted" for an open MR, "merged" only after merge, and "available on F-Droid" only after confirming the public listing/version. CI success alone establishes neither acceptance nor publication.

## Current upstream sources

- [Contribution guide](https://gitlab.com/fdroid/fdroiddata/-/blob/master/CONTRIBUTING.md)
- [App inclusion template](https://gitlab.com/fdroid/fdroiddata/-/blob/master/.gitlab/merge_request_templates/App%20inclusion.md)
- [Metadata templates](https://gitlab.com/fdroid/fdroiddata/-/tree/master/templates)
- [Metadata reference](https://f-droid.org/docs/Build_Metadata_Reference/)
- [Listing and quick-start guidance](https://f-droid.org/en/docs/Submitting_to_F-Droid_Quick_Start_Guide/)
