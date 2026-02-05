# Model Setup and Training Guide

This guide provides detailed instructions for setting up and training the three CNN models for brain hemorrhage classification.

## Prerequisites

### System Requirements
- **RAM**: Minimum 16GB (32GB recommended)
- **GPU**: NVIDIA GPU with CUDA support (Tesla, RTX series, or equivalent)
- **Storage**: At least 100GB free space for dataset
- **OS**: Linux (Ubuntu 20.04+), macOS, or Windows 10/11

### Software Requirements
- Python 3.8 or higher
- CUDA 11.2+ and cuDNN 8.1+ (for GPU acceleration)
- Jupyter Notebook or JupyterLab

## Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/brain-hemorrhage-classification.git
cd brain-hemorrhage-classification
```

### 2. Create Virtual Environment
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Verify TensorFlow GPU Installation (Optional)
```python
import tensorflow as tf
print("Num GPUs Available: ", len(tf.config.list_physical_devices('GPU')))
```

## Dataset Preparation

### RSNA 2019 Brain CT Hemorrhage Challenge Dataset

1. **Download Dataset**
   - Register at [RSNA Challenge Page](https://www.kaggle.com/c/rsna-intracranial-hemorrhage-detection)
   - Download training images and labels

2. **Dataset Structure**
   ```
   data/
   ├── train/
   │   ├── images/           # CT scan images
   │   └── labels.csv        # Hemorrhage labels
   ├── validation/
   │   ├── images/
   │   └── labels.csv
   └── test/
       ├── images/
       └── labels.csv
   ```

3. **Preprocessing**
   - Images are in DICOM format
   - Convert to PNG/JPG if needed
   - Apply brain windowing (included in notebooks)

## Model Training

### Model 1: Baseline 5-Class CNN

**Purpose**: Establish baseline performance  
**Notebook**: `models/model-1.ipynb`

```bash
jupyter notebook models/model-1.ipynb
```

**Key Parameters**:
- Learning Rate: 1e-4
- Batch Size: 32
- Epochs: 10
- Classes: 5 (EDH, SDH, SAH, IVH, IPH)

**Expected Results**:
- Validation Accuracy: ~28%
- Validation AUC: ~0.88
- Training Time: 1-2 days on Tesla GPU

### Model 2: Improved 5-Class CNN

**Purpose**: Optimized model with better performance  
**Notebook**: `models/model-2-class-specific-oversampling-approach.ipynb`

```bash
jupyter notebook models/model-2-class-specific-oversampling-approach.ipynb
```

**Key Improvements**:
- Class-specific oversampling
- Enhanced data augmentation
- Optimized learning rate schedule
- Better dropout regularization

**Expected Results**:
- Validation Accuracy: ~80%
- Validation AUC: ~0.95
- Training Time: 1-2 days on Tesla GPU

### Model 3: 4-Class CNN (Excluding Epidural)

**Purpose**: Address class imbalance by excluding rare EDH class  
**Notebook**: `models/model-3-exclude-epidural.ipynb`

```bash
jupyter notebook models/model-3-exclude-epidural.ipynb
```

**Key Parameters**:
- Classes: 4 (SDH, SAH, IVH, IPH - no EDH)
- Epochs: 6
- Enhanced F1-score focus

**Expected Results**:
- Validation Accuracy: ~82%
- Macro F1-Score: ~0.80
- Training Time: Less than 1 day on Tesla GPU

## Training Tips

### 1. Monitor Training Progress
```python
import matplotlib.pyplot as plt

# Plot training curves
plt.figure(figsize=(15, 5))

plt.subplot(1, 3, 1)
plt.plot(history.history['accuracy'], label='Train')
plt.plot(history.history['val_accuracy'], label='Validation')
plt.title('Model Accuracy')
plt.xlabel('Epoch')
plt.ylabel('Accuracy')
plt.legend()

plt.subplot(1, 3, 2)
plt.plot(history.history['loss'], label='Train')
plt.plot(history.history['val_loss'], label='Validation')
plt.title('Model Loss')
plt.xlabel('Epoch')
plt.ylabel('Loss')
plt.legend()

plt.subplot(1, 3, 3)
plt.plot(history.history['auc'], label='Train')
plt.plot(history.history['val_auc'], label='Validation')
plt.title('Model AUC')
plt.xlabel('Epoch')
plt.ylabel('AUC')
plt.legend()

plt.tight_layout()
plt.show()
```

### 2. Handle Class Imbalance
- Use class weights
- Implement oversampling for minority classes
- Consider focal loss for extreme imbalance

### 3. Early Stopping
```python
from tensorflow.keras.callbacks import EarlyStopping

early_stop = EarlyStopping(
    monitor='val_loss',
    patience=3,
    restore_best_weights=True
)
```

### 4. Model Checkpointing
```python
from tensorflow.keras.callbacks import ModelCheckpoint

checkpoint = ModelCheckpoint(
    'best_model.h5',
    monitor='val_auc',
    save_best_only=True,
    mode='max'
)
```

## Evaluation

### Computing Metrics
```python
from sklearn.metrics import classification_report, roc_auc_score, f1_score

# Predictions
y_pred = model.predict(X_val)
y_pred_binary = (y_pred > 0.5).astype(int)

# Classification report
print(classification_report(y_val, y_pred_binary, 
                          target_names=['EDH', 'SDH', 'SAH', 'IVH', 'IPH']))

# ROC AUC per class
for i, class_name in enumerate(['EDH', 'SDH', 'SAH', 'IVH', 'IPH']):
    auc = roc_auc_score(y_val[:, i], y_pred[:, i])
    print(f"{class_name} AUC: {auc:.4f}")

# Macro F1-Score
macro_f1 = f1_score(y_val, y_pred_binary, average='macro')
print(f"Macro F1-Score: {macro_f1:.4f}")
```

## Troubleshooting

### Common Issues

1. **Out of Memory Errors**
   - Reduce batch size
   - Use mixed precision training
   - Clear GPU memory between runs

2. **Slow Training**
   - Verify GPU is being used
   - Reduce image resolution if needed
   - Use data generators for large datasets

3. **Poor Performance**
   - Check data preprocessing
   - Verify class balance
   - Increase training epochs
   - Try different learning rates

### Performance Optimization

```python
# Mixed precision training (for faster training on modern GPUs)
from tensorflow.keras import mixed_precision
policy = mixed_precision.Policy('mixed_float16')
mixed_precision.set_global_policy(policy)

# Data pipeline optimization
dataset = tf.data.Dataset.from_tensor_slices((X_train, y_train))
dataset = dataset.cache()
dataset = dataset.shuffle(buffer_size=1000)
dataset = dataset.batch(batch_size)
dataset = dataset.prefetch(tf.data.AUTOTUNE)
```

## Results Reproduction

To reproduce the exact results from the paper:
1. Use the same dataset splits (provided in notebooks)
2. Set random seeds: `tf.random.set_seed(42)`, `np.random.seed(42)`
3. Use identical preprocessing parameters
4. Train for the specified number of epochs

## Citation

If you use these models in your research, please cite:

```bibtex
@article{shah2024brain_hemorrhage_classification,
  title={Classification of Brain Hemorrhages Using Convolutional Neural Networks: A Machine Learning Approach for Medical Imaging},
  author={Shah, Lakshya},
  institution={Norwich University},
  year={2024}
}
```

## Support

For questions or issues:
- Open an issue on GitHub
- Email: shahlakshya751@gmail.com

## Next Steps

After training:
1. Evaluate on test set
2. Generate confusion matrices
3. Visualize predictions with Grad-CAM
4. Consider ensemble methods
5. Validate on external datasets
