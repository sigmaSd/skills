# Skills

Reusable agent skills, maintained by [sigmaSd](https://github.com/sigmaSd).

| Skill | Purpose |
| --- | --- |
| [fdroid-submit](fdroid-submit/SKILL.md) | Prepare official F-Droid submissions, validate builds and reproducibility, resolve CI failures, and maintain merge requests. |

Each skill directory contains its `SKILL.md` entrypoint and any supporting references. Skills use the current upstream requirements rather than fixed project identifiers or tool versions.

## Installation

Install with the [skills CLI](https://www.npmjs.com/package/skills):

```sh
npx skills add sigmaSd/skills
```

To install only the F-Droid submission skill:

```sh
npx skills add sigmaSd/skills --skill fdroid-submit
```

The CLI lets you select your agent. Add `--global` to install for your user instead of the current project.

The `SKILL.md` instructions and references are portable across compatible agents. `agents/openai.yaml` provides optional Codex UI metadata.

## License

[MIT](LICENSE).
