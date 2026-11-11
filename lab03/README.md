# Lab 03 – Principal Components Analysis (PCA)

Solutions for the PCA exercise sheet of the course *Analytics and Knowledge Discovery* (FH Salzburg).

The lab applies PCA to three very different data sets: a small table of superheroes, images of handwritten digits and a
large, sparse matrix of movie ratings. For each one it covers data understanding, whether to normalize, how many dimensions
are needed and how to interpret the result.
Each exercise has its own Jupyter notebook. The code is kept simple, and every step is explained in Markdown.

## Contents

| Notebook | Data | Main result |
|---|---|---|
| [`PC1_superheroes_pca.ipynb`](solutions/PC1_superheroes_pca.ipynb) | 58 Marvel characters × 7 attributes | Standardize, because without it `Health` makes up PC1. PC1+PC2 = 57 %, 90 % needs 5 PCs. PC1 = overall physical power, PC2 = agile/fast vs. tall/heavy. |
| [`PC2_mnist_pca.ipynb`](solutions/PC2_mnist_pca.ipynb) | MNIST, 10 000 images × 784 pixels | No standardization (same scale), only divide by 255. PC1+PC2 = 17 %, 90 % needs 86 PCs. In 2-D only 0 and 1 stand apart, and kNN gets 42 % right. |
| [`PC3_movies_pca.ipynb`](solutions/PC3_movies_pca.ipynb) | MovieLens, 610 users × 9 724 movies | 98 % missing, so each user is centered and gaps are set to 0. PC1+PC2 = 8 %, 90 % needs about 250 PCs. PC1 = acclaimed classics vs. weak blockbusters (r = 0.87 with the average rating). The biplot is of limited use. |

📄 **Summary:** [`Lab03_PCA_Summary.pdf`](solutions/Lab03_PCA_Summary.pdf) gives the key points of the lab with short explanations.

## Project structure

```
lab03/
├── AKD_VO06 tSNE.pdf          # exercise sheet (PCA)
├── superheros.csv             # PC1: 58 x 7, no header
├── superherosReadme.txt       # PC1: hero names and variable names
├── ml-latest-small/           # PC3: MovieLens (ratings.csv, movies.csv, ...)
├── data/sklearn_cache/        # PC2: MNIST, downloaded automatically on first run
└── solutions/
    ├── PC1 … PC3 notebooks
    └── Lab03_PCA_Summary.pdf
```

## How to run

Requirements: Python 3.10 or newer.

```bash
pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open the notebooks in the `solutions/` folder. They load the data with relative paths (`../…`), so keep the folder layout as it is.
PC2 downloads MNIST from OpenML (about 50 MB) the first time it runs; you need an internet connection for that.

## Key points

- **Recipe:** data understanding / EDA → normalize? → PCA → choose p̃ from the cumulative explained variance (90–95 %) → interpret with the loadings and the biplot.
- **Normalization:** standardize when variables have different units or scales (superheroes). Don't standardize when all variables share the same meaning and range (pixels, ratings), because it would inflate columns that hardly vary.
- **Number of dimensions:** PCA compresses well only when the variables are strongly *linearly* correlated. Weak correlations (superheroes) or individual tastes (movies) mean many PCs are needed.
- **2-D plots:** a 2-D projection is only reliable if PC1+PC2 explain a large share of the variance. MNIST at 17 % shows heavy overlap of the digits, and non-linear methods like t-SNE do better.
- **Biplots** work for few variables and high explained variance. With hundreds of variables they become unreadable.
