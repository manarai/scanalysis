# scanalysis

A student-friendly **Scanpy** Conda environment for basic single-cell RNA-seq preprocessing and **Leiden graph clustering**. It includes **JupyterLab**, so students can clone the repository, create one environment, and start a notebook without separately installing analysis packages.

## What is included

| Workflow step | Packages provided |
|---|---|
| Read 10x matrices and `.h5ad` files | Scanpy, AnnData, h5py |
| Work with count matrices and metadata | NumPy, pandas, SciPy |
| QC, filtering, normalization, log transform, and HVGs | Scanpy, scikit-misc |
| Optional doublet scores | Scrublet |
| PCA and nearest-neighbor graph | Scanpy, scikit-learn, pynndescent |
| Louvain and Leiden graph clustering | python-igraph and leidenalg; Scanpy uses the igraph implementation for Louvain. |
| UMAP and diagnostic plots | umap-learn, matplotlib, seaborn |
| Interactive notebooks | JupyterLab, ipykernel |

> **Keep raw counts unchanged.** Use raw UMI counts for quality-control metrics and Scrublet. Derive normalized/log-transformed data separately. Treat clusters as exploratory results that require biological and technical validation.

## Studio 1 notebook

Run the commented [Studio 1 preprocessing-to-clustering notebook](notebooks/studio1_preprocessing_to_clustering.ipynb) after completing the Quick Start below. It supports one or two Cell Ranger `.h5` files or `.h5ad` objects, uses one small teaching action per code cell, and places each QC cutoff directly after the diagnostic plot it requires. It intentionally performs doublet scoring/review before normalization, PCA, UMAP, and clustering.

## Quick start

### 1. Clone once

This repository is private. Your instructor must give you access before you can clone it.

```bash
git clone https://github.com/manarai/scanalysis.git
cd scanalysis
```

### 2. Create the environment

Use **one** of the following commands. Mamba/Micromamba is typically faster than Conda.

```bash
# Preferred when Mamba is available
mamba env create -f environment.yml

# Or use Conda
conda env create -f environment.yml
```

### 3. Activate the environment

```bash
conda activate scanalysis
```

### 4. Verify the installation

```bash
python - <<'PY'
from importlib.metadata import version
import anndata
import scanpy
import scrublet
import igraph
import leidenalg
import umap

for package in ["scanpy", "anndata", "numpy", "pandas", "scrublet", "leidenalg", "umap-learn", "jupyterlab"]:
    print(f"{package}: {version(package)}")
print("Scanpy preprocessing, Leiden clustering, and JupyterLab are ready.")
PY
```

### 5. Launch JupyterLab

Register the environment as a named Jupyter kernel once:

```bash
python -m ipykernel install --user \
  --name scanalysis \
  --display-name "Python (scanalysis)"

jupyter lab
```

In JupyterLab, select **Kernel → Change Kernel → Python (scanalysis)** before running cells.

## Suggested preprocessing order

1. Load the approved count matrix and retain raw counts.
2. Calculate QC metrics and inspect their distributions.
3. Choose and record dataset-specific cell and gene filters.
4. Optionally score doublets using raw counts, then review the score alongside QC evidence.
5. Normalize total counts, log-transform, and select highly variable genes.
6. Run PCA using documented input features and PCs.
7. Construct a neighbor graph using a documented number of PCs.
8. Run Leiden clustering at documented resolution(s), calculate UMAP, and inspect results.
9. Save the processed object as `.h5ad` together with analysis parameters.

## Updating after a class change

```bash
git pull --ff-only
conda env update --name scanalysis --file environment.yml --prune
```

For reproducibility, record the resolved packages used for an analysis:

```bash
conda env export --name scanalysis --no-builds > scanalysis-resolved.yml
```

## Help

If environment creation fails, copy the complete error output and report your operating system, Conda/Mamba version, and the command you ran to your instructor or course support channel.
