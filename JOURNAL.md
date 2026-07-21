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