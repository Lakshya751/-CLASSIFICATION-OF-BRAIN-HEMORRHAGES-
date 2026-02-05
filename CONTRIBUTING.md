# Contributing to Brain Hemorrhage Classification

Thank you for your interest in contributing to this project! This document provides guidelines for contributing to the research and codebase.

## How to Contribute

### Reporting Issues

If you find a bug or have a suggestion for improvement:

1. Check if the issue already exists in the Issues tab
2. If not, create a new issue with:
   - Clear description of the problem or suggestion
   - Steps to reproduce (for bugs)
   - Expected vs actual behavior
   - System information (OS, Python version, TensorFlow version)

### Code Contributions

1. **Fork the repository**
   ```bash
   git clone https://github.com/yourusername/brain-hemorrhage-classification.git
   cd brain-hemorrhage-classification
   ```

2. **Create a new branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make your changes**
   - Follow PEP 8 style guidelines for Python code
   - Add comments to complex sections
   - Update documentation as needed

4. **Test your changes**
   - Ensure all code runs without errors
   - Verify results are reproducible

5. **Commit your changes**
   ```bash
   git add .
   git commit -m "Add: Brief description of your changes"
   ```

6. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

7. **Create a Pull Request**
   - Provide a clear description of your changes
   - Reference any related issues

## Areas for Contribution

We welcome contributions in the following areas:

### Model Improvements
- Implementing 3D CNN architectures
- Adding sequence models (LSTM, GRU)
- Experimenting with different backbone architectures
- Handling class imbalance with advanced techniques

### Data Processing
- Additional preprocessing techniques
- Data augmentation strategies
- Support for different medical imaging formats

### Evaluation
- Additional evaluation metrics
- Visualization tools for model predictions
- Confusion matrix and error analysis tools

### Documentation
- Improved code comments
- Tutorial notebooks
- Use case examples

### External Validation
- Testing on different datasets
- Cross-institutional validation studies

## Code Style Guidelines

- Use meaningful variable and function names
- Add docstrings to functions and classes
- Keep functions focused on a single task
- Limit line length to 100 characters
- Use type hints where appropriate

Example:
```python
def preprocess_ct_image(image: np.ndarray, window_width: int = 80, 
                       window_level: int = 40) -> np.ndarray:
    """
    Apply windowing and normalization to CT image.
    
    Args:
        image: Input CT image array
        window_width: HU window width for brain tissue
        window_level: HU window level for brain tissue
    
    Returns:
        Preprocessed image array normalized to [0, 1]
    """
    # Implementation here
    pass
```

## Research Contributions

If you're conducting research using or building upon this work:

1. Cite the original paper
2. Share your findings through pull requests
3. Consider collaborating on future publications

## Questions?

Feel free to open an issue with the "question" label or contact:

**Lakshya Shah**  
Email: shahlakshya751@gmail.com

## Code of Conduct

- Be respectful and constructive in discussions
- Focus on the research and technical aspects
- Help create a welcoming environment for all contributors

Thank you for contributing to advancing medical AI research!
