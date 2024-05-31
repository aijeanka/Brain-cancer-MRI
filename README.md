# Brain Cancer Classification using TCGA and REMBRANDT MRI Data

## Project Overview

This project focuses on classifying different types of brain tumors using MRI data from TCGA and REMBRANDT datasets. The process involves data merging, cleaning, encoding, feature extraction using Pyradiomics, and training machine learning models to predict the cancer type.

## Table of Contents
1. [Project Overview](#project-overview)
2. [Data Preprocessing Steps](#data-preprocessing-steps)
3. [Overview of the Models Used](#overview-of-the-models-used)
4. [Training Results](#training-results)
5. [Testing on Unseen Data](#testing-on-unseen-data)
6. [Conclusion](#conclusion)

## 1. Project Overview

This data science project aims to classify different types of brain tumors (Astrocytoma, GBM, Oligodendroglioma) using radiomic features combined with clinical data. The steps include data merging, cleaning, encoding, feature extraction, and training separate models for each disease type. The results include accuracy metrics and detailed classification reports for each model.

### Libraries Used

- Pandas: For data manipulation and analysis.
- NumPy: For numerical operations on arrays.
- SimpleITK: To handle medical images in conjunction with radiomics.
- PyRadiomics: For the extraction of radiomics features from medical images.
- Joblib: To serialize Python objects.
- Imbalanced-Learn: For handling imbalanced datasets.
- XGBoost: For model training and prediction.

## 2. Data Preprocessing Steps

### Data Loading
- Clinical data is loaded from an Excel file into a pandas DataFrame.
- Radiomics data is loaded from a CSV file into another DataFrame.

### Data Merging
- The radiomics and clinical data are merged on the patient ID column.
- The `Row.names` column is dropped after the merge.

### Data Encoding
- The `Gender` column is encoded into integers, with 'MALE' as 0 and 'FEMALE' as 1.
- The `Race` column is mapped to integers for different races, with 'UNKNOWN' as 4 and missing values filled with 0.

### Handling Missing Values
- Any remaining NaNs in the DataFrame are filled with 0.

### Descriptive Statistics
- Descriptive statistics for select columns in the DataFrame are calculated and displayed.

### Value Counting
- The script counts and displays the number of instances for each unique value in the `Disease_Type` column.

### Target: Disease_Type
- The 'Disease_Type' column is the target, classifying three categories of brain tumors: 'GBM', 'Astrocytoma', and 'Oligodendroglioma'.
- The disease type column is encoded using one hot encoding.

## 3. Overview of the Models Used

### I. SVM (Support Vector Machine)
- Initial SVM models are used, followed by enhanced versions using SMOTE to address data imbalance.
- Hyperparameter optimization via GridSearchCV and RandomizedSearchCV.

### II. AdaBoost with SVM
- AdaBoostClassifier with SVM as the base classifier is employed to improve model performance, especially for handling imbalanced classes.

### III. Isolation Forest
- Implemented for anomaly detection in 'Astrocytoma', distinguishing outliers crucial for identifying rare or atypical cases.

### IV. Stacking Classifier
- Combines SVM and AdaBoost under a Logistic Regression final estimator, leveraging the strengths of each model to enhance predictions.

### V. XGBoost
- Utilizes XGBoost for robust classification across each disease type, focusing on managing various data dimensions and complexities.

## 4. Training Results

### Accuracy and Performance Metrics
- Accuracy, confusion matrices, and classification reports are the primary metrics used to evaluate model performance.
- The multilabel approach achieved better results compared to the multiclass framing.

### Best Performing Models
- Disease_Type_GBM: Consistently strong performance with accuracy rates up to 80%.
- Disease_Type_Oligodendroglioma: High performance with accuracy up to 90% in some models.

### Detailed Analysis of the Models

#### I. SVM Models
- Initial setup and enhanced versions using SMOTE and hyperparameter optimization.
- Post Grid Search Optimization and Random Search Optimization showed improved accuracy.

#### II. AdaBoost with SVM
- Improved performance by aggregating the predictive power of multiple weak classifiers.
- Achieved high accuracy and robust performance across various datasets.

#### III. Isolation Forest
- Effective for the majority class but struggled with the minority class (True) for Astrocytoma.

#### IV. Stacking Classifier
- Strong performance, particularly for GBM, with the highest accuracy among models.

#### V. XGBoost
- Highly accurate for Disease_Type_Astrocytoma but faced challenges in predicting the True class across all disease types.

### Model Most Important Features
- Key features identified include diagnostics_Mask-original_VolumeNum, original_glcm_Idmn, original_shape_Sphericity, and others.

## 5. Testing on Unseen Data

### Data Preparation
- Features from the new dataset were loaded, and unnecessary columns were removed.
- Clinical ground truth data was loaded, and the `Disease_Type` was one-hot encoded.

### Model Application and Results
- Both SVM and XGBoost models were applied to the unseen data.
- The SVM model showed varying performance, with the highest accuracy for Oligodendroglioma (0.734375).
- XGBoost also performed best for Oligodendroglioma (0.796875) but had lower accuracy for GBM and Astrocytoma.

## 6. Conclusion

SVM and XGBoost models demonstrated varying success across disease types. Both models struggled with GBM but performed moderately well for Astrocytoma and showed strong predictive power for Oligodendroglioma. Further model refinement and additional data could enhance performance, especially for underrepresented classes.
