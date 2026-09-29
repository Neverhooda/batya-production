# Why the gates are what they are

Read when you want the argument, or when someone asks you to drop a gate and you
need more than "because it says so". Nothing here is a rule. Every rule is in
`SKILL.md`.

## Gate 1 - no code before a cleared plan

`Plan status` is the whole-plan counterpart of the per-step ladder. The step
statuses say nothing about whether the plan itself was ever reviewed, so without
this line a session that dies between Phase 1 and Phase 2 resumes into a plan no
reviewer has seen, and every step under it was reviewed against nothing.

`REVISE` is a state you leave, not one you sit in. A scope with no open `block` and
a disposition on every finding is settled; holding the pipeline for the word `PASS`
is how a plan reaches four rounds and three hundred lines with every step still
`planned`.

## Gate 4 - every finding gets a written disposition

Silence is not a disposition because silence is unauditable in the next session.
The reason for a rejection is the load-bearing half: if you cannot write the
technical reason, it was not a nitpick.

## Gate 5 - test first, failing for the right reason

A compile error hides the assertion you never verified. A driver that passes today
proves nothing about your change. A guard that fails today is not a guard.

The `### Step <N> red` block is the only proof the test came first, and it cannot
be reconstructed afterwards - by the time implementation exists, the driver is
green and the evidence is gone. That is why the block gates `implemented` rather
than merely being recommended.

## Gate 6 - reviews run in a fresh subagent

Review by the author is not review. An agent that just wrote a plan cannot see the
assumption it wrote the plan from.

## Gate 7 - one dispatch per scope, three at the most

One round finds what a fresh reader finds. A second round mostly finds what the
first round's fixes moved, and a third finds what the second moved: each round
manufactures the defect the next one reports. The loop has no natural end, so the
end is a person. The agent never earns a dispatch for itself because the agent is
the only party that benefits from another round - it is cheaper to ask again than
to be wrong in front of the human.

Applying a `block` used to force a re-run. It does not, because gate 10 already
covers the part of a fix that can be wrong in a way a reviewer would catch: a
command or a factual claim goes into the plan only after it was run. The rest of
a fix is text the human reads in the report.

An unparseable return does not count because nothing was reviewed, but it is
logged and capped: an uncapped exemption turns one paid round into two sent, and
an unlogged one hides a flaky review channel from the next session.

## Gate 8 - the human's go closes the scope

The pipeline exists to put decisions in front of the human with evidence. Once
they have decided, a round that re-raises the same finding is not review; it is
the agent relitigating a call that was not its to make. The log keeps their words
next to the finding so a later session can see who decided and why, and copies
the disposition instead of reopening it.

What can still reopen a cleared plan is text the reviewer never read and no
finding asked for: a Goal, Non-goal, or step the agent changed on its own. That
goes in front of the human because they cleared a document that no longer exists,
and the choice between a re-review and a go is theirs, not a rule's.

## Gate 9 - you do not commit, stage, or edit the ignore file

Their history, their tree. Files by name in the `git add` binds them too: the plan
file sits untracked in the same tree, and `git add -A` or `git commit -a` is exactly
how it lands in the history it was kept out of.

An unignored plan clutters `git status` and can ride along in a blanket stage. That
is the argument you hand them; it is not a reason to edit their ignore file, and it
is not a gate on the work.

## Gate 10 - every command was run before it was written

A command you never executed is a guess. So is a signature you did not look up:
your training has a cutoff and the project pins a version, which is two ways to be
wrong about the same call.

CMake presets are three separate namespaces. A configure preset name used where a
test preset is required gives `CMake Error: No such test preset` - a plan full of
commands nobody ran fails at the first one, in the middle of execution, with the
plan already cleared.

Never write a shortened command in the plan: a preset dropped into a narrow table
cell is a command that does not run, and nobody notices until it fails.

## The plan file, and its price

State on disk and nowhere else means a fresh clone, a second machine, or one
`git clean -fdx` takes the plan with it and the work restarts at Phase 0. If it has
to survive any of those, copy it outside the working tree first.

Append-only review log: history you can rewrite is history that agrees with you.

## Why blast radius is a command

Changing a widely included header is a different task, at a different price, than
changing one `.cpp`. A number from a grep can be wrong; a number from a feel cannot
even be checked.

## Why the plan is small

3 to 8 steps and two pages is not a style preference. An unread plan must at least
be short enough to be readable, and a plan that outgrew the budget while being
reviewed is telling you the design is not settled.

## Why sanitizers are not optional on the listed hazards

Small pointer changes are exactly what ASan catches. If `Risks` names a lifetime,
UB, overflow, or race hazard while `sanitizers` says not needed, the page
contradicts itself and the review will BLOCK it.

MSVC has no UBSan and LeakSanitizer does not run on Windows, so a Windows-only step
whose risk is undefined behaviour is not sanitizer-covered. Say what you check
instead rather than papering over it.
