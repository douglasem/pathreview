## Week 7 — Issue selection

**Issue link:** https://github.com/ascherj/pathreview/issues/157

**Issue title:** Relevance scorer “partial overlap” test fixture actually has full query overlap

**Tier:** [x] Tier 1  [ ] Tier 2  [ ] Tier 3

**Problem summary:**
The relevance scorer unit test named `test_query_with_partial_overlap` is intended
to verify scoring behavior when only some query terms appear in a document chunk.
However, its current fixture contains all four terms from the query, so the scorer
correctly calculates full keyword coverage and returns a score of 1.0. The test then
incorrectly expects the score to be below 0.9, causing it to fail even though the
scorer is behaving correctly. A successful fix will update the test fixture so that
it contains only some of the query terms and genuinely represents partial overlap.

**Branch name:** test/157-partial-overlap-fixture

**Setup confirmation:** [x] App runs locally at localhost:5173

**Cohort ledger:** [x] Issue added to cohort ledger

### Selection notes — “Is this right for me?”

This issue appears to have a small and clearly defined scope. It affects an existing
unit-test fixture rather than requiring a change to the relevance-scoring algorithm
or application architecture. The reproduction command identifies the exact test
file and failing assertion, and the expected outcome is clear: revise the fixture so
that only part of the query overlaps with the chunk. This makes the issue appropriate
for my current experience level while still allowing me to practice reading tests,
understanding expected behavior, running targeted checks, and submitting an
open-source pull request.


## Week 8 — Reproduction & solution planning

**Reproduction commit link:**
https://github.com/douglasem/pathreview/commit/1cab1ac765ea3c1809bbb277cbd196c877017b84

**PLAN.md link:**
https://github.com/douglasem/pathreview/blob/test/157-partial-overlap-fixture/PLAN.md

**Reproduction summary:**
I reproduced the issue by running:

```bash
.venv/bin/pytest tests/unit/test_relevance_scorer.py -q
```

The test test_query_with_partial_overlap failed because it expected a score below 0.9, but the scorer returned 1.0. After reviewing the issue description, I confirmed that the test fixture actually contains all of the query terms, so it represents full overlap rather than partial overlap.

## Week 9 — Solution building & PR submission

### Check-in 1 (mid-week)

**Current progress:**
I reproduced Issue #157 and confirmed that `test_query_with_partial_overlap` failed because its fixture contained all four query terms. I inspected `rag/evaluator/relevance_scorer.py` and verified that the scorer calculates keyword overlap as matched query tokens divided by total query tokens. I updated the fixture in `tests/unit/test_relevance_scorer.py` so only two of the four query terms overlap, producing a true partial-overlap score of 0.5. The targeted relevance scorer test file now passes all 19 tests.

**Next steps:**
Run `make check` and `make test-unit`, review the final diff, commit and push the fix, open a draft pull request, request peer or mentor feedback, and complete Check-in 2 before marking the PR ready for review.

**Blockers:**
None.

### Check-in 2 (end of week)

**PR link:** [paste the GitHub PR link]

**Branch:** `test/157-partial-overlap-fixture`

**What you built:**
I corrected the fixture in `test_query_with_partial_overlap` so it now represents genuine partial keyword overlap. The original chunk matched all four query terms and correctly received a score of `1.0`; the updated chunk matches two of four terms and produces the intended score of `0.5` without changing production code.

**Tests added or updated:**
Updated `tests/unit/test_relevance_scorer.py`, specifically
`TestRelevanceScorer.test_query_with_partial_overlap`. The test now covers the case where only some query tokens appear in the retrieved chunk and verifies that the scorer returns a middle-range value rather than full relevance.

**Self-review confirmation:** [ ] make check passes  [ ] make test-unit passes
Note: Both make check and make test-unit have pre-existing failures unrelated to relevance_scorer.py. I confirmed that the changes implemented remove the test failures related to relevance_scorer.py.

**Draft PR feedback received from:**
None.