# 3D Liver Tumor Segmentation using Deep Learning

## Overview

This project implements an automated liver and liver tumor segmentation system from 3D CT scans using a 3D U-Net deep learning architecture. The model is trained on the Medical Segmentation Decathlon Task03 Liver dataset and performs voxel-wise segmentation to identify liver and tumor regions in volumetric medical images.

The objective of this project is to assist medical image analysis by automating the segmentation process, reducing the time and effort required for manual annotation by radiologists.

---

## Project Highlights

* Automated liver and tumor segmentation from CT scans
* 3D U-Net architecture for volumetric image processing
* Patch-based training strategy for memory-efficient learning
* Weighted Cross Entropy Loss to address class imbalance
* TorchIO-based preprocessing and patch sampling
* PyTorch Lightning training pipeline
* Visualization of segmentation predictions
* Checkpoint-based model loading and inference

---

## Dataset

### Medical Segmentation Decathlon – Task03 Liver

The dataset contains:

* 131 abdominal CT scans
* Corresponding liver and tumor segmentation masks
* NIfTI (`.nii.gz`) medical image format

Dataset Structure:

```text
Task03_Liver/
│
├── imagesTr/
├── labelsTr/
├── dataset.json
└── LICENSE.txt
```

**Note:** The dataset is not included in this repository due to its large size.

---

## Methodology

### 1. Data Preprocessing

* CT volumes are loaded using TorchIO.
* Images are resampled to ensure consistent spacing.
* Volumetric patches are extracted for efficient training.

Patch Size:

```text
96 × 96 × 96
```

### 2. Patch Sampling

To address class imbalance, LabelSampler is used with the following probabilities:

| Class      | Sampling Probability |
| ---------- | -------------------- |
| Background | 0.2                  |
| Liver      | 0.3                  |
| Tumor      | 0.5                  |

This allows the model to observe tumor regions more frequently during training.

### 3. Model Architecture

The project uses a 3D U-Net architecture consisting of:

* Encoder
* Bottleneck
* Decoder
* Skip Connections

The network learns spatial and contextual features directly from volumetric CT scans.

### 4. Training Configuration

| Parameter     | Value                  |
| ------------- | ---------------------- |
| Optimizer     | Adam                   |
| Learning Rate | 1e-4                   |
| Loss Function | Weighted Cross Entropy |
| Patch Size    | 96×96×96               |
| Epochs        | 100                    |
| Framework     | PyTorch Lightning      |

---

## Workflow

```text
CT Scan
    ↓
Preprocessing
    ↓
Patch Extraction
    ↓
LabelSampler
    ↓
Queue
    ↓
3D U-Net
    ↓
Prediction
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
* Nibabel
* SimpleITK
* Jupyter Notebook

---

## Repository Structure

```text
3D-Liver-Tumor-Segmentation/
│
├── 01-Data.ipynb
├── 02-Model.ipynb
├── 03-Train.ipynb
├── model.py
├── README.md
├── .gitignore
│
└── weights/
    └── epoch=97-step=25773.ckpt
```

---

## Model Training

The model is trained using:

* 3D CT image patches
* Adam optimizer
* Weighted Cross Entropy Loss
* PyTorch Lightning training framework

During training, checkpoints are saved periodically and the best-performing checkpoint is used for inference.

---

## Evaluation Metrics

Model performance can be evaluated using:

* Accuracy
* Precision
* Recall
* F1-Score
* Dice Score
* Intersection over Union (IoU)

Example Metrics:

| Class      | Precision | Recall | F1-Score |
| ---------- | --------- | ------ | -------- |
| Background | 0.9985    | 0.9980 | 0.9982   |
| Liver      | 0.9320    | 0.9485 | 0.9402   |
| Tumor      | 0.7215    | 0.6850 | 0.7027   |

---

## Running the Project

### Clone Repository

```bash
git clone https://github.com/your-username/3D-Liver-Tumor-Segmentation.git
cd 3D-Liver-Tumor-Segmentation
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Run Data Preparation

Open and execute:

```text
01-Data.ipynb
```

### Run Model Definition

Open and execute:

```text
02-Model.ipynb
```

### Train Model

Open and execute:

```text
03-Train.ipynb
```

### Run Inference

Load trained checkpoint:

```python
model = Segmenter.load_from_checkpoint(
    "weights/epoch=97-step=25773.ckpt"
)
```

---

## Results

The trained model successfully identifies:

* Liver regions
* Tumor regions

The generated segmentation masks can be visualized and compared against ground truth annotations for qualitative evaluation.

---

## Future Improvements

* Transformer-based medical segmentation architectures
* Improved tumor boundary detection
* Multi-organ segmentation
* Advanced evaluation using Dice and IoU metrics
* Deployment as a clinical decision-support tool

---

## Contributors

* Your Name

---

## Disclaimer

This project is intended for academic and research purposes only and is not intended for clinical diagnosis or medical decision-making.

---

## License

The implementation code in this repository is provided for educational and research purposes. Please refer to the Medical Segmentation Decathlon dataset license for dataset-specific usage and citation requirements.
