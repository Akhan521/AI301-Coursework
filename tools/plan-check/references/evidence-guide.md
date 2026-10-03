# Evidence guide: where evidence lives in a plan package

A plan package is a candidate plan and a draft plan comment, read
against the issue they belong to and the reproduction the plan builds
on. Every family below names two places to look:

- **Snapshot**: a frozen bundle holding the whole world as text. Its
  sections are the repo facts (stated templates and contribution
  policy), the issue, the thread highlights, the reproduction evidence,
  the candidate plan, and the candidate plan comment. When grading a
  snapshot, use only its text and fetch nothing.
- **Live**: the same evidence on a real issue. The issue body and
  thread are at `gh issue view <n> -R <owner/repo> --comments`. Open
  pull requests come from `gh pr list -R <owner/repo> --search "<n>"`
  and the issue's linked PRs. The repo's policies are in
  `CONTRIBUTING.md` (root, `docs/`, or `.github/`), any AI policy file,
  and `.github/PULL_REQUEST_TEMPLATE.md`. The reproduction is the
  author's posted repro comment on the thread. The plan and comment are
  the author's drafts. Any other forge has equivalents.

The package is only what the drafts contain and quote. A plan that
leans on evidence it never quotes or links is graded as if that
evidence were absent.

## Diagnosis and grounding

- Where it lives. Snapshot: the cause statement at the top of the
  candidate plan (often headed Diagnosis or Cause), and the cause
  sentence in the plan comment, set against the reproduction evidence's
  failing run, its control runs, and any step that shows where the
  behavior first appears (a debug trace, an intermediate value, a
  timing matrix). Live: the draft plan's cause, set against the
  author's posted repro comment (its output, its controls), plus any
  cause analysis in the issue body or thread.
- What good looks like: the cause explains the failing output, and
  every control comes out the way that cause predicts. If a control
  holds the blamed component constant and the bug disappears, or keeps
  the bug while removing the blamed component, the cause is wrong
  whatever the prose says. A cause borrowed from the thread is fine
  when the reproduction does not contradict it.
- Red flags: "the X is a red herring" when X is the only variable a
  control changed; a cause stated with no reference to the
  reproduction at all; a confident thread claim adopted although the
  reproduction's own numbers point elsewhere.

## Scope

- Where it lives. Snapshot: the plan's change list, its in-scope and
  not-in-scope lines, its files and areas, and any "while in there" or
  "also" items, set against the behavior the issue reports. Live: the
  same parts of the draft plan, set against the issue body.
- What good looks like: one bounded change. Each listed item is needed
  to fix or verify the reported behavior (removal test: delete the
  item, and if the bug is still fixed and tested, the item was extra).
  Adjacent problems are named and deferred, not folded in. Regression
  tests, removing markers that track the bug, and docs for changed
  behavior belong in the change.
- Red flags: "rather than patch the one site, rebuild the whole
  subsystem"; dependency bumps, migrations, new options or settings, new
  abstractions, CI overhauls; "since we're touching this anyway".

## Executability

- Where it lives. Snapshot: the plan's Files list and its Approach or
  Change steps. Live: the same in the draft plan, checked against the
  repo tree (the named paths and functions should exist on the default
  branch).
- What good looks like: the plan names a file, function, or code path
  and the one change it makes there, in an order a stranger could
  follow without messaging the author. A remaining unknown about the
  exact line, or which of two adjacent layers holds it, is fine if the
  plan says how it will be pinned (a debug log, a trace, a failing
  test).
- Red flags: no files at all; "somewhere"; "A or B, whichever is
  easier"; numbered steps that are all investigate, look into, or
  explore; "fix it once the cause is clear".

## Test plan

- Where it lives. Snapshot: the plan's Test plan, set against the
  reproduction evidence's steps, commands, and recorded output. Live:
  the draft plan's test plan, set against the posted repro comment's
  commands and output.
- What good looks like: the reproduction's trigger is re-run (by hand
  or encoded as a test), and the plan states what will be observed
  after the fix: the output line, value, exit code, threshold, or
  visible state, which differs from what the reproduction recorded.
  Strong plans also re-run a control and expect it unchanged.
- Red flags: "the full suite passes" as the only check; "should feel
  faster" or "should work"; a test plan that never touches the
  reproduction's trigger.

## Honesty

- Where it lives. Snapshot: the plan's Risks or Unknowns, any "I have
  verified" or "confirmed" statements in the plan and comment, and
  promises about what the change achieves. Live: the same in the
  drafts, plus the plan's Deviations section after the build (what
  changed from the posted plan and why).
- What good looks like: what was verified is backed by output in the
  package; what was not is labeled as an assumption or open question,
  once, with how it will be settled. Saying "none identified" is honest
  when nothing in the plan rests on an unchecked assumption. A
  deviation recorded with its reason is honest work. A deviation that
  shows only in the diff is not.
- Red flags: "all N tools support this" with only some checked;
  "eliminates the whole category"; "guaranteed"; a build that changed
  course while the plan still describes the original approach.

## Comms

- Where it lives. Snapshot: the candidate plan comment, set against the
  thread highlights (who said what, and their role: OWNER, MEMBER,
  COLLABORATOR, CONTRIBUTOR, or NONE) and the repo-facts block's
  contribution policy and AI-use policy. Live: the draft comment, set
  against every comment on the thread (`gh issue view --comments`, with
  `authorAssociation` from `--json comments` for roles), open or linked
  PRs, and the repo's CONTRIBUTING and AI policy files.
- What good looks like: the comment stands alone as the plan in short
  form (what changes, where, how it will be checked) in the author's
  own words. It engages what the maintainers already said: it follows
  a stated direction or isolated culprit, or names it and explains the
  divergence, and it acknowledges any open PR, patch, or test build
  instead of racing it. If the repo's policy requires AI disclosure for
  comments (or "in any form"), the comment says what tool was used and
  how far, or says no AI was used.
- Red flags: a comment that could be pasted onto any issue; a plan that
  heads somewhere other than a maintainer's posted fix without saying
  why; silence on an open PR for the same fix; no disclosure where the
  policy asks for it; "same approach as above".
