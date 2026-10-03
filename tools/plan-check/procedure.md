# Procedure: how this skill grades a plan package

These steps grade a plan someone else wrote. They do not make or
improve one. Follow them in order. Wherever a step says "record", write
the note down in your working notes before moving on, because later
steps grade against those notes.

## Read order

Read the evidence before the plan, so the plan is held against what
you already know rather than read on its own terms. A confident plan
read first anchors the reader; a plan read after the reproduction gets
checked.

1. **Rules of the room.** Read `rubric.md` and list every check with
   its weight, plus the verdict rule. In live mode, first read
   `scope.md`, confirm the issue's repo is inside the scope, and stop
   without grading if it is not (or if the scope's repo line is still a
   placeholder). Then read `voice-guide.md`.
2. **The issue.** Read the issue title and body. Record the reported
   behavior in one line (input or trigger, then the wrong result) and
   the expected behavior in one line.
3. **The thread.** Read every thread comment in order. For each comment
   by a maintainer, member, collaborator, or owner, record any
   direction on the fix, isolated culprit, rejected approach, or
   "works as intended" call, with the author and date. Separately,
   record any open pull request, posted patch, or test build for this
   issue, whoever posted it. If there is none of either, record "thread:
   no direction, no competing work".
4. **The repo's rules.** Read the contribution policy (the snapshot's
   repo-facts block, or live, CONTRIBUTING, any AI policy file, and the
   PR template). Record two things: (a) the AI-use disclosure rule, in
   one of three forms: "comments must disclose", "PRs only / own
   words", or "none stated"; (b) anything the guide asks of every fix
   (tests, marker removal, docs, changelog).
5. **The reproduction.** Read the reproduction evidence end to end. For
   the failing run, record the exact trigger and the recorded output.
   For each control run and each intermediate step, record what was
   changed and what was observed, then write one line: "a correct cause
   must predict: <observation>". These lines are the diagnosis test.
6. **The plan.** Read the candidate plan. Record its stated cause
   (verbatim), its list of committed changes (one line each, including
   "while in there" items and anything deferred), its files and code
   paths, its test plan's expected outcomes, its stated risks and
   unknowns, and any Deviations section.
7. **The comment.** Read the plan comment last, as a thread reader
   would, without the plan beside it. Record what it says will change,
   where, how it will be checked, what it commits to, and whether it
   mentions AI use.

## Evidence gathering

For each check, gather from the notes above, and go back to the source
only for the quote. In live mode, fetch with the commands in
`references/evidence-guide.md`. In snapshot mode, quote only the
bundle text.

1. `diagnosis-fits-evidence`: put the plan's cause (step 6) next to
   every "a correct cause must predict" line (step 5). For each line,
   record whether the plan's cause predicts that observation, predicts
   something else, or says nothing about it.
2. `fix-acts-on-cause`: put the committed changes (step 6) next to the
   plan's cause. Record which change, if any, alters the code path the
   cause names. Also record whether any maintainer note (step 3) says
   the behavior is intended or the fix belongs elsewhere.
3. `scope-bounded`: take each committed change (step 6) and apply the
   removal test against the reported behavior (step 2). Mark it
   "needed", "deferred", or "extra", and quote the plan's words for
   every "extra".
4. `stranger-can-start`: record the named file, function, or code path
   for each change, or "none". Record any place the plan leaves a
   choice open or plans only investigation, quoting it.
5. `test-decisive`: record each expected outcome in the test plan.
   Mark each one "observable and differs from the recorded failure",
   "suite or no-regression only", or "no observable form".
6. `claims-backed`: list every statement in the plan or comment that
   something is verified, confirmed, guaranteed, or will be achieved,
   leaving out the cause statement. Beside each, record the package
   evidence that backs it, the label that marks it as an assumption, or
   "unbacked".
7. `comment-carries-plan`: from step 7's notes, record whether the
   comment alone says what, where, and how-checked. Then compare its
   commitments with step 6's change list and record anything it
   promises that the plan lacks.
8. `follows-thread`: for each item recorded in step 3, record whether
   the comment follows it, names it and explains how the plan relates,
   or ignores it.
9. `ai-disclosure-met`: put the disclosure rule (step 4a) next to step
   7's AI note.
10. `controls-retested`: record whether the test plan re-runs any
    control from step 5.
11. `repo-asks-planned`: put step 4b's asks next to the plan's changes
    and test plan, and record which are covered.

## Check execution

1. Grade the checks in the rubric's table order. Grade every check,
   even after a required check has failed, so the author sees every
   problem in one pass.
2. Each check reads only its own evidence row and its own pass
   condition. Do not let a failure on one check lower another check's
   grade. For example, a wrong cause does not fail `fix-acts-on-cause`
   if the change acts on the cause the plan states.
3. Grade `pass` or `fail` by matching the gathered notes to the pass
   condition's wording. When a pass condition lists failure examples,
   the case does not need to match one literally. It fails if it does
   the same thing.
4. When the evidence a check needs is absent, decide which kind of
   absence it is:
   - The source has nothing to offer (no thread comments, no AI
     policy, no controls in the reproduction). Apply the check's
     stated rule for that case. Most such cases pass, because there is
     nothing to contradict, follow, or disclose.
   - The plan or comment leaves out what the check grades (no cause,
     no files, no test plan). Grade `fail`. The author owed it.
   - The package itself contradicts itself or is unreadable at that
     point, or a live fetch failed. Grade `unclear` and say which.
5. Write a one-line evidence note for every grade that quotes or names
   the deciding fact: the control line the cause contradicts, the
   "extra" item, the missing file, the vague test phrase, the ignored
   maintainer comment. "Looks fine" is not a note.
6. Do not reread the whole package for a check whose notes already
   settle it. Go back only to fetch the quote for the evidence note, or
   when the notes conflict.

## Verdict assembly

1. Apply the rubric's verdict rule to the grades of the checks marked
   `required` in the table. Read the weights from the table at grading
   time; do not rely on a remembered list.
2. Count `unclear` on a required check as `fail`, as the verdict rule
   states.
3. The verdict is `accept` only if every required check is `pass`.
   Otherwise it is `reject`.
4. In the readable summary, give one line per check with its grade. For
   a `reject`, put each failing required check first, with its evidence
   note. For an `accept`, name any failing preferred check as what
   would make the plan stronger.
5. In live mode, after the verdict, hold the draft comment against
   `voice-guide.md` and list any rule it breaks, quoting the rule. This
   never changes the verdict.
6. If any step of this procedure did not fit the package (a gap), say
   so in one line in the summary instead of improvising silently.
7. End with the JSON block in the format `SKILL.md` specifies, with one
   entry per rubric check in table order, and nothing after it.
