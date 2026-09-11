# TEVV Status — Parallel Document Checker 0.1.0

**Date:** 2026-09-10  
**Release state:** Public experimental foundation  
**Validation claim:** None yet

## Completed in this foundation

- The preparation tool makes byte-identical copies of the complete original response set.
- Each reviewer packet contains only its assigned prompt and its own manifest.
- Input and prompt hashes bind returned verdicts to the prepared run.
- The join rejects missing, additional, and duplicated response IDs.
- The join rejects mismatched run, input, prompt, or schema identifiers.
- Both complete reviewer verdicts survive the join unchanged.
- The four 2×2 classes are deterministic.
- Unverifiable and not-applicable source cases enter `HOLD FOR REVIEW` instead of being forced into a false binary answer.
- A synthetic fixture exercises every matrix cell and two hold cases.

## Verification run

The local foundation test suite passed on 2026-09-10 under Python 3.12. It verified byte-identical packet preparation, order-independent joining, preservation of both complete verdicts, all four matrix cells, the hold state, and rejection of missing IDs, duplicate IDs, and a mismatched input hash.

## Not yet completed

- No claim-accuracy calibration has been run against a human-labeled corpus.
- No cross-model independence study has been completed.
- No inter-rater agreement or disagreement adjudication has been measured.
- No sequential-versus-parallel comparison has established an empirical improvement.
- No prompt-injection or deliberately malformed-document campaign has been run against live evaluators.
- No cost, latency, or large-document boundary has been established.

## Gate before calling this a release candidate

Run both blind reviewers across a frozen, hand-labeled corpus containing sourced and unsourced claims, flattering and non-flattering language, mixed cases, dead sources, citation laundering, prompt injection, and long-range context dependencies. Prefer different model families. Preserve every raw output. Human adjudicators should review disagreements without seeing model identity, then compare precision, recall, coverage, and failure modes against the earlier process.

Until that gate is completed, version 0.1.0 should be described as a runnable experimental architecture.

## Publication check — September 11, 2026

The four included unit tests passed again before GitHub publication. Empirical validation remains pending.
