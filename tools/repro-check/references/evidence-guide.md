# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

Every family below names two places to look. In an eval bundle, the
bundle is the whole world. For a real issue, the "live" location is
where the same proof sits on the issue tracker and in the repo itself;
the `gh` commands are GitHub's, and any other forge has equivalents.

## Environment

- Where it lives. Eval bundle: the environment line or block at the
  top of the candidate repro report, set against the issue section
  (the reporter's version, OS, and install method) and the thread
  highlights (maintainer notes such as "only in Release builds" or
  "confirmed on main"). Live: the draft repro comment's environment
  block, set against the issue body, its comments, and the version the
  repo's default branch or latest release is on (`gh issue view`,
  `gh release list`, the commit SHA of `main`).
- What good looks like: the software's version (or commit SHA) and the
  OS/platform are named, plus every factor the issue or thread says
  changes the behavior (driver, build profile, browser language,
  shell, dependency version). The version tested is the issue's, a
  newer one, or the branch the issue targets; any other version or
  build is named as a difference, not passed off as the target.
- Red flags: no environment at all; an older version than the one the
  issue was confirmed on; a different release channel (Store vs a
  source build) with no mention that it differs.

## Steps

- Where it lives. Eval bundle: the steps, commands, and file contents
  inside the candidate repro report, compared with the "steps to
  reproduce" and trigger in the issue section. Live: the draft repro
  comment's steps, compared with the issue body's reproduction.
- What good looks like: starting from a clean state, every command,
  input file, and setting needed to reach the trigger is shown, or is
  quoted verbatim from the issue with a pointer. The command and input
  match the issue's trigger token for token (same syntax form, same
  operator, same flags, same values), or any change is named and
  explained as still reaching the same code path. A small input that is
  described rather than pasted still counts when the description is
  exact enough to rebuild it, and names the element that triggers the
  bug.
- Red flags: private repos, configs, or data ("our internal
  monorepo"); "set up the project" with no commands; a step the issue
  names as required (a driver flag, a `--replace`) silently dropped; an
  input that differs from the issue's by one character (`:` for `=`,
  a prefix range for an offset-from-end range).

## Behavior shown

- Where it lives. Eval bundle: the fenced output blocks, logs,
  tracebacks, exit codes, and quoted captures in the candidate repro
  report, read against the error message, exit code, wrong value, or
  symptom stated in the issue section. Live: the same inside the draft
  repro comment, read against the issue body's observed output.
- What good looks like: at least one artifact shows the issue's own
  behavior (same exception class and message, same wrong output, same
  exit code or symptom). A control run (same setup, trigger removed,
  correct output) is a strong extra. For a cannot-reproduce, the
  artifact is the real output of the attempt.
- Adjacent behavior is not the issue's behavior: a graceful
  argument-validation error is not a panic, a compile error is not a
  runtime error, garbled output with the program still alive is not a
  crash, and a version banner or a running app is not the bug. Judge
  the artifact, not the sentence next to it.

## Honesty

- Where it lives: every sentence in the repro report's
  expected/actual/conclusion text and in the claim comment that states
  a result ("reproduced", "confirmed", "root cause is", "also affects"),
  each set against the artifact that is supposed to back it.
- What good looks like: each central claim (did it reproduce, what it
  confirms, any root cause, how far it generalizes) points at something
  shown. A side observation given with its exact command, such as a
  control run described in prose, is not overclaiming; it is just a
  thinner control.
  An honest cannot-reproduce states what was tried, shows its output,
  and names what differed from the issue's conditions; that is a
  complete, passing outcome.
- Red flags: "confirms the reported crash" over an artifact that shows
  something else; "I verified the race condition" with no transcript;
  certainty words ("guaranteed", "every single time", "ten runs")
  standing in for an artifact; widening the bug to a version, platform,
  or channel that was never shown.

## Comms

- Where it lives. Eval bundle: the candidate claim comment read
  against the issue section; both comments read against the repo-facts
  block's "bug reports" template line and "contribution policy" line
  (including any AI policy it names). Live: the draft claim comment
  against the issue; the repo's CONTRIBUTING file, any AI_POLICY file,
  and `.github/ISSUE_TEMPLATE/` for its asks.
- What specific looks like: the claim names something only this issue
  has (its symptom, a function, file, or component, a version, a
  thread pointer) and one concrete next step of investigation. What
  honest looks like: it promises investigation and a report, not a fix,
  a date, or an assignment; if the report is not posted yet, it
  promises the report instead of asserting a result.
- Boilerplate reads the same on any issue: "Kindly assign me", "I will
  fix it in 2 days", "+1, any update?", praise for the project in place
  of detail.
- AI disclosure: read the policy literally. If it requires disclosing
  AI use in issues, comments, or any form, a passing comment either
  discloses it (tool and extent) or says plainly that no AI was used;
  saying nothing fails. A policy that only covers pull requests, or only
  asks that comments be human-written in the author's own words, needs
  no disclosure line, and neither does a repo with no stated AI policy.
