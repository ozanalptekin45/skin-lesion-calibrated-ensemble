# skin-lesion-calibrated-ensemble
Code and reproducibility resources for a calibrated multimodal ensemble for skin lesion diagnosis.
# Skin Lesion Diagnosis: Calibrated Multimodal Ensemble

Code and reproducibility resources for research on a calibrated multimodal ensemble for multi-class skin lesion diagnosis.

## Project overview

This repository contains research notebooks for developing and evaluating a skin-lesion classification pipeline that combines visual features from Swin Transformer, EVA-02, and CoAtNet with patient-age and anatomical-site metadata. The workflow includes feature processing, classifier training, probability fusion, temperature scaling, and evaluation.

The repository is intended to support methodological transparency and reproducibility. It is research code, not a clinically validated diagnostic system.

## Repository contents

The `notebooks/` directory contains the experiment notebooks. Their intended roles are:

- `00_dataset_and_split_audit.ipynb` — inspect dataset splits, class distributions, and metadata quality.
- `01_swin_baseline.ipynb` — Swin baseline experiments.
- `02_eva02_baseline.ipynb` — EVA-02 baseline experiments.
- `03_coatnet_baseline.ipynb` — CoAtNet baseline experiments.
- Post-feature notebooks — classifier, fusion, calibration, and ablation experiments.
- Final evaluation notebook(s) — evaluation of a fixed model configuration.
- `08_ph2_external_melanoma_transfer_colab.ipynb` — PH2 external-validation workflow.

Notebook filenames and availability may evolve as the repository is organized. Each notebook should be read together with its own setup notes and stated experimental protocol. Exploratory experiments should not be treated as the final evaluation protocol unless explicitly identified as such.

## Datasets

This repository does **not** distribute the ISIC 2019 or PH2 images, metadata files, extracted features, or trained checkpoints. Obtain datasets from their respective official sources and comply with their access and use terms.

Before running experiments, prepare the data and feature files expected by the selected notebook. Check that image identifiers, labels, feature rows, and split files are aligned. Do not assume that matching row counts alone prove that the rows are aligned.

The eight-class label mapping used by the final ISIC 2019 pipeline is:

`AK`, `BCC`, `BKL`, `DF`, `MEL`, `NV`, `SCC`, `VASC`.

Some official-test analyses may exclude `UNK` samples and evaluate only the eight known classes. When applicable, results must be described explicitly as performance on the known-class subset, not as performance on the complete nine-category challenge task.

## Experimental protocol

The intended protocol separates model development from final evaluation:

1. Inspect the data splits, class labels, and metadata.
2. Train baseline feature/classifier pipelines using the development data.
3. Select hyperparameters, fusion weights, and calibration parameters using training/validation data only, as specified by the relevant notebook.
4. Freeze the selected configuration before evaluating it on the official test set.
5. Use PH2 only for the external-validation protocol described in its notebook.

Do not use official-test labels or metrics to select architectures, hyperparameters, fusion weights, calibration parameters, or decision thresholds. Exploratory test-set comparisons, if retained in the repository, must be clearly distinguished from the pre-specified final evaluation.

## Environment and execution

These notebooks were developed for Python-based environments, including Google Colab, and some experiments require a compatible GPU. Requirements may vary by notebook and by pretrained backbone.

Before running a notebook:

1. Review its first setup/configuration cells.
2. Set the dataset, split, metadata, feature, and output paths for your environment.
3. Install the compatible library versions required by that notebook.
4. Confirm that the selected pretrained checkpoints and feature files correspond to the intended model variants and image preprocessing.
5. Run the notebook from top to bottom in a clean runtime and verify that the saved results are generated without errors.

A complete end-to-end run has not necessarily been verified for every notebook. Treat the notebooks as research resources and check the relevant execution notes before relying on results.

## Reproducibility and outputs

Experiments may use fixed random seeds, validation-based model selection, temperature scaling, ensemble-weight optimization, and bootstrap confidence intervals. The precise settings for each experiment are defined in its notebook. Reported tables and figures should be traceable to the corresponding evaluation code and a fixed model configuration.

Generated outputs, extracted features, checkpoints, dataset files, and local environment paths should remain outside version control unless there is a clear need and the relevant sharing terms permit them. Never commit API keys, access tokens, credentials, or private patient-level information.

## Limitations

Performance estimates depend on the exact data split, preprocessing, feature-extraction checkpoints, metadata availability, and evaluation protocol. External validation and test-set evaluation do not, by themselves, establish clinical utility or readiness for deployment. Verify data alignment, metadata mapping, group/lesion separation, and checkpoint loading before interpreting results.

## Citation

If you use this code, please cite the associated manuscript once its final bibliographic details and DOI are available. Until then, manuscript citation information will be added here when confirmed.

## License

A repository license has not yet been specified. Unless and until a license is added by the copyright holders, do not assume that the code may be redistributed or reused beyond the rights provided by the hosting platform.

## Contact

For questions about the code or experimental protocol, please open a GitHub issue if repository access and project policy permit it.
