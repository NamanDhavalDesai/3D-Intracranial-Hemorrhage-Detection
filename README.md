# CNN-RNN Hybrid: 2.5D Intracranial Hemorrhage Classification

![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)
![Medical AI](https://img.shields.io/badge/Domain-Neuroradiology-blue)
![Architecture](https://img.shields.io/badge/Architecture-ResNet50--LSTM-red)

## 📌 Project Overview
Building upon traditional 2D slice classification (separate repository linked [here](https://github.com/NamanDhavalDesai/Intracranial-Hemorrhage-Detection)), this project introduces a **Hybrid CNN-RNN architecture** to detect and classify Intracranial Hemorrhage (ICH) subtypes. By treating CT scans as **sequences of slices (triplets)**, the model captures inter-slice spatial dependencies, significantly improving diagnostic reliability.

The model classifies five specific subtypes: **Epidural, Subdural, Subarachnoid, Intraventricular, and Intraparenchymal.**

## 🚀 Key Improvements (The "2.5D" Approach)
Unlike single-slice models, this project utilizes a **Volumetric Pipeline**:
*   **Series Reconstruction**: Grouped 800,000+ individual DICOM slices into their original 3D volumes using `SeriesInstanceUID`.
*   **Consecutive Triplets**: Extracted sequences of 3 consecutive slices sharing identical labels to provide the model with "contextual depth."
*   **Multi-Tissue Windowing**: Each slice in the triplet undergoes triple windowing (Brain, Subdural, and Bone densities), creating a rich, multi-channel input for the feature extractor.

## 🛠️ Hybrid Architecture: ResNet-50 + LSTM
The model combines the strengths of Computer Vision and Sequence Modeling:
1.  **Feature Extractor (CNN)**: A pre-trained **ResNet-50** processes each slice in the triplet independently to extract 2048-dimensional spatial feature vectors.
2.  **Sequential Aggregator (RNN)**: A **two-layer Bidirectional LSTM** processes the sequence of feature vectors, capturing the anatomical continuity across the CT volume.
3.  **Classification Head**: Fully connected layers with Sigmoid activation for multi-label subtype prediction.

## 📊 Performance Results
The hybrid approach achieved an average **Accuracy of 91.1%** and a **Mean ROC AUC of 0.954** across all subtypes.

| Subtype | Accuracy | Sensitivity | Specificity | F1-Score | AUC |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Epidural** | 0.948 | 0.727 | 0.959 | 0.574 | 0.95 |
| **Subdural** | 0.930 | 0.857 | 0.954 | 0.857 | 0.96 |
| **Subarachnoid** | 0.918 | 0.923 | 0.915 | 0.834 | 0.96 |
| **Intraventricular** | 0.884 | 0.667 | 0.969 | 0.767 | 0.96 |
| **Intraparenchymal** | 0.884 | 0.787 | 0.917 | 0.781 | 0.93 |

## 🔍 Interpretability with Grad-CAM
The model's decisions were validated using **Grad-CAM heatmaps**. The visualizations show that the LSTM-augmented network accurately focuses on hyperdense hemorrhagic patterns within the brain matter, aligning with actual radiologist annotations.

## 📂 Repository Structure
*   `Kaggle_Code.ipynb`: **Data Engineering** — Run this on Kaggle to process the RSNA ICH dataset, reconstruct volumes and export the triplet-based windowed dataset.
*   `Slice_2.5D_Model.ipynb`: **Deep Learning** — Contains the Hybrid ResNet-LSTM architecture, weighted loss functions and the training/evaluation pipeline.
*   `Report.pdf`: The full technical study.
*   `Poster.pdf`: Summary of the entire technical study.

## 🚀 How to Use
1.  **Preprocessing**: Use the `Kaggle_Code.ipynb` on the [RSNA 2019 Dataset](https://www.kaggle.com/competitions/rsna-intracranial-hemorrhage-detection) to generate the windowed triplets.
2.  Open the `Slice_2.5D_Model.ipynb` file in Google Colab or Jupyter.
3.  Run all cells to reproduce the results.

## 👥 Contributors
*   **Naman Dhaval Desai**
*   **Jan Marten Winkler**
