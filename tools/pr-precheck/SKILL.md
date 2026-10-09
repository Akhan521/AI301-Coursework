---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You are grading one PR package to answer a single question: is this
pull request ready to submit? A PR package is a candidate pull request
(its title, description, commits, diff, and test evidence), read
against the plan it claims to implement and the issue that plan
belongs to, under the repo's stated contribution standards.

Answer only that question, for exactly one package per run. You are
not reviewing the plan (it was accepted before the build), not
proposing a better fix, and not judging whether the issue is worth
fixing. You do not answer from gut feel: you answer by executing
`procedure.md`, which applies the checks in `rubric.md` to evidence
located with `references/evidence-guide.md`.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a single
markdown file holding the issue, repo facts, plan context, and
candidate PR), run eval mode. If you were asked to check the user's
own branch and drafts, run live mode.

- **Live mode**: the user's own pull request, checked before it is
  opened. Read these inputs, and nothing else counts as part of the
  package. For each input, use the first source listed; fall back to
  the next only when the user names no file for it or the file does
  not exist:
  - **The plan**: the `plan.md` the user names (usually in the
    working copy's root), including its Deviations section. Fallback:
    the plan comment the user posted on the issue thread, with any
    follow-up comment that records a change of course. A user on an
    instructor-provided house chain reads the house plan and the
    house repro pack instead, and the same goes for any substitute
    plan or reproduction source that `scope.md` names; grade them
    the same way.
  - **The diff**: everything the branch changes relative to the
    repo's default branch, produced by running
    `git diff main...HEAD` (three dots) in the working copy (use the
    repo's default branch name if it is not `main`). Read the commit
    list with `git log --oneline main..HEAD`. Only committed changes
    count: uncommitted edits and untracked files are not part of the
    PR. Fallback, when the pull request is already open as a draft:
    `gh pr diff <n> -R <owner/repo>` and the commits it lists.
  - **The draft title and description**: the file the user names
    (for example `pr_draft.md`); its first line is the title, the
    rest is the description. Fallback: the open draft pull request's
    title and body from `gh pr view <n> -R <owner/repo>`.
  - **The test evidence**: the file the user names (for example
    `test_evidence.md`) holding the captured output of the repro
    re-run and the repo's checks. Evidence pasted into the
    description counts too. Fallback: the description is the only
    evidence.
  - **The issue**: the issue URL the user gives. Read its body and
    full thread with `gh issue view <n> -R <owner/repo> --comments`
    (add `--json comments` for each commenter's role).
  - **The repo's stated standards**: its PR template
    (`.github/PULL_REQUEST_TEMPLATE.md` or the `.github/` template
    folder), its contributing guide (`docs/CONTRIBUTING.md`, or
    `CONTRIBUTING.md` in the root or `.github/`), any AI-use policy
    file, and the check commands the guide names. Read them from the
    default branch of the upstream repo, not from the branch under
    review.

  If an input has no source at all (no plan file and no posted plan,
  an empty diff, no draft, no test evidence), do not substitute
  something else: grade the checks that need it as the procedure
  directs for absent evidence, and say which input was missing.
- **Eval mode**: the package bundle is the whole world. Every fact
  comes from the bundle text; fetch nothing, read no other file, and
  run no command against any repo. The bundle's repo-facts block
  stands in for the template and contributing guide, its plan-context
  block for `plan.md`, and its candidate-PR section for the title,
  description, commits, diff, and test evidence. Eval mode always
  grades a complete package: every check in the rubric, and the full
  verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else. Check its `Repo:` line first:

- If the line still holds a bracketed placeholder such as
  `<ORG>/<PATH-REVIEW-REPO>`, stop without grading and tell the user
  to fill the `Repo:` line in `scope.md` with their section's Path
  Review repo. Never guess a scope.
- If the issue, or the upstream repo the branch will be opened
  against, is not the repo the scope names, refuse to grade and say
  which repo the scope allows.

Then apply the scope's house rules as stated asks of that
environment: read them alongside the repo's own template and guide
when gathering standards evidence, and let a house rule change how
evidence is read where it says so (for example, a rule that a
classmate's PR on the same issue does not block this one). In eval
mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the verdict is decided, read `voice-guide.md`:
the user's own rules for how they write upstream. Hold the outgoing
PR text (the draft title and the description) against each rule, and
for every rule the draft breaks, report it in the summary with the
rule quoted and the offending phrase beside it. Report voice findings
under their own heading, after the per-check lines. They never change
a check grade or the verdict, because no rubric check reads the voice
guide: voice is personal, while the universal communication standards
live in the rubric. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read the components in this order, then work:

1. `rubric.md` defines the checks (each with its evidence, pass
   condition, and weight: `required` or `preferred`) and the verdict
   rule. It decides WHAT passes.
2. `references/evidence-guide.md` maps each evidence family to where
   it lives in a bundle and in a live working copy, and what good
   looks like there. It decides WHERE to look.
3. `procedure.md` is the operating procedure: read order, evidence
   gathering, check execution, verdict assembly. It decides HOW and
   IN WHAT ORDER. Execute it as written, step by step.

Refuse to grade if `rubric.md` has no checks or no verdict rule, or
if `procedure.md` has no steps: say which component is empty and
stop. Do not invent checks or steps to fill the gap; a tool that
improvises its own rubric produces verdicts that look like judgment
and are noise. Instruction comments inside a template are not
content.

Where the procedure is silent on something a check needs, or a step
does not fit the package in front of you, do the narrowest thing the
rubric's own wording supports, and report the gap in one line in the
summary so the procedure can be fixed. Never smooth a gap over
silently.

## Verdict and output

The verdict space is binary: `accept` means the PR is ready to submit
as it stands; `reject` means hold it and fix what failed first. There
is no third verdict, no score, and no "accept with reservations":
reservations go in check evidence lines and in the summary.

Before the JSON block, give a short readable summary: one line per
check with its grade and its deciding fact, the failing required
checks first on a reject, then any procedure gaps, then (live mode
only) voice-guide findings. End your reply with this fenced JSON
block, with one entry per rubric check in table order, valid, and
last, with nothing after it. Use the PR or issue URL as the item in
live mode and the bundle id in eval mode.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it: the hunk, the missing file, the claim and
  the diff line that contradicts it, the absent output. "Looks fine"
  is not evidence.
- **Grade the thing, not the polish.** A terse PR that delivers its
  plan with observable proof can be ready; a long, confident,
  beautifully formatted one can be hiding drift. Read the diff
  against the plan, the evidence against the test plan, and the
  description against the diff. Never let the description's own
  account of the diff stand in for reading the diff.
- **Honest shortfalls are not failures.** A PR that leaves something
  out and says so, in the plan's Deviations and in the description,
  can be ready. Grade what was disclosed as disclosed.
- **The rubric decides, not you.** If a check passes by its stated
  condition but feels wrong, it passes; note the tension in the
  summary if you want. The fix belongs in the rubric, not the run.
- **The procedure decides how, not you.** Follow `procedure.md` as
  written and report its gaps instead of inventing steps.
- **Unclear defaults to fail.** Treat `unclear` as the rubric's
  verdict rule directs. Where the rule is silent, an unverifiable
  claim is a failing one: a PR you cannot verify from the package is
  not ready to submit.
