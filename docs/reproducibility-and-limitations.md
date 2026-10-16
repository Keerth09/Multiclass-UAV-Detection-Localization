# Reproducibility, Limitations, and Repository Scope

## What this repository documents

This repository is intended as a clean public-facing record of the IIITDM internship project: problem statement, dataset workflow, architecture, knowledge-distillation approach, compression and quantization pipeline, evaluation results, embedded target, and related literature.

## What is not included by default

- Completed Jupyter Notebook
- Original dataset or augmented images
- Trained teacher checkpoints
- Trained student checkpoint
- Quantized `.tflite` model
- Arduino firmware / source sketch
- Evaluation CSV/JSON files
- Local environment-specific paths and credentials

Only claim that an artifact is included when the file is actually present in this repository.

## To make the project reproducible later

If redistribution is permitted, consider adding:
- A cleaned notebook or modular training scripts
- Exact dependency versions in `requirements.txt`
- Dataset acquisition and directory-layout instructions
- Model conversion and calibration scripts
- Arduino firmware and TFLite Micro integration instructions
- Evaluation scripts and raw metric files
- A versioned release containing approved model artifacts

## Known reporting details to verify before publication

1. The dataset section says RGB images were converted to grayscale, while the model pipeline describes RGB inputs. Check the notebook and document the actual path used for final training.
2. The report contains comparison-table metrics and separate final-run metrics for YOLO11. Keep these contexts distinct.
3. The report describes four TinyissimoYOLO teachers as separate experiments; do not call them an ensemble.
4. Latency/FPS should identify the exact environment and timing scope.
5. The report's literature comparisons should not be used to claim superiority unless the datasets and evaluation protocols are comparable.

## Ethical and legal considerations

Check the dataset's terms before sharing images or labels. Verify redistribution rights for third-party code, model configurations, and pretrained weights. Cite the source paper or repository when using third-party architectures.
