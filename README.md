# Classification of Brain Hemorrhages Using Convolutional Neural Networks

A machine learning approach for automated classification of intracranial hemorrhage (ICH) subtypes on head CT images using deep convolutional neural networks.

## 📄 Research Paper

**Title**: Classification of Brain Hemorrhages Using Convolutional Neural Networks: A Machine Learning Approach for Medical Imaging

**Author**: Lakshya Shah  
**Institution**: Norwich University - Senator Patrick Leahy School of Cybersecurity and Advanced Computing  
**Contact**: shahlakshya751@gmail.com

[Read the full paper](paper/CLASSIFICATION_OF_BRAIN_HEMORRHAGES_USING_CONVOLUTIONAL_NEURAL_NETWORKS-4.pdf)

## 🎯 Project Overview

This project investigates the automated classification of five intracranial hemorrhage subtypes on head CT images using three different CNN model architectures:

- **Model 1**: Initial 5-class CNN baseline
- **Model 2**: Improved 5-class CNN with optimized hyperparameters
- **Model 3**: 4-class CNN (excluding epidural hemorrhage)

### Hemorrhage Subtypes Classified

1. **Epidural (EDH)** - Bleeding between skull and dura mater
2. **Subdural (SDH)** - Bleeding beneath the dura mater
3. **Subarachnoid (SAH)** - Bleeding in the subarachnoid space
4. **Intraventricular (IVH)** - Bleeding within brain ventricles
5. **Intraparenchymal (IPH)** - Bleeding within brain tissue

## 📊 Dataset

**RSNA 2019 Brain CT Hemorrhage Challenge Dataset**
- 25,312 CT exams
- ~674,000 axial CT slice images
- Binary labels for each hemorrhage subtype per slice

## 🏗️ Model Architecture

All three models utilize:
- **Base Architecture**: EfficientNet-B0 with ImageNet pre-trained weights
- **Input**: 224×224 grayscale CT slices (converted to 3-channel)
- **Preprocessing**: Brain windowing (HU normalization), intensity normalization
- **Data Augmentation**: Random rotations, horizontal flips, shifts/zooms
- **Output**: Sigmoid activation for multi-label classification

### Architecture Details
```
Input (224×224×3)
    ↓
EfficientNet-B0 (pre-trained)
    ↓
Global Average Pooling
    ↓
Dense Layer (128 neurons, ReLU)
    ↓
Dropout (0.3)
    ↓
Output Layer (5 or 4 neurons, Sigmoid)
```

## 📈 Performance Results

| Model | Accuracy | Macro F1 | Avg AUC | Val Loss |
|-------|----------|----------|---------|----------|
| Model 1 (5-class) | 28% | 0.32 | 0.88 | 0.254 |
| Model 2 (5-class) | **80%** | 0.75 | **0.95** | 0.104 |
| Model 3 (4-class, no EDH) | **82%** | **0.80** | 0.85 | 0.120 |

### Key Findings

- **Model 2** achieved the best overall performance with 95% AUC and 80% accuracy
- **Model 3** showed improved F1-score consistency by excluding the rare epidural class
- Epidural hemorrhage (EDH) was the most challenging class due to extreme rarity (~1-2% of cases)
- Intraparenchymal hemorrhage (IPH) was the easiest to detect with ~90% F1 and 98% AUC

## 🛠️ Technologies Used

- **Framework**: TensorFlow/Keras
- **Pre-trained Model**: EfficientNet-B0
- **Hardware**: NVIDIA Tesla GPU
- **Languages**: Python
- **Key Libraries**: 
  - TensorFlow
  - NumPy
  - Pandas
  - Matplotlib
  - scikit-learn

## 📁 Repository Structure

```
.
├── models/
│   ├── model-1.ipynb                              # Baseline 5-class CNN
│   ├── model-2-class-specific-oversampling.ipynb  # Improved 5-class CNN
│   └── model-3-exclude-epidural.ipynb             # 4-class CNN (no EDH)
├── paper/
│   └── CLASSIFICATION_OF_BRAIN_HEMORRHAGES_USING_CONVOLUTIONAL_NEURAL_NETWORKS.pdf
├── results/
│   └── figures/                                    # Training curves and visualizations
├── README.md
└── LICENSE

```

## 🚀 Getting Started

### Prerequisites

```bash
pip install tensorflow
pip install numpy pandas matplotlib scikit-learn
pip install efficientnet
```

### Running the Models

1. Download the RSNA 2019 Brain CT Hemorrhage Challenge dataset
2. Open the desired Jupyter notebook in `models/`
3. Update the data paths to point to your dataset location
4. Run all cells to train and evaluate the model

### Training Configuration

- **Optimizer**: Adam (learning rate: 1e-4)
- **Loss Function**: Binary Cross-Entropy
- **Batch Sampling**: Class-balanced oversampling
- **Epochs**: 6-10 (with early stopping)
- **Validation Split**: Stratified by hemorrhage subtype

## 📊 Evaluation Metrics

The models were evaluated using:
- **Accuracy**: Exact-match classification accuracy
- **AUC (Area Under ROC Curve)**: Per-class discrimination ability
- **F1-Score**: Harmonic mean of precision and recall (macro-averaged)
- **Validation Loss**: Binary cross-entropy loss

## 🔍 Clinical Implications

This work demonstrates that CNN-based models can:
- Assist radiologists with rapid ICH subtype identification
- Serve as triage tools in emergency settings
- Reduce diagnostic errors for less experienced readers
- Prioritize critical cases for immediate review

### Limitations & Future Work

- **Class Imbalance**: Rare hemorrhage types (especially epidural) remain challenging
- **2D Context**: Single-slice analysis misses 3D spatial information
- **External Validation**: Models need testing on diverse datasets from different institutions
- **Deployment**: Real-time performance and clinical workflow integration require further study

**Proposed Improvements**:
- Incorporate 3D context using sequence models or 3D CNNs
- Implement multi-window inputs (bone + brain windowing)
- Apply advanced techniques: focal loss, synthetic data generation
- Develop multi-stage detection/classification pipelines

## 📚 References

Key papers cited in this work:
1. Flanders et al. (2020) - RSNA 2019 Brain CT Hemorrhage Challenge dataset
2. Wang et al. (2021) - 1st place RSNA challenge solution (AUC ~0.99)
3. Rajagopal et al. (2023) - Hybrid CNN-LSTM model (~95% accuracy)
4. Wu et al. (2021) - EfficientNet-B0 ensemble (95.7% accuracy)

[See full paper for complete references]

## 📝 Citation

If you use this work in your research, please cite:

```bibtex
@article{shah2024brain_hemorrhage_classification,
  title={Classification of Brain Hemorrhages Using Convolutional Neural Networks: A Machine Learning Approach for Medical Imaging},
  author={Shah, Lakshya},
  institution={Norwich University},
  year={2024}
}
```

## 📧 Contact

**Lakshya Shah**  
Email: shahlakshya751@gmail.com  
Institution: Norwich University - Senator Patrick Leahy School of Cybersecurity and Advanced Computing

## 📄 License

This project is available for academic and research purposes. Please contact the author for commercial use inquiries.

---

**Acknowledgments**: This research was conducted using the RSNA 2019 Brain CT Hemorrhage Challenge dataset provided by the Radiological Society of North America (RSNA).
