# 3D Liver Tumor Segmentation using 3D U-Net

## Overview

This project presents an automated liver and liver tumor segmentation system using deep learning on 3D Computed Tomography (CT) scans. The system employs a 3D U-Net architecture to perform voxel-wise segmentation of liver and tumor regions from volumetric medical images.

The project is based on the Medical Segmentation Decathlon (Task03 Liver) dataset and demonstrates the complete workflow from data preprocessing and patch extraction to model training, evaluation, and inference.

---

## Problem Statement

Manual liver and tumor segmentation is a time-consuming process that requires expert radiologists and is prone to inter-observer variability. Automated segmentation can significantly improve efficiency in diagnosis, treatment planning, and disease monitoring.

This project aims to automatically identify and segment liver and tumor regions from abdominal CT scans using a deep learning-based approach.

---

## Dataset

**Medical Segmentation Decathlon – Task03 Liver**

Dataset Characteristics:

* 131 abdominal CT volumes
* Corresponding liver and tumor masks
* NIfTI (`.nii.gz`) format
* Multi-class segmentation

Classes:

| Label | Class      |
| ----- | ---------- |
| 0     | Background |
| 1     | Liver      |
| 2     | Tumor      |

---

## Methodology

### Data Preprocessing

The CT scans undergo preprocessing steps including:

* Loading volumetric medical images
* Resampling to a uniform spacing
* Patch extraction for memory-efficient training
* Dataset preparation using TorchIO

### Patch-Based Training

Large 3D volumes are divided into smaller patches:

```text
96 × 96 × 96
```

This enables efficient GPU memory utilization while preserving local anatomical information.

### Label Sampling

To address class imbalance, patches are sampled using weighted probabilities:

| Class      | Sampling Probability |
| ---------- | -------------------- |
| Background | 0.2                  |
| Liver      | 0.3                  |
| Tumor      | 0.5                  |

This ensures that tumor regions are observed more frequently during training.

---

## Model Architecture

The project utilizes a 3D U-Net architecture consisting of:

* Encoder
* Bottleneck
* Decoder
* Skip Connections

The encoder extracts hierarchical features while the decoder reconstructs the segmentation mask. Skip connections preserve spatial information and improve boundary localization.

---

## Training Configuration

| Parameter     | Value                  |
| ------------- | ---------------------- |
| Model         | 3D U-Net               |
| Optimizer     | Adam                   |
| Learning Rate | 1e-4                   |
| Loss Function | Weighted Cross Entropy |
| Epochs        | 100                    |
| Framework     | PyTorch Lightning      |
| Patch Size    | 96×96×96               |

---

## Project Workflow

```text
CT Scan
    ↓
Preprocessing
    ↓
Patch Extraction
    ↓
LabelSampler
    ↓
Patch Queue
    ↓
3D U-Net
    ↓
Training
    ↓
Checkpoint Generation
    ↓
Inference
    ↓
Segmentation Mask
```

---

## Technologies Used

* Python
* PyTorch
* PyTorch Lightning
* TorchIO
* NumPy
* Matplotlib
* SimpleITK
* Nibabel
* Jupyter Notebook

---

## Repository Structure

```text
.
├── 01-Data.ipynb
├── 02-Model.ipynb
├── 03-Train.ipynb
├── model.py
├── README.md
├── .gitignore
└── weights/
    └── epoch=97-step=25773.ckpt
```

---

## Model Checkpoint

The repository includes the final trained checkpoint:

```text
weights/epoch=97-step=25773.ckpt
```

This checkpoint can be loaded directly for inference and evaluation.

Example:

```python
model = Segmenter.load_from_checkpoint(
    "weights/epoch=97-step=25773.ckpt"
)
```

---

## Results

The model successfully performs automated segmentation of:

* Liver regions
* Tumor regions

Evaluation can be performed using:

* Precision
* Recall
* F1-Score
* Dice Score
* Accuracy

Visualization of predicted masks demonstrates the capability of the model to identify liver and tumor structures from unseen CT volumes.

---

## Future Improvements

* Dice Loss based optimization
* Multi-organ segmentation
* Improved tumor boundary localization
* Clinical deployment and validation

---



