# Benchmark Results

These values are transcribed from the final internship report. Treat them as **reported project results**, not as independently reproduced measurements.

## A. Final custom CNN INT8 TFLite software results

| Metric | Value | Unit |
|---|---:|---|
| mAP@0.50 | 60.970 | % |
| mAP@0.50:0.95 | 32.640 | % |
| Precision | 73.210 | % |
| Recall | 70.820 | % |
| F1-score | 71.990 | % |
| Mean latency | 1.384742 | ms/image |
| FPS | 722.156185 | frames/s |
| INT8 model size | 229.414 | KiB |
| Parameters | 133,305 | parameters |
| Approximate computation | 0.033337344 | GFLOPs |
| Final pruning sparsity | ~10.001 | % |

## B. Knowledge-distillation software comparison

The report compares the same CustomCNN128 student distilled from different teachers. Student architecture complexity is reported as 133,305 parameters and 0.033337344 GFLOPs for these rows.

| Teacher experiment | Student INT8 size (KiB) | mAP@0.50 (%) | Precision (%) | Recall (%) | F1 (%) | Latency (ms) | FPS |
|---|---:|---:|---:|---:|---:|---:|---:|
| V1 | 234.804 | 64.180 | 69.710 | 73.670 | 71.640 | 1.052 | 950.387 |
| V1-Small | 237.125 | 64.080 | 72.680 | 72.910 | 72.800 | 0.368 | 2711.085 |
| V5-B | 235.179 | 63.600 | 74.720 | 72.370 | 73.530 | 0.920 | 1085.927 |
| V8-B | 229.414 | 59.800 | 73.870 | 69.290 | 71.510 | 0.831 | 1202.629 |
| YOLO11 / YOLOV11 row as named in report | 229.414 | 64.238 | 69.809 | 75.412 | 72.502 | 1.073 | 931.904 |

**Important:** Preserve the report's experiment naming and metric conventions. The report's YOLO11 software comparison row uses mAP@0.50 = 64.238% in the comparison table, while a separate note discusses a deployment-threshold comparison. Do not mix these values without explaining the evaluation convention.

## C. Software vs Arduino Nano 33 BLE Sense hardware

| Model row | Environment | mAP@0.50 (%) | Precision (%) | Recall (%) | F1 (%) | Latency (ms/image) | FPS |
|---|---|---:|---:|---:|---:|---:|---:|
| Custom CNN | Software TFLite INT8 | 60.970 | 73.210 | 70.820 | 71.990 | 1.384 | 722.156 |
| Custom CNN | Arduino Nano 33 BLE Sense / TFLite Micro | 60.490 | 72.846 | 70.625 | 71.718 | 1703.106 | 0.587 |
| V1 | Arduino Nano 33 BLE Sense / TFLite Micro | 64.278 | 69.687 | 73.737 | 71.655 | 1702.647 | 0.587 |
| V1-Small | Arduino Nano 33 BLE Sense / TFLite Micro | 63.805 | 72.519 | 72.657 | 72.588 | 1702.655 | 0.587 |
| V5-B | Arduino Nano 33 BLE Sense / TFLite Micro | 64.157 | 74.194 | 72.403 | 73.288 | 1696.370 | 0.589 |
| V8-B | Arduino Nano 33 BLE Sense / TFLite Micro | 59.091 | 74.087 | 69.006 | 71.456 | 1696.785 | 0.589 |
| YOLO11 | Arduino Nano 33 BLE Sense / TFLite Micro | 64.582 | 70.162 | 75.349 | 72.663 | 1696.363 | 0.589 |

The report also records a separate YOLO11 final hardware run over all 3,072 test images with final total mAP@0.50 = 68.104% and mAP@0.50:0.95 = 36.624%. Do not silently replace the comparison-table row with this separate final-total result; explain the evaluation context when reporting both.

## Interpretation cautions

- FPS is the reciprocal of latency only when the measurement definitions and timing scope match.
- Do not compare software CPU and microcontroller latency as if they were measured under identical conditions.
- The report itself warns against comparing literature mAP values unless dataset, test split, IoU convention, and evaluation protocol match.
- For public claims, retain the exact metric definitions and test-set size alongside the numbers.
