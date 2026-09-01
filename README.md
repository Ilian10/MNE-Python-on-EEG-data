# EEG Analysis in Parkinson's Disease with MNE-Python

This repository contains a Jupyter Notebook workflow for EEG preprocessing, source reconstruction, feature extraction, and machine-learning decoding using [MNE-Python](https://mne.tools/).

The project analyzes EEG data from Parkinsonian participants during two movement conditions, **STOP-IN (SI)** and **PUNCH-THROUGH (PT)**, and compares experimental conditions related to the presence and difficulty/skill of a social partner.

## Objectives

The main goals of the project are to:

- preprocess and organize EEG recordings;
- analyze EEG activity in **sensor space**;
- reconstruct and analyze EEG activity in **source space**;
- study frequency-domain information in the **alpha**, **beta**, and **gamma** bands;
- extract features such as spectral power and correlation/connectivity measures;
- classify experimental conditions using machine-learning methods;
- identify informative EEG features/channels using feature selection.

## Repository structure

```text
MNE-Python-on-EEG-data/
├── FIgures/
│   ├── easy_PT.webm
│   ├── hard_PT.webm
│   ├── solo_PT.webm
│   ├── easy_SI.webm
│   ├── solo_SI.webm
│   ├── hard_SI.webm
├── Scripts/
│   ├── Decoding in sensor's space.ipynb
│   ├── Decoding in source's space.ipynb
│   ├── Download_'fsaverage'_data_from_mne.ipynb
│   ├── Preprocessing sensor's space.ipynb
│   ├── Preprocessing source's space.ipynb
├── Reports/
│   ├── master_thesis_eeg_parkinson.pdf
│   ├── presentation_eeg_parkinson.pptx
├── environment.txt
└── README.md
```

## Analysis workflow

### 1. EEG preprocessing in sensor space

`Scripts/Preprocessing sensor's space.ipynb`

The sensor-space preprocessing notebook includes:

- loading cleaned EEG data stored in MATLAB `.mat` files;
- selection of STOP-IN and PUNCH-THROUGH trials;
- construction of experimental labels;
- removal of invalid, silent, or NaN trials;
- creation of the EEG montage;
- spectral and signal-processing operations;
- extraction of features for subsequent decoding.

The notebook uses a **GSN129** electrode montage and works with EEG data containing 128 analyzed channels.

### 2. Sensor-space decoding

`Scripts/Decoding in sensor's space.ipynb`

This notebook performs decoding of experimental conditions from sensor-space EEG features.

The machine-learning workflow includes methods such as:

- logistic regression;
- grid-search hyperparameter optimization;
- stratified cross-validation;
- recursive feature elimination (RFE);
- k-nearest neighbors;
- random forest classification;
- confusion-matrix analysis;
- feature-ranking stability analysis.

### 3. Downloading the `fsaverage` template

`Scripts/Download_'fsaverage'_data_from_mne.ipynb`

This notebook downloads the MNE-Python **fsaverage** anatomical template used for EEG source modeling when individual MRI data are not available.

> Source localization based on a template MRI is useful for exploratory analysis but is less anatomically precise than reconstruction using each participant's individual MRI.

### 4. EEG preprocessing in source space

`Scripts/Preprocessing source's space.ipynb`

This notebook extends the analysis to source space using MNE-Python source-modeling tools and the `fsaverage` template.

### 5. Source-space decoding

`Scripts/Decoding in source's space.ipynb`

This notebook applies the decoding workflow to source-space features.

## Frequency bands

The analyses focus mainly on:

| Band | Typical frequency range |
|---|---:|
| Alpha | 8-12 Hz |
| Beta | 13-30 Hz |
| Gamma | >30 Hz |

Exact frequency limits should be checked in the corresponding notebooks because they are defined directly in the analysis code.

## Main Python libraries

The project relies primarily on:

- **MNE-Python** - EEG/MEG analysis and source reconstruction;
- **NumPy** - numerical computing and array manipulation;
- **SciPy** - signal processing, MATLAB file loading, and statistics;
- **Matplotlib** and **Seaborn** - visualization;
- **Pandas** - tabular result handling;
- **scikit-learn** - classification, cross-validation, grid search, and feature selection;
- **NetworkX** - graph/network analysis;
- **tqdm** - progress bars;
- **PyVista / PyVistaQt** - 3D visualization support for MNE;
- **NiBabel** - MRI/neuroimaging file support.

## Installation

### Option 1 - `pip`

Clone the repository:

```bash
git clone git@github.com:Ilian10/MNE-Python-on-EEG-data.git
cd MNE-Python-on-EEG-data
```

Create a virtual environment:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Start JupyterLab:

```bash
jupyter lab
```

### Option 2 - Conda

Create the environment from the package list:

```bash
conda create -n mne-eeg -c conda-forge --file environment.txt
conda activate mne-eeg
jupyter lab
```

## Data

The EEG data files used by the notebooks are **not included in the current repository**.

The preprocessing code expects cleaned MATLAB files with names similar to:

```text
dataClean-ICA-<subject>-T1.mat
```

These files contain variables used by the analysis such as EEG data arrays and block-order information.

Because the notebooks were originally developed with local file paths, you may need to update the data paths before running them on another computer.

A recommended project organization is:

```text
MNE-Python-on-EEG-data/
├── data/                 # local EEG data, not committed to Git
├── FIgures/
├── Scripts/
├── results/
├── README.md
└── environment.txt 
```

If the EEG data contain participant information, keep them outside the public repository and follow the applicable data-protection and research-ethics requirements.

## Running the notebooks

A practical execution order is:

```text
1. Download_'fsaverage'_data_from_mne.ipynb   # once, for source-space analysis

Sensor-space pipeline:
2. Preprocessing sensor's space.ipynb
3. Decoding in sensor's space.ipynb

Source-space pipeline:
4. Preprocessing source's space.ipynb
5. Decoding in source's space.ipynb
```

### Important reproducibility note

Some decoding cells use variables produced during preprocessing. If a decoding notebook is executed independently and a variable is undefined, first run the corresponding preprocessing notebook or modify the workflow to save and reload intermediate results explicitly.

It is also recommended to replace machine-specific `os.chdir(...)` calls with paths relative to the repository.

## Reproducibility

The dependency files in this repository were reconstructed from the libraries imported and required by the current notebooks. The original package versions were not recorded in the repository, so the dependency constraints are intended to provide a modern compatible environment rather than reproduce an exact historical environment.

For an exact snapshot after successfully running the notebooks, you can export your installed versions with:

```bash
pip freeze > requirements-lock.txt
```

or, with Conda:

```bash
conda env export --from-history > environment.yml
```

## Project outputs

The repository also contains:

- `master_thesis_eeg_parkinson.pdf` - project/master thesis report;
- `presentation_eeg_parkinson.pptx` - oral presentation;
- `FIgures/` - figures generated for the analysis and project documentation.

## Author

**Ilian Salmi**

GitHub: [@Ilian10](https://github.com/Ilian10)

## Acknowledgements

This project uses the open-source [MNE-Python](https://mne.tools/) ecosystem for EEG processing, visualization, and source modeling.
