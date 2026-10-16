# Architecture and Methodology

## 1. End-to-end workflow

1. Prepare the four-class aerial-object dataset: Airplane, Bird, Drone, UAV.
2. Keep YOLO-format bounding-box annotations.
3. Resize teacher inputs to 256 × 256 × 3 and student inputs to 128 × 128 × 3, as described in the project workflow.
4. Run each selected teacher independently; teachers remain frozen during a corresponding distillation run.
5. Convert teacher predictions into soft-target information aligned with the student target format.
6. Encode original ground-truth labels into an 8 × 8 grid with nine values per cell.
7. Train the custom CNN student using hard-label detection losses and knowledge-distillation losses.
8. Apply approximately 10% magnitude pruning to the student.
9. Fine-tune after pruning to recover performance.
10. Perform full INT8 quantization using representative-data calibration.
11. Convert to TensorFlow Lite and prepare the Arduino C-header / TFLite Micro deployment artifacts.
12. Evaluate software inference and embedded hardware inference separately.

## 2. Teacher models

The report documents four TinyissimoYOLO teacher configurations:

| Teacher | Configuration | Parameters in supplied repository configuration before project-specific class-head changes |
|---|---|---:|
| TinyissimoYOLO V1-Small | `tinyissimo-v1-small.yaml` | 353,512 |
| TinyissimoYOLO V1 | `tinyissimo-v1.yaml` | 910,840 |
| TinyissimoYOLO V5-B | `tinyissimo-v5.yaml`, scale b | 888,784 |
| TinyissimoYOLO V8-B | `tinyissimo-v8.yaml`, scale b | 839,392 |

The report notes these counts were instantiated from the supplied YAMLs in their original 20-class configuration; project-specific four-class training can change detection-head parameter counts.

YOLO11n is also present in the reported model comparison and teacher-KD comparison. Describe the experiment actually performed rather than implying all teachers were ensembled.

## 3. Student CNN

The custom student is a 15-major-block depthwise-separable CNN.

- Input image: 128 × 128 × 3
- Output tensor: 8 × 8 × 9
- Parameter count: 133,305
- Approximate compute: 0.033337344 GFLOPs
- 105 Keras layers including input
- 34 convolution/depthwise layers
- Final filter flow reported: 16 → 16 → 32 → 64 → 96 → 112 → 128 → 104 → 80 → 48 → 9

The architecture progressively reduces spatial resolution from 128 × 128 to 8 × 8. Depthwise-separable convolutions reduce computation, residual blocks preserve feature information, and 1 × 1 pointwise layers mix or compress channels.

Each 8 × 8 grid cell predicts:
- x and y box values
- width and height
- objectness
- four class logits: Airplane, Bird, Drone, UAV

## 4. Hard-label and distillation losses

The report describes hard-label detection losses for:
- Bounding-box localization using CIoU
- Objectness
- Multiclass classification

Knowledge-distillation terms use teacher soft class information and teacher box guidance. The student is updated by backpropagation; the selected teacher remains frozen during each individual run.

## 5. Compression and conversion

The reported student compression path is:

`Custom CNN → ~10% magnitude pruning → recovery fine-tuning → full INT8 quantization → INT8 TFLite → TFLite Micro deployment`

The report specifies representative-data calibration for full INT8 quantization. The resulting model size reported for the final custom CNN is 229.414 KiB.

## 6. Inference post-processing

The project workflow applies confidence filtering and class-aware non-maximum suppression (NMS) to the raw detector output to produce bounding boxes, class labels, and confidence scores.

## 7. Evaluation

The report includes mAP@0.50, mAP@0.50:0.95, precision, recall, F1-score, model size, parameter count, approximate GFLOPs, latency, and FPS. Software CPU results and Arduino/TFLite Micro results must be reported separately.
