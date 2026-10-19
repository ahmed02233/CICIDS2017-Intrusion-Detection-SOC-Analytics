# CICIDS2017 Intrusion Detection & SOC Analytics — Project 13

## Executive summary
Defensive analysis of sampled **historical CICIDS2017 academic network flows** using Python, SQLite, a RandomForest baseline, a static SOC-style dashboard and seven evidence-based recommendations. **Not a live SOC monitoring system.**

## Sources
- Official research source: https://www.unb.ca/cic/datasets/ids-2017.html
- Automatic mirror: https://www.kaggle.com/datasets/cicdataset/cicids2017
- Input: working-hours labeled CSV flow files; original source records not redistributed.

## Sampling and scope
- Sampled **15,603** rows from **1** CSV files with seed **42**, Bernoulli probability **7%**, cap **220,000** records.
- Observed labels and rates describe **this sample**, not population prevalence, live traffic or all CICIDS2017 files.
- Some datasets may contain timestamps but this project intentionally does **not** claim timeline analysis.
- Data loading skips malformed CSV lines and numeric conversion errors are imputed for modeling; see audit outputs.

## Baseline method and caution
- 75/25 stratified **random split after sampling**, train-only median imputation, RandomForestClassifier. This may overstate transfer to future days and related flows; **not temporal holdout, not production evaluation**.
- Held-out training rows: **11,702**, testing rows: **3,901**.
- Precision: **1.0000**; recall: **0.9982**; F1: **0.9991**.
- TN **1,696**, FP **0**, FN **4**, TP **2,201**.

## Visualization
![Historical SOC-style analytical dashboard](figures/soc_executive_dashboard.png)

## Seven recommendations
1. **Avoid live-SOC interpretations** — Evidence: CICIDS2017 is a historical research benchmark. Action: Label every artifact as retrospective; do not claim current threats. KPI: No production claims.
2. **Treat random-split scores as a baseline** — Evidence: Rows were randomly stratified after subsampling; related flows may span train/test. Action: Run cross-day or group-held-out follow-up where feasible. KPI: Publish temporal/group evaluation.
3. **Review missed attacks** — Evidence: Held-out false negatives: 4; recall 99.8%. Action: Inspect top missed attack labels in category_detection_performance.csv. KPI: Track missed counts and recall by label.
4. **Control false alerts** — Evidence: Held-out false positives: 0; FPR 0.0%. Action: Review benign false flags before operational escalation plans. KPI: Track false-positive rate alongside recall.
5. **Validate ingestion and schema** — Evidence: 1 source CSVs selected; 15,603 sampled rows. Action: Track file coverage, schema, missingness, infinity and row filtering. KPI: Reconcile sampled and SQL totals.
6. **Make class imbalance visible** — Evidence: Observed BENIGN records: 6,782; attack records: 8,821. Action: Preserve per-label and per-file distributions; avoid blanket prevalence claims. KPI: Publish category supports.
7. **Require contemporary authorized validation** — Evidence: Historical academic flows do not establish deployed accuracy. Action: Test under privacy-approved, modern, distinct traffic conditions. KPI: No deployment decision based on this notebook alone.

## Reproduce on Google Colab
1. Open `Project_13_CICIDS2017_SOC_Analytics_AutoDownload.ipynb`.
2. Run cells 1–8 in order; Cell 1 tries a public Kaggle download and offers upload fallback.
3. Review the source file list and sample counts before drawing conclusions.
4. Cell 8 writes a project ZIP and downloadable CSV/PNG/docs, excluding upstream source flow records.
5. Upload the notebook and extracted ZIP contents **with folders intact** to GitHub.

## Generated files
- `figures/soc_executive_dashboard.png`
- `outputs/*.csv`, `outputs/holdout_model_metrics.json`
- `README.md`, `requirements.txt`, `LICENSE_DATA_NOTICE.md`, `FILE_MANIFEST.csv`, `PROJECT_STATUS.md`

## Limitations
Historical experimental traffic; sampled data; variable mirror availability; no claim of operational threat prevalence or real-time alerts; random-row split has potential related-flow leakage. Validate on recent authorized data before use.
