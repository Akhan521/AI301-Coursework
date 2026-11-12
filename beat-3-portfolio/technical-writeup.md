# Found and fixed a bug in PUDL's open-source energy data pipeline

## Overview

PUDL, which stands for Public Utility Data Liberation Project, is an open source repository managed by Catalyst Cooperative. It's an open source data pipeline for energy data. It takes messy U.S. government energy data, such as spreadsheets, CSVs, and databases, and it turns them into clean, unified tables meant for researchers, activists, journalists, and policymakers. In the particular issue that I took on, the tables for rural electric co-ops' RUS Form 7 balance sheets checked only 2 of 5 asset totals. I checked the other three, and in the process, I found that there were 1,782 of 8,483 co-op-years that were off due to a bug that named a column wrong (line 6 was named `non_utility_property_asset`, missing the final "s"). The pipeline kept only columns ending in `_assets`, so many rows were dropped from both tables. I asked the maintainer whether I should fix it here or only provide tests, which was the goal of the issue. The maintainer got back to me mentioning that we should fix it within the current PR, and so I did just that. After renaming the column, all 8,483 rows were restored per table, and we had zero gaps. I then added three tests. I removed two outdated allowances thereafter, from the maintainer's review. By the end, my PR (https://github.com/catalyst-cooperative/pudl/pull/5729) was approved and merged on October 30th, and the maintainer even invited me for a follow-up for the same typo in RUS 12, which was also merged on November 9th.

## What happened

## Limitations and what's next
