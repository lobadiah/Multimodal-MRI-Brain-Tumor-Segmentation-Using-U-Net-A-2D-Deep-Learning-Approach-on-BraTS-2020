# Brain Tumor Segmentation with U-Net and Streamlit

End-to-end brain tumor segmentation project built around a 2D U-Net workflow for multimodal MRI slices, with a Streamlit application for interactive inference and a Jupyter notebook for training, evaluation, and artifact generation.

[Project demo video](https://youtu.be/9wd21YIcHZA)

## About

Description: Multimodal MRI brain tumor segmentation project using a 2D U-Net model with a Streamlit interface for interactive inference and visualization.

Website: [Project demo video](https://youtu.be/9wd21YIcHZA)

Suggested topics: brain-tumor-segmentation, medical-imaging, deep-learning, u-net, tensorflow, keras, streamlit, mri, computer-vision, brats2020

## Overview

This repository combines two practical deliverables:

1. A training and experimentation notebook for preparing BraTS-style MRI data, training a segmentation model, and evaluating predictions.
2. A Streamlit application for loading four MRI modalities and visualizing the predicted tumor overlay.

The project is intended as a portfolio-ready deep learning application that demonstrates:

- Multimodal MRI preprocessing
- 2D semantic segmentation with U-Net
- Model evaluation with Dice and IoU metrics
- Interactive deployment with Streamlit

## Problem Statement

Brain tumor segmentation is a pixel-wise computer vision task where the objective is to identify tumor regions from MRI scans. Manual delineation is time-consuming and depends heavily on clinical expertise. This project explores how a convolutional encoder-decoder network can assist this workflow by generating tumor masks from multimodal MRI inputs.

## Key Features

- Multimodal MRI inference using FLAIR, T1, T1CE, and T2 inputs
- U-Net based segmentation pipeline implemented with TensorFlow and Keras
- Training notebook with preprocessing, slicing, visualization, training, and evaluation steps
- Streamlit interface for uploading images and viewing predicted segmentation overlays
- Post-processing with connected components filtering and largest-region selection
- Export of validation arrays and evaluation metrics for inspection

## Repository Scope

This repository contains source code and notebook logic. Large generated artifacts and local environment files are intentionally excluded from version control.

Examples of excluded local artifacts:

- Trained model weights in the `models` directory
- Generated `.npy` validation arrays
- CSV evaluation outputs in the `outputs` directory
- Local virtual environments and editor settings

## Repository Structure

```text
.
|-- app.py
|-- requirements.txt
|-- notebooks/
|   `-- brain_tumor_segmentation.ipynb
|-- models/
|   `-- brain_tumor_segmentation_model.h5      # expected locally, not committed
|-- outputs/                                   # generated locally, not committed
`-- glioma-segmentation-project/               # supporting project assets and experiments
```

## Tech Stack

| Area | Tools |
|---|---|
| Language | Python |
| Deep Learning | TensorFlow, Keras |
| Data Processing | NumPy, OpenCV |
| Visualization | Matplotlib |
| Medical Imaging | nibabel |
| App Layer | Streamlit |
| Post-processing | scikit-image |
| Experimentation | Jupyter Notebook |

## Requirements

### Software

- Python 3.10+ recommended
- Git
- pip
- Jupyter support if you plan to run the notebook

### Python dependencies

The root `requirements.txt` now includes both the Streamlit app dependencies and the notebook dependencies required for training and evaluation:

```text
streamlit
tensorflow
numpy
opencv-python
pillow
matplotlib
scikit-image
nibabel
scikit-learn
pandas
```

## Dataset

This project is designed around BraTS-style brain MRI data. The training notebook expects local access to a BraTS dataset directory and reads modality volumes from patient folders.

Expected modalities per case:

- FLAIR
- T1
- T1CE
- T2
- Segmentation mask

The exact local dataset path used in the notebook is environment-specific and should be updated before running the notebook on another machine.

## Model Pipeline

### Input preparation

- MRI modalities are loaded per patient
- Relevant slices are selected from the 3D volumes
- Intensities are normalized
- Slices are resized for 2D training/inference
- Modalities are stacked into a 4-channel tensor

### Network

- 2D U-Net style encoder-decoder architecture
- Convolutional blocks with skip connections
- Sigmoid output for binary tumor mask prediction

### Evaluation

- Binary cross-entropy training objective in the current implementation
- Pixel-wise accuracy
- Dice coefficient
- Intersection over Union

### Inference post-processing

- Binary thresholding of the predicted mask
- Connected-component filtering to suppress small regions
- Largest-region selection to keep the most prominent tumor candidate

## How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/lobadiah/Multimodal-MRI-Brain-Tumor-Segmentation-Using-U-Net-A-2D-Deep-Learning-Approach-on-BraTS-2020.git
cd BrainTumor-Segmentation-Streamlit
```

### 2. Create and activate a virtual environment

Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Windows Command Prompt:

```bat
python -m venv .venv
.venv\Scripts\activate.bat
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Provide the trained model

The Streamlit app expects the trained model file at:

```text
models/brain_tumor_segmentation_model.h5
```

Because model weights are excluded from Git, you must do one of the following:

1. Place an existing trained model file in the `models` directory.
2. Re-run the notebook training pipeline and export the model locally.

### 5. Launch the Streamlit application

```bash
streamlit run app.py
```

Or use the VS Code task already configured in the workspace.

### 6. Use the application

Upload one image for each modality:

- FLAIR
- T1
- T1CE
- T2

The app will:

- Resize and normalize the images
- Stack the four modalities into one input tensor
- Run inference with the trained model
- Refine the predicted mask
- Display an overlay of the tumor region on the FLAIR image

## Notebook Workflow

The notebook in `notebooks/brain_tumor_segmentation.ipynb` contains the experimental workflow used for:

- Loading BraTS patient volumes
- Exploring MRI modalities and masks
- Building training slices from 3D scans
- Defining and training the U-Net model
- Evaluating performance on validation data
- Saving model and output artifacts

Use the notebook if you want to retrain, reproduce metrics, or inspect intermediate outputs.

## Expected Outputs

Typical local outputs generated by the notebook or app include:

- Trained model weights
- Validation arrays
- Predicted masks
- Segmentation metrics CSV
- Visual overlays of predictions

These files are intentionally ignored by Git to keep the repository clean and lightweight.

## Reproducibility Notes

- File paths inside the notebook are currently local-machine specific and may need to be updated on another system.
- The Streamlit app depends on a locally available trained model file.
- Training results can vary depending on dataset split, preprocessing choices, and hardware.
- Inference and notebook code should use the same input sizing strategy to avoid shape mismatches.

## Known Limitations

- The repository does not ship with the full training dataset.
- The repository does not commit model weights due to size and portability concerns.
- The current application is designed for 2D slice-based inference rather than full 3D segmentation.
- This project is intended for educational and portfolio use, not for clinical deployment.

## Troubleshooting

### Model file not found

If the app fails on startup because the model file is missing, place `brain_tumor_segmentation_model.h5` in the `models` directory.

### Shape mismatch during training or inference

Ensure the data preprocessing pipeline and the model input shape use the same resize dimensions.

### Missing notebook packages

If notebook cells fail on imports such as `nibabel`, `pandas`, or `sklearn`, install the missing packages into the active environment.

### Streamlit app opens but inference fails

Verify that:

- All four modality images are uploaded
- Images are readable by OpenCV
- The model file matches the expected input shape used by the app

## Professional Use Notes

For a production-grade extension of this project, consider adding:

- Automated tests for preprocessing and inference utilities
- Separate training and inference modules instead of notebook-only training logic
- Versioned model registry or release assets for trained weights
- Data validation and schema checks for uploaded images
- CI for linting, notebook validation, and packaging

## Contributing

Contributions are welcome if they improve reproducibility, code quality, model robustness, or deployment quality.

Recommended contribution areas:

- Refactoring notebook logic into reusable Python modules
- Improving model evaluation and metric reporting
- Adding inference tests
- Improving error handling in the Streamlit app
- Packaging model download instructions or release assets

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE).

## Acknowledgments

- BraTS dataset and research community
- TensorFlow and Keras ecosystem
- Streamlit for rapid model demo deployment
- pahujadiljeet

## Author

Louis Obadiah
