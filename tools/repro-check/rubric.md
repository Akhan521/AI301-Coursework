# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment record (version of the software under test, OS/platform, install or build method), read against the issue body and thread for any factor they say decides whether or how the bug appears. | Passes if the report names the version of the software under test and the OS/platform it ran on, AND records every factor the issue or thread marks as decisive for this bug (for example a driver, a debug vs release build, a browser language setting, a shell). A terse one-line record passes if it covers these. Fails if there is no environment record, or if a factor the issue marks as decisive is missing, because a stranger then cannot place the attempt. | required |
| target-version-faithful | The version, build, or channel named in the environment record, read against what the issue targets: the reporter's version, any "confirmed on latest / main" note, and the build the maintainers are discussing in the thread. | Passes if the report tested the issue's version, a newer release, or the branch the issue targets; or tested something different and says so plainly without treating the result as proof about the target. Fails if it tested an older version, or a different build or channel than the one the issue reports, and presents the result as confirming the reported bug without acknowledging the difference. | required |
| steps-rerunnable | The repro report's steps, from starting state through the trigger. | Passes if a stranger with only public resources could re-run the attempt: every command, input, file content, and setting needed is shown, taken verbatim from the issue with a pointer to it, or described precisely enough to rebuild, including the exact element that triggers the bug (for example "the default config file with one unrecognized top-level key added" is rebuildable; "our usual config" is not). Fails if the steps depend on private code, config, or data the reader cannot get, or if a step needed to reach the trigger is left out or only gestured at ("set up the project"). | required |
| trigger-faithful | The exact command, input, or configuration the report ran, compared token by token with the trigger the issue describes (syntax form, flags, input values, and any extra condition the thread adds). | Passes if the report ran the issue's trigger as described, or a variation it explicitly names and explains as still exercising the same trigger. Fails if the command or input differs from the issue's trigger in a way that changes which code path runs (a different syntax form, a swapped operator, a removed binding, a dropped flag) and the report does not acknowledge the difference. | required |
| artifact-shows-behavior | The artifacts in the repro report (output excerpts, logs, tracebacks, exit codes, captured output), read against the specific behavior the issue describes (its error type and message, exit code, wrong value, or symptom). | Passes if at least one captured artifact shows the issue's specific behavior (the same error class or message, the same wrong output, the same symptom), or, for a report that says it could NOT reproduce, shows the real output of a genuine attempt at the trigger. Fails if there is no artifact, if the artifacts only show that the software runs, or if they show a different behavior from the issue's (a graceful validation error where the issue reports a panic, a compile error where it reports a runtime error, garbled output where it reports a crash), whatever the prose around them says. | required |
| outcome-honest | The central claims of result in the repro report and claim comment (whether the bug reproduced, what it confirms, any root cause, the scope it applies to, stated certainty), read against what the artifacts actually show. | Passes if each central claim is backed by a shown artifact, and a cannot-reproduce says what was tried and what differed from the issue's conditions. A supporting observation given with its exact command (for example a control run described in prose) does not need its output pasted to be honest; `control-run` is where pasting it counts. Fails if the words claim more than the artifacts show: confirmation narrated over an artifact of a different behavior, a root cause stated as verified with nothing shown, certainty ("guaranteed", "ran it ten times") standing in for evidence, or a generalization to a version, platform, or channel that was not shown. | required |
| claim-specific | The claim comment, read against the issue. | Passes if the claim names something true only of this issue (its specific symptom, the component, function, or file involved, a version, or a pointer from the thread) AND states a concrete next investigative step. Fails if it is a +1 or me-too, a bare assign-me request, or wording that could be pasted onto any other issue unchanged. | required |
| claim-promises-honestly | The claim comment's commitments, and any statement in it about work already done. | Passes if it commits only to work the author controls (investigating, reproducing, reporting back what they find), and any statement that it has already reproduced is backed by a repro report in the same package; a claim posted before its report promises the report rather than asserting a result. Stating an intent to work toward a fix is fine. Fails if it commits to delivering a fix, names a deadline, guarantees an outcome, asks maintainers to assign or reserve the issue, assigns itself, or asserts a reproduction or diagnosis with no report behind it. | required |
| ai-disclosure-met | The repo's stated contribution policy and any AI-use policy it links (CONTRIBUTING, an AI policy file, issue or PR templates), read against the text of the claim comment and repro report. | If the policy requires disclosing AI use in issues or comments, or in any form, passes only if the comments disclose it (the tool or kind of tool, and its extent) or state plainly that no AI was used; silence fails, because a reader cannot tell which is true. If the policy asks for disclosure only in pull requests, only requires that comments be written or reviewed by a human in their own words, or states no AI policy, passes when the comments read as the author's own specific words; no disclosure line is needed. | required |
| template-asks-covered | The repo's bug-report template or contributing guide's asks for a bug report, read against the repro report. | Passes if the report supplies each item the template asks for that applies to a reproduction (version, OS, install method, input, command, expected and actual behavior). | preferred |
| control-run | The artifacts in the repro report. | Passes if the report shows a control run: the same setup with the trigger removed or changed, behaving correctly, which isolates the trigger. | preferred |

## Verdict rule

Accept only if every required check (`env-recorded`,
`target-version-faithful`, `steps-rerunnable`, `trigger-faithful`,
`artifact-shows-behavior`, `outcome-honest`, `claim-specific`,
`claim-promises-honestly`, `ai-disclosure-met`) grades `pass`. Reject
if any required check grades `fail`. `unclear` on a required check
counts as `fail`: proof that cannot be verified is not ready to post.

`template-asks-covered` and `control-run` never change the verdict;
report them so the author knows what would make a passing report
stronger.

When a claim comment is graded on its own, before any repro report
exists, only `claim-specific`, `claim-promises-honestly`, and
`ai-disclosure-met` are graded; every other check reports `unclear`
with "not yet applicable: claim-only draft" and is left out of the
verdict. The verdict then answers only whether the claim is ready to
post.
