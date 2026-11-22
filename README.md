# proteomic_MLGo
Small project where I analyze a public proteomics dataset (doi.org/10.1016/j.ccell.2022.06.010) using Python, fitting a machine learning algorithm (classiefier) on it and extracting Go-Terms from the most important features (proteins) for GO-Terms Enrichment Analysis. Finally I identified certain biological functions of interest for determining the tissue of origin of certain cancer cell lines.

AI WARNING: I openly declare that this project contains code generated with Artificial Intelligence/Super Intelligence.
While the project structure, methods and workflow was determined and selected by me, some parts of the code have been generated with Artificial Intelligence to aid in translation of my ideas into actuality, but also to drastically reduce production time and improve on the results by assessing performance of multiple models easily and reproducibly.

...........................................................
---

## Prediction of Tissue of Origin in Cancer Samples

### Overview & Aim

This project uses machine learning to predict the tissue of origin of cancer cell lines based on their proteomic profiles. Using a publicly available mass-spectrometry dataset covering 6,692 proteins across 949 cancer cell lines, the goal is to identify tissue- and tumor-specific proteomic markers that could ultimately help track the origin of metastases.

### Methodology

* **Data Cleaning:** Dropped 3,303 protein columns with >50% missing values to reduce statistical noise, imputed remaining `NaN` values with `0` (representing theoretical mean abundance), and filtered out tissue classes with fewer than 10 cell lines.


* **Pipeline & Preprocessing:** Performed a stratified 70/30 train-test split, followed by a scikit-learn pipeline incorporating Z-score standardization, Principal Component Analysis (reducing 3,266 features to 433 principal components explaining 95% of variance), and a custom transformer to remove highly correlated proteins. Note that using PCA was an attempt at reducing the dimensionality of the data but I ended up continuing the analysis on the larger dataset (3,266 proteins).


* **Modeling & Tuning:** Benchmarked four classifiers (Logistic Regression, XGBoost, Bagging Random Forest, and SVM) via cross-validation, then optimized the top candidates using Randomized Search CV (30 iterations, 3-fold CV).


* **Biological Interpretation:** Extracted the top 500 most important proteins (festures), found their corresponding Go-Terms and ran Gene Ontology (GO) enrichment analysis against the full dataset background.



### Key Results
### Confusion Matrices:
<img width="1736" height="790" alt="image" src="https://github.com/user-attachments/assets/92571997-6754-4c42-b274-06d5e9958054" />
"Confusion Matrix for the top scorer model, in this case LogisticRegressor from SciKit-learn."

<br>
<br>

### Go-Term Enrichment analysis:
<img width="2490" height="1790" alt="image" src="https://github.com/user-attachments/assets/26fa326b-4828-4edb-8faa-1cddaa28b93c" />
"Top 30 enriched Go-Terms, they showcase a distinct preference for distinct biological functions which could discern various tissue types."

<br>
<br>

**Machine Learning Performance**

* **Model Comparison:** Logistic Regression outperformed the other classifiers in initial cross-validation (macro F1: **0.50 ± 0.05**), followed by XGBoost (macro F1: **0.44 ± 0.03**).


* **Hyperparameter Tuning:** Tuning improved the Logistic Regression classifier to a final **macro F1 score of 0.58**.


* **Limitations & Next Steps:** In light of the strong class-specific predictions (e.g., 100% accuracy on Peripheral Nervous System samples), the model showed overfitting—likely driven by high dimensionality and L2 regularization (does not drop features but shrinks their coefficients towards zero). Future iterations should explore L1 regularization (dropping features), retaining correlated protein networks and l2/l1 regularization, or deeper XGBoost tuning.



**GO-Term Enrichment Analysis**

* **Top Enriched Processes:** The model prioritized proteins involved in tissue-specific functions and migratory/metastatic behavior, including *membrane-to-membrane docking*, *sensory perception of light stimulus*, *visual perception*, *vascular transport*, and *heterotypic cell-cell adhesion*.


* **Depleted Processes (Fold Enrichment < 1):** The model actively ignored broad, ubiquitous metabolic processes that are commonly altered across all cancer types as *maturation of SSU-rRNA*, *RNA export from the nucleus*, and *translation initiation*.


* **Biological Takeaway:** Rather than relying on generic metabolic phenotypes, the classifier successfully isolated genuine, tissue-specific proteomic differences within the cancer cell lines.

<br>
<br>

-----------------------------------------------------------


### How to run the code yourself:

If you desire to rerun the google colab yourself, you must have the correct data on your google drive and the colab must be able to see and read said data.

1. From the figshare entry of the data featured in this article (1 Gonçalves E et al., 2022), visible at: (https:figshare.com/articles/dataset/Pan-cancer_proteomic_map_of_949_human_cell_lines/19345397?file=34411133)
   - Download the mapping file: "ProCan-DepMapSanger_mapping_file_averaged.txt"
   - Download the data: "ProCan-DepMapSanger_protein_matrix_6692_averaged.txt"

  The proteomics data that must be downloaded also NEEDS THE MAPPING FILE as the data is encoded in a way where certain necessary information (cancer cell line, tissue of origin etc.) is not directly present in the
  proteomics data file.
  
2. The code in the colab notebook, by default, will try and read both files from your Google Drive directory: "/content/drive/MyDrive/data/" using their default file name:
  - Either upload the downloaded data to this directory ("/content/drive/MyDrive/data/") on your own Google Drive
  - Modify the code cell titled "Data Paths" at the start of the colab notebook, to read the mapping and data files from a different directory on your Google Drive
  - Download a local copy of the code, and manually edit the "Data Paths" code cell, to read the necessary files from your machine:
      - In this case you will need to download and install all the necessary Python libraries manually and troubleshoot yourself
    
3. Press Run and wait...

<br>
<br>

-----------------------------------------------------------

Sources:
1. Gonçalves E, Poulos R, Cai Z et al., Pan-cancer proteomic map of 949 human cell lines, Cancer Cell, 2022; 40, 835-849.e8; DOI: doi.org/10.1016/j.ccell.2022.06.010
