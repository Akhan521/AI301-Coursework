# Evidence guide: where evidence lives in a PR package

A PR package is a candidate pull request (title, description, commits,
diff, test evidence) read against the plan it implements, the issue
that plan belongs to, and the repo's stated standards. Every family
below names two places to look:

- **Snapshot**: a frozen bundle holding the whole world as text. Its
  sections are the repo facts (the PR template's asks, the
  contributing guide's asks, any AI-use policy), the issue and its
  thread highlights, the plan context (the accepted plan with the
  reproduction evidence it built on), and the candidate PR (title,
  description, commits, diff, test evidence). Use only its text and
  fetch nothing.
- **Live**: the same evidence for a real pull request before it is
  opened. The plan is the author's plan file (for example `plan.md`,
  with its Deviations section) or, failing that, the plan comment
  they posted on the issue. The diff is `git diff <default>...HEAD`
  and the commits are `git log --oneline <default>..HEAD`, run in the
  working copy (or `gh pr diff` for a draft already open). The title
  and description are the author's draft (for example `pr_draft.md`,
  title on the first line) or the open draft's body. The test
  evidence is the author's captured output (for example
  `test_evidence.md`) plus anything pasted into the description. The
  issue and thread come from
  `gh issue view <n> -R <owner/repo> --comments`. The repo's
  standards are `.github/PULL_REQUEST_TEMPLATE.md`, the contributing
  guide (`docs/`, the root, or `.github/`), any AI policy file, and
  the gates the repo enforces (`.pre-commit-config.yaml`, the jobs in
  `.github/workflows/`, the check targets in the `Makefile` or
  equivalent), all read from the upstream default branch. Any other
  forge has equivalents.

The package is what these sources contain. A claim the description
makes about evidence that appears nowhere in the package is graded as
if that evidence were absent.

## Plan fidelity (harness category: silent-drift)

- **Where it lives.** Snapshot: the plan context's committed changes,
  its Files list, its scope and not-in-scope lines, and any deferral
  or deviation note, set against every hunk of the candidate PR's
  diff and the commit list; and the description's claims about the
  change, set against the same diff. Live: the plan file (for example
  `plan.md`, or the posted plan comment) and its Deviations section,
  set against `git diff <default>...HEAD`, and the draft description
  (for example `pr_draft.md`) set against that diff.
- **What good looks like.** Every functional hunk maps to a planned
  change, its test, something it needs, or an edit the repo requires
  (a gate-forced fix on a touched file, a changelog entry, removing a
  marker that tracks the bug). Every item the plan commits to shows
  up in the diff, or the plan's notes and the description say it was
  left out and why. The description's account of the diff survives a
  hunk-by-hunk read: each file, test, or entry it names is there, and
  "nothing beyond the plan" is true.
- **Drift in each direction.** More than the plan: a commit or hunk
  touching a file the plan never names, a new option or flag, a
  refactor riding along with the fix, a dependency bump. Less than
  the plan: a planned test, doc, or second site missing with no note.
  A description that claims either fidelity or a deliverable the diff
  contradicts is drift too, however polished the rest is.

## Test evidence (harness category: not-tested)

- **Where it lives.** Snapshot: the candidate PR's test-evidence
  section (and any evidence in the description), set against the plan
  context's test plan (each trigger it will re-run and each
  expected-after result), the reproduction's trigger and recorded
  failure, and the check commands the repo facts name. Live: the
  author's captured output (for example `test_evidence.md`) and the
  draft description, set against the
  plan's test plan, the reproduction the plan built on, and the check
  commands in the contributing guide, PR template, and `Makefile`.
- **What good looks like.** The reproduction's own trigger is re-run
  on the branch: the command, input, or UI steps are named and the
  observed result is shown (output, value, exit code, exact on-screen
  text, or a screenshot), matching the plan's expected-after and
  differing from the recorded failure. Every failure the
  reproduction recorded gets the same treatment, or the description
  says which one was not re-run and why. Other outcomes the test plan
  names (a new behavior or edge case the change adds) can be shown
  the same way or pinned by an added test reported passing. The repo's named checks appear with their
  outcomes; a check that failed or could not run is reported with its
  output and reason, which is honest evidence. A strong PR adds a
  test that drives the defect's trigger and would fail without the
  fix.
- **Not decisive.** A sentence asserting success with nothing
  observed; evidence from a control or a different input than the one
  that failed; one recorded failure of several re-run with the rest
  silent;
  "tests pass" with no command named; a test that exercises code the
  change does not touch.

## Diff quality (harness category: unreviewable)

- **Where it lives.** Snapshot: the candidate PR's unified diff, line
  by line, with the commit list as a hint to where debris sits.
  Live: `git diff <default>...HEAD` and
  `git log --oneline <default>..HEAD`.
- **What good looks like.** A reviewer reading the diff sees the fix,
  its tests, and its required companions, and nothing else to read
  past. Comments that explain the change are welcome. Formatting a
  repo gate forces on lines the change already edits is expected.
- **Debris tells.** Print, log, or trace lines left from debugging
  (live or commented out); commented-out code, including earlier
  attempts at the fix; functions or variables added and never used,
  or kept alive with a lint suppression; a TODO or note to self the
  change does not resolve; blocks whose only change is whitespace,
  re-indentation, or import order; commit messages that describe the
  author's process rather than a change.

## Standards and comms (harness category: standards-wall)

- **Where it lives.** Snapshot: the repo facts' PR-template asks,
  contributing-guide asks, and AI-use policy, and the thread
  highlights (with each commenter's role), set against the candidate
  PR's title, description, commits, and diff. Live: the PR template,
  the contributing guide, any AI policy file, and the issue thread
  (roles from `gh issue view --json comments`), set against the draft
  title and description and the branch's commits and diff. In live
  mode, the scope file's house rules count as stated asks too.
- **What good looks like.** Every section the template lists is
  present with real content or marked not applicable with a reason,
  and every checklist item is answered truthfully. The description
  names the issue it resolves in the form the repo asks for. Stated
  conventions (title or commit format, a changelog or release-note
  entry) are followed. Where the repo's policy asks for AI-use
  disclosure, the description says what tool was used and how far,
  or that none was. Explicit maintainer direction in the thread is
  followed, or the description says why the PR diverges.
- **Visibly ignored.** A template section deleted or left as
  placeholder text; a required checklist missing; no issue reference;
  a required changelog entry absent with no reason; a policy that
  asks for disclosure met with silence. (Whether the description's
  claims match the diff belongs to plan fidelity, above.)
