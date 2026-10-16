# Hardware and Embedded Deployment

## Target board

The project report describes the Arduino Nano 33 BLE Sense as a compact development board based on the Nordic nRF52840:

- 32-bit ARM Cortex-M4 processor at 64 MHz
- 1 MB Flash
- 256 KB SRAM
- 3.3 V logic
- Bluetooth 5
- USB, I2C, SPI and UART interfaces

Follow the board manufacturer's electrical guidance. The report warns that applying 5 V directly to its I/O pins can damage the board.

## Deployment chain

1. Train the custom CNN student.
2. Apply approximately 10% magnitude pruning.
3. Fine-tune after pruning.
4. Quantize to full INT8 using representative calibration data.
5. Convert to TensorFlow Lite.
6. Prepare the TFLite Micro-compatible model and C-header artifacts.
7. Integrate the model into the embedded firmware.
8. Evaluate inference on the Arduino board and compare with software inference.

## Reported custom CNN hardware measurements

| Metric | Reported result |
|---|---:|
| mAP@0.50 | 60.490% |
| Precision | 72.846% |
| Recall | 70.625% |
| F1-score | 71.718% |
| Mean latency | 1703.106 ms/image |
| FPS | 0.587 |

These are the report's recorded hardware figures. The repository documentation does not contain the board firmware or benchmark logs needed to independently reproduce them.

## Practical limitation

A small model file does not by itself guarantee that a full object-detection pipeline fits or runs efficiently on a microcontroller. Tensor arena allocation, input/output buffers, operator support, image preprocessing, and post-processing all affect memory and latency. Report measured device results separately from desktop TFLite results.
