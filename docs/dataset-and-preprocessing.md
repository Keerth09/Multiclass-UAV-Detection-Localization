# Dataset and Preprocessing

## Dataset

The project report identifies Roboflow as the source of a multiclass aerial-object detection dataset. Four labels are used throughout the project:

1. Airplane
2. Bird
3. Drone
4. UAV

Bounding-box annotations use YOLO format.

## Dataset evolution reported in the project document

| Stage | Count / split | Notes |
|---|---:|---|
| Original dataset | 30,720 images | Before project preprocessing |
| Initial training split | 24,576 | 80% |
| Initial validation split | 3,072 | 10% |
| Initial test split | 3,072 | 10% |
| After preprocessing and augmentation | 48,554 images | Report does not list individual augmentation operators |
| Final training set | 19,997 | Reduced to remove exact and visually redundant samples |
| Final validation set | 3,072 | Kept unchanged |
| Final test set | 3,072 | Kept unchanged |
| Final total | 26,141 | 19,997 + 3,072 + 3,072 |

The report says RGB images were converted to grayscale during preprocessing. The model workflow diagram later describes RGB inputs for teacher and student processing. Confirm the exact notebook implementation before treating those details as a single consistent preprocessing configuration.

## Duplicate and similarity reduction

- **SHA-256 exact hashing:** identifies byte-identical image files.
- **Perceptual hashing (pHash):** represents visual content so visually similar images can be detected even when file bytes differ.
- **BK-tree:** indexes perceptual hashes to efficiently find similar images.
- **Hamming-distance threshold of 5:** used to identify highly similar candidates for reduction.

The stated goal was to remove redundancy and reduce training cost while keeping validation and test splits fixed for fair comparison.

## Dataset publication and privacy

Do not upload the dataset automatically. Check the dataset's license, attribution requirements, and redistribution permissions first. If the dataset cannot be redistributed, publish a dataset description and instructions for obtaining it from its original source instead.
