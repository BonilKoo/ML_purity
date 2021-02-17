# Tumor purity prediction from RNA sequencing-based gene expression data
The machine learning models to estimate tumor purity trained on TCGA RNA sequencing-based gene expression data. Bulk tumor samples used for high-throughput molecular profiling are often an admixture of cancer cells and non-cancerous cells. The proportion of tumor cells in the admixture is refer to as tumor purity. The mixed composition can confound the analysis and affect the biological interpretation of the results, and thus, accurate prediction of tumor purity is critical.

## Data preparation
To use the models, log-transformed values of quantified FPKM (log2(FPKM+1)) are required. The FPKM values shoud be calculated through the mRNA analysis pipeline of the GDC (https://docs.gdc.cancer.gov/Data/Bioinformatics_Pipelines/Expression_mRNA_Pipeline/).

In addition, the order of genes should be arranged in the same order as the example data. (The gene lists are uploaded in GeneList directory.)

## Usage
The example ipython notebook (ipynb) file is in the example directory. Please refer it.

scikit-learn (<= 0.23.2) must be installed to scale input data.
```
import pandas as pd
import joblib
from keras.models import load_model # when using MLP model

# Load your gene expression (FPKM) data as numpy array (sample x gene).
example_data = pd.read_csv('example_data.tsv', sep='\t', index_col='Sample ID')
X = example_data.values

# Data scaling is needed except for RFR model
Scaler = joblib.load('../models/Scaler/Scaler.joblib')
X_scaled = Scaler.transform(X)

Ridge = joblib.load('../models/Ridge/Ridge.joblib')
RFR = joblib.load('../models/RFR/RFR.joblib')
MLP = load_model('../models/MLP/MLP.h5') # When using the MLP models, use function 'load_model' for loading the model.

Ridge_purity = Ridge.predict(X_scaled)
RFR_purity = RFR.predict(X) # When using the RFR models, use not scaled data.
MLP_purity = MLP.predict(X_scaled).reshape(-1)
```
