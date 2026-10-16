# Multiclass UAV Detection and Localization using TinyML and Knowledge Distillation

**IIITDM Kurnool Internship Project**  
**Research topic:** Embedded-Aware Knowledge Distillation for TinyML Multiclass UAV Detection and Localisation  
**Institution:** Indian Institute of Information Technology Design and Manufacturing, Kurnool  
**Student:** Avula Keerthana  
**Supervisor:** Dr. M. Kiran Reddy, Assistant Professor, Department of Electronics and Communication Engineering  
**Internship period:** 11 May 2026 – 6 September 2026

## Project overview

This project investigates multiclass aerial-object detection and localization using compact neural networks designed for resource-constrained embedded platforms. It compares YOLO-family teacher detectors and studies knowledge distillation into a custom lightweight CNN student, followed by pruning, recovery fine-tuning, INT8 quantization, TensorFlow Lite conversion, and TensorFlow Lite Micro deployment evaluation for the Arduino Nano 33 BLE Sense.

The four detection classes are **Airplane, Bird, Drone, and UAV**. The detector predicts bounding boxes, objectness, and class scores.

> **Scope note:** The accompanying internship report documents TinyissimoYOLO V1, V1-Small, V5-B, and V8-B as separate frozen-teacher knowledge-distillation experiments. YOLO11n is also included in the comparison experiments. These should not be described as a single teacher ensemble.

## Project goals

- Prepare and reduce a multiclass aerial-object detection dataset while retaining fixed validation and test splits.
- Compare lightweight YOLO-family teacher models under the project's evaluation setup.
- Design a compact depthwise-separable CNN student for 128 Ã— 128 RGB inputs.
- Transfer teacher information to the student through a combination of hard-label detection supervision and knowledge-distillation losses.
- Reduce the student model's storage footprint using pruning and full INT8 quantization.
- Evaluate the TensorFlow Lite model on CPU and the TensorFlow Lite Micro implementation on Arduino Nano 33 BLE Sense.
- Report accuracy, model size, computational cost, latency, and FPS together rather than judging models on accuracy alone.

## Classes and dataset

| Item | Reported details |
|---|---|
| Dataset source | Roboflow multiclass aerial-object detection dataset |
| Classes | Airplane, Bird, Drone, UAV |
| Annotation format | YOLO-format bounding boxes |
| Original dataset | 30,720 images |
| Initial split | 24,576 train / 3,072 validation / 3,072 test |
| Working pool after preprocessing and augmentation | 48,554 images |
| Final training set after redundancy reduction | 19,997 images |
| Final validation set | 3,072 images |
| Final test set | 3,072 images |
| Final total | 26,141 images |

The report describes RGB-to-grayscale conversion during preprocessing, augmentation, SHA-256 exact-duplicate detection, perceptual hashing (pHash), a BK-tree for similarity search, and a Hamming-distance threshold of 5 for near-duplicate candidates. The validation and test sets were kept unchanged during training-set reduction.

## Methods and tools

| Technique / tool | Role in the project |
|---|---|
| TinyissimoYOLO V1 / V1-Small / V5-B / V8-B | Separate frozen teacher candidates for knowledge-distillation experiments |
| YOLO11n | Additional teacher/comparison model in the reported experiments |
| Custom lightweight CNN | Student detector designed around a compact 128 Ã— 128 Ã— 3 input and 8 Ã— 8 Ã— 9 output |
| Knowledge distillation | Transfers useful teacher prediction information to the student |
| CIoU localization loss | Bounding-box localization supervision |
| Objectness and multiclass classification losses | Hard-label detection supervision |
| Teacher soft class information and box guidance | Distillation supervision |
| Magnitude pruning (~10%) | Reduces student weight redundancy |
| Recovery fine-tuning | Recovers performance after pruning |
| Full INT8 quantization | Reduces model storage and inference cost |
| TensorFlow Lite | Portable quantized student inference format |
| TensorFlow Lite Micro (TFLM) | Microcontroller inference runtime |
| PyTorch | Teacher training workflow, depending on model implementation |
| TensorFlow / Keras | Custom CNN student development and conversion workflow |
| Arduino Nano 33 BLE Sense | Embedded deployment evaluation target |

## Student architecture

The report describes a 15-major-block depthwise-separable CNN:

- Input: `128 Ã— 128 Ã— 3` RGB image
- Output: `8 Ã— 8 Ã— 9` raw detection tensor
- Parameters: `133,305`
- Approximate computation: `0.033337344 GFLOPs`
- Keras layers: `105` including the input layer
- Convolution/depthwise layers: `34`
- Final INT8 TFLite model size: `229.414 KiB`
- Final pruning sparsity: approximately `10.001%`

Each output grid cell represents four box values, objectness, and four class scores. Post-processing uses confidence filtering and class-aware non-maximum suppression (NMS).

See [`docs/architecture-and-methodology.md`](docs/architecture-and-methodology.md).

## Reported final software results

The final custom CNN INT8 TFLite software results in the report are:

| Metric | Reported value |
|---|---:|
| mAP@0.50 | 60.970% |
| mAP@0.50:0.95 | 32.640% |
| Precision | 73.210% |
| Recall | 70.820% |
| F1-score | 71.990% |
| CPU latency | 1.384742 ms/image |
| CPU FPS | 722.156 |
| INT8 model size | 229.414 KiB |

These are the values reported by the project document. Runtime numbers depend on the evaluation environment and should not be presented as Arduino hardware latency.

## Embedded hardware comparison

The report's software-versus-hardware table reports the Custom CNN on Arduino Nano 33 BLE Sense using TFLite Micro at approximately:

- mAP@0.50: 60.490%
- Precision: 72.846%
- Recall: 70.625%
- F1-score: 71.718%
- Mean latency: 1703.106 ms/image
- FPS: 0.587

The report also contains a separate YOLO11 hardware run with final total mAP@0.50 of 68.104% and mAP@0.50:0.95 of 36.624%. Refer to the full report before comparing rows because the report distinguishes deployment-threshold comparison values from final total values.

## Repository contents

- `docs/architecture-and-methodology.md` â€” pipeline, student architecture, losses, compression and deployment
- `docs/dataset-and-preprocessing.md` â€” dataset counts, splits and duplicate-reduction methods
- `results/benchmark-results.md` â€” reported metrics and interpretation cautions
- `docs/hardware-deployment.md` â€” Arduino Nano 33 BLE Sense and TFLM notes
- `references/literature-survey.md` â€” papers and DOI links listed in the project report
- `docs/reproducibility-and-limitations.md` â€” what is and is not included in this documentation repository

## Important reproducibility note

This repository documents the project and its reported results. It does **not** contain the completed Jupyter Notebook, dataset images, trained weights, quantized model binaries, Arduino firmware, or generated evaluation files unless these are added separately. Therefore, the documentation alone is not a runnable reproduction of the experiments.

Dataset licensing and redistribution permissions should be checked before publishing dataset files. Large model files and datasets should generally be stored separately (for example, in an approved artifact repository) and linked here rather than committed directly to Git.

## Citation and attribution

This work was conducted as an internship research project at IIITDM Kurnool under the supervision of Dr. M. Kiran Reddy. The project report includes a literature survey of 17 papers; see [`references/literature-survey.md`](references/literature-survey.md).

---

