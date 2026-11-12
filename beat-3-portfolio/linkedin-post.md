I made my very first contribution to an open source project. I found and fixed a bug. In particular, it was in the PUDL repository, which provides free U.S. energy data for researchers, journalists, and policymakers. My pull request was specifically around validation tests. Fixing the bug was just one byproduct of going through the validation tests because that is how I uncovered the typo in the first place.

As for the bug, it was a one-letter typo on a column name, and yet this turned out to be one line of every rural electric co-op's balance sheet that was missing from the published data. There were 8,483 records per table from 2006 to 2021, so 1,782 reported totals didn't add up.

I had a great interaction with the maintainer, and I was able to get both of my pull requests (specifically for the issue that I took on, as well as the follow-up fix for the same typo in a second co-op dataset) approved and merged. Now we have validation tests for all five asset totals on the rural electric co-ops' balance sheets. Researchers now get complete data instead of missing data and wrong totals.

https://github.com/catalyst-cooperative/pudl/pull/5729
