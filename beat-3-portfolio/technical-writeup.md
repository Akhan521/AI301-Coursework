# Found and fixed a bug in PUDL's open-source energy data pipeline

## Overview

PUDL, which stands for Public Utility Data Liberation Project, is an open source repository managed by Catalyst Cooperative. It's an open source data pipeline for energy data. It takes messy U.S. government energy data, such as spreadsheets, CSVs, and databases, and it turns them into clean, unified tables meant for researchers, activists, journalists, and policymakers. In the particular issue that I took on, the tables for rural electric co-ops' RUS Form 7 balance sheets checked only 2 of 5 asset totals. I checked the other three, and in the process, I found that there were 1,782 of 8,483 co-op-years that were off due to a bug that named a column wrong (line 6 was named `non_utility_property_asset`, missing the final "s"). The pipeline kept only columns ending in `_assets`, so many rows were dropped from both tables. I asked the maintainer whether I should fix it here or only provide tests, which was the goal of the issue. The maintainer got back to me mentioning that we should fix it within the current PR, and so I did just that. After renaming the column, all 8,483 rows were restored per table, and we had zero gaps. I then added three tests. I removed two outdated allowances thereafter, from the maintainer's review. By the end, my PR (https://github.com/catalyst-cooperative/pudl/pull/5729) was approved and merged on October 30th, and the maintainer even invited me for a follow-up for the same typo in RUS 12, which was also merged on November 9th.

## What happened

Before I actually started on my issue, it was opened on September 8, and it was one of five validation issues under the umbrella epic (#5154). The previous assignee actually stepped back on October 22nd. The schema already checked two of the five asset totals. And so I posted my claim that the schema already checks `total_utility_plant` and `total_current_and_accrued`. I would go through the RUS 7 balance sheet form and the table's `is_total` rows to find the other totals and what adds up to each. Then I would post a plan before opening a draft PR. I utilized Claude Code in my PUDL clone to read through the codebase with the repo's `AGENTS.md` and my own `CLAUDE.local.md` file. In particular, I used it to read the dbt files, the transform, and the blank RUS 7 forms. Through this, I was able to identify that tests pick rows by their `asset_type` label and `error_if` is an allowance for known failures. `is_total` only flags names that start with the `total_` prefix. `net_utility_plant` and `total` (lines 5 and 29) don't start that way, so the flag would have missed two of the three totals that we needed. I took the formulas from the blank forms instead of the flag and recorded the `is_total` rule in `CLAUDE.local.md`. The formulas that I ended up with were included in my plan comment. And through tracing these formulas, I was able to identify that line 14 was the total that turned out not to add up, which led to the bug investigation.

On October 24, my read-only check script ran on the nightly data. Lines 5 and 29 matched for all 8,483 co-op-years. However, line 14 was off by more than $10 for 1,782 of them, and in 1,774 of those, the reported total was higher than its lines. The largest gap was $37,117,630.

```
core_rus7__yearly_balance_sheet_assets: 8483 borrower-years, 2006-2021
  rows with an asset_type starting 'non_utility_propert': 0
  net_utility_plant: 0 borrower-years off by more than $10 (0 with the reported total higher), largest gap $0
  total_other_property_and_investments: 1782 borrower-years off by more than $10 (1774 with the reported total higher), largest gap $37,117,630
  total: 0 borrower-years off by more than $10 (0 with the reported total higher), largest gap $0
```

When I was looking through the trace, I found that the column map names line 6 `non_utility_property_asset`, which has no final letter s, while every other asset column ends in `_assets`. The transform keeps only columns matching `_assets`, so line 6 never reached either table, and this fit the 1,774 gaps. As for the other eight gaps, the reported total was lower.

```python
value_vars = list(df.filter(regex=r"_assets$").columns)
```

```diff
-...,net_utility_plant_assets,non_utility_property_asset,investments_subsidiary_companies_assets,...
+...,net_utility_plant_assets,non_utility_property_assets,investments_subsidiary_companies_assets,...
```

## Limitations and what's next
