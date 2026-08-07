# Longitudinal Bead Width Reconstruction and Analysis

This repository contains the main notebooks developed for the reconstruction, repeatability assessment, and exploratory analysis of the longitudinal width of beads produced by **Directed Energy Deposition (DED)**.

Because the datasets, images, annotations, and generated results are too large to be stored directly on GitHub, the large project folders are hosted on **OneDrive**. Please download them before running the notebooks.

## Repository contents

The final workflow is organized into three notebooks:

| Notebook                               | Purpose                                                                                                                    |
| -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `y_reconstruction_validation.ipynb`    | Reconstructs the longitudinal bead-width profiles from the physical bead images and generates the updated target variable. |
| `y_reconstruction_repeatability.ipynb` | Evaluates the conditional repeatability of the reconstruction procedure using repeated reconstructions by two operators.   |
| `eda.ipynb`                            | Performs the consolidated exploratory data analysis using the reconstructed target and the repeatability results.          |

The notebooks should be executed in this order when reproducing the full pipeline:

```text
y_reconstruction_validation.ipynb
        ↓
y_reconstruction_repeatability.ipynb
        ↓
eda.ipynb
```

---

## Download the large project files

The large folders required by the project are not versioned in this GitHub repository.

Download the following files from OneDrive and extract them into the **root directory of the repository**.

### 1. Datasets — required

Contains the datasets used by the notebooks.

[Download datasets from OneDrive](https://ufprbr0-my.sharepoint.com/:u:/g/personal/lafis_sophia_ufpr_br/IQAKGS5R3pQmRainHpOekhBDAZpjVN1EVRD73KA9BNy-I8Y?e=VK9hnH)

After extracting, the folder must be named:

```text
datasets/
```

### 2. Y reconstruction results — required

Contains the bead images, reconstruction annotations, calibration files, dense width profiles, reconstructed datasets, repeatability-study files, and other intermediate results required by the reconstruction and analysis notebooks.

[Download Y reconstruction results from OneDrive](https://ufprbr0-my.sharepoint.com/:u:/g/personal/lafis_sophia_ufpr_br/IQA736Wo3gW0S6Bz3gIvADZUAbNzZ2tRxkl8QLTFILLHIpg?e=GXolRX)

After extracting, the folder must be named:

```text
y_reconstruction_validation_results/
```

It also contains the persisted annotations and reconstruction products used to reproduce the current version of the target.

### 3. EDA results — generated outputs

Contains the tables, figures, manifests, and environment information exported by the current consolidated EDA.

[Download EDA results from OneDrive](https://ufprbr0-my.sharepoint.com/:u:/g/personal/lafis_sophia_ufpr_br/IQDb3mxTKtQhSpkISy6zTahWAbJtluXiKt4DnNs7I02AaLA?e=k3sQmN)

After extracting, the folder must be named:

```text
eda_sophia_results/
```
---

## Expected folder structure

After cloning this repository and downloading the large files, the project root should look approximately like this:

```text
.
├── README.md
├── y_reconstruction_validation.ipynb
├── y_reconstruction_repeatability.ipynb
├── eda.ipynb
│
├── datasets/
│   └── static_inceptionresnetv2_merged_df.csv
│
├── y_reconstruction_validation_results/
│   ├── raw/
│   ├── figures/
│   ├── resultados_calibracao/
│   └── repeatability_study/
│
└── eda_sophia_results/
    ├── figures/
    ├── tables/
    ├── execution_environment.json
    └── output_manifest.csv
```

If OneDrive downloads any of the folders as a compressed `.zip` file, extract it before running the notebooks.

If the extracted folder has a different name, rename it to match the names shown above.

---

## Project overview

The main target of the project is the bead width, in millimeters, along the longitudinal direction of each deposited bead.

Two target versions are preserved:

* `y_original`: the target used in the previous pipeline;
* `y_reconstruido`: the target obtained from the current semi-automatic geometric reconstruction procedure.

The reconstructed target provides a more traceable reconstruction workflow, but it should not automatically be interpreted as the physical ground truth. No independent metrological measurement is available in the current dataset to determine which target is closer to the true bead width.

---

## Notebook 1 — Y reconstruction

`y_reconstruction_validation.ipynb` reconstructs the longitudinal width profile of each bead.

The workflow includes:

1. loading and validating the bead images;
2. dimensional calibration using a 20 mm coin;
3. selecting the bead region of interest;
4. defining the longitudinal axis;
5. manually marking guide points for the upper and lower boundaries;
6. tracking the boundaries using dynamic programming;
7. converting the detected width from pixels to millimeters;
8. generating dense longitudinal width profiles;
9. defining the longitudinal orientation;
10. integrating the reconstructed target into the static dataset;
11. exporting audit files, figures, profiles, annotations, and manifests.

Some steps are interactive and use OpenCV graphical windows. 

### The L9 experiment

The original individual photograph of L9, equivalent to the images available for the other experiments, was lost and could not be recovered. As an alternative, a photograph containing all experiments was provided, and the L9 bead was manually cropped from this image together with its reference coin.

As a result, the L9 image has a substantially lower pixel resolution than the individual photographs used for the other experiments. This difference in image resolution should be considered when interpreting the reconstruction and repeatability results for L9.

The cropped L9 image was also physically oriented in the opposite direction relative to the other bead images. It was therefore rotated before the current reconstruction, and the calibration, annotation, tracking, and profile-generation steps were repeated using the corrected image.

---

## Notebook 2 — Repeatability study

`y_reconstruction_repeatability.ipynb` evaluates the **conditional repeatability** of the reconstruction procedure.

The study includes:

* three experiments: L2, L6, and L9;
* two operators;
* 18 reconstructions in total.

The selected cases represent different reconstruction conditions:

* **L2:** more favorable visual condition;
* **L6:** intermediate condition;
* **L9:** more challenging condition, with lower image resolution and more ambiguous boundaries.

---

## Notebook 3 — Exploratory data analysis

`eda.ipynb` performs the consolidated exploratory analysis.

The notebook includes:

* input validation and integrity checks;
* analysis of the nine experiments;
* comparison between `y_original` and `y_reconstruido`;
* longitudinal profile comparison;
* differences, percentiles, correlations, and amplitudes;
* sensitivity to longitudinal cuts;
* analysis of initial, middle, and final profile regions;
* per-experiment distributions;
* intra-operator repeatability;
* comparison between operators;
* descriptive relationships with deposition conditions;
* modeling implications;
* limitations and methodological decisions;
* export of figures, tables, environment information, and SHA-256 manifests.

The EDA uses the reconstruction and repeatability products as inputs. 

---

## Requirements

The notebooks were developed in Python and use the following main packages:

```text
numpy
pandas
matplotlib
opencv-python
ipython
jupyterlab
```

---

## Project path configuration

The notebooks attempt to locate the project root automatically.

If automatic detection fails, set the project path directly in the notebook:

```python
PROJECT_DIR_OVERRIDE = r"/path/to/project"
```

or define the environment variable:

```text
IC_ML_DED_PROJECT_DIR
```

with the absolute path to the repository root.

---

## Running the project

### Full reconstruction

For a complete reproduction:

1. Clone this repository.
2. Download and extract the three OneDrive folders.
3. Check that the folder names and structure match the layout shown above.
4. Open `y_reconstruction_validation.ipynb`.
5. Run the reconstruction and visually inspect the calibration and boundary-tracking outputs.
6. Open `y_reconstruction_repeatability.ipynb`.
7. Run the repeatability consolidation.
8. Open `eda.ipynb`.
9. Run the complete exploratory analysis.
10. Check the generated manifests and exported results.

### Using the existing reconstruction results

If the goal is only to inspect or reproduce the final analysis, the persisted reconstruction files from the OneDrive `y_reconstruction_validation_results/` folder can be reused instead of repeating all manual annotations.
