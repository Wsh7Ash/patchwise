# Fix plan - Cross-tenant order read via supplier-supplied identifier
Skills matched: 4 of 50   Plan steps: 4

Thin coverage (no matched skill in these phases): triage, contain, document

## fix

regression-safety-mesh-for-fixes  (match 1)
  Build the safety net before the fix lands: negative and positive tests that capture both the vulnerable behavior and the legitimate behavior the fix must preserve
   1. Write the failing test that reproduces the defect against the current code, and run it once to see it fail for the right reason.
   2. Write the passing test for the legitimate behavior the fix must not break using the exact same access surface.
   3. Compare the two tests: they should differ only in the precondition that triggers the defect, which is the boundary the fix must locate.
   4. Add boundary cases just outside the fix: slightly-off inputs that should behave like the legitimate side, not like the defect.
   5. Run the full suite before and after the fix, recording which other tests changed and why.
   6. Commit the tests together with the fix, with a commit message that names the defect and the preserved behavior.

## verify

regression-watch-window  (match 1)
  Ahead of the fix, define how long re-observation should stay active and what evidence ends the watch cleanly
   1. Define the regression criteria in advance: which metrics, logs or events always signal regression has restored the old behavior.
   2. Define the watch duration, derived from deployment and evidence-gathering speed rather than calendar intuition.
   3. Pick the data source for each criterion: logs, alerts, dashboards or manual sampling, and store a link to each.
   4. Run the watch until the criteria are checked one by one and none indicates regression.
   5. Record the end-of-watch summary: what evidence closed it, any events that may have looked like regressions.
   6. Archive the fix record's history so a regression later can re-open the record without the original evidence lost to time.

## harden

access-portal-query-refusal-audit  (match 1)
  Re-audit the affected surface's query refusals after the fix, confirming every entry path's verdict aligns, not only the path the report described
   1. Enumerate the entry paths into the fixed surface: API handlers, jobs, scheduled tasks, upstream led reads, direct DB checks.
   2. Prepare the test credential pair used during the original finding, matching exactly the permissions each path needs.
   3. For each entry path, fire the identical peer-request and confirm the refusal response, recording each independently.
   4. Check consistency of the status and body shape across paths, since a mismatch leaks that the check differs per surface.
   5. Track any path that refuses with a different shape, and mark the inconsistency as a separate finding rather than one blob.
   6. Record the audit matrix in the finding record so future reviewers can see that entry-path parity, not just the reported one, held.

anti-regression-guard-rails  (match 1)
  Install mechanical guard rails that prevent this defect class from re-appearing, so the fix's protection does not rest on the next reviewer's memory
   1. Express the defect class as a mechanical rule: what constitutes violation, what constitutes compliance, and what is out of scope.
   2. Choose the enforcement point: lint rule, static analysis, type-level constraint, database default, CI check or test scaffold.
   3. Implement the rule with a suppression mechanism for genuine exceptions, so suppressions are explicit and reviewable.
   4. Run it against the fixed code first, then against the codebase's clean history and report how many pre-existing violations it catches.
   5. Tune it against false positives and record the tuning margin, so later engineers can see the guard's precision claims honestly.
   6. Document the rule next to the finding's closure, so its existence is tied to the defect class rather than to a personal preference.
