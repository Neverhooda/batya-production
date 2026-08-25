# doc-template

What Phase 5 writes, and the rules the audited repo's own `doc/` lives by
afterward. Two things live here: a block written **verbatim** into the
audited repo as `doc/CLAUDE.md`, and the shapes Phase 5 fills in from the
state file for `doc/index.md` and `doc/domains/<name>.md`. The shapes are
not verbatim - they are filled from evidence, never copied with the
placeholders still in them, and a section a domain has nothing to fill is
removed rather than left standing empty. That rule is restated at the top
of the domain shape below because it is the one gate 10 exists to enforce.

## `doc/CLAUDE.md`

Write this block verbatim as `doc/CLAUDE.md` in the audited repo, once, the
first time Phase 5 runs. If it already exists, leave it: it belongs to that
repo from then on, not to this audit.

The outer fence below is **four** backticks, and it has to stay four: the block
it delimits contains three-backtick examples of its own, and a closing fence may
not carry an info string, so a three-backtick outer fence would be closed by the
first bare three-backtick line inside it - cutting the block off mid-Links-rule
and leaving the TOC and `index.md` rules outside the thing being copied. What
gets written to `doc/CLAUDE.md` is everything between the four-backtick fences,
inner fences included, and not the fences themselves.

````markdown
# Documentation authoring rules

These rules apply to every Markdown file under `doc/`.

## Table of Contents

1. [Headings](#headings)
1. [Links](#links)
1. [Maintaining the Table of Contents](#maintaining-the-table-of-contents)
1. [The documentation index (`index.md`)](#the-documentation-index-indexmd)

## Headings

- **No section numbers in heading text.** Write `## Session Management`,
  never `## 2. Session Management`. Numbers in headings end up inside the
  anchor (`#2-session-management`), so every renumber breaks links. Visible
  numbering in the Table of Contents comes from the ordered-list markup,
  not the heading.
- Anchors are derived from the heading text: lowercase, spaces become
  hyphens, punctuation is dropped (`& ` -> `-`; `/`, `.`, `(`, `)` removed).
  Duplicate headings get `-1`, `-2` suffixes in document order.

## Links

Same-file links use `#slug`; cross-file links use a relative path plus
`#slug`:

```markdown
[Session Management](#session-management)
[Session Management](domains/session.md#session-management)
```

- When you rename a heading, update its Table of Contents entry *and*
  every link that points at it.
- Reference another section by its **title**, not by a number. A bare
  reference like "see 2.1.3" goes stale the moment a section moves and
  means nothing once headings are numberless.

## Maintaining the Table of Contents

- A document with more than a few sections carries a hand-maintained
  `## Table of Contents`, written as a **nested ordered list** (use `1.`
  for every item; Markdown renders the running numbers):

  ```markdown
  ## Table of Contents

  1. [Overview](#overview)
     1. [Key Details](#key-details)
  1. [Usage](#usage)
  ```

- The TOC is **hand-maintained**. When one exists it lists every `##` and
  `###` heading the document actually has (deeper headings are optional).
  Add, rename, or remove a heading -> update the TOC in the same edit. A
  section that was removed because it had nothing to say loses its TOC
  entry in that same edit, never later.
- **Never use auto-generation markers** (`<!-- toc -->` / `<!-- tocstop -->`)
  or any TOC generator, ever. Every document under `doc/` is authored
  directly; the TOC is content, not output.
- A short document may skip the TOC entirely. `index.md` is itself a link
  index and never needs one.

## The documentation index (`index.md`)

`doc/index.md` lists every documented domain. Adding a new domain document
adds its entry to `index.md` in the same change - under the group heading
that fits it, with a one-paragraph description matching the style of the
existing entries. Renaming or removing a document updates or removes its
entry the same way. `index.md` lists only documents that exist, never a
placeholder for one planned.
````

## `doc/index.md`

One `##` heading per group of domains, then one `###` entry per documented
domain under it:

```markdown
# <Repo Name> Documentation Index

## <Group Name>

### [Domain Name](domains/<name>.md)

<one paragraph: what the domain owns, in the vocabulary the code uses.>
```

Grouping is a judgement call the ledger's `Notes` and the domain's own
`## What it owns` paragraph already support - group domains the same way
Phase 2 clustered them, not by inventing new categories. A single-domain
repo needs no grouping at all: one `##` heading is enough. Add exactly one
entry per Phase 5 run, in the same edit that writes the domain document -
never write an entry for a domain that has no file behind it yet, and never
batch several entries ahead of the documents they point at.

## `doc/domains/<name>.md`

`<name>` is a slug derived from the ledger's `Domain` column, not copied
from it verbatim: lower case, spaces become hyphens, and anything in
parentheses is dropped along with the parentheses themselves - a ledger row
named `join_server (hw11 + hw01 bundled)` files as
`doc/domains/join_server.md`, not `join_server-hw11-hw01-bundled.md`, and
not `join_server.md` arrived at by improvising a different rule on a
different run. The slug is fixed the first time this domain is documented
and never changes after - `index.md` and the ledger's own row both link to
it by that name, and a later rename would orphan both.

**A section with nothing to fill it is removed, not left empty - Table of
Contents entry and all.** Before writing any section below, ask what goes
under it when the state file gives this domain nothing: if the honest
answer is a sentence saying there is nothing there, that is not content,
it is a stub, and the section does not appear in the document at all. Every
section that does appear is grounded in something the state file already
measured; none of them are written from impression to fill space.

```markdown
# <Domain Name>

## Table of Contents

1. [What it owns](#what-it-owns)
1. [Where it lives](#where-it-lives)
1. [Entities](#entities)
1. [How it talks to other domains](#how-it-talks-to-other-domains)
1. [Invariants](#invariants)
1. [Known rot](#known-rot)

## What it owns

<the responsibility, in the vocabulary the code uses>

## Where it lives

targets: <names>
directories: <paths>
entry points: <the files someone changing this domain opens first>

## Entities

| Name | Declared in | What it is | Also called |
|---|---|---|---|

## How it talks to other domains

<the headers it exports, who includes them, what it includes from others;
the targets it links, and the targets that link it>

## Invariants

<what must stay true, and what enforces it>

## Known rot

<findings from the audit that touch this domain, by id>
```

List only the sections the document actually has in the Table of Contents
above - it is hand-maintained per `doc/CLAUDE.md`, not a fixed list.

- **What it owns** and **Where it lives** are never absent for a domain
  this phase writes at all: a `mapped` ledger row always carries a Notes
  entry (or, failing that, its Targets/Directories name what it is) and
  always carries Targets, Directories, and Entry points - that is what
  `mapped` means. If a row reaches here without them, it was not ready for
  Phase 5; fix the ledger, don't paper over it here.
- **Entities** exists only when Phase 3's `git grep 'class \|struct '` pass
  for this domain's directories found at least one declaration. A domain
  with zero hits - data-driven code, a thin executable, C-style headers -
  gets no Entities section and no empty header-only table. `Name` and
  `Declared in` are the two fields of that pass's second line - the
  `sort -u` one, which prints the declared name and the file it was found in,
  one row per pair. Read them off that list and nothing else: the histogram
  line has counts and no paths, and `Declared in` is **not** sourced from
  Doxygen tags, a fresh grep, or anything else back in the source tree - this
  phase assembles, it does not measure. A name the `sort -u` list shows on two
  rows is one entity per row, each with its own file, which is how a homonym
  reaches the table honestly. `What it is` has no phase that
  produces it directly - Phase 3 outputs grep histograms and findings about
  synonyms, homonyms, and buckets, none of which gloss an individual entity
  that isn't itself the subject of a finding. Source it from the ledger's
  `Notes` for this domain when that already names the entity's role, or from
  a naming or verdict finding that discusses it by name; an entity neither
  source glosses gets a blank cell rather than an invented sentence - gate
  10 forbids the impression, not the blank. `Also called` is
  filled **only** from a confirmed synonym finding in `## Naming findings`
  that names this entity - the finding's `Evidence` already lists the other
  words and the files that use each; copy the other words in, not the file
  list. An entity with no synonym finding gets a blank cell there too, which
  is the normal, honest state of a table cell and not the stub gate 10 is
  about - the heading above the table already earned its place from the
  grep hit.
- **How it talks to other domains** exists only when the Repo map's
  Cross-boundary includes, Blast radius, or Target links tables show at
  least one row touching this domain - its directory on either side of an
  include row, or one of its own targets naming another domain's target
  (or being named by one) in a Target links row. Coupling through either
  kind counts; a domain with link rows but no include rows, or the reverse,
  still gets the section, built from whichever kind it has. Zero rows
  across all three tables is a real, evidence-backed fact - the domain is a
  leaf - but the only sentence available to say it is "this domain talks to
  nothing," which is exactly the filler gate 10 forbids. Cut the section
  instead; the same fact is already visible to anyone reading the Repo map
  itself.
- **Invariants** exists only when something already on the page names a
  rule the code enforces - a Naming or Verdict finding, a Repo map fact,
  something the domain ledger's Notes already said. Nothing upstream of
  Phase 5 exists specifically to mine invariants out of the code, so most
  domains will not have this section on their first document, and that is
  correct: an invented invariant is worse than a missing section, because
  the next agent will trust it and design against a rule nobody enforces.
- **Known rot** exists only when this domain has something tied to it in
  either place a finding can be tied to a domain: a `## Refactor backlog`
  row whose `Domain` column names it, or a finding id listed on the
  `Findings:` line of this domain's own `### <domain>` entry under
  `## Naming findings`. The finding format itself carries no `Domain` field -
  `Task`, `Failure`, `Evidence`, `Fix`, `Disposition`, and severity are all it
  has - so domain membership for a naming finding comes from that `Findings:`
  pointer line and nowhere else. **Do not look for finding blocks sitting
  under the domain heading; none ever do.** Phase 3 puts every `F<N>` block in
  a sibling `### Findings` subsection and leaves the domain's own heading
  carrying the pointer line alone, so a document written by scanning under the
  heading finds nothing and silently drops the section - throwing away exactly
  the output this audit ran to produce. List matches by id
  (`F<n>`) and one line of why, don't restate the whole finding. A domain
  with nothing open here is good news that speaks for itself in the ledger
  and the backlog - it does not need a section to repeat it.
