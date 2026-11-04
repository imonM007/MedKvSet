# MedKvSet: Enhancing Medical Image Segmentation through Negative Sample Integration

Code and evaluation notebooks for the paper:

**"Enhancing Medical Image Segmentation through Negative Sample Integration: A Study on Kvasir-SEG and Augmented Datasets"**

*Imon Mahbub, Ahmed Zoha Zim, Md Talha Bin Imran, K. M. Fatin Mesbah, Md Jobayer, and Md. Mehedi Hasan Shawon*  
BRAC University & Linköping University

---

## Overview

Medical image segmentation models trained exclusively on positive samples (images containing polyps) often generate elevated false-positive rates when tested on normal mucosa. This study evaluates deep learning segmentation performance when augmenting polyp images with polyp-free (negative) samples.

We evaluate three architectures using 5-fold cross-validation:
* **ResNet**: ResNet-18 residual encoder with transposed convolution decoder
* **U-Net**: Symmetric encoder-decoder with feature skip connections
* **SegNet**: VGG16-based encoder-decoder with pooling indices reuse

Experiments compare two dataset configurations:
1. **1000 Images (Baseline)**: 1000 polyp images from the Kvasir-SEG dataset.
2. **1800 Images (Expanded)**: 1000 polyp images (Kvasir-SEG) combined with 800 polyp-free colon images (WCE Curated Colon Dataset).

*Note: Because dataset size and sample composition were expanded simultaneously, individual contributions of negative samples cannot be fully separated from the effect of dataset scaling without an 1800 positive-only control.*

---

## Repository Structure

```
MedKvSet/
├── ResNet/
│   ├── 1000 Image/
│   │   ├── resnet_1000.ipynb
│   │   └── resnet_1000.csv
│   └── 1800 Image/
│       ├── resnet_1800.ipynb
│       └── resnet_1800.csv
├── SegNet/
│   ├── 1000 Image/
│   │   ├── segnet_1000.ipynb
│   │   └── segnet_1000.csv
│   └── 1800 Image/
│       ├── segnet_1800.ipynb
│       └── segnet_1800.csv
├── U-Net/
│   ├── 1000 Image/
│   │   ├── unet1000.ipynb
│   │   └── unet_1000.csv
│   └── 1800 Image/
│       ├── unet1800.ipynb
│       └── unet_1800.csv
├── AvgValue.ipynb
├── ModelCompareForAll.ipynb
└── README.md
```

### Directory Contents

* **Model Directories (`ResNet/`, `SegNet/`, `U-Net/`)**:
  * `*.ipynb`: Self-contained training and 5-fold cross-validation notebooks.
  * `*.csv`: Validation metrics logged across each fold (Accuracy, Dice, IoU, Precision, Recall, F1-Score).
* **`AvgValue.ipynb`**: Automated aggregation script (`compute_all_models_table`) that imports fold-level CSV files and outputs the mean ± standard deviation summary reported in Table 2.
* **`ModelCompareForAll.ipynb`**: Performs two-tailed paired t-tests ($p < 0.05$) and plots cross-model performance distributions and radar charts.

---

## Hyperparameters and Training Setup

* **Input Resolution**: $256 \times 256 \times 3$ pixels
* **Optimizer**: Adam (learning rate = $1 \times 10^{-4}$)
* **Batch Size**: 8
* **Max Epochs**: 50 (early stopping patience = 10 on validation loss)
* **Loss Function**: Combined Binary Cross-Entropy + Dice Loss
* **Data Augmentations**: Random rotation ($\pm 15^\circ$), horizontal/vertical flips, brightness/contrast jitter ($\pm 10\%$), Gaussian noise ($\sigma = 0.01$)
* **Validation Strategy**: 5-fold cross-validation

---

## Instructions

### 1. Requirements

Install required dependencies via `pip`:

```bash
pip install torch torchvision numpy pandas matplotlib scipy scikit-learn jupyter
```

### 2. Dataset Setup

1. **Polyp Samples (Positive)**: Obtain the 1000 annotated images and binary masks from the [Kvasir-SEG dataset](https://datasets.simula.no/kvasir-seg/).
2. **Polyp-Free Samples (Negative)**: Obtain the 800 healthy mucosal images from the [WCE Curated Colon Dataset](https://www.kaggle.com/datasets/francismon/curated-colondataset-for-deep-learning).
3. Standardize all images to $256 \times 256$ resolution. For polyp-free negative images, generate corresponding null (all-black, pixel value 0) binary ground-truth masks.

### 3. Model Training

Navigate to the target architecture and dataset folder and execute the notebook:
* For ResNet with expanded data: `ResNet/1800 Image/resnet_1800.ipynb`
* For U-Net with expanded data: `U-Net/1800 Image/unet1800.ipynb`
* For SegNet with expanded data: `SegNet/1800 Image/segnet_1800.ipynb`

### 4. Aggregating Table 2 Summary Metrics

Run `AvgValue.ipynb` to aggregate the 5-fold evaluation metrics into the final summary table (`model_comparison_summary.csv`).

---

## Quantitative Results (Table 2)

Metrics are reported as mean $\pm$ standard deviation across 5 validation folds at the pixel level:

| Model | Accuracy | Dice | IoU | Precision | Recall | F1-Score |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Dataset Size: 1000 Images** | | | | | | |
| ResNet | $0.8075 \pm 0.0634$ | $0.8122 \pm 0.0756$ | $0.6891 \pm 0.1049$ | $0.7840 \pm 0.0714$ | $0.8553 \pm 0.1293$ | $0.8122 \pm 0.0756$ |
| U-Net  | $0.9230 \pm 0.0163$ | $0.7130 \pm 0.0529$ | $0.6102 \pm 0.0594$ | $0.7473 \pm 0.0770$ | $0.7741 \pm 0.0485$ | $0.7130 \pm 0.0529$ |
| SegNet | $0.8751 \pm 0.0070$ | $0.4296 \pm 0.0343$ | $0.3091 \pm 0.0287$ | $0.5420 \pm 0.0417$ | $0.4044 \pm 0.0336$ | $0.4296 \pm 0.0343$ |
| **Dataset Size: 1800 Images** | | | | | | |
| ResNet | $0.9869 \pm 0.0068$ | $0.9869 \pm 0.0067$ | $0.9743 \pm 0.0130$ | $0.9971 \pm 0.0021$ | $0.9771 \pm 0.0145$ | $0.9869 \pm 0.0067$ |
| U-Net  | $0.9653 \pm 0.0021$ | $0.8562 \pm 0.0157$ | $0.8041 \pm 0.0163$ | $0.4461 \pm 0.0223$ | $0.4434 \pm 0.0398$ | $0.4246 \pm 0.0295$ |
| SegNet | $0.9303 \pm 0.0036$ | $0.2237 \pm 0.0266$ | $0.1597 \pm 0.0226$ | $0.2996 \pm 0.0234$ | $0.2017 \pm 0.0316$ | $0.2237 \pm 0.0266$ |

---

## Citation

```bibtex
@article{mahbub2026enhancing,
  title={Enhancing Medical Image Segmentation through Negative Sample Integration: A Study on Kvasir-SEG and Augmented Datasets},
  author={Mahbub, Imon and Zim, Ahmed Zoha and Imran, Md Talha Bin and Mesbah, K. M. Fatin and Jobayer, Md and Shawon, Md. Mehedi Hasan},
  journal={PLOS ONE},
  year={2026}
}
```

---

## Contact

* **Imon Mahbub**: imon.mahbub@g.bracu.ac.bd  
* BSRM School of Engineering, BRAC University