# Evaluating the Impact of Low-Light Image Enhancement on Monocular Depth Estimation Using DepthAnythingV2

MSc Dissertation Project  
School of Computer Science, University of Leeds, 2025/2026

This repository contains the implementation and experimental evaluation accompanying the MSc dissertation of the same title. The project investigates whether low-light image enhancement can improve the predictions of a fixed, pre-trained monocular depth estimator, DepthAnythingV2, across multiple datasets and evaluation conditions.

## Experimental Pipeline

The experiments follow a common processing pipeline:

```text
Low-light image
      ↓
Dark / Gamma / CLAHE / MSR / LLFormer
      ↓
DepthAnythingV2
      ↓
Depth prediction
      ↓
Quantitative / structural evaluation
```

The DepthAnythingV2 model is kept fixed across the enhancement pipelines so that differences in the resulting depth predictions can be attributed to the preceding enhancement stage rather than changes to the depth estimator.

## Enhancement Methods

Five input pipelines are evaluated:

- **Dark baseline** — original low-light image without enhancement
- **Gamma correction** — global intensity adjustment
- **CLAHE** — local contrast enhancement
- **Multiscale Retinex (MSR)** — multi-scale illumination processing
- **LLFormer** — learning-based low-light image enhancement

## Datasets

The evaluation uses four datasets with complementary characteristics:

- **ExDark** — low-light images used for qualitative and structural evaluation
- **nuScenes-Night** — nighttime driving scenes with LiDAR-derived reference depth
- **MS2 Road3** — low-light road scenes with sensor-derived reference depth
- **MIT-Adobe FiveK** — paired images used for pseudo-reference relative-depth evaluation

Raw dataset files are not included in this repository.

## Evaluation

Different evaluation strategies are used according to the reference information available for each dataset.

For datasets with suitable metric reference depth, prediction accuracy is evaluated using:

- **MAE** — Mean Absolute Error
- **RMSE** — Root Mean Squared Error
- **AbsRel** — Absolute Relative Error

Where suitable metric ground-truth depth is unavailable, structural consistency is assessed using an **edge-alignment F1 score**, which compares RGB image edges with discontinuities in the predicted depth map.

For MIT-Adobe FiveK, relative-depth predictions generated from the corresponding high-quality images are used as pseudo-references for evaluating consistency across enhancement pipelines.

## Notebooks

The `notebooks/` directory contains the final experimental notebooks.

| Notebook | Description |
| --- | --- |
| `exdark-final.ipynb` | ExDark enhancement and edge-alignment F1 evaluation |
| `nuscenes-depthmetric-final.ipynb` | nuScenes-Night enhancement and metric-depth evaluation |
| `ms2-road3-depthmetric-final.ipynb` | MS2 Road3 enhancement and metric-depth evaluation |
| `mit-adobe-fivek-final.ipynb` | MIT-Adobe FiveK enhancement and pseudo-reference relative-depth evaluation |

Each notebook contains the dataset-specific preprocessing, enhancement pipelines, depth inference, evaluation procedures, and result generation required for the corresponding experiment.

## Environment

The experiments were conducted using Kaggle notebooks with an NVIDIA T4 GPU.

The main software components include:

- Python
- PyTorch
- OpenCV
- NumPy
- pandas
- Matplotlib
- DepthAnythingV2
- LLFormer

Additional dependencies required by individual notebooks are installed or imported within the corresponding notebook.

## Datasets

The datasets are not redistributed in this repository. They should be obtained from their official sources:

- **ExDark:** https://github.com/cs-chan/Exclusively-Dark-Image-Dataset
- **nuScenes:** https://www.nuscenes.org/nuscenes
- **MS2:** https://github.com/UkcheolShin/MS2-MultiSpectralStereoDataset
- **MIT-Adobe FiveK:** https://data.csail.mit.edu/graphics/fivek/

Dataset paths in the notebooks should be updated to match the user's local or Kaggle environment.

## Pretrained Model Weights

Pretrained model weights are not included in this repository.

The experiments use:

- **DepthAnythingV2** for monocular depth estimation
- **LLFormer** for learning-based low-light enhancement

The required model weights should be obtained from their official sources and the corresponding paths updated in each notebook before execution.

Different LLFormer checkpoints are used where appropriate for the evaluated datasets, as specified within the individual notebooks.

## Reproducing the Experiments

To reproduce an experiment:

1. Open the corresponding notebook in a Python/Jupyter environment or Kaggle.
2. Obtain the required dataset from its official source.
3. Obtain the required pretrained model weights.
4. Update the dataset and model-weight paths in the notebook.
5. Install any required dependencies specified in the notebook.
6. Run the notebook cells sequentially.
7. The notebook will generate the corresponding depth predictions, evaluation metrics, summary tables, and visualisation outputs.

The notebooks use fixed sampling procedures or random seeds where applicable to support reproducibility.

## Repository Structure

```text
low-light-depth-estimation-msc/
│
├── notebooks/
│   ├── exdark-final.ipynb
│   ├── nuscenes-depthmetric-final.ipynb
│   ├── ms2-road3-depthmetric-final.ipynb
│   └── mit-adobe-fivek-final.ipynb
│
├── results/
│   ├── exdark/
│   ├── nuscenes/
│   ├── ms2-road3/
│   └── fivek/
│
├── .gitignore
└── README.md
```

Large datasets, pretrained weights, checkpoints, and generated experimental outputs are excluded from version control.

## Project Scope

This repository accompanies an MSc dissertation investigating low-light image enhancement as a standalone pre-processing stage for monocular depth estimation. The objective is to evaluate whether enhancement improves, degrades, or leaves unchanged the predictions of an existing depth estimator without jointly training or fine-tuning the enhancement and depth models.
