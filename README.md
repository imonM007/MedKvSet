MedKvSet: Enhancing Medical Image Segmentation through Negative Sample IntegrationCode and evaluation notebooks for the paper:"Enhancing Medical Image Segmentation through Negative Sample Integration: A Study on Kvasir-SEG and Augmented Datasets"Imon Mahbub, Ahmed Zoha Zim, Md Talha Bin Imran, K. M. Fatin Mesbah, Md Jobayer, and Md. Mehedi Hasan ShawonBRAC University & Linköping UniversityOverviewMedical image segmentation models trained only on images containing polyps often produce high false positive rates when shown healthy tissue. This project studies the impact of adding negative (polyp-free) samples during training.We evaluate three architectures using 5-fold cross-validation:ResNet (ResNet-18 encoder with transposed convolution decoder)U-NetSegNetExperiments are run on two dataset setups:1000 Images: Polyp images from Kvasir-SEG (positive samples only).1800 Images: 1000 polyp images (Kvasir-SEG) + 800 polyp-free colon images (WCE Curated Colon Dataset).Repository StructureMedKvSet/
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
File ExplanationsModel folders (ResNet/, SegNet/, U-Net/):*.ipynb: Training and 5-fold cross-validation notebooks.*.csv: Evaluation results per fold across standard metrics (Accuracy, Dice, IoU, Precision, Recall, F1-Score).AvgValue.ipynb:Reads the result CSV files and calculates the mean and standard deviation across all 5 folds to reproduce Table 2 of the manuscript.ModelCompareForAll.ipynb:Computes paired t-tests and creates performance comparison plots.HyperparametersInput Size: $256 \times 256 \times 3$Optimizer: Adam (learning rate = $1 \times 10^{-4}$)Batch Size: 8Epochs: Up to 50 (early stopping patience = 10 on validation loss)Loss Function: Binary Cross-Entropy + Dice LossAugmentations: Random rotations ($\pm 15^\circ$), flips, brightness/contrast jitter, Gaussian noiseInstructions1. RequirementsInstall required Python packages:pip install torch torchvision numpy pandas matplotlib scipy scikit-learn jupyter
2. Dataset SetupDownload Kvasir-SEG (1000 polyp images and masks).Download WCE Curated Colon Disease Dataset (800 healthy images).Resize all images to $256 \times 256$. For the 800 negative images, create all-black masks (all values set to 0).3. Training ModelsRun the corresponding notebook inside any model folder. For example:ResNet/1800 Image/resnet_1800.ipynbU-Net/1800 Image/unet1800.ipynb4. Computing Summary Metrics (Table 2)Run AvgValue.ipynb to aggregate the CSV outputs from each fold into the final $mean \pm std$ table.Quantitative Results (Table 2)Results reported as mean $\pm$ standard deviation across 5 folds:ModelAccuracyDiceIoUPrecisionRecallF1-Score1000 ImagesResNet$0.8075 \pm 0.0634$$0.8122 \pm 0.0756$$0.6891 \pm 0.1049$$0.7840 \pm 0.0714$$0.8553 \pm 0.1293$$0.8122 \pm 0.0756$U-Net$0.9230 \pm 0.0163$$0.7130 \pm 0.0529$$0.6102 \pm 0.0594$$0.7473 \pm 0.0770$$0.7741 \pm 0.0485$$0.7130 \pm 0.0529$SegNet$0.8751 \pm 0.0070$$0.4296 \pm 0.0343$$0.3091 \pm 0.0287$$0.5420 \pm 0.0417$$0.4044 \pm 0.0336$$0.4296 \pm 0.0343$1800 ImagesResNet$0.9869 \pm 0.0068$$0.9869 \pm 0.0067$$0.9743 \pm 0.0130$$0.9971 \pm 0.0021$$0.9771 \pm 0.0145$$0.9869 \pm 0.0067$U-Net$0.9653 \pm 0.0021$$0.8562 \pm 0.0157$$0.8041 \pm 0.0163$$0.4461 \pm 0.0223$$0.4434 \pm 0.0398$$0.4246 \pm 0.0295$SegNet$0.9303 \pm 0.0036$$0.2237 \pm 0.0266$$0.1597 \pm 0.0226$$0.2996 \pm 0.0234$$0.2017 \pm 0.0316$$0.2237 \pm 0.0266$Citation@article{mahbub2026enhancing,
  title={Enhancing Medical Image Segmentation through Negative Sample Integration: A Study on Kvasir-SEG and Augmented Datasets},
  author={Mahbub, Imon and Zim, Ahmed Zoha and Imran, Md Talha Bin and Mesbah, K. M. Fatin and Jobayer, Md and Shawon, Md. Mehedi Hasan},
  journal={PLOS ONE},
  year={2026}
}
ContactImon Mahbub: imon.mahbub@g.bracu.ac.bdBSRM School of Engineering, BRAC University# BIOSE
