# Procedure: how this tool grades a PR package

These steps grade a pull request; they do not fix or improve it.
Follow them in order. Wherever a step says "record", write the note
down in your working notes before moving on, because later steps grade
against those notes.

## Read order

Read what the pull request was supposed to be before reading what it
says about itself. The plan and the repo's standards come first, then
the diff and the evidence, and the description last. A description
read first anchors you on its own account of the change; a diff read
against the plan gets checked, and a description read after the diff
gets checked too.

1. **Rules of the room.** Read `rubric.md` and list every check with
   its weight, plus the verdict rule. In live mode, first read
   `scope.md` and stop without grading if its `Repo:` line is a
   placeholder or the pull request targets another repo, as
   `SKILL.md` directs.
2. **The issue and thread.** Record the reported behavior (trigger,
   then wrong result) and the expected behavior, one line each. For
   each comment by a maintainer (OWNER, MEMBER, or COLLABORATOR),
   record any direction on the fix (a requested or rejected approach,
   a stated constraint, a call that behavior is intended) with its
   author and date. If there is none, record "no maintainer
   direction".
3. **The repo's standards.** Record four things:
   (a) every section and checklist item in the PR template;
   (b) every other ask the contributing guide makes of a pull request:
   the issue-reference format, title or commit conventions, and
   required changelog or release-note entries;
   (c) every check the repo names for a change, and every gate it
   enforces. Live, read the guide's commands, `.pre-commit-config.yaml`,
   the jobs under `.github/workflows/`, and the `Makefile` targets.
   In a snapshot, record what the repo facts name, or "none named";
   (d) the AI-use policy, in one of three forms: "PRs must disclose",
   "own words or AI welcome, no disclosure asked", or "none stated".
   In live mode, add the scope file's house rules to (a) and (b).
4. **The plan.** Record its core change (the change at the code path
   where the defect lives) and every item it commits to, one line
   each: changes, sites, tests, docs, entries. Record its stated
   boundary and not-in-scope lines, each deferral or Deviations entry
   with its reason, and its test plan as a list: each trigger it will
   re-run with its expected-after result, then each suite or check it
   names.
5. **The reproduction.** Record the failing trigger with its recorded
   output, and each control run with what it showed.
6. **The diff.** Read it hunk by hunk with the commit list beside it.
   For each hunk, record its file, one line on what it does, and one
   tag: *functional* (changes behavior, names, structure,
   configuration, dependencies, or docs), *test*, *entry* (a changelog
   or release-note entry, or removing a marker or suppression), or
   *debris* (debug output, commented-out code, code never used, a
   TODO or note to self, or a block whose only change is formatting,
   whitespace, or import order). Do not compare with the plan yet.
7. **The test evidence.** For each run shown, record what was run,
   on which code (before, after, or a control), and the observed
   result as written. For each check outcome, record the command and
   the outcome as written.
8. **The title and description, last.** Record each statement about
   what the diff contains or how far it goes, each disclosure (a
   shortfall, a deviation, AI use), the issue reference, and each
   template section as "filled", "placeholder", or "missing".

## Evidence gathering

Build each side-by-side below from the notes; go back to the source
only to fetch a quote. In live mode, fetch with the commands in
`references/evidence-guide.md`; in snapshot mode, quote only the
bundle text. Each check's Evidence column in `rubric.md` names the
side-by-side it reads.

1. **Diff against the plan.** For each *functional* hunk (step 6),
   name the plan item it delivers (step 4), or mark it: "test",
   "needed for" (say what for), "gate-forced" (name the gate from
   step 3c), "repo-required entry" (from step 3b), "recorded
   deviation" (quote the note and its reason), or "unplanned" (quote
   the hunk). *Debris* hunks are not matched here.
2. **The plan against the diff.** For each item the plan commits to
   (step 4), other than changelog or release-note entries, record
   "present" (name the hunk), "present elsewhere" (an equivalent
   place doing the same job), "disclosed absent" (quote the note), or
   "missing".
3. **The description against the diff.** For each statement from
   step 8 about the diff's contents or reach, record "holds",
   "imprecise summary" (a count or paraphrase of something the diff
   does contain), or "misstated" (quote the diff fact that shows it).
4. **The evidence against the test plan.** For each item in the
   plan's test plan (step 4) other than a control run, first mark it
   "recorded failure" (a failure the reproduction recorded, step 5)
   or "other outcome" (a new behavior or edge case the change adds).
   Then record one of:
   "re-run, specific result" (quote it), "pinned by an added test
   reported passing" (name the test; counts only for an other
   outcome), "asserted, nothing observed", "only a control or another
   path", "not re-run, disclosed", or "silently absent".
5. **Check outcomes against the named checks.** For each check from
   step 3c and each suite the plan names, record "outcome stated"
   (quote it), "reported failing or unable, with reason", "no
   outcome", or "reported passing against the evidence". Mark checks
   that run only after the pull request opens (CI) "not expected
   yet".
6. **The debris list.** List every *debris* hunk from step 6, quoting
   its line.
7. **The standards against the pull request.** For each item from
   steps 3a and 3b, record "met", "marked not applicable with a
   reason", "placeholder", "missing", or "broken". Record whether the
   description names the issue it resolves.
8. **The disclosure against the policy.** Put the policy form (step
   3d) next to the AI-use disclosure, if any (step 8).
9. **The tests against the trigger.** For each test the diff adds or
   changes, record whether it drives the reproduction's trigger
   (step 5) and asserts the fixed outcome.
10. **Thread direction against the pull request.** For each
    maintainer direction from step 2, record "followed", "diverged
    with a reason given", or "ignored".

## Check execution

1. Grade the checks in the rubric's table order. Grade every check,
   even after a required check has failed, so the author sees every
   problem in one pass.
2. For each check, take the side-by-side its Evidence column names
   and match those notes to its pass condition's wording. When a pass
   condition gives failure examples, they illustrate the rule: a case
   fails if it does the same thing, whether or not it matches an
   example literally.
3. Each check reads only its own evidence and its own pass condition.
   A failure on one check never lowers another check's grade, and
   where a pass condition says something is not its question, do not
   count it there.
4. When the evidence a check needs is absent, decide which kind of
   absence it is:
   - The source has nothing to offer (no template, no AI policy, no
     maintainer comments, no checks named). Apply the check's stated
     rule for that case; most such cases pass.
   - The pull request leaves out what the check grades (no test
     evidence, no description, no re-run). Grade `fail`: the author
     owed it.
   - The package contradicts itself or is unreadable at that point,
     or a live fetch failed. Grade `unclear` and say which.
5. Write a one-line evidence note for every grade that quotes or names
   the deciding fact: the unplanned hunk, the missing item, the
   contradicted claim beside the diff fact, the trigger with no
   observed result, the debris line, the missing section.
6. Do not reread the whole package for a check whose side-by-side
   already settles it. Go back only to fetch the quote for the
   evidence note, or when two notes conflict.

## Verdict assembly

1. Read each check's weight from the table at grading time; do not
   rely on a remembered list.
2. Apply the rubric's verdict rule, counting `unclear` on a required
   check as `fail`.
3. The verdict is `accept` only if every required check is `pass`;
   otherwise it is `reject`.
4. On a `reject`, the deciding check is the first failing or unclear
   required check in table order. Put it first in the summary with its
   evidence note, then the other failing required checks in table
   order. On an `accept`, name any failing preferred check as what
   would make the pull request stronger.
5. In live mode, after the verdict, hold the draft title and
   description against `voice-guide.md` as `SKILL.md` directs, and
   list any rule they break. This never changes the verdict.
6. If any step of this procedure did not fit the package, say so in
   one line in the summary instead of improvising silently.
7. End with the JSON block in the format `SKILL.md` specifies: one
   entry per rubric check in table order, and nothing after it.
