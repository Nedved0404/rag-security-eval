# Rethinking RAG Security: Code and Results

Code and full results for the paper:

> M. N. B. Islam, S. Saha, and A. Hirt, "Rethinking RAG Security: A Unified Evaluation of Attacks and Defenses Across the Pipeline," submitted to *IEEE Access*, 2026.

The study tests 9 attacks (A1 to A9) and 6 defenses (D1 to D6) on one shared RAG pipeline: a GTR-T5-base retriever, a FAISS index and a Flan-T5-Large reader. It uses four query samples (seeds 42, 7, 123 and 2024).

## What is in this repository

```
notebooks/
  RAG_Security_Rerun.ipynb      main experiments: all attacks, defenses, privacy, utility
  RAG_Generalization.ipynb      reranker, BGE retriever and Mistral reader
  RAG_Matched_Control.ipynb     defense-matched clean answers and final significance tests
results/
  main/                         output of RAG_Security_Rerun.ipynb
  generalization/               output of RAG_Generalization.ipynb
  matched_control/              output of RAG_Matched_Control.ipynb
```

## Where each table comes from

| Paper | File |
|---|---|
| Table 4 (success rules) | `results/main/success_rules.csv` |
| Table 5 (integrity attacks) | `results/matched_control/table2_matched.csv`, `significance_matched.csv` |
| Table 6 (privacy attacks) | `results/main/table3_A8.csv`, `table3_A9.csv` |
| Table 7 (D2 thresholds) | `results/main/d2_tau_sweep.csv` |
| Table 8 (utility cost) | `results/main/utility_summary.csv` |
| Table 9 (generalization) | `results/generalization/generalization_summary.csv` |

**Note:** `results/main/table2_integrity.csv` is from the first judging, which compared defended answers with the undefended clean answer. The paper's Table 5 uses the defense-matched version in `results/matched_control/`.

## Other files in `results/main/`

- `run_config.json`: every setting used, including prompts, seeds, thresholds, model names, decoding settings and the PyTorch version.
- `corpus.jsonl` and `holdout.jsonl`: indexed passages (with the restricted flag used by D4 to D6) and the held-out non-member passages.
- `query_pool_*.jsonl` and `queries_seed*_*.json`: query pools and the queries drawn for each seed.
- `artefacts_seed*_A*.jsonl`: every generated adversarial document (wrong answers, poisoned passages, topic passages, optimized A4 documents with their target queries, and so on).
- `records_seed*_A*.csv`: per-instance results, including the D1 paraphrase used (`gen_query`).
- `privacy_*.csv`, `ppl_by_attack.csv`, `retrieval_diagnostics.csv`: per-instance privacy, perplexity and retrieval data.

## How to run

The notebooks were run in Google Colab on one NVIDIA A100 (40 GB). They read and write to `MyDrive/rag_rerun_full` in Google Drive.

1. Open the notebook in Colab and choose a GPU runtime (an A100 is recommended).
2. Mistral-7B-Instruct is a gated model. Accept its terms on Hugging Face, then add your token in Colab under **Secrets** with the name `HF_TOKEN`. Do not paste the token into the notebook.
3. Run the notebooks in this order: `RAG_Security_Rerun.ipynb`, then `RAG_Generalization.ipynb`, then `RAG_Matched_Control.ipynb`.

The full main run takes many hours, so the notebooks save backups to Drive along the way. To check the results without re-running anything, use the files in `results/`.

Answer generation is greedy (deterministic). Sampling is used only to create paraphrases and adversarial text, and every random generator is seeded. GPU kernels can still cause tiny differences, which is why all generated texts are included here.

## Data

Passages and queries come from Natural Questions and MS MARCO, which are covered by their own licences. The license below covers the code only.

## License

The code is released under the MIT License (see `LICENSE`).
