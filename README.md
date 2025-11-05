# Temperature Prediction with Apache Spark

## 1.Project Overview
This project implements a large-scale machine learning pipeline using PySpark MLlib to predict global air temperature. The system processes over 120 million meteorological records from Google Cloud Storage (GCS) and evaluates two distinct regression models.

This repository contains all materials for the submission as required by `MLlib.pdf`:
1. **Source Code**: The complete PySpark implementation (`spark_temp_predict_cloud_with_results.ipynb`)
2. **README**: This file, explaining the setup and execution.
3. **Trained Models**: The saved Spark models (`lr_cv_model.zip`, `rf_cv_model.zip`), available in the GitHub Releases or the Canvas submission .zip.
4. **Report**: The final analysis in PDF format (`DSA5208_pj2_MLlib.pdf`).
- Author (Code & Pipeline): Jiayu Shen (A0329922X)
- Technical Stack: Google Cloud Dataproc, PySpark 3.5.3, Spark MLlib, GCS

## 2. Pipeline Methodology
The core logic in the PySpark notebook defines an end-to-end pipeline. It loads over 120M records from GCS, performs specialized cleaning based on NOAA documentation (parsing TMP, WND, etc.), and engineers temporal/cyclical features. The processed DataFrames (85M train, 36M test) are cached in memory. Finally, a 3-fold CrossValidator is used to train and tune Linear Regression and Random Forest models, with the final artifacts saved to disk for persistence.

## 3.How to Run
This project is designed to run on a Google Cloud Dataproc cluster.  

**Step 1: Create the Dataproc Cluster**  
Create a cluster using the gcloud command with the exact image version and machine types used for this project:
```
gcloud dataproc clusters create cluster-spark-mem \
    --region us-central1 \
    --image-version 2.2-debian12 \
    --master-machine-type n4-standard-4 \
    --master-boot-disk-size 100 \
    --num-workers 2 \
    --worker-machine-type n4-standard-8 \
    --worker-boot-disk-size 100 \
    --optional-components JUPYTER \
    --enable-component-gateway
```
**Step 2: Access the Notebook**  

Use the Dataproc Component Gateway to access the JupyterLab interface on the master node and upload the `spark_temp_predict_cloud_with_results.ipynb` notebook.  

**Step 3: Configure and Run**  

Open the notebook.  
In Cell 1, ensure the RUN_MODE variable is set to "CLOUD":
```
RUN_MODE = "CLOUD"
```
Ensure the GCS data path in Cell 1 is correct (it is set by default):
```
df = spark.read.csv(
    "gs://dsa5208--project--2--data/csv_data/*.csv", 
    header=True, 
    inferSchema=True
)
```
Run all cells sequentially (Cells 1 through 10).

## 4. Model Artifacts
The final trained models (Linear Regression and Random Forest) are saved in Spark ML format and compressed.
```
lr_cv_model.zip
```
```
rf_cv_model.zip
```
These files are included in the Canvas submission and are also available in the repository's **"Releases"** tab.  

## 5. Results and Analysis

A detailed analysis of the results, model performance comparison (RMSE, R², MAE), hyperparameter selection, and feature importance analysis is available in the official project report:
```
DSA5208_pj2_MLlib.pdf
```
