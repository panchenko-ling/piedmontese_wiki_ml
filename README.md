# Italian → Piedmontese MT with NLLB-200

Code for building Piedmontese training data from Wikipedia, adapting NLLB-200 to Piedmontese, and evaluating the outputs with automatic metrics. It accompanies:

- Yulia Panchenko. "When metrics miss the language: Machine translation evaluation for Piedmontese". In: *Proceedings of the Twelfth Italian Conference on Computational Linguistics*. CLiC-it '26. Palermo, Italy: CEUR Workshop Proceedings, 2026. Forthcoming.
- Yulia Panchenko. *Low-Resource Machine Translation for Piedmontese: Model Training, Human Evaluation, and Metric Reliability*. Master's thesis, University of Turin, 2026.

Data and models are on Hugging Face:

- Data: [offelia39/pms-wiki-mined-corpora](https://huggingface.co/datasets/offelia39/pms-wiki-mined-corpora), with the Piedmontese Wikipedia corpus, the mined Italian–Piedmontese pairs, and the test translations of all four models
- Models: one repo per condition (table below)

## Conditions

All four start from `facebook/nllb-200-distilled-600M`, with a new `pms_Latn` language token whose embedding is the average of Italian, Occitan, Ligurian and Lombard.

| Condition | Training | Model |
|---|---|---|
| NLLB-avg | none (zero-shot) | [nllb-600M-pms-avg](https://huggingface.co/offelia39/nllb-600M-pms-avg) |
| NLLB-avg+MT | MT fine-tuning on the mined pairs | [nllb-600M-pms-avg-mt](https://huggingface.co/offelia39/nllb-600M-pms-avg-mt) |
| NLLB-avg+DAE-50K+MT | denoising on 50K Wikipedia sentences, then MT | [nllb-600M-pms-avg-dae50k-mt](https://huggingface.co/offelia39/nllb-600M-pms-avg-dae50k-mt) |
| NLLB-avg+DAE-full+MT | denoising on all 191,884 sentences, then MT | [nllb-600M-pms-avg-daefull-mt](https://huggingface.co/offelia39/nllb-600M-pms-avg-daefull-mt) |

The CLiC-it paper reports the first three, and calls NLLB-avg+DAE-50K+MT `NLLB-avg+DAE+MT`. NLLB-avg+DAE-full+MT appears in the thesis only.

## Notebooks

All notebooks are written for Google Colab and keep their files in one Google Drive folder (`BASE` at the top of each).

| Notebook | What it does | Output |
|---|---|---|
| [`Piedmontese_wiki_release.ipynb`](Piedmontese_wiki_release.ipynb) | filters and cleans the Piedmontese Wikipedia dump, removes sentences that are in the parallel corpus | `pmswiki_noleak.jsonl` |
| [`wiki_miner_release.ipynb`](wiki_miner_release.ipynb) | pairs Piedmontese and Italian articles via Wikidata, aligns sentences with LaBSE | `wiki_mined.csv` |
| [`NLLB_IT_PMS_release.ipynb`](NLLB_IT_PMS_release.ipynb) | adds `pms_Latn`, runs DAE and MT training, translates the test set | models, `translations.csv` |
| [`NLLB_IT_PMS_metrics_release.ipynb`](NLLB_IT_PMS_metrics_release.ipynb) | Vico & Libovický (2026) and Casola et al. (2025) metrics, paired bootstrap tests | tables, plots, `significance.csv` |

The two data notebooks depend on each other: run §1 of the Wiki corpus notebook first, then the miner, then the rest of the Wiki corpus notebook. The training and metrics notebooks read their inputs from Hugging Face, so they can be run without the first two. To get the translations without training, run §0, §1 and §5 of the training notebook with `FROM_HUB = True`.

Training and evaluation need a GPU. The metrics notebook runs in separate sessions, since COMET and BLEURT need conflicting package versions; the steps are at the top of the notebook.

## Data notes

- Evaluation uses the 140 unique FLORES+ dev/devtest sentences of [ufal/CrowdsourcingPiedmontese](https://huggingface.co/datasets/ufal/CrowdsourcingPiedmontese) (Vico & Libovický, 2026). The test file on Hugging Face also has the references respelled in standard Piedmontese orthography (`reference_std_revised`).
- The released models were trained on the original mined corpus (`wiki_mined.csv`, 5,304 pairs). The Hugging Face dataset has a cleaned version (`wiki_mined_clean.csv`, 4,714 pairs) without identical pairs and near-copies, non-Italian sources and non-Piedmontese targets. The training notebook uses the cleaned version by default.
- The Wikidata links are fetched live, so rerunning the miner can give slightly different pairs.

## Citation

```bibtex
@inproceedings{panchenko2026metrics,
  author    = {Panchenko, Yulia},
  title     = {When metrics miss the language: Machine translation evaluation for {P}iedmontese},
  booktitle = {Proceedings of the Twelfth Italian Conference on Computational Linguistics (CLiC-it '26)},
  address   = {Palermo, Italy},
  publisher = {CEUR Workshop Proceedings},
  year      = {2026},
  note      = {Forthcoming}
}

@mastersthesis{panchenko2026thesis,
  author = {Panchenko, Yulia},
  title  = {Low-Resource Machine Translation for {P}iedmontese: Model Training, Human Evaluation, and Metric Reliability},
  school = {University of Turin},
  year   = {2026}
}
```

## License

CC BY-SA 4.0 (see `LICENSE`). The models on Hugging Face are CC BY-NC 4.0, inherited from NLLB-200.

## Contact

Yulia Panchenko, yulia.panchenko@edu.unito.it
