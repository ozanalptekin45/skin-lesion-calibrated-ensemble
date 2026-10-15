# Skin Lesion Diagnosis: Calibrated Multimodal Ensemble

Code and reproducibility resources for research on a calibrated multimodal ensemble for multi-class skin-lesion diagnosis.

## Project overview

This repository contains research notebooks for developing and evaluating a skin-lesion classification workflow that combines visual features from Swin Transformer, EVA-02, and CoAtNet with patient-age and anatomical-site metadata. The workflow includes feature processing, classifier training, probability fusion, temperature scaling, and performance evaluation.

This repository provides research code, not a clinically validated diagnostic system. Results depend on the exact data splits, feature-extraction checkpoints, metadata availability, software environment, and evaluation protocol.

## Repository contents

```text
skin-lesion-calibrated-ensemble/
├── README.md
├── requirements.txt
├── .gitignore
└── notebooks/
    ├── README.md
    ├── 00_dataset_and_split_audit.ipynb
    ├── 01_swin_baseline.ipynb
    ├── 02_eva02_baseline.ipynb
    ├── 03_coatnet_baseline.ipynb
    ├── 04_post_feature_exploratory_comparisons.ipynb
    ├── 05_final_metadata_and_calibration_ablation.ipynb
    ├── 06_final_model.ipynb
    ├── 07_official_test_strict_ablation.ipynb
    └── 08_ph2_external_melanoma_transfer_colab.ipynb
```

The intended role of each notebook is listed below. Some notebooks contain exploratory experiments; they should not be interpreted as part of the final evaluation protocol unless the manuscript explicitly identifies them as such.

| Notebook | Purpose |
|---|---|
| `00_dataset_and_split_audit.ipynb` | Preliminary audit of split files, class distributions, and metadata completeness. |
| `01_swin_baseline.ipynb` | Swin Transformer baseline training and evaluation. |
| `02_eva02_baseline.ipynb` | EVA-02 baseline training and evaluation. |
| `03_coatnet_baseline.ipynb` | CoAtNet baseline training and fine-tuning experiments. |
| `04_post_feature_exploratory_comparisons.ipynb` | Exploratory post-feature classifier, fusion, and comparison experiments. Treat these results as exploratory unless explicitly pre-specified in the manuscript. |
| `05_final_metadata_and_calibration_ablation.ipynb` | Metadata, ensemble, and calibration ablation experiments. |
| `06_final_model.ipynb` | Final model pipeline and publication-oriented evaluation outputs. |
| `07_official_test_strict_ablation.ipynb` | Experiments involving the official ISIC 2019 test data. Test results must not be used to choose or modify the final model. |
| `08_ph2_external_melanoma_transfer_colab.ipynb` | External evaluation on PH2 for the melanoma/non-melanoma task. |

`requirements.txt` lists the Python packages used across the notebooks. The `notebooks/README.md` provides a short description of that directory.

> **Repository consistency:** Keep the filenames in this table synchronized with the actual files in `notebooks/`. Upload `00_dataset_and_split_audit.ipynb` if it is not yet present in the repository.

## Datasets and input files

The repository does **not** distribute ISIC 2019 or PH2 images, metadata tables, extracted feature arrays, or trained checkpoints. Obtain datasets from their respective official sources and comply with applicable access and use conditions.

Before running a notebook, review its setup cells and configure the paths to the split CSV files, metadata files, extracted features, checkpoints, and output directory. The required files differ by notebook. In particular, the baseline and post-feature notebooks may expect previously extracted `.npy` feature arrays.

The final ISIC 2019 pipeline uses the following canonical eight-class order:

`AK`, `BCC`, `BKL`, `DF`, `MEL`, `NV`, `SCC`, `VASC`.

Some official-test analyses exclude samples labelled `UNK` and report results on the eight known classes only. When that applies, describe the result as performance on the **known-class subset with `UNK` excluded**, not as performance on the complete nine-category challenge task.

Verify that feature rows are aligned with the image IDs and labels in the corresponding split files. Matching row counts alone do not establish correct alignment. For metadata experiments, verify that age and anatomical-site values are correctly mapped to image IDs and are available consistently for each split.

## Installation and environment

The notebooks were developed for Python-based environments, including Google Colab. Some baseline training and feature-extraction stages benefit from a compatible GPU. Dependencies are collected in [`requirements.txt`](requirements.txt).

### Google Colab

1. Open a notebook in Google Colab and select a runtime appropriate for the experiment.
2. Check the preinstalled PyTorch and TorchVision versions before installing packages. These two packages must be compatible with each other and with the selected runtime.
3. Install the project dependencies in a code cell:

   ```python
   %pip install -r requirements.txt
   ```

4. If the installation changes core libraries, restart the runtime and verify the environment before running the notebook.
5. Set the data, metadata, feature, checkpoint, and output paths in that notebook's configuration cell.
6. Run the notebook from top to bottom in a clean runtime.

For a local environment, create and activate a virtual environment first, then run `python -m pip install -r requirements.txt`. For GPU execution, install a PyTorch/TorchVision build compatible with the installed CUDA/runtime before executing the model notebooks.

The dependency ranges are intended to support the repository's combined notebook set; they are not a substitute for recording the exact package versions used to produce a particular reported result. Each experiment should record its Python, PyTorch, TorchVision, CUDA, scikit-learn, and other relevant package versions.

## Experimental protocol

The intended protocol separates model development from final evaluation:

1. Inspect the split files, class labels, feature alignment, and metadata completeness.
2. Train baseline models using the designated development data.
3. Select hyperparameters, ensemble weights, calibration parameters, and any decision rules using training/validation data only, according to the relevant notebook's protocol.
4. Freeze the model configuration before evaluating it on the official test set.
5. Use PH2 only according to the external-validation protocol described in its notebook.

Do not use official-test labels or metrics to select architectures, hyperparameters, fusion weights, calibration parameters, or decision thresholds. Where exploratory test-set comparisons are retained, distinguish them from the pre-specified final evaluation and do not use them to revise the final model.

## Outputs and reproducibility

Depending on the notebook, execution may generate classification metrics, calibration metrics (including ECE, NLL, and Brier score), ROC curves, confusion matrices, bootstrap confidence intervals, predictions, or parameter manifests. Generated outputs, extracted features, checkpoints, dataset files, and local environment paths should generally remain outside version control unless there is a clear scientific or sharing reason to include them and the relevant terms permit it.

Never commit API keys, access tokens, credentials, private data, or unnecessary patient-level information. The `.gitignore` file excludes common local data, feature, checkpoint, and output files; check staged changes before each commit because `.gitignore` does not remove files that have already been committed.

Not every notebook has been verified in a complete end-to-end run on the same GPU/software environment. Static checks and notebook cleanup should not be interpreted as proof that every experiment runs successfully or reproduces a reported result. Before relying on a result, verify data alignment, metadata mapping, group/lesion separation, checkpoint loading, and the exact evaluation protocol.

## Citation

If you use this code, cite the associated manuscript once its final bibliographic details and DOI are available. The citation information can be added here when confirmed.

## License

A repository license has not yet been specified. Unless and until a license is added by the copyright holders, do not assume that the code may be redistributed or reused beyond the rights provided by the hosting platform.

## Contact

For questions about the code or experimental protocol, open a GitHub issue if repository access and project policy permit it.
