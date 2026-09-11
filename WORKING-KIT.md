# Download the working kit

[Download parallel-document-checker-0.1.0.zip](parallel-document-checker-0.1.0.zip). Extract it and run the commands below from the extracted directory. The archive includes readable source, prompts, synthetic fixtures, and tests.

# Parallel Document Checker — TEVV Working Kit

**Status:** Experimental working kit, version 0.1.0  
**Publication status:** Public experimental foundation  
**Purpose:** Test the proposed blind parallel document-checking architecture without pretending that the evaluators have already been validated.

## What this kit does

The kit prepares two byte-identical copies of one complete response set. One copy goes to a source and epistemic reviewer; the other goes to a sycophancy reviewer. Each reviewer works in a separate context and returns a complete verdict for every response. The kit then validates and joins the two result sets.

The join preserves both full verdicts. It never lets one review filter the material seen by the other.

```text
                 COMPLETE ORIGINAL RESPONSE SET
                         /            \
                        /              \
                       v                v
             SOURCE / EPISTEMIC     SYCOPHANCY
                  REVIEW               REVIEW
                       \              /
                        \            /
                         v          v
                      VALIDATED JOIN
```

## What this kit does not prove

Passing the included tests proves that the preparation and join machinery behaves as specified. It does not prove that an AI evaluator is accurate, independent, unbiased, or consistent. Those claims require blind runs over a hand-labeled corpus, preferably with different model families, followed by human adjudication.

## Running a review

Python 3.10 or newer is sufficient; the tool uses only the standard library.

1. Prepare the two blind review packets:

   ```bash
   python3 parallel_document_checker.py prepare my-response-set.json --output run-001
   ```

2. Give the contents of `run-001/source_review/` to the source and epistemic evaluator in a fresh session.

3. Give the contents of `run-001/sycophancy_review/` to the sycophancy evaluator in a different fresh session. Prefer a different model family.

4. Save their exact JSON outputs as `source-results.json` and `sycophancy-results.json`.

5. Join the verdicts:

   ```bash
   python3 parallel_document_checker.py join run-001/run-manifest.json source-results.json sycophancy-results.json --output joined-results.json
   ```

The join stops instead of guessing when an ID is missing, duplicated, added, or tied to the wrong input or prompt hash.

## Input format

The original response set is one JSON object:

```json
{
  "document_id": "example-001",
  "context": "Optional shared context supplied to both reviewers.",
  "responses": [
    {
      "response_id": "R001",
      "message_index": 12,
      "text": "The complete response text."
    }
  ]
}
```

Every `response_id` must be unique and every response must contain non-empty text. Extra fields are retained.

## Classification rule

The source reviewer records a nuanced `source_status`. The join maps only `SUPPORTED` to the sourced side of the 2×2 matrix. `PARTIAL` and `UNSUPPORTED` enter the unsourced side while their full detail remains attached. `UNVERIFIABLE` and `NOT_APPLICABLE` enter `HOLD FOR REVIEW` instead of being forced into a misleading binary cell.

The four primary cells remain:

- `FLAGGED + SOURCED`
- `FLAGGED + UNSOURCED`
- `NOT FLAGGED + SOURCED`
- `NOT FLAGGED + UNSOURCED`

## TEVV files

- `TEVV-REPORT.md` records what has actually been tested and what remains open.
- `fixtures/` contains a synthetic response set and independent golden verdicts covering every 2×2 cell plus hold cases.
- `tests/` checks byte-identical preparation, order-independent joining, complete verdict preservation, and rejection of missing IDs, duplicate IDs, and mismatched hashes.

