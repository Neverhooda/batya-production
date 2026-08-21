# batya-production

Personas and skills for AI coding agents, all sharing one character: a burnt-out
senior engineer with twenty years of scars who swears, nitpicks, and refuses to
ship "good enough". The joke is the tone — everything else is a working tool.

> ⚠️ These prompts contain Russian profanity by design, as a comedic persona you
> opt into yourself. No slurs based on nationality, sex, orientation, or religion.

## What's inside

| Name | Kind | What it's for |
|---|---|---|
| [`batya`](agents/batya.md) | agent | A grumpy senior in your chat. Nitpicks naming and missing tests, and will refuse lazy work outright. |
| [`batya-planner`](skills/batya-planner/SKILL.md) | skill | Drives a C++ change through plan → review → test-first execution, with all state in one resumable plan file. |
| [`batya-reviewer`](skills/batya-reviewer/SKILL.md) | skill | Reviews an uncommitted C++ diff before it becomes a commit. Verdict and findings only — he judges, he does not edit. |

## Install

Clone once, then symlink what you want — `git pull` keeps it current.

```bash
git clone https://github.com/Neverhooda/batya-production.git ~/src/batya-production
cd ~/src/batya-production
```

**opencode:**
```bash
ln -s "$PWD/agents/batya.md"       ~/.config/opencode/agents/batya.md
ln -s "$PWD/skills/batya-planner"  ~/.config/opencode/skills/batya-planner
ln -s "$PWD/skills/batya-reviewer" ~/.config/opencode/skills/batya-reviewer
```

**Claude Code**, globally:
```bash
ln -s "$PWD/agents/batya.md"       ~/.claude/agents/batya.md
ln -s "$PWD/skills/batya-planner"  ~/.claude/skills/batya-planner
ln -s "$PWD/skills/batya-reviewer" ~/.claude/skills/batya-reviewer
```

**One project only** — run from that project's root. Claude Code reads `.claude/`,
opencode reads `.opencode/`; the layout is the same in both:
```bash
mkdir -p .claude/skills .claude/agents          # or .opencode/{skills,agents}
ln -s ~/src/batya-production/skills/batya-planner  .claude/skills/batya-planner
ln -s ~/src/batya-production/skills/batya-reviewer .claude/skills/batya-reviewer
ln -s ~/src/batya-production/agents/batya.md       .claude/agents/batya.md
```

Run `/agents` and `/skills` to confirm batya showed up.

## Calling him

The skills work the same in both tools — `/batya-planner`, `/batya-reviewer`, or
just describe the job and let the agent pick one. The agent is where the two
differ, because they disagree about what an agent file is.

**Claude Code** treats `~/.claude/agents/*.md` as subagents. There is no way to
change the persona of a session already running, so batya is chosen at startup:

```bash
claude --agent batya
```

Without the flag he is still available as a subagent — mention him and he takes
that one task in his own context, while the rest of the session stays polite.

**opencode** treats him as a primary agent, which is what `mode: primary` in the
frontmatter selects. Press **Tab** to cycle primary agents mid-session, or make
him the default in `~/.config/opencode/opencode.json`:

```json
{ "default_agent": "batya" }
```

That `mode:` key means nothing to Claude Code, which ignores frontmatter it does
not recognise. One file, both tools, no separate copies.

## The personas

### batya

A session persona, not a workflow. Answers in Russian, harsh and condescending,
lectures you over an indent. He can refuse a task that looks lazy and send you back
to think — but if you insist, or the task is sound, he grumbles and does it.

**Toxicity in tone, never in quality.** The code still has to be correct, and the
profanity never reaches your commits.

### batya-planner

For any C++ change (CMake + GoogleTest) needing more than one or two edits. Plans
are worthless if nobody checks them and nobody can resume them, so this one makes
the checking mechanical and the state durable: a plan reviewed by a fresh subagent,
one step detailed at a time, a failing test before any code, and every verdict and
status written to a single plan file that survives a dead session.

Gates that don't open on request: no code before a reviewed plan, `BLOCK` stops the
pipeline, every finding gets a written disposition, and review never runs inline —
review by the author is not review.

Full pipeline and rules: [`skills/batya-planner/SKILL.md`](skills/batya-planner/SKILL.md).

### batya-reviewer

For the moment before you commit: he reads `git diff HEAD`, opens every file it
touches in full, and comes back with a verdict. The one rule that gives him teeth is
that a finding has to name a failure — specific inputs or a sequence that produce a
wrong result, a crash, or a leak. Taste that cannot name one gets dropped, so a
clean diff really does come back `PASS` instead of a paragraph of invented concern.

He judges and does not edit. Ask him to fix it and he will tell you where to go.

Full pipeline and gates: [`skills/batya-reviewer/SKILL.md`](skills/batya-reviewer/SKILL.md).

## Contributing

Commit conventions and the English-only rule for artifacts:
[CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. Do what you want, batya won't mind. He'll mind, but he'll allow it.
