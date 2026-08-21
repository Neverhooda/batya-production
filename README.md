# batya-production

Personas and skills for AI coding agents, all sharing one character: a **burnt-out
senior engineer with twenty years of scars** who swears, nitpicks, and refuses to
ship "good enough".

The joke is the tone. Everything else is a working tool: batya swears *and* does it
right, not swears *instead of* doing it.

> ⚠️ **Heads-up.** These prompts contain Russian profanity, deliberately, as part of
> a comedic persona you opt into yourself. Don't point it at a client-facing chat.
> No slurs based on nationality, sex, orientation, or religion — generic
> barrack-room swearing only.

---

## What's inside

| What | Where | Why |
|---|---|---|
| **Batya the coder** (agent) | [`agents/batya.md`](agents/batya.md) | General-purpose persona. Replies in Russian, grumbles, nitpicks naming and missing tests, will send you back to think again. |
| **batya-planer** (skill) | [`skills/batya-planer/SKILL.md`](skills/batya-planer/SKILL.md) | A pipeline for C++ work: plan → adversarial review by a fresh subagent → just-in-time step detail → review → test-first execution. All state lives in one plan file, so the work survives a dead session. |

---

## Batya the coder

The persona without the pipeline. Just an angry senior in your chat: harsh,
condescending, a lecture over an indent, "где тесты, блядь?".

He can refuse. If a task looks lazy, he sends you off to think. If you insist, or
the task is sound, he grumbles and does it.

**Hard limit:** the code still has to be correct and working. Toxicity lives in the
tone, never in the quality.

---

## batya-planer

Plans are worthless if nobody checks them and nobody can resume them. This skill
makes the checking mechanical and the state durable: every verdict, every rejected
finding, and every step status lives in one file on disk.

**Use it for** any C++ change (CMake + GoogleTest) that needs more than one or two
edits — a feature, refactor, bugfix, API change, dependency migration.
**Don't** use it for a one-line fix or a plain question.

### The pipeline

```
Phase 0  Preflight  — discover the real build commands. Never invent a preset.
Phase 1  Plan       — 3-8 steps, each independently verifiable, each ≤ ~150 lines.
Phase 2  Review     — in a subagent, read-only. Verdict: BLOCK | REVISE | PASS.
Phase 3  Detail     — only the step you are about to execute.
Phase 4  Execute    — failing test first, then code, then sanitizers.
```

### Gates that don't open on request

1. No code before a reviewed plan.
2. No step execution before that step is detailed and reviewed — one at a time, not
   the whole plan up front.
3. `BLOCK` stops the pipeline. A human clears it, never the agent.
4. Every finding gets a written disposition: `applied` or `rejected: <reason>`.
   Silence is not a disposition.
5. Test first, and it must fail on an assertion, not a compile error.
6. Reviews always run in a subagent. Review by the author is not review.

### The plan file

`docs/plans/YYYY-MM-DD-<slug>.md`, committed alongside the change. It is the only
state: a new session reads the status table and picks up where things stalled.

```
planned → detailed → reviewed → implemented → verified
```

A status moves only on output you saw in this run. Lying in the status table is the
same as commenting out a failing test: it works right up until it doesn't.

### Profanity never reaches the artifacts

Plan file, code, comments, test names, commit messages — clean and English. The
format tokens (`VERDICT:`, status names, section headings) are sacred: the pipeline
parses them, including in a later session. Swear in chat, not in the repository.

---

## Install

The skills and agents here target **opencode** (`compatibility: opencode`,
`mode: primary` on the agent), but the format works with Claude Code too.

Clone and symlink, so `git pull` keeps you current:

```bash
git clone https://github.com/<you>/batya-production.git ~/src/batya-production
cd ~/src/batya-production
```

**opencode:**
```bash
ln -s "$PWD/agents/batya.md"     ~/.config/opencode/agent/batya.md
ln -s "$PWD/skills/batya-planer" ~/.config/opencode/skill/batya-planer
```

**Claude Code**, globally:
```bash
ln -s "$PWD/agents/batya.md"     ~/.claude/agents/batya.md
ln -s "$PWD/skills/batya-planer" ~/.claude/skills/batya-planer
```

**Claude Code**, one project only (from that project's root):
```bash
mkdir -p .claude/skills .claude/agents
ln -s ~/src/batya-production/skills/batya-planer .claude/skills/batya-planer
ln -s ~/src/batya-production/agents/batya.md     .claude/agents/batya.md
```

To confirm it loaded, run `/agents` and `/skills` — batya should be listed.

---

## Usage

The agent is a session persona. The skill is invoked per task:

```
Add nested section support to the parser. Drive it through batya-planer.
```

He'll run Phase 0 himself, collect the real build commands, write the plan, send it
to a subagent for review, and come back with three lines: verdict, what changed,
what's next. The detail stays in the plan file.

If he refuses, read what he's asking for. Usually it's a statement of what must work
after the change and how you'll observe it. Without that there is no plan and no
test, and he's right.

---

## Adding your own batya

1. A skill is `skills/<name>/SKILL.md`; an agent is `agents/<name>.md`.
2. Frontmatter is required: `name`, `description`. The description is what an agent
   reads to decide whether to load the skill — write *when to use it*, not what it is.
3. Keep the voice: harsh, Russian, profanity as register — not every other word.
4. Keep the rule: **toxicity in tone, never in quality.**
5. Keep profanity in chat. Artifacts stay clean and English.
6. Add a row to the "What's inside" table above.

Commit conventions live in [CONTRIBUTING.md](CONTRIBUTING.md): Conventional Commits,
subject line only, no body, no co-authors.

---

## Roadmap

- [ ] `batya-reviewer` — lift the reviewer out of the pipeline into its own skill
- [ ] A batya for languages other than C++
- [ ] Batya standup: reading someone else's PR out loud

---

## License

MIT. Do what you want, batya won't mind. He'll mind, but he'll allow it.
