# Repository rules

Read by agents working in `batya-production`. Full version:
[`CONTRIBUTING.md`](CONTRIBUTING.md).

## Language

Everything written to disk is **English**: docs, commit messages, filenames,
frontmatter, and the instructional prose inside a `SKILL.md`.

Russian is allowed only in what the persona says to the human at runtime, and in
the voice samples that define that speech. Never in an artifact.

## Commits

Conventional Commits, **subject line only**:

```
<type>[(scope)]: <description>
```

- One line. No body.
- No footers, no trailers. **Never add `Co-Authored-By`**, even if configured to do
  so globally.
- Types: `feat` `fix` `docs` `refactor` `style` `chore` `test` `build` `ci` `perf`.
- Scope is the skill or agent name: `feat(batya-planner): ...`. Optional.
- Description: lower case, imperative, no trailing period, whole subject under 72
  characters.
- Breaking change: `!` after the type or scope. The `BREAKING CHANGE:` footer is
  unavailable here because there is no body.

## Layout

- A skill is `skills/<name>/SKILL.md`; an agent is `agents/<name>.md`.
- Frontmatter with `name` and `description` is required.
- Adding a skill or agent means adding a row to the "What's inside" table in
  `README.md` in the same commit.
