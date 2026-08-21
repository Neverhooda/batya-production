# Contributing

## Language

Everything written down is in **English**: documentation, commit messages, file and
directory names, frontmatter, and the instructional prose inside a `SKILL.md`.

Russian appears in exactly two places:

- what the persona says to the human at runtime;
- the voice samples and catchphrases that define that speech.

If it's an artifact, it's English. If it's a line batya says out loud, it can be
Russian. There is no third case.

## Commits

We follow [Conventional Commits 1.0.0](https://www.conventionalcommits.org/en/v1.0.0/),
trimmed to the subject line:

```
<type>[(scope)]: <description>
```

One line. **No body. No footers. No `Co-Authored-By` or any other trailer** — an
agent wrote it, you reviewed it, it's your commit.

### Types

| Type | When |
|---|---|
| `feat` | a new skill, a new agent, a new capability in an existing one |
| `fix` | broken frontmatter, dead link, a command in a skill that doesn't run |
| `docs` | README, this file, prose inside a `SKILL.md` |
| `refactor` | reworded prompt, same behaviour |
| `style` | formatting, wrapping, whitespace — meaning untouched |
| `chore` | `.gitignore`, housekeeping files, renames, cleanup |
| `test` | skill checks, fixtures |
| `build` / `ci` | build and automation, once there is any |
| `perf` | shorter prompt, same behaviour |

### Scope

A noun in parentheses naming the part of the repository you touched — usually the
skill or agent name:

```
feat(batya-planer): ...
fix(batya): ...
docs(readme): ...
```

Omit it for repository-wide changes.

### Description

- Directly after `: ` (colon and one space).
- Lower case, imperative mood: `add`, `fix`, `drop` — not `added` / `adds`.
- No trailing period.
- The whole subject fits in 72 characters. If it doesn't, the commit is too big.

### Breaking changes

There is no body, so there is no `BREAKING CHANGE:` footer. That leaves the
exclamation mark after the type or scope:

```
feat(batya-planer)!: rename skill directory to batya-planner
```

Breaking means someone's symlink or usual invocation stops working: a renamed
skill, a changed `name` in frontmatter, a deleted agent.

### Examples

```
feat(batya-reviewer): add standalone review skill
fix(batya): unindent frontmatter delimiters
docs(readme): document install paths for opencode and claude code
chore: ignore .DS_Store
refactor(batya-planer): tighten phase 2 review prompt
feat(batya-planer)!: rename skill directory to batya-planner
```

Not like this:

```
Added new skill                  ← no type, past tense, capitalised
feat: changes                    ← says nothing
fix: bug.                        ← trailing period, and which bug?
feat(batya-planer): add skill

Co-Authored-By: ...              ← body and trailer
```

## Repository rules

- One commit, one coherent change. A new skill and its README row go together; a
  new skill and an unrelated typo fix do not.
- Adding a skill or agent means adding a row to the "What's inside" table in
  `README.md` in the same commit.
- Frontmatter needs `name` and `description`. Write the description as *when to use
  this*, since that is what an agent reads to decide.
