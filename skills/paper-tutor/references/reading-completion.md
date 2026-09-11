# Reading Completion Contract

Use this reference only after a complete-paper analysis, not for a local
formula, figure, table, section, or follow-up question. Its job is to prove
that the paper was read into reusable research assets without duplicating the
deep Paper-Tutor analysis.

## Trigger and source of truth

Trigger this contract when the user asks to read or analyze the whole paper,
complete a paper review, finish reading, create a reading note/card, leave
learning assets, or produce a reconstructed method diagram and a verification
question. A request for only one section, equation, figure, table, or concept
stays local unless it also explicitly asks for a complete-paper workflow.

Use this order:

```text
existing Paper-Tutor analysis or matching upstream analysis
  → fixed Reading Card projection
  → targeted gap check
  → learning assets
  → completion gate
```

`paper-tutor.md` remains the source of truth for the deep explanation. Reuse
its evidence, labels, citations, and Evidence IDs. In Integrated Mode, read
matching Scholar-Slides artifacts only; write the new Paper-Tutor assets to
the requested Paper-Tutor output directory, never to Scholar-Slides or Mode B.
In Standalone Mode, preserve the existing `Not verified through Scholar-Slides
CKPT-1` disclosure. Do not change Scholar-Slides.

Preserve a supplied matching paper identity in the mode block even when other
fields are sparse (for example, keep `Paper identity: Paper P`). Use
`Paper identity: Not verifiable from available evidence` only when identity
matching genuinely fails. A gate-only status check does not authorize a fake
mode or source line: follow the missing-input behavior in
`integration-and-evidence.md` when no mode can be selected.

Do not upgrade an evidence class merely because a prompt calls a dossier
“complete evidence.” Preserve the mode and `Evidence source` chosen during the
initial analysis; classify a user-provided summary only according to the
evidence class it actually identifies.

Before adding any content, map existing sections to the card:

| Reading Card field | Prefer existing analysis from |
| --- | --- |
| Problem | gap, motivation, limitations, argument thread |
| Setting | overall pipeline, modules, experimental design |
| Baseline | gap and baseline rationale in experiments |
| Method | method framework, module breakdown, pipeline/figure reasoning |
| Objective | formulas and objective explanation |
| Evaluation | experimental design |
| Result | reported experimental results |
| Ablation | ablation analysis |
| Failure | error analysis, limitations, qualitative results, research assessment |
| Your idea | assumptions and clearly labeled researcher analysis |

Only investigate a missing field in the paper or available evidence. Never
create a parallel `Method Analysis`, `Method Explanation`, `Method Summary`,
`Reading Method`, or `Method Card` section. The card is a compressed
projection, not a second analysis.

## Fixed Reading Card

Use these field names exactly and include every field once. Keep the answer
high-density enough for a three-minute review; do not paste the Tutor
Explanation or the full evidence appendix into this file.

```markdown
# <Paper Title>

## Reading Card

| Field | Answer |
|---|---|
| Problem | ... |
| Setting | Input: ...; Output: ...; Supervision / Feedback: ... |
| Baseline | Primary baseline: ... |
| Method | Input → Module A → Module B → Output; failure repaired by each module: ... |
| Objective | ... |
| Evaluation | Dataset: ...; Metric: ...; Model / Backbone: ...; Shot setting: ... |
| Result | ... |
| Ablation | ... |
| Failure | ... |
| Your idea | Assumption/scenario changed: ... Prediction and reason: ... |

## One-sentence takeaway

...

## Verification Question

Question: ...
Hypothesis: ...
Test: ...
Disconfirming evidence: ...
```

The ten answers must satisfy these semantic tests:

- `Problem` states the pre-method failure in one sentence, not only the
  research topic.
- `Setting` distinguishes Input, Output, and Supervision / Feedback. For an
  inference-only method write `Supervision: None / inference-time only` when
  supported; do not invent labels, rewards, or training signals.
- `Baseline` names the strongest reasonable no-new-mechanism comparator. Use
  `Primary baseline` and add `Strongest reported baseline` only when needed.
- `Method` gives one real `Input → ... → Output` flow and says which failure
  each material module repairs. The same flow must be used by `method.svg`.
- `Objective` explains the optimization pressure removed with each loss,
  reward, regularizer, or constraint. If none is introduced, write
  `No trainable objective introduced.` and explain the inference-time setting.
- `Evaluation` includes Dataset, Metric, Model / Backbone, and Shot setting;
  add split, decoding, retrieval, tools, or environment only when decision-
  relevant.
- `Result` states the important improvement, comparison, coverage across
  datasets/models, and any degradation, tie, or instability. Do not reduce it
  to `SOTA`.
- `Ablation` identifies the removal with the largest supported drop or the
  experiment that best establishes necessity. If absent, write
  `Not reported by the authors.` If the evidence cannot determine a dominant
  component, write `Evidence insufficient to identify a single dominant component.`
- `Failure` prioritizes author error analysis, limitations, qualitative cases,
  appendix, and directly observed results. Mark `Author-reported failure` and
  `Reader inference` separately; do not turn a plausible guess into an author
  claim.
- `Your idea` performs one mechanism-grounded assumption removal, distribution
  shift, scenario transfer, component substitution, scale change, or
  supervision change. State what is changed, what is held constant, the
  predicted outcome, and why. It is not a generic future-work list.

## Evidence and missing information

Keep the existing `Paper Fact`, `Tutor Explanation`, `Tutor Analysis`, and
`Unsupported` labels. Preserve Evidence IDs and locations; do not create a
second evidence taxonomy. The card may use compact inline labels, for example:

```text
Paper Fact: +5 F1 over the primary baseline (Table 2, Evidence ID E7).
Tutor Analysis: the gain is concentrated on the shifted split.
```

Use the following distinction when a field is sparse:

| Evidence situation | Card wording | Gate effect |
| --- | --- | --- |
| Full paper/verified analysis establishes that the paper has no such item | `Not reported by the authors.` | Field can pass |
| Current source does not reveal the item | `Not verifiable from available evidence.` | Reading remains incomplete |
| Interpretation is not an author claim | `Reader inference: ...` or `Tutor Analysis: ...` | Pass only if clearly labeled |
| Evidence conflicts or identity is unresolved | State the conflict; do not merge | Reading remains incomplete |

For an inference-only paper, do not manufacture a loss. For absent ablation or
failure analysis, say so explicitly and do not guess which component matters or
which samples fail. A sparse abstract/caption is not evidence that a full paper
was completed.

## Learning assets

For a complete-paper workflow, create these in the Paper-Tutor output
directory, following any existing output naming convention:

1. `paper-tutor.md`: the existing full deep analysis, exactly once.
2. `reading-note.md`: Artifact A, the compact Reading Card above plus one
   takeaway and one verification question. Do not duplicate the long tutor
   explanation.
3. `method.svg`: Artifact B, a real standalone SVG reconstructed from the
   Method field. Artifact C is the single `Verification Question` highlighted
   in `reading-note.md`; a separate question file is not required.

`method.svg` must be valid, self-contained SVG/XML with an `xmlns`, a visible
`Input`, the material processing stages, the core new module(s), a visible
`Output`, and arrows showing data flow. Add short failure-repair labels near
key modules. Draw separate training/inference lanes when both matter, and draw
retrieval, memory, tool, or environment feedback loops when they are part of
the mechanism. Do not use Mermaid, ASCII, external images, or a copied paper
figure as the final artifact. If the method cannot be reconstructed without
guessing, do not draw invented boxes; record the missing evidence and keep the
status incomplete.

The SVG labels and arrow order must be semantically identical to the card's
`Method` flow. Treat the card flow as the canonical short representation and
derive both outputs from it.

`Verification Question` must be exactly one experimentally answerable
question, grounded in `Failure + Assumption + Ablation + Your idea`. Include
as many of these as the evidence supports:

- manipulated variable;
- comparison or control;
- measurable outcome;
- hypothesis;
- disconfirming evidence.

Avoid questions such as `Can performance be improved?` or a generic future-work
prompt.

## Completion gate

Run this check after the assets are written. Keep it internal or compact; show
only missing items during ordinary delivery.

```text
READING COMPLETION CHECK

[✓/ ] Problem
[✓/ ] Setting
[✓/ ] Baseline
[✓/ ] Method
[✓/ ] Objective
[✓/ ] Evaluation
[✓/ ] Result
[✓/ ] Ablation
[✓/ ] Failure
[✓/ ] Your idea
[✓/ ] Markdown note
[✓/ ] SVG method diagram
[✓/ ] Verification question

Status: READING COMPLETE / READING INCOMPLETE
```

Mark `READING COMPLETE` only when all ten fields are present exactly once,
each answer is evidence-safe or truthfully marked unavailable, the source is
sufficient for a full-paper reading, `reading-note.md` exists, `method.svg`
exists and is valid and flow-consistent, and exactly one verification question
with a measurable test is present. A truthful `Not reported by the authors.`
answer can pass; an unresolved `Not verifiable from available evidence.`
answer, missing source, missing artifact, unresolved evidence conflict, or
invented detail keeps the result `READING INCOMPLETE`.

Never claim completion merely because the checklist was printed. If any check
fails, report `READING INCOMPLETE` and name only the missing or blocked checks.
