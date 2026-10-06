# Skills

Reusable agent skills, maintained by [sigmaSd](https://github.com/sigmaSd).

| Skill | Purpose |
| --- | --- |
| [fdroid-submit](fdroid-submit/SKILL.md) | Prepare official F-Droid submissions, validate builds and reproducibility, resolve CI failures, and maintain merge requests. |

Each skill directory contains its `SKILL.md` entrypoint and any supporting references. Skills use the current upstream requirements rather than fixed project identifiers or tool versions.

## Installation

Copy the skill directory into your agent's supported skills location, following its installation instructions. The `SKILL.md` instructions and supporting references are portable; `agents/openai.yaml` provides optional Codex UI metadata.

For agents without native skill support, provide `SKILL.md` as instructions and make its referenced files available. Tool access and permissions still depend on the agent.

## License

[MIT](LICENSE).
