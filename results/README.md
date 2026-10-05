# ScriptoNet: AI-Assisted Mapping of Therapeutic Processes in Literary Fiction

> 🚧 **Exploratory pilot project.** One novel, one annotator, small data. Results are preliminary and reported honestly, including what didn't work.

ScriptoNet asks whether computational methods can help literary scholars find **representations of therapeutic processes** in fiction (writing, poetry, music, narrative reconstruction and emotional catharsis) while keeping **human interpretation central**.

The pilot corpus is Mary Shelley's *Frankenstein* (public domain).

---

## Research question

> Can AI assist literary scholars by identifying and mapping representations of therapeutic processes in literary text, while interpretation stays with the human reader?

**Boundaries.** This project does **not** claim that AI can diagnose mental health conditions, understand emotion as humans do, or that literature is clinical therapy. It studies how narratives *represent* emotional processing.

---

## Key findings (so far)

1. **Early high scores were misleading.** On a pilot of 39 passages, single 80/20 splits suggested 75% (TF-IDF) and 87.5% (Sentence-BERT) accuracy. With only 8 test passages, the same model scored anywhere from 37.5% to 87.5% depending on the random split. Repeated cross-validation gave ~0.62–0.64 accuracy against a 0.51 majority baseline.
2. **Word-based models learned topic, not function.** A model trained on the pilot labelled **17 of 37** deliberately chosen non-therapeutic passages (music, poetry, reading, support scenes) as therapeutic. Feature weights showed it relied on plot words such as character names.
3. **With consistent strict labels, TF-IDF models performed at chance** (balanced accuracy ≈ 0.49). Sentence-BERT showed only a weak signal (≈ 0.57 on v3).
4. **A zero-shot LLM was no better than a trivial baseline.** Qwen2.5-7B, given the annotation rules, reached balanced accuracy 0.55, F1 0.35 (vs 0.34 for "always say therapeutic") and Cohen's kappa 0.05. It labelled 53 of 79 passages therapeutic (annotator: 16), attributed the narration to the **wrong speaker** in 11 cases and misquoted its evidence in 15.

**Takeaway:** in this pilot, therapeutic *function* in fiction was not recoverable from word patterns or zero-shot LLM judgments. Human literary interpretation remains essential. AI is most useful as an assistant that flags candidate passages for a reader to judge.

---

## Annotation framework

Each passage receives:

| Field | Values |
|---|---|
| `therapeutic_label` | 1 = therapeutic representation present, 0 = absent |
| `mechanism` | Narrative Reconstruction · Emotional Catharsis · Scriptotherapy · Poetry-mediated Processing · Music-mediated Processing |
| `secondary_mechanism` | optional, when two mechanisms overlap |
| `intensity` | 1 (very weak) to 5 (very strong); 0 if not therapeutic |
| `annotator_confidence` | sure / unsure |
| `annotator_notes` | reasoning in the annotator's own words |

**Guideline versions**

- **v1.0 (lenient):** an *attempt* at processing counts; receiving emotional support can count as catharsis.
- **v1.1 (strict, current):** label 1 only if processing reaches an **outcome** (new understanding, release, calm, changed perspective). Reflection ending in despair or revenge without resolution = 0. Support or comfort without emotional release = 0. Six pilot labels were re-checked and changed under v1.1.

Full guidelines: [`docs/annotation_guidelines.md`](docs/annotation_guidelines.md)

---

## Dataset

- **Source:** *Frankenstein*, Project Gutenberg #84 (≈ 75,000 words) → **229 passages**
- **Current labelled set:** `frankenstein_training_dataset_v4_strict.csv`: **79 passages, 16 therapeutic, 63 not**

| Stage | Total labelled | Therapeutic | How passages were chosen |
|---|---|---|---|
| Pilot (NB02) | 10 | 5 | random |
| Batch 01 (NB05) | 29 | 11 | ranked by emotion/memory keywords |
| Batch 02 (NB07) | 39 | 19 | keyword-ranked, hand-picked |
| Batch 03 (NB12) → v3 | 79 | 22 | 26 targeted, 10 hard negatives, 4 random |
| Strict re-check → **v4** | 79 | **16** | 6 pilot labels changed under v1.1 |

Each passage in v3/v4 records its `selection_method` (pilot / targeted / hard_negative / random). Batches 01–02 were keyword-selected rather than random, which likely made early models look stronger than they were.

---

## Notebooks

| # | Notebook | What it does | Key result |
|---|---|---|---|
| 01 | Understanding literary data | Load and clean the novel, split into passages | 229 passages |
| 02 | Annotation framework | Define the label scheme, pilot-annotate 10 passages | 5 / 5 split |
| 03 | Baseline ML model | TF-IDF + Logistic Regression on 10 passages | 50% on 2 test passages |
| 04 | Model comparison | Random Forest vs Logistic Regression | 0% vs 50%: too little data |
| 05 | Dataset expansion | Keyword-prioritised Batch 01 | 29 labelled |
| 06 | Expanded baselines | LR and RF on 29 passages | 67% accuracy, but predicted only "not therapeutic" |
| 07 | Batch 02 | 10 more passages | v2: 39 labelled, balanced |
| 08 | Retraining | LR, RF, SVM on v2 | 75% / 50% / 75% on 8 test passages |
| 09 | Explainability | Feature weights, error analysis | plot words drive predictions |
| 10 | Semantic embeddings | Sentence-BERT + PCA + LR | 87.5% on 8 test passages |
| 11 | Cross-validation (v2) | Repeated stratified CV, class balancing, baselines, pilot → Batch 03 test, strict re-check | early scores were luck; topic ≠ function |
| 12 | Targeted passage search | Find music, poetry, writing, reading, support and despair candidates; build Batch 03 | v3: 79 labelled |
| 12b | Merge Batch 03 | Merge labelled batch into the dataset | v3 file |
| 13 | Zero-shot LLM classifier | Qwen2.5-7B applies the v1.1 rules; compare with annotator | kappa 0.05; speaker confusion |

---

## Results summary

**Cross-validation (5 folds × 20 repeats)**

| Data | Model | Metric | Score |
|---|---|---|---|
| v2 (39) | TF-IDF + SVM | accuracy | 0.642 ± 0.166 |
| v2 (39) | Sentence-BERT + LR | accuracy | 0.620 ± 0.162 |
| v2 (39) | Majority baseline | accuracy | 0.514 |
| v3 (79) | Sentence-BERT + LR | balanced accuracy | 0.567 |
| v3 (79) | TF-IDF + LR | balanced accuracy | 0.520 |
| v4 strict (79) | TF-IDF + LR | balanced accuracy | ≈ 0.49 (chance) |

**Zero-shot LLM (Qwen2.5-7B-Instruct, 4-bit, v1.1 rules, v4 data)**

| | Balanced acc. | Precision | Recall | F1 | Kappa |
|---|---|---|---|---|---|
| Qwen 7B | 0.55 | 0.23 | 0.75 | 0.35 | 0.05 |
| Always say 1 | 0.50 | 0.20 | 1.00 | 0.34 | 0 |

Of 45 annotator–LLM disagreements: 41 judged LLM errors, 3 debatable (passages 111, 152, 225), 1 open.

---

## Limitations

- **One novel, one annotator.** No inter-annotator agreement yet; agreement with an LLM is not a substitute.
- **Small data:** 79 labelled passages, only 16 therapeutic. Mechanism-level classification isn't feasible yet.
- **Selection effects:** Batches 01–02 were keyword-selected; Batch 03 deliberately over-samples hard cases.
- **Rule change:** labels moved from v1.0 to v1.1 during the project; v4 applies v1.1 throughout.
- **LLM caveats:** the model has likely seen *Frankenstein* in training; results are zero-shot with one model and one prompt.

---

## Open questions for literary-expert review

- Should "therapeutic" require an outcome, or does an attempt at processing count?
- Is receiving comfort a form of catharsis?
- Is *reading* (e.g. the creature reading Victor's journal) scriptotherapy, or narrative reconstruction?
- Does telling one's whole story in a frame narrative count as storytelling therapy?
- Is **healing through nature** a missing category?
- Boundary passages: 111, 152, 185, 225 (debatable); 2, 13, 17, 56, 108, 124 (annotator unsure)

---

## Next steps

1. Refine the guidelines with a literary expert; add a **second annotator** and measure inter-annotator agreement
2. Structured / few-shot LLM prompting (identify the speaker → processing → outcome) as a follow-up to NB13
3. A small pilot on a contemporary corpus (e.g. Ian McEwan), with copyright-safe handling (no novel text in this repository)

---

## How to run

All notebooks run in **Google Colab**. Open a notebook and upload the CSV it asks for from `data/processed/`.
Notebook 13 needs a GPU: **Runtime → Change runtime type → T4 GPU**.

Main libraries: `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `sentence-transformers`, `transformers`, `accelerate`, `bitsandbytes`, `torch`.

---

## Repository structure

```
ScriptoNet-Digital-Humanities/
├── README.md
├── docs/                 # research question, methodology, roadmap, annotation guidelines
├── notebooks/            # 01–13 (+ 12b)
├── data/
│   └── processed/        # passages, labelled datasets v2 → v4_strict, Batch 03 files
└── results/              # cross-validation tables, figures, LLM predictions and disagreements
```

---

## Data and copyright

*Frankenstein* is in the public domain (Project Gutenberg #84). Any future work on copyrighted novels will keep the full text **out of this repository**; only passage IDs, labels, scores and figures will be shared.

---

## How this was built

All annotation decisions (labels, mechanisms, intensity, notes, verdicts on disagreements) were made by the author. AI assistants (including Anthropic's Claude) were used to help write code and documentation; the author ran every notebook, checked outputs and interpreted the results.

<!-- ✍️ Keep ONE of these two sentences, delete the other: -->
Batch 03 was labelled by the author independently, without AI-suggested labels.
Batch 03 was labelled by the author after reviewing AI-suggested labels; every final label is the author's decision.

---

## Author

**Rupa**, 2nd-year Electronics and Communication Engineering student, Mallareddy Engineering College for Women
<!-- ✍️ Add LinkedIn / email, and any team members or supervisors involved -->

## License

MIT. See [`LICENSE`](LICENSE).
