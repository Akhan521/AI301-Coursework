# Contribution: Validate `core_rus7__yearly_balance_sheet_assets` and corresponding output table

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Aamir Khan
**GitHub Username:** Akhan521
**Issue:** https://github.com/catalyst-cooperative/pudl/issues/5566
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Turned my issue-select rubric into issue-scout, wrote its search strategy, and patched it after two real scans**

issue-scout is a Claude Code skill that searches GitHub with queries I wrote, ranks the results with my own filters, and grades the top five against my Unit 1 rubric; I aimed its queries at the orgs I care about (LLM evals, agents, ML tools, data tools) because keyword searches like "llm" mostly found tiny personal projects. My first scan only accepted 1 of its top 5 issues, because the other 4 already had someone's pull request open, and my rubric also missed an issue that someone was already fixing in their own fork. I fixed this by filtering out issues with linked pull requests, searching for labeled bugs and preferring issues that are a few months old, and updating two rubric checks, and my Unit 1 eval still scored 20/20 afterward. My second scan accepted all 5 with no pull request conflicts, but each one still had a problem my rubric can't see yet, like needing a GPU or only taking pull requests from approved contributors.

**Unit 6: Installed the voice guard with an upstream section, fixed how my skills find my guide, and wrote a scoped memory file for PUDL**

I added an Upstream section to my voice guide with rules for claims, questions, and pings, moved the guide to one home that the voice guard and my three earlier skills all read, and installed the guard as a hook; it stopped the `--help` wiring test, and then passed my real claim on pudl#5566 on its way out. Two things broke: first, repro-check kept saying it couldn't find my voice guide, because Claude Code's Glob tool doesn't see the symlinks the install creates, so I replaced them with the install guide's one-line pointer files. Second, my new rule asking for a report-back time like "this week" clashed with my Unit 2 rubric, which failed that phrase as a deadline in one run and passed it in another, so I reworded the rule to allow a report-back point tied to finishing my first step. For PUDL's memory file I left out the `AGENTS.md` import, since the repo's `CLAUDE.md` already points to it, and ran one orientation pass over just the RUS 7 dbt files; that pass showed me the tests pick rows by `asset_type` rather than `is_total`, and that `error_if` is an allowance for known failures, all in 55 lines kept out of git with `.git/info/exclude`.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

None of my Unit 5 picks survived this week. My scout accepted my lead, opensearch-benchmark#714, again after this week's scope patch, with an "ambitious" difficulty note, but reading the code showed it wasn't the rename the thread suggested: the transform runners still call Elasticsearch's API and the workload sends an Elasticsearch-format request, so the fix was a port across two repos. My two backups, galaxy#16079 and mahout#1507, both got pull requests this week, and my regrade rejected both on `not-claimed`. A fresh GitHub scan accepted 4 of 5 issues, but each had a blocker my rubric can't see, so I screened the Issue Selection site itself with my rubric's rules: every AI/ML and Python issue on it (2,517), dropping anything assigned, with an open or merged PR, or with any claim comment, and then had the scout grade the best two dozen. It accepted catalyst-cooperative/pudl#5566 with a "moderate" difficulty note, passing every required check and all three preferred ones. PUDL is Catalyst Cooperative's open pipeline for US energy data, built on Dagster and dbt, which is close to the production data platform work I do at Tatari. The issue adds dbt checks that each total on a rural electric co-op's balance sheet (USDA RUS Form 7) equals the sum of its line items, which is data-quality testing on a real pipeline and the same skill as the evals work I want to get better at. I also picked it because it's the most likely to land: all four sibling issues under the same umbrella issue (#5154) were finished by outside contributors and merged within 11 to 20 days, and the work left is about three test blocks, since the schema already checks two totals and the test macro already supports subtraction. I want to learn how a real data team designs its validation tests and how to work through a maintainer's review.

My scout held up on the calls that mattered: it rejected galaxy and mahout the day their pull requests appeared, caught an open PR on zulip#33413 that my own screening script missed, and noticed that Phoenix's "good student issue" label is reserved for another course. It fell short in four ways. Its "ambitious" note on #714 couldn't tell me the fix was a two-repo port; only reading the code did. It can't tell when an issue is already fixed on `main` (the third time this has come up), or when a repo only accepts pull requests from approved contributors or after a maintainer says yes. And it grades the whole issue, so it rejected cpython#133049 even though maintainers had agreed on half of it. The scope patch helped me compare accepts instead of treating them as equal, but "ambitious" now covers both "one open question" and "a port across two repos", which are very different amounts of work. Claim comment: https://github.com/catalyst-cooperative/pudl/issues/5566#issuecomment-6380898918

**Toolkit**

- Tool: issue-scout, my search strategy, rubric, and fit profile. I ran one scan of GitHub and graded about two dozen issues in direct mode (`claude -p "issue-scout: grade <URLs>"`), most of them picks from my own screen of the Issue Selection site.
- What it produced: for pudl#5566, accept with every required check passing, 3 of 3 preferred checks, and a "moderate" difficulty note. It also noted that the maintainers assign the issue from a comment, and that PUDL's LLM policy asks for a short note in the PR on how AI was used.
- Difficulties: it accepted #714 without seeing how big the port was, and it has no check for a fix already on `main`, contribution gates (approved-contributor lists, a maintainer go-ahead, labels held for another course), or a settled half inside a disputed issue. `no-abandoned-history` also never says how many closed PRs count as "several".
- What changed: I applied the scope patch. I replaced my `single-bounded-goal` check with `scope-feasible`, which only fails an issue that isn't one piece of work, and added a difficulty note (straightforward, moderate, or ambitious) to each accept. I also changed the verdict rule to "every check weighted required" instead of a hand-copied list of names, so renaming a row can't silently drop it. My Unit 1 eval still agreed 7 of 7 with the gold labels on every scope item plus two canaries. Next, I'd add checks for "still needed on `main`" and for contribution gates, and put a number on "several" closed PRs.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

macOS 26.6.2 on Apple silicon (arm64), PUDL `main` @ `705c5117c` (2026-10-23) cloned from my fork, pixi 0.83.0 (installed with Homebrew), Python 3.14.8, dbt-core 1.12.6 with dbt-duckdb 1.11.0, and duckdb 1.5.6. PUDL only supports pixi, so `pixi install` builds the whole environment inside the clone (`.pixi/`, 3.3 GB, gitignored) from conda-forge; a plain pip venv can't, since `pyproject.toml` declares no pip dependencies and several are native libraries (GDAL, PROJ). I kept PUDL's data folders outside the repo, as its docs ask, in `~/pudl-dev/input`, `~/pudl-dev/output`, and `~/pudl-dev/dagster-home`, set per session with `source ~/pudl-dev/env.sh` rather than in my shell config. Instead of running the ETL or downloading the 12 GB nightly bundle, I validated against PUDL's nightly Parquet files for the two tables plus `core_rus7__entity_borrowers` (for the foreign-key test), about 2 MB in total, last modified 2026-10-24 08:06 UTC; the issue allows validating the nightly build.

Challenges: the first `dbt_helper validate` failed because dbt's package dependencies weren't installed, which a one-time `dbt deps` fixed (I avoided PUDL's `dbt-deps-upgrade` task, since it rewrites their tracked lockfile). A `.env` file looked like a cleaner way to set the paths, but dbt's `env_var()` only reads real environment variables, so I export them instead, with absolute paths, because PUDL's settings don't expand `~`. pixi skipped three post-link scripts (gtk3, gdk-pixbuf, graphviz) that only matter for GUI and graph rendering, so I left its safe default on.

**Steps to Reproduce**

1. Clone PUDL, check out `705c5117c`, and run `pixi install`.
2. Set `PUDL_INPUT` and `PUDL_OUTPUT`, create `$PUDL_OUTPUT/parquet/`, and download `core_rus7__yearly_balance_sheet_assets`, `out_rus7__yearly_balance_sheet_assets`, and `core_rus7__entity_borrowers` from `https://s3.us-west-2.amazonaws.com/pudl.catalyst.coop/nightly/<table>.parquet`.
3. Run `(cd dbt && pixi run dbt deps)` once.
4. Run `pixi run dbt_helper validate --asset-select "key:core_rus7__yearly_balance_sheet_assets"`, then the same for `out_rus7__yearly_balance_sheet_assets`.
5. Run `pixi run python rus7_asset_totals_check.py <table>` for both tables. The script (below) compares each reported total with the sum of its lines, using the RUS Form 7 Part C formulas, which are the same on the 2014 and pre-2014 blank forms in `docs/data_sources/rus7/`: line 5 = 3 - 4, line 14 = 6 through 13, line 29 = 5 + 14 + 26 + 27 + 28.
6. Observed result: both tables pass all five existing tests, but only two of them check asset totals. Of the three untested totals, `net_utility_plant` and `total` match for every borrower-year, while `total_other_property_and_investments` misses by more than $10 for 1,782 of 8,483.

<details>
<summary><code>rus7_asset_totals_check.py</code></summary>

```python
"""Read-only check of the RUS 7 balance-sheet asset totals that have no dbt test yet.

For each borrower-year, compares the reported total with the sum of its line items
(formulas from RUS Form 7 Part C) and counts gaps larger than the $10 tolerance the
existing tests use. Usage, from a PUDL clone with $PUDL_OUTPUT set:

    pixi run python rus7_asset_totals_check.py core_rus7__yearly_balance_sheet_assets
"""

import os
import sys

import duckdb

# total label: (lines added, lines subtracted), following the Part C formulas
TOTALS = {
    "net_utility_plant": (["total_utility_plant"], ["depreciation_and_amortization"]),
    "total_other_property_and_investments": (
        [
            "non_utility_property",
            "investments_subsidiary_companies",
            "investments_associated_orgs_patronage_capital",
            "investments_associated_orgs_other_general_funds",
            "investments_associated_orgs_other_non_general_funds",
            "investments_economic_development",
            "investments_other",
            "special_funds",
        ],
        [],
    ),
    "total": (
        [
            "net_utility_plant",
            "total_other_property_and_investments",
            "total_current_and_accrued",
            "regulatory",
            "other_deferred_debits",
        ],
        [],
    ),
}


def quoted(labels):
    return ", ".join(f"'{label}'" for label in labels) or "NULL"


def main(table):
    path = f"{os.environ['PUDL_OUTPUT']}/parquet/{table}.parquet"
    con = duckdb.connect()
    con.execute(f"CREATE VIEW t AS SELECT * FROM read_parquet('{path}')")
    years = con.execute("SELECT min(year(report_date)), max(year(report_date)) FROM t").fetchone()
    groups = con.execute("SELECT count(*) FROM (SELECT DISTINCT borrower_id_rus, report_date FROM t)").fetchone()[0]
    print(f"{table}: {groups} borrower-years, {years[0]}-{years[1]}")
    # Catches the line-6 label under either spelling, with or without the final "s".
    line6 = con.execute("SELECT count(*) FROM t WHERE asset_type LIKE 'non_utility_propert%'").fetchone()[0]
    print(f"  rows with an asset_type starting 'non_utility_propert': {line6}")
    for total, (plus, minus) in TOTALS.items():
        con.execute(f"""
            CREATE OR REPLACE TEMP TABLE g AS
            SELECT borrower_id_rus, report_date, reported, reported - summed AS gap FROM (
              SELECT borrower_id_rus, report_date,
                MAX(CASE WHEN asset_type = '{total}' THEN ending_balance END) AS reported,
                COALESCE(SUM(CASE WHEN asset_type IN ({quoted(plus)}) THEN ending_balance END), 0)
                  - COALESCE(SUM(CASE WHEN asset_type IN ({quoted(minus)}) THEN ending_balance END), 0) AS summed
              FROM t GROUP BY 1, 2)
            WHERE reported IS NOT NULL""")
        off, higher, worst = con.execute(
            "SELECT count(*) FILTER (WHERE abs(gap) > 10), count(*) FILTER (WHERE gap > 10), max(abs(gap)) FROM g"
        ).fetchone()
        print(f"  {total}: {off} borrower-years off by more than $10 "
              f"({higher} with the reported total higher), largest gap ${worst:,.0f}")
        for label, order in [("largest reported-higher gap", "gap DESC"), ("largest reported-lower gap", "gap ASC")]:
            row = con.execute(f"SELECT borrower_id_rus, report_date, reported, gap FROM g "
                              f"WHERE abs(gap) > 10 ORDER BY {order} LIMIT 1").fetchone()
            if row and (row[3] > 0) == (order == "gap DESC"):
                print(f"    {label}: {row[0]} {row[1]}, reported ${row[2]:,.0f}, "
                      f"lines sum to ${row[2] - row[3]:,.0f}, gap ${row[3]:,.0f}")


if __name__ == "__main__":
    main(sys.argv[1])
```

</details>

**Reproduction Evidence**

- **Commit showing reproduction:** none. The reproduction changes no code; it runs the existing tests and a read-only query against PUDL's published data.
- **Screenshots/logs:** existing validations, the same on both tables (test names trimmed):

  ```
  4 of 5 PASS source_subcomponents_sum_to_total_..._total_current_and_accrued__ending_balance
  5 of 5 PASS source_subcomponents_sum_to_total_..._total_utility_plant__ending_balance
  Done. PASS=5 WARN=0 ERROR=0 SKIP=0 NO-OP=0 REUSED=0 TOTAL=5
  ```

  The three untested totals on the core table (the output table gives identical lines):

  ```
  core_rus7__yearly_balance_sheet_assets: 8483 borrower-years, 2006-2021
    rows with an asset_type starting 'non_utility_propert': 0
    net_utility_plant: 0 borrower-years off by more than $10 (0 with the reported total higher), largest gap $0
    total_other_property_and_investments: 1782 borrower-years off by more than $10 (1774 with the reported total higher), largest gap $37,117,630
      largest reported-higher gap: NM0011 2017-12-01, reported $40,678,635, lines sum to $3,561,005, gap $37,117,630
      largest reported-lower gap: NM0011 2012-12-01, reported $9,429,749, lines sum to $11,971,993, gap $-2,542,244
    total: 0 borrower-years off by more than $10 (0 with the reported total higher), largest gap $0
  ```

- **My findings:** the issue is real today: the tables validate only two of their five asset totals. Checking the other three turned up a data bug. PUDL's RUS 7 column map (`src/pudl/package_data/rus7/column_maps/balance_sheet.csv`) names line 6, "Non-Utility Property (Net)", `non_utility_property_asset`, with no final "s", while every other asset column ends in `_assets`. The transform keeps only columns matching `_assets$` (`src/pudl/transform/rus7.py:51`), so line 6 never reaches either table, which fits 1,774 of the 1,782 gaps where the reported total is higher. The other 8 go the other way; my guess is a negative "(Net)" value on line 6, but I haven't checked it against the raw data. The typo dates to the first RUS 7 extraction (#4906). `net_utility_plant` and `total` act as a control, since the same query finds no gaps on them. This turns my plan into a question for the maintainers: fix the column-map typo in this PR, or add the test with an `error_if` allowance and report the typo separately.

**Toolkit**

- Tool: repro-check, my Unit 2 rubric, evidence guide, and voice guide, which now live behind the voice guard. I graded my claim alone five times before posting it, then the claim and the repro report together three times (`claude -p "repro-check: grade my claim comment in claim-draft.md and my repro report in repro-draft.md for issue <url>"`).
- What it produced: the posted claim, after it rejected an assignment request and then "this week" as a deadline; for the report, a reject on the first draft and an accept on the final one, with all 9 required checks and both preferred checks passing. It checked the code claims against the repo itself (column 11 of the column map, `rus7.py:51`, the existing tests) and confirmed that the script in the report matches the one I ran.
- Difficulties: it couldn't see my voice guide at first, because Claude Code's Glob tool doesn't return the symlinks the install creates. Its deadline rule graded "this week" both ways across two runs. On the report, it caught that my Result line said the 1,782 gaps happened "because" of line 6, when my own evidence only showed it fit 1,774 of them. It also couldn't rerun my script, since that needs the data download, and it suggested a `PASS` might hide failures, which I checked and found doesn't hold, because dbt still warns on any failing row.
- What changed: I replaced the symlinks with the install guide's pointer files, narrowed `claim-promises-honestly` so only a fix or PR date counts as a deadline (the Unit 2 eval still scores 20/20), and reworded my voice rule to match. In the report I rewrote the headline to "fits 1,774", had the script print every number I cite, pinned the commit, and added one sentence explaining what `PASS` means.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
