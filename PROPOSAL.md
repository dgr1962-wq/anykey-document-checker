# Why the Document-Checking Process Changed

The first AnyKey conversation checker grew from a practical need: examine long human–AI conversations without mistaking confidence, agreement, or a compelling narrative for evidence. It looks for source loss, unsupported certainty, correction failures, reinforcement loops, and other ways a conversation can drift away from what the record actually supports.

The earlier published plan was to run one document through three separate AI reviewers, then apply an epistemological and source check afterward. That was meant to reduce reliance on any one model. During later design discussions among Darren, Codex, and Claude, Claude identified a structural weakness: placing one kind of review after another can allow the earlier stage to shape or reduce what the later stage examines.

That instrument remains useful, but one weakness became clear while we examined how its different checks interact. If one evaluator filters a document before a second evaluator sees it, the second pass can inspect only what survived the first. Reversing the order merely reverses the information loss.

We agreed with Claude's proposal and adopted the blind parallel architecture as the direction for the next checker. That decision establishes what we intend to build; it does not claim that the new method has already been validated.

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

Both evaluators receive the same complete original material. Neither sees the other evaluator's prompt, notes, or verdicts. One examines factual support, citations, provenance, and epistemic treatment. The other examines whether the assistant bends its analysis, certainty, or recommendations toward pleasing or mirroring the user. Their full verdicts are joined only after both reviews are complete.

The joined record makes four cases visible:

1. **Flagged + Unsourced** — behaviorally suspect material that also lacks adequate support.
2. **Flagged + Sourced** — material whose tone or agreement pattern is suspect even though its factual basis survives review.
3. **Not Flagged + Unsourced** — confident or sober-sounding material whose factual basis fails. This is the especially dangerous case a style-first filter can miss.
4. **Not Flagged + Sourced** — the strongest surviving material under this limited test.

An additional **Hold for Review** state protects material that cannot fairly enter the binary matrix because its source is unavailable or it contains no applicable factual claim.

This repository is the experimental foundation for that adopted direction, not a declaration that the method has been validated. It currently provides two separated reviewer prompts, byte-identical input preparation, strict coverage and provenance checks, a deterministic join, and synthetic tests for the four matrix cells and hold cases. The next stage is empirical TEVV: independent model runs over a hand-labeled corpus, human adjudication, disagreement analysis, threshold calibration, and direct comparison with the earlier process.

The work is being published early so the architecture can be inspected, criticized, reproduced, and improved. Useful support includes access to independent flagship models, evaluation credits, domain reviewers, adversarial test documents, and infrastructure for preserving repeatable runs.
